## google2fa-playbook

SSH服务器启用google authenticator的剧本.

安装完本套剧本后,使用ssh登陆时在输入密码之前需要验证google二步验证器.

### requirement:

1. CentOS 7
2. Ansible execute env


### WARNNING!
确保公钥文件可以使用,否则剧本执行完毕后没有可以维护远程主机的通道.


### 使用方法:

1. 确保将你自己的ssh公钥以文件名id_rsa.pub放入roles/google2fa/files文件夹中.
2. 修改hosts文件,将目标主机的IP地址填入.
3. 运行命令, 输入目标主机的ssh密码
    ```
    # ansible-playbook -i hosts site.yml -k
    SSH password:
    ```
4. 运行过程中有一个task返回的是google二步验证的二维码图片信息
    ```
    TASK [google2fa : 二维码链接] *******************************************************
    ok: [192.168.92.153] => {
        "output.stdout": "https://www.google.com/chart?chs=200x200&chld=M|0&cht=qr&chl=otpauth://totp/root@localhost.localdomain....."
    }

    ```
    将这段网址复制到浏览器中打开
5. 打开手机APP扫描该二维码,获得一份二步验证即时密码
6. 使用ssh登陆目标主机, 这时应该是通过公钥登陆,所以会免密码直接登陆. 为了测试二步验证的效果, 这里使用命令
    ```
    #ssh -o PubkeyAuthentication=no root@192.168.92.153
    Verification code: (此处输入APP中显示的目前二步校验密码)
    Password: (输入登陆用户的密码)
    ```


### 问答
Q: 手机APP有什么选择?  
A: google官方有google authenticator应用,可以从APP Store下载. 我推荐的是1Password.

Q: 为什么使用ssh -o PubkeyAuthentication=no方式登陆的时候, 远程主机还是没有要求我输入校验码?  
A: 因为默认剧本中是对局域网环境作免校验处理的, 请检查是否在同一个192.168.段中


#### TODO:
- [x] sshd服务配置文件修改
- [x] 本地登录时忽略二步验证
