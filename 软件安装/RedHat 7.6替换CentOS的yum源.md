原因：RedHat不是正版，使用yum命令安装软件时候报错，**This system is not registered to Red Hat Subscription Management.You can use subscription-manager to register**

- 检查原有的yum源：`rpm -qa | grep yum`
  
- 删除原有的yum源：`rpm -qa | grep yum | xargs rpm -e --nodeps`
  
- 下载并安装CentOS的yum源：
  
  - `wget https://mirrors.aliyun.com/centos/7/os/x86_64/Packages/yum-3.4.3-168.el7.centos.noarch.rpm`
    
  - `wget https://mirrors.aliyun.com/centos/7/os/x86_64/Packages/yum-metadata-parser-1.1.4-10.el7.x86_64.rpm`
    
  - `wget https://mirrors.aliyun.com/centos/7/os/x86_64/Packages/yum-plugin-fastestmirror-1.1.31-54.el7_8.noarch.rpm`
    
  - 安装上面三个包：`rpm -ivh yum-*`
    
  - 检查yum是否安装成功：`rpm -qa | grep yum`
    
- 替换并repo文件：
  
  - 备份旧repo文件：`mv /etc/yum.repos.d/CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo.backup`
    
  - 下载新的repo文件：`wget -O /etc/yum.repos.d/CentOS-Base.repo https://mirrors.aliyun.com/repo/Centos-7.repo`
    
  - 编辑repo文件：
    
    - `vi CentOS-Base.repo`
      
    - 将所有的$releasever全部替换成版本号7：
      
      - 键盘按键，**`shift`** 加上 **`:`**
        
      - `%s/$releasever/7/g`
        
      - 回车，并保存文件。
        
  - 清理缓存：`yum clean all`
    
  - 重新生成缓存：`yum makecache`
    
- 非阿里云ECS用户会出现 Couldn't resolve host 'mirrors.cloud.aliyuncs.com' 信息，不影响使用。用户也可自行修改相关配置：`sed -i -e '/mirrors.cloud.aliyuncs.com/d' -e '/mirrors.aliyuncs.com/d' /etc/yum.repos.d/CentOS-Base.repo`
  
<br/><br/>
镜像源地址：

    - 阿里云镜像源地址：https://mirrors.aliyun.com/centos/7/os/x86_64/Packages/

    - 网易163镜像源地址：http://mirrors.163.com/
