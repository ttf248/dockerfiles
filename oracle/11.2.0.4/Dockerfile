# HOW TO BUILD THIS IMAGE
# -----------------------
# Put all downloaded files in the same directory as this Dockerfile
# Run: 
#       缩减镜像的体积，用于开发环境
#      $ docker build -t registry.cn-shenzhen.aliyuncs.com/tianlongxiang51/oracle:20190914 . 
#
# Pull base image
# ---------------
FROM registry.cn-shenzhen.aliyuncs.com/tianlongxiang51/oracle:20190718

COPY --chown=oracle:oinstall createDB.sh /opt/oracle/

RUN chmod ug+x /opt/oracle/createDB.sh