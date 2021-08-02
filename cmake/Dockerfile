# HOW TO BUILD THIS IMAGE
# -----------------------
# Put all downloaded files in the same directory as this Dockerfile
# Run: 
#      $ docker build -t registry.cn-shenzhen.aliyuncs.com/tianlongxiang51/cmake-gcc:20190718-1 -f cmake .

FROM centos:7

ENV LC_ALL en_US.UTF-8

RUN yum -y install epel-release && yum repolist && yum makecache && yum install -y cmake3 && yum makecache && yum groups install -y "Development Tools" && yum install -y wget make gcc binutils gcc-c++ doxygen