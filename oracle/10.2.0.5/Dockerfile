# LICENSE UPL 1.0
#
# Copyright (c) 2014-2015 Oracle and/or its affiliates. All rights reserved.
#
# ORACLE DOCKERFILES PROJECT
# --------------------------
#
# Dockerfile template for Oracle Instant Client
#
# REQUIRED FILES TO BUILD THIS IMAGE
# ==================================
# 
# From http://www.oracle.com/technetwork/topics/linuxx86-64soft-092277.html
#  Download the following three RPMs:
#    - oracle-instantclient-basic-10.2.0.5-1.x86_64.rpm
#    - oracle-instantclient-sqlplus-10.2.0.5-1.x86_64.rpm
#    - oracle-instantclient-devel-10.2.0.5-1.x86_64.rpm 
#
# HOW TO BUILD THIS IMAGE
# -----------------------
# Put all downloaded files in the same directory as this Dockerfile
# Run: 
#      $ docker build -t xiangtl/oracle:client . 
#
#
FROM xiangtl/centos

ADD oracle-instantclient*.rpm /tmp/

RUN  yum -y install /tmp/oracle-instantclient*.rpm && \
     rm -rf /var/cache/yum && \
     rm -f /tmp/oracle-instantclient*.rpm && \
     echo /usr/lib/oracle/10.2.0.5/client64/lib > /etc/ld.so.conf.d/oracle-instantclient10.2.0.5.conf && \
     ldconfig

ENV PATH=$PATH:/usr/lib/oracle/10.2.0.5/client64/bin

CMD ["sqlplus", "-v"]

