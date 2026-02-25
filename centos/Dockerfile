# CentOS Stream 9 容器镜像
FROM quay.io/centos/centos:stream9

ENV TZ=Asia/Shanghai
ENV ROOT_PASSWORD=123456

# 安装必要工具
RUN dnf update -y && \
    dnf install -y --setopt=tsflags=nodocs \
    openssh-server openssh-clients curl wget bash vim htop procps-ng \
    net-tools cronie tzdata which iproute hostname sudo shadow-utils \
    systemd systemd-sysv \
    && dnf clean all && rm -rf /var/cache/dnf

# SSH 配置
RUN ssh-keygen -A && \
    mkdir -p /root/.ssh && chmod 700 /root/.ssh && \
    sed -i 's/#PermitRootLogin.*/PermitRootLogin yes/' /etc/ssh/sshd_config && \
    sed -i 's/#PasswordAuthentication.*/PasswordAuthentication yes/' /etc/ssh/sshd_config && \
    sed -i 's/#UsePAM.*/UsePAM yes/' /etc/ssh/sshd_config && \
    systemctl enable sshd && \
    systemctl enable crond

# 修复一些系统服务问题 (适配容器环境)
RUN (cd /lib/systemd/system/sysinit.target.wants/; for i in *; do [ "$i" = systemd-tmpfiles-setup.service ] || rm -f "$i"; done) && \
    rm -f /lib/systemd/system/multi-user.target.wants/* && \
    rm -f /lib/systemd/system/network-online.target.wants/* && \
    rm -f /lib/systemd/system/network.target.wants/* && \
    rm -f /lib/systemd/system/sockets.target.wants/*udev* && \
    rm -f /lib/systemd/system/sockets.target.wants/*initctl* && \
    rm -f /lib/systemd/system/basic.target.wants/* && \
    rm -f /lib/systemd/system/anaconda.target.wants/*

# 入口脚本
COPY docker-entrypoint.sh /usr/local/bin/
RUN chmod +x /usr/local/bin/docker-entrypoint.sh

# MOTD
COPY banner.txt /etc/motd

WORKDIR /root
EXPOSE 22
STOPSIGNAL SIGRTMIN+3
ENTRYPOINT ["/usr/local/bin/docker-entrypoint.sh"]
CMD ["/usr/sbin/init"]