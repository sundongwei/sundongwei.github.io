---
title: 🎉 Install Protobuf
summary: Intall protobuf on Linux 
date: 2024-11-01

# Featured image
# Place an image named `featured.jpg/png` in this page's folder and customize its options here.
image:
  caption: 'Image credit: [**Unsplash**](https://unsplash.com)'

authors:
  - Sun Dongway

tags:
  - Academic
  - Technology
  - AI
---
1. **依赖**

```Shell
sudo apt-get install autoconf automake libtool
curl make g++ unzip
```

```Shell
git clone
https://github.com/protocolbuffers/protobuf.git
cd protobuf
git submodule update --init --recursive
./autogen.sh
```

1. **to build and install the C++ Protocol Buffer runtime and the Protocol Buffer compiler (protoc) execute the following:**

    ```Shell
    ./configure --prefix=指定路径
    make
    make check
    sudo make install
    sudo ldconfig # refresh shared library cache.
    
    到此步还没有安装完毕，
    ####### add protobuf lib path ######
    
    #（动态库搜索路径）程序加载运行期间查找动态链接库时指定除了系统默认路径之外的其他路径
    export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/protobuf/lib/ 
    
    #（静态库搜索路径）程序编译期间查找动态链接库时指定查找共享库的路径
    export LIBRARY_PATH=$LIBRARY_PATH:/usr/local/protobuf/lib/
    
    #执行程序搜索路径
    export PATH=$PATH:/usr/local/protobuf/bin/ 
    
    #c程序头文件搜索路径
    export C_INCLUDE_PATH=$C_INCLUDE_PATH:/usr/local/protobuf/include/ 
    
    #C++程序头文件搜索路径
    export CPLUS_INCLUDE_PATH=$CPLUS_INCLUDE _PATH:/usr/local/protobuf/include/ 
    
    #pkg— config 路径
    export PKG_CONFIG PATH=/usr/local/protobuf/lib/pkgconfig/
    ###############################################
    ```

    ![image.png](https://s2.loli.net/2024/11/01/F1nboieVIu7Yy39.png)

    ![image 1.png](https://s2.loli.net/2024/11/01/G2HsScLXIBFaw49.png)

1. **编译Python**

    ```Shell
    cd python
    python setup.py build 
    python setup.py test
    python setup.py install
    ```

     验证

    ```Shell
    python
    import google.protobuf
    ```

    卸载

    ```Shell
    sudo rm /usr/local/bin/protoc //执⾏⽂件
    sudo rm -rf /usr/local/include/google //头⽂件
    sudo rm -rf /usr/local/lib/libproto* //库⽂件
    ```



