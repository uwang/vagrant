## vagrant

Vagrant CentOS7 开发环境

[PuPHPet —— A simple GUI to set up virtual machines for Web development.](https://puphpet.com/)

>Windows 一定要开启 VT-x/AMD-V 硬件加速

>开机进入BIOS选项 ，依次选Config->CPU->Intel Virtualization Technology，里面有个Intel VT-d Feature ，改成Enabled ，保存退出，关机，然后启动机器。

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

>以上命令实在 Host 上执行，简单说就是 MacOS 下。而 Vagrant 的虚拟机称作 Guset。

## Linux 基本配置

## Nginx 基本配置

## MariaDB 基本配置

## PHP 基本配置

## Node.js 基本配置

## Python 基本配置

## Java 基本配置