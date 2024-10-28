# 安装DeepStream

---

### 安装依赖

```Shell
$ sudo apt install libjson-glib-dev libjansson4 libvtk6-dev python-vtk6 openssl libssl-dev ffmpeg libx11-dev gstreamer1.0-plugins-base gstreamer1.0-plugins-good gstreamer1.0-plugins-bad gstreamer1.0-plugins-ugly gstreamer1.0-libav gstreamer1.0-doc gstreamer1.0-tools gstreamer1.0-x gstreamer1.0-alsa gstreamer1.0-gl gstreamer1.0-gtk3 gstreamer1.0-qt5 gstreamer1.0-pulseaudio gstreamer1.0-rtsp libgstreamer-plugins-base1.0-dev libgstreamer1.0-0 libgstreamer1.0-dev libgstrtspserver-1.0-0 libgstrtspserver-1.0-dev 
```

```Shell
sudo apt-get install libv4l-dev
cd /usr/include/linux
sudo ln -s ../libv4l1-videodev.h videodev.h
```

---

### 提前下好deepstream6

下载地址：[https://developer.nvidia.com/deepstream-getting-started](https://developer.nvidia.com/deepstream-getting-started)

```Shell
sudo tar -zxvf deepstream_sdk_v6.0.0_x86_64.tbz2 -C /
```

---

### 命令行安装gcc

```Shell
sudo apt-get install build-essential
```

### 源码编译安装gcc

下载地址：[http://mirrors.nju.edu.cn/gnu/gcc/gcc-7.5.0/](http://mirrors.nju.edu.cn/gnu/gcc/gcc-7.5.0/)

#### 安装依赖：

下载其他依赖文件，从这个地址都能找到:[http://www.mirrorservice.org/sites/sourceware.org/pub/gcc/infrastructure/](http://www.mirrorservice.org/sites/sourceware.org/pub/gcc/infrastructure/)

- gmp-6.1.0.tar.bz2

- mpfr-3.1.4.tar.bz2

- mpc-1.0.3.tar.gz

- isl-0.16.1.tar.bz2
每个依赖都使用相同的安装步骤，如下：

```Shell
./configure
make -j2
sudo make install
```

#### 编译安装gcc

```Shell
编译安装gcc
./configure --enable-checking=release --enable-languages=c,c++ --disable-multilib
make -j128
sudo make install

如果出现gcc和g++版本不一致的问题,很可能是配置的问题,因为目前系统里有多个gcc了,调用如下命令,即可解决
/usr/local/bin/gcc是我的安装路径

sudo update-alternatives --install /usr/bin/gcc gcc /usr/local/bin/gcc  100
sudo update-alternatives --install /usr/bin/gcc gcc /usr/local/bin/g++  100
```

---

### 安装Cmake

下载地址：[https://cmake.org/files/](https://cmake.org/files/)

```Shell
tar -xvf cmake-3.14.5.tar
cd cmake-3.14.5
./bootstrap 
make
make install
```

---

### TensorRT-8.0.1.6

下载地址：[https://developer.nvidia.com/nvidia-tensorrt-download](https://developer.nvidia.com/nvidia-tensorrt-download)

#### 安装pycuda

```Shell
# 第一种方式 手动下载安装<https://www.lfd.uci.edu/~gohlke/pythonlibs/#pycuda>
pip install {pycuda}.whl
# 第二种方式 直接pip
pip install pycuda==2021.1
```

#### 安装tensorrt

```Shell
#在home下新建文件夹，命名为tensorrt_tar，然后将下载的压缩文件拷贝进来解压
tar xzvf TensorRT-8.0.1.6.Linux.x86_64-gnu.cuda-11.3.cudnn8.2.tar.gz
 
#解压得到TensorRT-8.0.1.6的文件夹，将里边的lib和include添加都系统搜索路径中
mv TensorRT-8.0.1.6/ /usr/local/
mkdir /usr/lib/TensorRT-8.0.1.6
mkdir /usr/include/TensorRT-8.0.1.6
sudo ln -s /usr/local/TensorRT-8.0.1.6/lib /usr/lib/TensorRT-8.0.1.6/lib
sudo ln -s /usr/local/TensorRT-8.0.1.6/include /usr/include/TensorRT-8.0.1.6/include

# 安装TensorRT python API
cd /usr/local/TensorRT-8.0.1.6/python
pip install tensorrt-8.0.1.6-py2.py3-none-any.whl
 
# 安装UFF,支持tensorflow模型转化
cd TensorRT-8.0.1.6/uff
pip install uff-0.5.5-py2.py3-none-any.whl
 
# 安装graphsurgeon，支持自定义结构
cd TensorRT-8.0.1.6/graphsurgeon
pip install graphsurgeon-0.3.2-py2.py3-none-any.whl

#配置
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/home/sdw/TensorRT-8.4.3.1/lib
```

---

### 安装librdkafka

```Shell
git clone https://github.com/edenhill/librdkafka.git
cd librdkafka
git reset --hard 7101c2310341ab3f4675fc565f64f0967e135a6a
./configure
make
sudo make install
sudo cp /usr/local/lib/librdkafka* /opt/nvidia/deepstream/deepstream-6.0/lib
```

---

### 源码编译GPU版OpenCV

** 使用源码安装opencv，注意要带cuda和gstreamer，这一步容易出现问题，将一些关键步骤做些说明，按照下边指令安装 **

#### 安装依赖

```Shell
sudo apt-get install libavcodec-dev libavformat-dev libavdevice-dev ffmpeg
```

#### 编译OpenCV

```Shell
wget https://github.com/opencv/opencv/archive/3.4.0.zip -O opencv-3.4.0.zip
unzip opencv-3.4.0.zip
cd opencv-3.4.0
wget https://github.com/opencv/opencv_contrib/archive/3.4.0.zip -O opencv_contrib-3.4.0.zip
unzip opencv_contrib-3.4.0.zip 
mkdir build && cd build

cmake -D CMAKE_BUILD_TYPE=Release -D CMAKE_INSTALL_PREFIX=/usr -D BUILD_PNG=OFF -D BUILD_TIFF=OFF -D BUILD_TBB=OFF -D BUILD_JPEG=OFF  -D BUILD_JASPER=OFF -D BUILD_ZLIB=OFF -D BUILD_EXAMPLES=OFF -D BUILD_opencv_java=OFF -D BUILD_opencv_python2=ON -D BUILD_opencv_python3=ON -D ENABLE_PRECOMPILED_HEADERS=OFF -D WITH_OPENCL=OFF -D WITH_OPENMP=OFF -D WITH_FFMPEG=ON -D WITH_GSTREAMER=ON -D WITH_CUDA=ON -D WITH_GTK=ON -D WITH_VTK=ON -D WITH_TBB=ON -D WITH_1394=OFF -D WITH_OPENEXR=OFF -D OPENCV_EXTRA_MODULES_PATH=/opt/opencv-3.4.0/opencv_contrib-3.4.0/modules  -D CUDA_TOOLKIT_ROOT_DIR=/usr/local/cuda -D CUDA_ARCH_BIN=7.5 -D INSTALL_C_EXAMPLES=ON -D INSTALL_TESTS=OFF ..

make -j128
sudo make install
```

查看显卡CUDA_ARCH_BIN

[https://developer.nvidia.com/zh-cn/cuda-gpus#compute](https://developer.nvidia.com/zh-cn/cuda-gpus#compute)

- 方法1：利用cuda_sample查看

```Shell

cd /usr/local/cuda/samples/1_Utilities/deviceQuery
sudo make
./deviceQuery
```

输出其中的** CUDA Capability Major/Minor version number:    7.5 **

- 方法2：git下载代码编译

```Shell
git clone https://github.com/NVIDIA-AI-IOT/deepstream_tlt_apps.git
cd deepstream_tlt_apps/TRT-OSS/x86
nvcc deviceQuery.cpp -o deviceQuery
./deviceQuery
```

#### 检测版本

```Shell
pkg-config --modversion opencv
pkg-config --cflags --libs opencv
# or
python
import cv2
cv2.getBuildInformation()
```

#### 安装编译好的opencv python库

```Shell
cd opencv-3.4.0/build/python_loader/
python setup.py install
```

#### 可能的报错

```Plain Text
1.找不到gst/gstxxx.hpp问题
在/usr/include下新建软连接
$ cd /usr/include
$ sudo ln -s gstreamer-1.0/gst/ gst

2.dynlink_nvcuvid.h问题
cuda10不再提供dynlink_nvcuvid.h功能，修改opencv-3.4.0/modules/cudacodec/src目录下的文件

modules/cudacodec/src/precomp.hpp
modules/cudacodec/src/video_decoder.hpp
modules/cudacodec/src/video_parser.hpp
modules/cudacodec/src/cuvid_video_source.hpp
modules/cudacodec/src/frame_queue.hpp
找到下边的代码

#if CUDA_VERSION >= 9000
#include <dynlink_nvcuvid.h>
#else
#include <nvcuvid.h>
#endif
替换为

#if CUDA_VERSION >= 9000 && CUDA_VERSION < 10000
#include <dynlink_nvcuvid.h>
#else
#include <nvcuvid.h>
#endif
如果改完还报错的话，需要下载 nvidia-sdk，下载链接：https://developer.nvidia.com/designworks/video_codec_sdk/downloads/v9.0

下载解压后将其中的 nvcuvid.h, cuviddec.h copy 到 /usr/local/cuda/include

然后重新make即可 

3.ippicv_2017u3_lnx_intel64_general_20170822.tgz下载慢问题
https://github.com/opencv/opencv_3rdparty/tree/ippicv/master_20170822/ippicv
查看build目录下的CmakeDownloadLog.txt，里边记录了下载地址，我这里的路径是

use_cache "/home/nvidia/opencv_3.4.0/opencv-3.4.0/.cache"
do_unpack "ippicv_2017u3_lnx_intel64_general_20170822.tgz" "4e0352ce96473837b1d671ce87f17359" "https://raw.githubusercontent.com/opencv/opencv_3rdparty/dfe3162c237af211e98b8960018b564bc209261d/ippicv/ippicv_2017u3_lnx_intel64_general_20170822.tgz" "/home/nvidia/opencv_3.4.0/opencv-3.4.0/build/3rdparty/ippicv"
#check_md5 "/home/nvidia/opencv_3.4.0/opencv-3.4.0/.cache/ippicv/4e0352ce96473837b1d671ce87f17359-ippicv_2017u3_lnx_intel64_general_20170822.tgz"

可以看到是把这个文件下载到/home/nvidia/opencv_3.3.1/opencv-3.3.1/.cache/ippicv/下，并命名为4e0352ce96473837b1d671ce87f17359-ippicv_2017u3_lnx_intel64_general_20170822.tgz，我们只需要把手动下载的文件然后复制到该路径下即可

4 cblas_xxx问题
如果出现编译问题“undefined reference to `cblas_sgemm(CBLAS_ORDER, CBLAS_TRANSPOSE, CBLAS_TRANSPOSE, int, int, int, float, float const, int, float const, int, float, float*, int)'”

可能需要重新编译BLAS，CBLAS，LPACK，参考链接《blas、lapack、cblas在Ubuntu上的安装》<https://www.jianshu.com/p/33c4aea6117b>

5 boostdesc_bgm.i等问题
在opencv_contrib目录中报错fatal error: boostdesc_bgm.i: No such file or directory

这个跟2.4.2类似，是有些包下载不成功导致的，可以直接下载https://github.com/nanmi/myresource/tree/main/opencv_3rdparts/下面的资源，参考链接：<https://github.com/opencv/opencv_contrib/issues/1301>，然后拷贝到opencv_contrib/modules/xfeatures2d/src/ 路径下，重新编译即可

6 多版本问题
如果之前安装过其它版本没有卸载，可以采用《Ubuntu下多个版本OpenCV管理（Multiple Opencv version）》来指定编译时用到的opencv版本，连接：<https://www.cnblogs.com/cmt/p/14580194.html?from=https%3A%2F%2Fwww.cnblogs.com%2Fxzd1575%2Fp%2F5555523.html>

7 opencv2/xfeatures2d/cuda.hpp问题
修改/opencv-3.4.0/modules/stitching/CMakeLists.txt文件，在开头增加下边内容，然后重新cmake和make

INCLUDE_DIRECTORIES("/<your location>/opencv_contrib-3.4.0/modules/xfeatures2d/include") 

如果修改后还报错，则找到报错的文件，将头文件改成绝对路径 

8 "xxxx/test_detectors_invariance.impl.hpp"问题
一般是说在features2d/test目录下没有XXX.hpp什么的，处理方式是将opencv-3.3.1/modules/features2d/test该目录下对于的缺少文件复制到opencv_contrib-3.3.1/modules/xfeatures2d/test该目录下，然后修改报错的文件的#include，将前面的地址删除，就让其在本地找

例如 :报错说在文件test_rotation_and_scale_invariance.cpp中找不到#include "xxxx/test_detectors_invariance.impl.hpp"，那么就在opencv-3.3.1/modules/features2d/test下去找test_detectors_invariance.impl.hpp文件，将其复制到opencv_contrib-3.3.1/modules/xfeatures2d/test目录，然后打开test_rotation_and_scale_invariance.cpp文件，修改#include "xxxx/test_detectors_invariance.impl.hpp"为#include "test_detectors_invariance.impl.hpp"即可

如果觉得难得每个文件去找，那么干脆将目录中的所有文件复制过去，之后就该对于报错文件的#include位置就好了。
```

---

### 安装DeepStream

```Shell
cd /opt/nvidia/deepstream/deepstream-6.0/
sudo ./install.sh
sudo ldconfig

# 查看安装的版本
deepstream-app --version-all
```

#### 可能的报错

```Plain Text
1. 如果运行时报错提示找不到一些库，如libnvdsgst_meta.so，则需要把deepstream-6.0/lib添加到系统lib路径中，如下
sudo vim /etc/ld.so.conf
/opt/nvidia/deepstream/deepstream-6.0/lib/       //在文本后边添加该路径
sudo ldconfig    //执行ldconfig立即生效

2. 使用这个shell命令来测试 deepstream-app -c source30_1080p_dec_infer-resnet_tiled_display_int8.txt
如果报错：
** ERROR: <create_multi_source_bin:714>: Failed to create element 'src_bin_muxer'
** ERROR: <create_multi_source_bin:777>: create_multi_source_bin failed
** ERROR: <create_pipeline:1045>: create_pipeline failed
** ERROR: <main:632>: Failed to create pipeline
Quitting
App run failed


则是因为gstreamer缓存问题，运行下边指令删除即可，运行成功后，会显示检测画面。

rm ${HOME}/.cache/gstreamer-1.0/registry.*


如果是在服务器上运行，没有显示界面的话会报错如下
No EGL Display 
nvbufsurftransform: Could not get EGL display connection
需要修改环境变量，如下
vim ~/.bashrc
#打开后在最后边加下边语句
unset DISPLAY
export DISPLAY=:0 
#或者 export DISPLAY=:1
 
#保存退出
source ~/.bashrc
rm ${HOME}/.cache/gstreamer-1.0/registry.*
 
#然后再运行
deepstream-app -c source30_1080p_dec_infer-resnet_tiled_display_int8.txt
```

