---
layout: post
title: "Install MPI for Python"
description: ""
category: "Programming"
tags: [MPI, python]
---
{% include JB/setup %}


## 安装MPICH2


### 下载MPICH2

[http://www.mpich.org/static/tarballs/3.0.2/mpich-3.0.2.tar.gz](http://www.mpich.org/static/tarballs/3.0.2/mpich-3.0.2.tar.gz)

### 安装MPICH2

    ./configure  --enable-shared --disable-f77 --disable-fc
    make
    sudo make install


安装MPI4py需要MPICH2的动态库安装 '--enable-shared',
'--disable-f77 --disable-fc' 是禁止fortran语言（我的ubuntu上没有fortran）


### 添加动态库地址

    export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/lib

不添加动态库地址，运行mpiexec失败

### 测试MPI

    mpiexec -n 2 ./cpi

运行正常说明MPI安装成功

### 多机测试MPI

多机测试需要打通ssh信任关系

    mpiexec -n 2 -hosts "192.168.1.2,192.168.1.3" ./cpi


## 安装mpi4py依赖软件

### 安装libssl（编译mpi4py需要ssl库）

    sudo apt-get install libssl-dev

### 安装python-dev

    sudo apt-get install python-dev

### 下载mpi4py

[http://code.google.com/p/mpi4py/downloads/list](http://code.google.com/p/mpi4py/downloads/list)


## 安装mpi4py

### 编译安装

    python setup.py build
    sudo python setup.py install

### 测试mpi4py

    from mpi4py import MPI

    comm = MPI.COMM_WORLDrank = comm.Get_rank()

    if rank == 0:
        data = {'a': 7, 'b': 3.14}
        comm.send(data, dest=1, tag=11)

        print "this rank 0"

    elif rank == 1:
        data = comm.recv(source=0, tag=11)

        print "rank:",rank, "data:", data

    mpiexec python -n 2 test_mpi.py

输出：

    this rank 0
    rank: 1 data: {'a': 7, 'b': 3.1400000000000001}


## 参考

[http://www.mpich.org/static/docs/guides/mpich2-1.5-installguide.pdf](http://www.mpich.org/static/docs/guides/mpich2-1.5-installguide.pdf)
[http://mpi4py.scipy.org/docs/usrman/index.html](http://mpi4py.scipy.org/docs/usrman/index.html)
