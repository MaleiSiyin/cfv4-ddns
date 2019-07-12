访问 https://dash.cloudflare.com/profile，在页面下方找到 Global API Key，点击右侧的 View 查看 Key，并保存下来
设置用于 DDNS 解析的二级域名
在 Cloudflare 中新建一个A记录，如：ddns.yourdomain.com，指向 1.1.1.1，并确保流量不走 CloudFlare（云朵变灰）
下载 DNNS 脚本
```curl https://raw.githubusercontent.com/MaleiSIyin/cfv4-ddns/master/cf-v4-ddns.sh > /root/cf-v4-ddns.sh && chmod +x /root/cf-v4-ddns.sh
```

修改 DDNS 脚本并补充相关信息

```
vim cf-v4-ddns.sh
```

```
# incorrect api-key results in E_UNAUTH error
# 填写 Global API Key
CFKEY=

# Username, eg: user@example.com
# 填写 CloudFlare 登陆邮箱
CFUSER=

# Zone name, eg: example.com
# 填写需要用来 DDNS 的一级域名
CFZONE_NAME=

# Hostname to update, eg: homeserver.example.com
# 填写 DDNS 的二级域名(只需填写前缀)
CFRECORD_NAME=
```

运行脚本，如果配置正确，输出内容会显示当前IP，登录 Cloudflare 查看之前设置的 1.1.1.1 已变更为当前IP
```
./cf-v4-ddns.sh
```

设置定时任务
```
crontab -e
*/2 * * * * /root/cf-v4-ddns.sh >/dev/null 2>&1
```

# 如果需要日志，替换上一行代码
```
*/2 * * * * /root/cf-v4-ddns.sh >> /var/log/cf-ddns.log 2>&1
```
