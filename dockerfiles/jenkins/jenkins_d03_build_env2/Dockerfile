FROM jenkinsci/jenkins:2.141

MAINTAINER dodo_lihao <lihao@lihao8.cn>

USER root

SHELL ["/bin/bash", "-c"]

# Set the env variable DEBIAN_FRONTEND to noninteractive
#ENV DEBIAN_FRONTEND noninteractive

# 运行脚本， 脚本放在 bash 里面， 这样节约layer
ADD jenkins-install.sh /
RUN /bin/bash /jenkins-install.sh

# entry-point
COPY entry-point.sh /entry-point.sh
RUN chmod +x /entry-point.sh && chown jenkins:jenkins /entry-point.sh

USER jenkins

ENTRYPOINT ["/entry-point.sh"]