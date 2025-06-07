---

# Unbound DNS 服务监控面板 (DNS Monitoring Dashboard)

这是一个轻量级、自托管的监控面板，旨在通过一个现代、响应式的网页界面，实时监控 [Unbound](https://nlnetlabs.nl/projects/unbound/about/) 和 [Redis](https://redis.io/) 服务的状态。它非常适合部署在运行 DNS 服务的 Linux 服务器（如 Debian/Ubuntu）上，例如作为 Pi-hole 的上游，或独立的 DNS 解析器。

# 🚀 主要功能

*   **一体化监控：** 在一个页面上同时展示 Unbound 和 Redis 的关键性能指标。
*   **实时数据：** 自动刷新数据，让您随时掌握最新状态。
*   **可视化图表：** 通过 Chart.js 动态展示查询趋势 (QPS)，并提供平滑窗口选项。
*   **现代 UI/UX：**
    *   简洁的“色块画”设计风格，带有优雅的渐变色块装饰。
    *   支持亮色/暗色模式切换，并自动保存您的偏好。
    *   完全响应式设计，在 PC 和手机端都有出色的浏览体验。
    *   清晰的状态指示器（🟢 正常 / 🔴 异常）。
*   **轻量级后端：** 使用 Flask 和 Gunicorn 构建，资源占用极低。
*   **一键部署：** 提供 Bash 脚本，可在 Debian/Ubuntu 系统上实现一键部署、回滚和诊断。

# 🚀 快速部署

此脚本设计用于在 Debian 或 Ubuntu 系统上运行。请确保您以 `root` 用户或具有 `sudo` 权限的用户执行。

### 先决条件

*   一个运行 Debian 或 Ubuntu 的服务器。
*   `unbound` 已安装并正在运行。
*   `redis-server` 已安装并正在运行。
*   `curl` 或 `wget` 已安装，用于下载部署脚本。

### 部署步骤

1.  **下载部署脚本：**

    ```bash
    curl -o deploy_dns_monitor.sh https://raw.githubusercontent.com/Jimmyzxk/UbuntuDNS-UI/refs/heads/main/deploy_dns_monitor.sh
    # 或者使用 wget:
    # wget -O deploy_dns_monitor.sh https://raw.githubusercontent.com/Jimmyzxk/UbuntuDNS-UI/refs/heads/main/deploy_dns_monitor.sh
    ```

2.  **赋予脚本执行权限：**

    ```bash
    chmod +x deploy_dns_monitor.sh
    ```

3.  **运行部署脚本：**

    ```bash
    sudo ./deploy_dns_monitor.sh
    ```

4.  **选择操作：**
    脚本会显示一个菜单，请选择 **`1) 部署监控页面`**。

    ```
    --- DNS 服务监控页面部署与回滚脚本 ---
    请选择一个操作: 
    1) 部署监控页面
    2) 回滚/清理部署
    3) 一键诊断并尝试修复
    4) 退出
    #? 1
    ```

脚本将自动完成以下所有工作：
*   安装必要的 Python 依赖。
*   创建项目目录 (`/opt/dns_monitor`) 和 Python 虚拟环境。
*   配置 `sudoers` 文件，允许 web 用户安全地执行监控命令。
*   创建 Flask 应用、HTML 模板和网站图标。
*   创建并启动一个 Systemd 服务 (`dns_monitor.service`)，确保监控面板开机自启。
*   （可选）配置 UFW 防火墙，开放监控面板的端口 (默认为 `5000`)。

部署成功后，您将看到访问地址。

## 访问监控页面

在浏览器中打开以下地址即可访问您的监控面板：

`http://<您的服务器IP>:5000`

## 🛠️ 管理与维护

部署脚本还提供了方便的管理工具。再次运行 `sudo ./deploy_dns_monitor.sh` 即可看到菜单。

### 一键诊断与修复

如果监控页面显示“连接错误”，或数据不正常，请选择 **`3) 一键诊断并尝试修复`**。此功能会检查：
*   Unbound 和 Redis 服务的运行状态。
*   `sudoers` 权限配置是否正确。
*   `www-data` 用户是否在 `redis` 组中。
*   Unbound 远程控制接口是否启用。
*   防火墙规则是否正确。

并尝试自动修复检测到的常见问题。

### 回滚/清理部署

如果您想完全移除监控面板，请选择 **`2) 回滚/清理部署`**。此操作会：
*   停止并禁用 `dns_monitor` Systemd 服务。
*   删除所有项目文件 (`/opt/dns_monitor`)。
*   删除相关的 `sudoers` 配置文件。
*   删除 Systemd 服务文件。

这是一个 **不可逆** 的操作，请谨慎使用。

## 📄 技术栈

*   **前端:** HTML5, CSS3 (Flexbox, Grid, 响应式设计), JavaScript
*   **图表库:** [Chart.js](https://www.chartjs.org/)
*   **后端:** [Flask](https://flask.palletsprojects.com/) + [Gunicorn](https://gunicorn.org/)
*   **部署:** Bash, Systemd

## 💡 贡献

欢迎提交 Pull Request 或创建 Issue 来报告 Bug 或提出功能建议。如果您对 UI/UX 有更好的想法，也请随时分享！

## 📜 许可证

本项目采用 [MIT 许可证](LICENSE)。

---

**使用提示：**

*   **截图：** 请务必将 `` 中的 URL 替换为您自己的项目截图。一张吸引人的截图对项目非常重要。
*   **下载链接：** 在 "下载部署脚本" 部分，您需要先将最终版的 Bash 脚本上传到 GitHub Gist 或您的项目仓库中，然后获取其 "Raw" 文件的链接，并替换掉模板中的占位符 URL。
*   **许可证：** 如果您希望使用其他开源许可证，请相应修改。如果您的仓库中没有 `LICENSE` 文件，可以考虑创建一个。GitHub 提供了模板，非常方便。

这个 README 模板应该能很好地介绍您的项目，并指导用户完成部署和维护。祝您的项目在 GitHub 上大受欢迎！
