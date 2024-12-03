---
title: 🧠 记录并行训练并且定制一份自己特色的代码
summary: 'PyTorch DDP training'.
date: 2024-12-03
authors:
  - admin
tags:
  - Academic
  - Technology
  - AI
image:
  caption: 'Image credit: [**Unsplash**](https://unsplash.com)'
---


```
import os
import torch
import torch.nn as nn
import torch.optim as optim
from torch.utils.data import Dataset, DataLoader
from torch.utils.data.distributed import DistributedSampler
import torch.distributed as dist
from torch.nn.parallel import DistributedDataParallel

def setup():
    dist.init_process_group('nccl')

def clearnup():
    dist.destroy_process_group()

class ToyModel(nn.Module):
    def __init__(self, *args, **kwargs):
        super().__init__(*args, **kwargs)
        self.layer = nn.Linear(1, 1)
    
    def forward(self, x):
        return self.layer(x)

class ToyDataset(Dataset):
    def __init__(self):
        super().__init__()
        self.data = torch.tensor([1, 2, 3, 4], dtype=torch.float32)
    
    def __len__(self):
        return len(self.data)
    
    def __getitem__(self, index):
        return self.data[index, index+1]

ckpt_path = ''

def main():
    setup()
    rank = dist.get_rank()
    pid = os.getpid()
    print(f'current pid is {pid}')
    print(f'current rank is {rank}')
    deviced_id = rank % torch.cuda.device_count()

    dataset = ToyDataset()
    sampler = DistributedSampler(dataset=dataset)
    dataloader = DataLoader(dataset, batch_size=2, sampler=sampler)

    model = ToyModel().to(deviced_id)
    model = torch.nn.SyncBatchNorm.convert_sync_batchnorm(model) # 设置多个gpu的BN同步
    ddp_model = DistributedDataParallel(model, device_ids=[deviced_id])

    loss_fn = nn.MSELoss()
    optimizer = optim.SGD(ddp_model.parameters(), lr=0.001)

    if rank == 0:
        torch.save(ddp_model.state_dict(), ckpt_path)
    dist.barrier()

    map_location = {'cuda:0': f'cuda:{deviced_id}'}
    state_dict = torch.load(ckpt_path, map_location=map_location)
    print(f'rank {rank}: {state_dict}')
    ddp_model.load_state_dict(state_dict)

    for epoch in range(2):
        sampler.set_epoch(epoch) 
        """这句莫忘，否则相当于没有shuffle数据, dataloader设置方式如下，注意shuffle与sampler是冲突的，并行训练需要设置sampler，此时务必要把shuffle设为False。
        但是这里shuffle=False并不意味着数据就不会乱序了，而是乱序的方式交给sampler来控制，实质上数据仍是乱序的。
        """
        for x in dataloader:
            print(f'epoch {epoch}, rank{rank} data: {x}')
            x = x.to(deviced_id)
            y = ddp_model(x)
            
            optimizer.zero_grad()
            loss = loss_fn(x, y)
            loss.backward()
            optimizer.step()
    
    clearnup()

if __name__ == "__main__":
    main()
```

- 假设有4张卡，使用第三和第四张卡的并行运行命令（torch v1.10 以上）：
    ```
    export CUDA_VISIBLE_VERSION=2,3 torchrun --nproc_per_node=2 dldemos/PyTorchDistributed/main.py
    ```

- 较老版本的PyTorch应使用下面这条命令（这种方法在新版本中也能用，但是会报Warning）：
    ```
    export CUDA_VISIBLE_VERSION=2,3 python -m torch.distributed.launch --nproc_per_node=2 dldemos/PyTorchDistributed/main.py
    ```
这份代码演示了一种较为常见的PyTorch并行训练方式：一台机器，多GPU。一个进程管理一个GPU。每个进程共享模型参数，但是使用不同的数据，即batch size扩大了GPU个数倍。

**为了实现这种并行训练,需要解决以下几个问题：**

1. 怎么开启多进程？
    ```
    torchrun --nproc_per_node=GPU_COUNT main.py
    ```
    去跑这个脚本，就能用GPU_COUNT个进程来运行这个程序，每个进程分配一个GPU。我们可以用dist.get_rank()来查看当前进程的GPU号。

2. 模型怎么同步参数与梯度？

    接下来，我们来解决数据并行的问题。我们要确保一个epoch的数据被分配到了不同的进程上，以实现batch size的扩大。在PyTorch中，只要在生成Dataloader时把DistributedSampler的实例传入sampler参数就行了。DistributedSampler会自动对数据采样，并放到不同的进程中。我们稍后可以看到数据的采样结果。

3. 数据怎么划分到多个进程中？

    接下来来看模型并行是怎么实现的。在这种并行训练方式下，每个模型使用同一份参数。在训练时，各个进程并行；在梯度下降时，各个进程会同步一次，保证每个进程的模型都更新相同的梯度。PyTorch又帮我们封装好了这些细节。我们只需要在现有模型上套一层DistributedDataParallel，就可以让模型在后续backward的时候自动同步梯度了。其他的操作都照旧，把新模型ddp_model当成旧模型model调用就行。

    sampler自动完成了打乱数据集的作用。因此，在定义DataLoader时，不用开启shuffle选项。而在每个新epoch中，要用sampler.set_epoch(epoch)更新sampler，重新打乱数据集。通过输出也可以看出，数据集确实被打乱了。

这里还有一个重要的细节。使用DistributedDataParallel把model封装成ddp_model后，模型的参数名里多了一个module。这是因为原来的模型model被保存到了ddp_model.module这个成员变量中（model == ddp_model.module）。在混用单GPU和多GPU的训练代码时，要注意这个参数名不兼容的问题。最好的写法是每次存取ddp_model.module，这样单GPU和多GPU的checkpoint可以轻松兼容。

**参考资料**

https://zhouyifan.net/2022/12/19/20221029-torch-parallel-training/