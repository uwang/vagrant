## vagrant

Vagrant CentOS7 开发环境

>Windows 一定要开启 VT-x/AMD-V 硬件加速。开机进入BIOS选项 ，依次选Config->CPU->Intel Virtualization Technology，里面有个Intel VT-d Feature ，改成Enabled ，保存退出，关机，然后启动机器。

相关概念：

- provision - 字面意思是准备，实现的功能是在原生镜像的基础上，进行一些附加的操作，以改变虚拟机的环境，比如安装应用，发布程序等。
    
- Host - 安装 VirtualBox 和 Vagrant 的物理机，通常是系统为 Windows、MacOS。
- Guest - 被 Vagrant 维护的虚拟机。

### [Basic Usage of Provisioners →](https://www.vagrantup.com/docs/provisioning/basic_usage.html)

provision 的字面意思是准备。

通常情况下Box只做最基本的设置，而不是设置好所有的环境，因此Vagrant通常使用Chef或者- [PuPHPet](https://puphpet.com/)来做进一步的环境搭建。
那么Chef或者Puppet称为provisioning，而该命令就是指定开启相应的provisioning。

provisioner 在三种情况下运行：

- 第一次 `vagrant up`
- `vagrant provision`
- `vagrant reload --provision`

### 开始

>Minimum required Vagrant version is 2.0

>Minimum suggested Virtualbox version is 5.0

在满足上述基本条件的前提下，尽量选用最新的版本。

首先，[下载安装 VirtualBox](https://www.virtualbox.org/)
然后，[下载安装 Vagrant](https://www.vagrantup.com/)

然后初始化：

```shell
vagrant up
```

等出现下载地址后，Ctrl+C 终止掉，复制 url 下载。下载完成后，将 virtualbox.box 重名后移动到指定目录下，并创建对应的文件 metadata.json，内容如下：

Windows 下：

```json
{
    "name": "centos/7",
    "versions": [{
        "version": "1804.02",
        "providers": [{
            "name": "virtualbox",
            "url": "file:///d:/path/to/file.box"
        }]
    }]
}
```

MacOS homestead v6.1.0 下的样例：

```json
{
    "name": "laravel/homestead",
    "versions": [{
        "version": "6.1.0",
        "providers": [{
            "name": "virtualbox",
            "url": "file:///Users/用户名/Downloads/file.box"
        }]
    }]
}
```

手动将 box 导入到系统：

```shell
vagrant box add metadata.json
vagrant box list
```

### Shared Folder Type

NFS is highly recommended for `MacOS` and `Linux`! Make sure to install the vagrant-bindfs plugin with:

```shell
vagrant plugin install vagrant-bindfs
```

网络有时不好会安装是被，可以多试几次，正常情况下：

>Installing the 'vagrant-bindfs' plugin. This can take a few minutes...
Fetching: vagrant-bindfs-1.1.0.gem (100%)
Installed the plugin 'vagrant-bindfs (1.1.0)'!

>以上命令实在 Host 上执行，简单说就是 MacOS、Windows 下。而 Vagrant 的虚拟机称作 Guset。

Windows 下会忽略 NFS 类型的同步方式，建议使用 [RSync](https://www.vagrantup.com/docs/synced-folders/rsync.html)

#### 默认类型

```
drwxr-xr-x.   4 vagrant vagrant   136 3月  20 01:28 default
drwxr-xr-x.  29 vagrant vagrant   986 3月  20 02:16 drupal
drwxr-xr-x.  24 vagrant vagrant   816 3月  20 04:13 laravel
```

Nginx 和 PHP-FPM 的运行身份都应该改为 vagrant。否则 Nignx 恒为 404，PHP-FPM 恒为 403。

#### Mac/Linux 上使用 nfs

```
drwxr-xr-x.   4 501 games   136 3月  20 01:28 default
drwxr-xr-x.  29 501 games   986 3月  20 02:16 drupal
drwxr-xr-x.  24 501 games   816 3月  20 04:13 laravel
```

>用户名 501 ，用户组 games。这是因为 NFS 表示的是 Network Files Share，网络文件共享，这就相当于，您自己的电脑变成了一台共享文件的服务器，与运行的虚拟机共享一些文件，所以，你看到的文件或目录的拥有者与用户组，应该属于你的本机系统。在虚拟机上运行的服务，比如 PHP-FPM、NGINX 对共享目录不存在权限问题。

Ubuntu 可能符合上述原则。但是，实测 CentOS 依然需要将 Nignx 和 PHP-FPM 的运行用户改为 vagrant！

## 系统配置

```shell
sudo yum install git -y
git clone https://github.com/haobingwang/scripts.git
```

[CentOS7 常用脚本](https://github.com/haobingwang/scripts/tree/master/centos)
