# ssh启用root登录

## 背景

- 领了一台云虚拟机。仅在内网内用作远程开发
- 内网是安全的，有网络隔离。并且云虚拟机上不会有什么重要数据以沙盒形式存在。**安全上的风险可以接受**
- 因此想着省事，直接开了root登录好了

## 步骤

- 先登录到root下
  - `sudo -i`
- 如果没装ssh的话先装一下。
  - `apt-get install openssh-server`
- 改一下配置文件
  - `vi /etc/ssh/sshd_config`
  - 输入`/#PermitRootLogin`找到那行配置
  - 按`i`进入编辑模式
  - 把前面的`#`注释去掉，并把值改为yes
  - `PermitRootLogin yes`这样就完事了
  - 按esc然后`:wq`保存退出
- 重启下ssh服务
  - /etc/init.d/ssh restart
