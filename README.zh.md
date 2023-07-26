# oemaker

第一次接触iso定制, 被傻逼欧拉折磨的不行, 文档写的屎一样. 只能读源码一步一步来, 修改了一下原来的isocut, 让它可以支持从`product.xml`生成iso, 从而可以下制anaconda的安装选择界面.
注: 2023年7月26日 22.03-lts-sp2 官方的ukui-theme包内部的grub主题文件写的有问题, 带此包打出来的镜像会在`grub2-mkconfig`时挂掉. 可以用https://www.aliyundrive.com/s/fWvM5XFwLdQ 我重新打包的换掉, 也可以用rpmrebuild之类的工具自己重新打包(只需要删掉那个/etc/grub.d/06开头的文生就行,还有spec里对应的)

## quick start

You don't need to build this project to rpm package, just download it and put `isocut/isocut.py` to somewhere and run it. But you should better to run `dnf install isocut` to install official isocut to generate the configurations or dirs. You also can replace that `/usr/bin/isocut` with `isocut.py`(don't forget to rename it and chmod +x) from this repo.

```
./isocut -h
Checking input ...
usage: isocut [-h] [-temp temporary_workspace] [-rpms rpm_path] [-xml product_xml] [-install_pic install_picture_path]
              [-kickstart kickstart_file_path] [-product product_name] [-version version_number] [-verbose]
              source_iso dest_iso

Cut openEuler iso to small one

positional arguments:
  source_iso            source iso image
  dest_iso              destination iso image

optional arguments:
  -h, --help            show this help message and exit
  -temp temporary_workspace
                        temporary path
  -rpms rpm_path        extern rpm packages path
  -xml product_xml      provide a anaconda product.xml and disable rpmlist
  -install_pic install_picture_path
                        install bg picture path
  -kickstart kickstart_file_path
                        kickstart file path
  -product product_name
                        product name
  -version version_number
                        version number
  -verbose              print all cmdline

```
example
```
./isocut -t /home/openeuler/isocut_tmp/ -x /home/openeuler/product.xml -r /home/openeuler/usr_rpms/ -verbose /mnt/hgfs/vm/openEuler-22.03-LTS-SP2-everything-x86_64-dvd.iso  /home/openeuler/ukui.iso
```

-----------------------------------------------------------------

# doc from openeuler

#### 介绍

`oemaker`源码包拥有三部分功能：iso 格式光盘映像的制作和裁剪和通用编译环境制作。相应地，`oemaker` 源码包会生成三个软件包：`oemaker` 和 `isocut` 和 `envmaker`。

生成的二进制 RPM 包 `oemaker` 是用于制作 DVD 光盘映像的构建工具，可制作的映像包括 standard iso、debug iso、source iso、everything iso、everything source iso、everything debug iso 和 netinst iso。

生成的二进制 RPM 包 `isocut` 是用于裁剪光盘映像的构建工具，支持在 RPM 包级别进行裁剪。

生成的二进制 RPM 包 `envmaker` 是用于制作通用编译环境的构建工具。

#### 安装教程

可以使用 `rpm` 或 `dnf` 软件包管理器命令通过 openEuler repository 来安装 `oemaker` 和 `isocut` 和 `envmaker`。

使用 `dnf` 安装 `oemaker`
```sh
dnf install -y oemaker
```

使用 `dnf` 安装 `isocut`
```sh
dnf install -y isocut
```

使用 `dnf` 安装 `envmaker`
```sh
dnf install -y envmaker
```

#### 使用说明

一般要求磁盘空间大于 50G。

#### 使用方法

##### oemaker

oemaker <font color=#0000FF >_[-h] [-t Type] [-p Product] [-v Version] [-r RELEASE] [-s REPOSITORY]_</font>

  Optional arguments:

    -t    ISO type, including standard, debug, source, everything, everything_debug, everything_src, and netinst 

    -p    Product name, for example, openEuler

    -v    Version number

    -r    Release information

    -s    Source dnf repository address link (may be listed multiple times)

    -h    Show the help message and exit

##### isocut

isocut <font color=#0000FF >_[-h] [-t temporary path] [-r extern rpm path] [-k kickstart file path] origin-iso dest-iso_</font>

  Positional arguments:

    origin-iso    origin iso image
    dest-iso      destination iso image

  Optional arguments:

    -t    The temporary path, which must be an absolute path and must be greater than 8 GB

    -r    The external RPM package path

    -k    The kickstart file path

    -p    The product name

    -v    The version number

    -i    The path of background pictures during the installation

    -h    Show the help message and exit

##### envmaker

envmaker <font color=#0000FF >_[-p Product] [-v Version]_</font>

  Optional arguments:
  
    -p    Product name,for example, openEuler_compile_env

    -v    Version identifier
