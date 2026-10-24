# ARM2 私有部署

目标：SSH 别名 arm2，Oracle Linux 9 / ARM64。用 Podman 构建 Dockerfile，由 systemd Quadlet 管理。
仅绑定 `127.0.0.1:8787`，不开放公网后台。

服务器路径：`/opt/tg-signpulse/src`（代码）、`/opt/tg-signpulse/data`（私密持久数据）。
数据目录权限 700；数据库、Telegram session、后台初始密码和应用密钥不得入 Git。

构建与安装（服务器执行）：

```sh
sudo podman build -t localhost/tg-signpulse:local /opt/tg-signpulse/src
sudo mkdir -p /etc/containers/systemd
sudo install -m 644 /opt/tg-signpulse/src/deploy-arm2.container /etc/containers/systemd/tg-signpulse.container
sudo systemctl daemon-reload
sudo systemctl start tg-signpulse.service
curl -f http://127.0.0.1:8787/readyz
```

本机访问：`ssh -N -L 8787:127.0.0.1:8787 arm2`，打开 `http://127.0.0.1:8787`。
管理员 admin 的随机初始密码在服务器数据目录 `.admin_bootstrap_password`；本人私下读取并改密码。
在面板配置个人 Telegram API ID/Hash，再自行输入手机号、验证码和两步验证密码。不要在聊天或日志分享。

待登录后配置：目标 @sheeridverifier_bot，动作仅发送 `/checkin`，每日一次。
已有当天签到不要重复执行；不要触碰升级、支付或 Gmail 登录功能。
建立任务后核对调度时区、下一次执行时间及成功回复，不以发送成功代替签到成功。

本项目没有推荐签到机器人清单；不同机器人的奖励和积分规则由其经营者决定。
