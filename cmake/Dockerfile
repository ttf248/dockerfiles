# HOW TO BUILD THIS IMAGE ref github workflow

FROM centos:7

ENV LC_ALL en_US.UTF-8

RUN yum -y install epel-release && yum repolist && yum makecache && yum install -y cmake3 && yum makecache && yum groups install -y "Development Tools" && yum install -y wget make gcc binutils gcc-c++ doxygen