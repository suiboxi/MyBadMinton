
# 🏸 羽毛球计分板 - 服务器运维手册

这份手册记录了项目的基础资产、部署指令、关键路径及故障排查流程，用于保障网站的长期稳定运行。

---

## 1. 基础资产信息
* **服务器系统**：Ubuntu (阿里云轻量应用服务器)
* **服务器公网 IP**：`47.86.236.254`
* **绑定域名**：`https://mybadminton.top`
* **Web 服务器**：Nginx
* **核心架构**：单文件 HTML (数据通过 LocalStorage 存储于用户本地浏览器)

---

## 2. 日常代码发布 (部署) 指令
当您在本地电脑上修改并保存了 `index.html` 后，请在 PowerShell 中执行以下命令进行同步：

```powershell
scp "C:\Users\ROG\Desktop\badminton\index.html" root@47.86.236.254:/var/www/html/

```

* **维护笔记**：执行后需输入 root 密码。
* **生效方式**：上传成功后即刻生效，若页面未更新，请在浏览器中按 `Ctrl + F5` 强制刷新。

---

## 3. 服务器核心文件路径

通过 SSH 连接服务器后，以下是您最常打交道的路径：

* **网站代码存放处**：`/var/www/html/index.html`
* **Nginx 主配置文件**：`/etc/nginx/sites-available/default`
* **SSL 证书公钥文件**：`/etc/nginx/ssl/cert.pem`
* **SSL 证书私钥文件**：`/etc/nginx/ssl/cert.key`

---

## 4. Nginx 常用管理指令

每次修改 Nginx 配置或跳转逻辑后，必须按顺序执行以下命令：

* **测试配置文件语法**：
```bash
sudo nginx -t

```


*(预期输出：`syntax is ok` 和 `test is successful`)*
* **平滑重启 Nginx**（应用新配置且不中断访问）：
```bash
sudo systemctl reload nginx

```


* **查看运行状态**：
```bash
systemctl status nginx

```



---

## 5. SSL 证书续期指南

阿里云个人测试证书（免费版）通常有 3 个月的有效期。过期后会导致网站提示“不安全”。
**续期操作流程**：

1. 在阿里云控制台“数字证书管理服务”中重新签发一张新证书。
2. 使用“部署到云服务器”功能，将新证书下发到预设路径：
* **证书路径**：`/etc/nginx/ssl/cert.pem`
* **私钥路径**：`/etc/nginx/ssl/cert.key`
* **重启命令**：`systemctl reload nginx`


3. 部署成功后，新证书即刻生效。

---

## 6. 故障排查 (Troubleshooting)

若网站无法打开，请按此顺序排查：

* **检查防火墙规则**：确认阿里云控制台防火墙中，**80 端口 (HTTP)** 和 **443 端口 (HTTPS)** 已开启。
* **查看 Nginx 错误日志**（排查服务器错误）：
```bash
sudo cat /var/log/nginx/error.log

```


* **清理客户端缓存**：若代码更新但页面未变，可能是手机浏览器缓存。请尝试在微信中清理缓存，或使用浏览器的“无痕模式”打开测试。

```

```
