# 开始使用

有关背景信息，请参阅 [什么是 Beszel？](./what-is-beszel.md) 页面。

## 1. 启动中心 (hub)

Beszel 支持通过 Docker/ Podman 或单个二进制文件进行安装。

- [Docker / Podman](./hub-installation#docker-或-podman)
- [二进制文件](./hub-installation#二进制文件)

<div style="height: 1px; margin: -5px 0 0"></div>

::: details 点击查看完整的 `docker-compose.yml` 配置，包括本地代理

以下示例应该可以直接使用。

在步骤 3 中，使用 `host.docker.internal` 作为 **主机/IP** 值。

记得用你的公钥更新 `KEY` 值，然后再次运行 `docker compose up -d`。

::: code-group

```yaml [docker-compose.yml]
services:
  beszel:
    image: henrygd/beszel:latest
    container_name: beszel
    restart: unless-stopped
    extra_hosts:
      - host.docker.internal:host-gateway
    ports:
      - 8090:8090
    volumes:
      - ./beszel_data:/beszel_data

  beszel-agent:
    image: henrygd/beszel-agent:latest
    container_name: beszel-agent
    restart: unless-stopped
    network_mode: host
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock:ro
    environment:
      PORT: 45876
      # 保留引号，不要删除
      KEY: '使用“添加系统”对话框复制的公钥进行更新'
```

:::

## 2. 创建管理员用户

启动中心 (hub) 后，导航至 http://localhost:8090 或您选择的地址。

系统将提示您创建帐户：

<a href="/image/admin-creation.png" target="_blank">
<img src="/image/admin-creation.png" height="485" width="671" alt="用户创建表单截图" />
</a>

## 3. 配置您的第一个系统

单击右上角的 添加系统 (Add System) 按钮以打开系统创建对话框。本示例中我们使用的是 localhost，但您也可以使用远程代理。

在启动代理之前，请勿点击对话框中的 添加系统 (Add System) 按钮。

<a href="/image/add-system.png" target="_blank">
<img src="/image/add-system.png" height="506" width="681" alt="系统创建表单截图" />
</a>

## 4. 启动代理

:::: details Docker 指令

::: tip 注意
如果您更喜欢以不同的方式设置容器，请随意操作。
:::

1. 从 添加系统 (Add System) 对话框复制 docker-compose.yml 内容。

2. 创建一个目录来存储代理的 docker-compose.yml 文件。

```bash
mkdir beszel-agent
cd beszel-agent
```

3. 创建一个名为 docker-compose.yml 的文件，并粘贴 添加系统 (Add System) 对话框中提供的内容。

::: code-group

```bash [nano]
nano docker-compose.yml
```

```bash [vim]
vim docker-compose.yml
```

```bash [emacs]
emacs docker-compose.yml
```

```bash [vscode]
code docker-compose.yml
```

:::

4. 启动代理

```bash
docker compose up -d
```

::::

:::: details 二进制文件安装说明
::: warning 安装脚本仅适用于 Linux 系统
如果您使用的是其他操作系统，请手动下载并运行适合您系统的正确二进制文件。有关详细信息，请参阅 代理安装 页面或 编译 页面。
:::

从 添加系统 (Add System) 对话框复制二进制安装命令。

打开终端并运行该命令。

这将下载正确的二进制文件，创建一个名为 beszel 的用户，并启动代理。它还将创建一个服务以使其在重新启动后继续运行，并可以选择启用自动每日更新。
::::

## 5. 完成添加系统

现在代理正在运行，请单击对话框中的 添加系统 (Add System) 按钮。

您将在表格中看到新系统。如果它变为绿色，则表示一切正常。

<a href="/image/new-system.png" target="_blank">
<img src="/image/new-system.png" height="263" width="1418" alt="系统创建表单截图" />
</a>

如果它变为红色，请查看 [常见问题](./common-issues.md) 页面。