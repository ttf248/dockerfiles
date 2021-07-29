# HOW TO BUILD THIS IMAGE
# -----------------------
# Put all downloaded files in the same directory as this Dockerfile
# Run: 
#      $ docker build -t registry.cn-shenzhen.aliyuncs.com/tianlongxiang51/cmake-gcc:20190718-1 -f cmake .

FROM centos:7

ENV LC_ALL en_US.UTF-8

RUN yum install -y wget && \
    cd /etc/yum.repos.d && \
    wget -O CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo && \
    wget -O epel.repo http://mirrors.aliyun.com/repo/epel-7.repo 

RUN yum install -y cmake3

RUN yum makecache && yum groups install -y "Development Tools"

RUN yum install -y wget make gcc binutils gcc-c++ doxygen