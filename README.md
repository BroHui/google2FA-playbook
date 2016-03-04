### google2fa-playbook

SSH服务器启用google authenticator的剧本.

安装完本套剧本后,使用ssh登陆时在输入密码之前需要验证google二步验证器.

requirement:

1. CentOS 7
2. Ansible execute env

TODO:
- [ ] sshd服务配置文件修改
- [ ] 本地登录时忽略二步验证