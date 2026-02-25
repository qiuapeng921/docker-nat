# docker-nat

专为 NAT 小鸡设计的轻量级容器镜像,内置常用网络工具,支持端口转发。

## ✨ 功能特性

- ✅ **三版本支持**: 提供 Debian (bookworm-slim)、Alpine Linux 和 CentOS Stream 8 三个版本
- ✅ **常用工具箱**: 内置 30+ 工具 (curl, wget, ping, telnet, traceroute, dig, vim, htop, iotop, lsof, zip, tree 等)
- ✅ **灵活认证**: 支持自定义 root 密码或自动生成随机密码
- ✅ **精美 Banner**: 登录时显示系统信息和命令速查
- ✅ **NAT 优化**: 专为端口段映射场景优化,主机与容器端口完美对应

## 🚀 一键管理脚本 (nat.sh)

无需下载源码，直接复制下方命令即可使用：

### 1. 交互式菜单 (推荐)
进入图形化菜单，支持新建容器、管理列表、启动/停止、查看日志等操作。

```bash
bash <(curl -sSL https://raw.githubusercontent.com/code-gopher/docker-nat/master/nat.sh)
```

### 2. 命令行自动部署 (CLI)
适合批量开通或自动化场景。

```bash
# 用法: bash <(curl ...) -t <镜像类型> [选项]

# 示例: 启动一个密码为 123456 的 Debian 小鸡
bash <(curl -sSL https://raw.githubusercontent.com/code-gopher/docker-nat/master/nat.sh) -t debian -p 123456

# 示例: 启动一个密码为 123456 的 Alpine 小鸡
bash <(curl -sSL https://raw.githubusercontent.com/code-gopher/docker-nat/master/nat.sh) -t alpine -p 123456

# 示例: 启动一个密码为 123456 的 CentOS 小鸡
bash <(curl -sSL https://raw.githubusercontent.com/code-gopher/docker-nat/master/nat.sh) -t centos -p 123456
```

**参数说明:**
- `-t`: 镜像类型 (`debian`、`alpine` 或 `centos`)，**必填**。
- `-p`: root 密码，如果不填则自动生成 **8-10 位**随机密码。
- `-c`: CPU 限制，默认 `1` 核。
- `-m`: 内存限制 (MB)，Debian 默认 `512`，Alpine 默认 `128`，CentOS 默认 `512`。

---

## ⚙️ 自动化逻辑
- **内网 IP**: 自动从 `192.168.10.2` 开始递增分配（`.1` 预留给网关）。
- **SSH 端口**: `10000 + IP最后一位`（如 IP `.2` -> 端口 `10002`）。
- **NAT 端口**: `20000 + IP最后一位 × 10` 开始的 **10 个端口**。
  - 例如：IP `192.168.10.2` -> NAT 端口 `20020-20029`

## 🛠️ 常用管理命令
如果你不想用脚本，也可以直接用 Docker 命令：
```bash
# 查看小鸡状态
docker ps | grep nat-

# 查看小鸡日志(包含密码信息)
docker logs nat-debian-2
docker logs nat-alpine-2
docker logs nat-centos-2

# 停止并删除小鸡
docker rm -f nat-debian-2
docker rm -f nat-alpine-2
docker rm -f nat-centos-2
```

## 📦 包含的常用工具
- **网络**: `ping`, `telnet`, `traceroute`, `dig`, `curl`, `wget`, `ifconfig`, `ip`, `netstat`
- **监控**: `htop`, `iotop`, `lsof`, `ps`
- **编辑**: `vim`
- **工具**: `tar`, `gzip`, `unzip`, `tree`

## 许可证
MIT License


