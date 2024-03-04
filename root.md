# 本项目适用于 Ubuntu 22.04 LTS
# Ubuntu安装Cloudflare Warp
卸载后无需重新执行
```sh
curl -fsSL https://pkg.cloudflareclient.com/pubkey.gpg | gpg --yes --dearmor --output /usr/share/keyrings/cloudflare-warp-archive-keyring.gpg
echo "deb [signed-by=/usr/share/keyrings/cloudflare-warp-archive-keyring.gpg] https://pkg.cloudflareclient.com/ $(lsb_release -cs) main" | tee /etc/apt/sources.list.d/cloudflare-client.list
```
安装Cloudflare Warp`apt-get update && apt-get install cloudflare-warp`
# warp-cli所需命令介绍
```sh
warp-cli mode <MODE>                    # Set the client's general operating mode
warp-cli registration new               # Register this client, replacing any existing registration (Must be run before first connection!)
warp-cli registration license <KEY>     # Attach the current registration to a different account using a license key
warp-cli connect                        # Maintain a connection whenever possible
warp-cli tunnel endpoint reset          # Remove any configured override
warp-cli tunnel endpoint set <SOCKADDR> # Set an override
```
# 初始化warp-cli
```sh
warp-cli mode proxy                                      # 设置warp的模式为代理模式，默认监听127.0.0.1:40000
warp-cli registration new                                # 向Cloudflare注册本客户端
warp-cli registration license GH7f28p9-9uC726Gw-Y47f1yh9 # 设置WARP+许可证(使用tg bot生成)
warp-cli connect                                         # 连接WARP服务
```
# 查看Ubuntu监听的tcp端口
`ss -tlp` -t查看tcp，-l查看LISTEN状态套接字, -p查看套接字关联的进程

本命令可看到warp-svc进程监听了127.0.0.1:40000
# 优选IP
https://gitlab.com/Misaka-blog/warp-script/-/blob/main/files/warp-yxip/warp-yxip.sh
```sh
wget https://gitlab.com/Misaka-blog/warp-script/-/raw/main/files/warp-yxip/warp-yxip.sh
bash warp-yxip.sh
```
使用Bash运行即可,生成result.csv

运行后选1,即IPv4
# 设置IP
```sh
warp-cli tunnel endpoint reset                    # 清除endpoint
warp-cli tunnel endpoint set 162.159.192.236:8854 # 设置endpoint的地址(从优选IP的result.csv获取)
```
# 测试代理可用性和是否成功切换IP
```sh
curl https://api.ipify.org
curl -x 127.0.0.1:40000 https://api.ipify.org
```
