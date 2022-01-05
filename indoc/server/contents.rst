2.服务器安装
=============
2.1.操作系统安装

服务器安装之前，请提前准备好操作系统安装光盘ISO文件：

操作系统的安装有两种方式：

1．采用技术支持部提供定制化的ISO，该ISO已集成备份系统需要的所有依赖包，安装完后即可直接使用；

2．采用标准的CentOS 7.5安装操作系统，安装完成后，还需要通过在线或离线的方式安装依赖包；

如无特殊情况，建议使用第一种方式安装操作系统；第二种方式一般适用于云平台，使用标准的云主机镜像创建的VM；

2.1.1. 操作系统安装（定制化ISO）

2.1.1.1 获取ISO文件

操作系统为定制化的Linux系统，包括了软件运行所需的依赖包，安装光盘ISO文件的下载链接请联系技术支持获取。

ISO映像文件下载完成后，请刻录成DVD光盘用于安装。

安装光盘同时支持传统BIOS（Legacy）模式和UEFI模式安装。

2.1.1.2 安装操作系统

光盘启动时，界面如下：

.. image:: /_static/image/indoc/server01.jpg

选择“Install CentOS 7.5”，系统启动安装进程。

稍后提示界面如下图，在图标的安装选项中，只需要选择配置“安装位置”和“网络和主机名”，其它保持缺省即可：

.. image:: /_static/image/indoc/server02.jpg

点击“安装位置”，选择需要安装系统的磁盘（如果系统带有SSD硬盘，请优先将系统安装在SSD硬盘上）。

磁盘分区时选择“我要配置分区”，建议根分区划分容量在100GB以上，或使用整个SSD磁盘的容量。

.. image:: /_static/image/indoc/server03.jpg

系统根分区及数据分区均采用EXT4文件系统。

.. image:: /_static/image/indoc/server04.jpg

安装位置完成后，进行网络配置

.. image:: /_static/image/indoc/server05.jpg

点击“配置”，配置网卡的IP地址：

.. image:: /_static/image/indoc/server06.jpg

.. image:: /_static/image/indoc/server07.jpg

网络配置完成后，点击“开始安装”，系统即开始自动化安装，不再需要人为干预。

安装完成后，系统会自动重启，并进入登陆状态：

.. image:: /_static/image/indoc/server08.jpg

———————————————————————————————————————————————————————

提示：操作系统的root用户名密码为Jrsa1234/，请根据需要修改密码并牢记新的密码！

———————————————————————————————————————————————————————

2.1.2. 操作系统安装（标准ISO）

2.1.2.1 安装操作系统

采用标准的CentOS 7.5 ISO镜像文件安装操作系统，选择最小化安装即可，安装操作系统的步骤和要求与2.1.1.2章节中描述基本一致，不再重复说明。

安装完成后，执行以下命令关闭SELINUX：

sed -i 's/SELINUX=enforcing/SELINUX=disabled/g' /etc/selinux/config/usr/sbin/setenforce 0

2.1.2.2 在线安装依赖包

下载安装脚本：

curl -O http://183.221.243.20:5080/yum/online_update.sh

执行安装脚本：

sh online_update.sh

即可自动安装所有系统依赖包。

2.1.2.2 离线安装依赖包

获取定制化操作系统ISO，上传到备份服务器，mount到本地目录，按照setup目录中READM.txt文件中的说明操作即可：

.. image:: /_static/image/indoc/server09.jpg

2.2.网卡绑定（可选）

可以选择将两块物理网卡绑定成一个逻辑网卡，以提高数据备份与复制的效率，绑定网卡操作方式如下：

以root用户登陆系统的Cockpit管理界面：https://server_ip:9090/;

.. image:: /_static/image/indoc/server10.jpg

点击进入网络-添加绑定：

.. image:: /_static/image/indoc/server11.jpg

选择要绑定的物理网卡和工作模式，完成。

.. image:: /_static/image/indoc/server12.jpg

2.3.服务器软件安装

数据安全管理平台提供本地安装和在线安装两种方式：

2.3.1. 数据安全管理平台本地安装

2.3.1.1 前提条件

1）备份服务器已安装定制化操作系统或标准操作系统并已安装依赖包；

2）准备数据安全管理平台备份服务器软件安装包，文件列表如下：

.. image:: /_static/image/indoc/server13.jpg

使用install_all.sh或uninstall_all.sh一键式安装或卸载。

2.3.1.2 安装软件包

上传软件包至服务器，并通过SSH以root用户登陆服务器，进入软件包目录。

.. image:: /_static/image/indoc/server14.jpg

执行sh install_all.sh，并按提示输入Yes，大约1分钟左右即可完成安装：

.. image:: /_static/image/indoc/server15.jpg

———————————————————————————————————————————————————————

提示：输入Yes进行确认安装，一定要注意字母的大小写！如不输入匹配，将会退出安装程序。

———————————————————————————————————————————————————————

2.3.2. 数据安全管理平台在线安装

2.3.2.1 前提条件

1）备份服务器已安装定制化操作系统或标准操作系统并已安装依赖包；

2）备份服务器能够访问互联网；

3）仅用于初次安装备份软件；

2.3.2.2 配置YUM源

1.下载yum源配置文件，为避免依赖包冲突，可临时性将其它全部yum源移除或禁用。

wget -P /etc/yum.repos.d/ http://183.221.243.20:5080/yum/runstor.repo

2.更新yum源缓存

yum makecache

2.3.2.3 安装软件包

1.首先安装依赖包；

yum install -y Backup _Depends;ACCEPT_EULA=Y rpm -ihv /tmp/msodbc*.rpm --nosignature

2.然后安装RunStor软件包

yum install -y Backup_Studio Backup _Server

至此，在线安装操作全部完成。

2.4.初始化系统

通过浏览器（建议使用Chrome内核，1920*1080分辨率），访问：http://server_ip ，首先选择产品部署的安全级别。

.. image:: /_static/image/indoc/server16.jpg

.. image:: /_static/image/indoc/server17.png

2.4.1基础模式

账号:admin  初始密码:admin

如无涉密相关的特殊要求，选择“基础”等级即可，然后配置产品标识及系统绑定的本机IP

.. image:: /_static/image/indoc/server18.jpg

在导入许可页面，将机器码复制并发送给技术支持，由技术支持提供试用或正式授权码和授权文件。

.. image:: /_static/image/indoc/server19.jpg

导入授权码和授权文件成功后，进行下一步有关介质的配置，该部分配置可以在后期调整。

.. image:: /_static/image/indoc/server20.jpg

参数配置说明：

.. image:: /_static/image/indoc/server21.png

点击完成后，系统初始化工作完成，登录数据安全管理平台，进入数据备份-服务器-系统状态检查5项服务进程是否正常启动。

.. image:: /_static/image/indoc/server22.jpg

至此，服务器的所有安装工作完成！

2.4.2涉密模式

涉密模式有系统管理员、安全保密员、安全审计员、操作员、巡检员五种角色。

其中系统管理员、安全保密员、安全审计员相互独立、相互制约，配合制度建设，可以有效的加强涉密信息系统保密管理，减少泄密风险，满足国家保密局“三员”保密要求，完全可以在涉密网络安全中运行。

操作员和巡检员相互配合保证备份恢复管理业务的正常运行。

系统管理员：负责系统参数配置、网络配置、磁盘管理、租户管理、用户管理。

安全保密员：负责用户权限管理、密码策略管理、审计系统用户的日志。

安全审计员：负责系统管理员、安全保密员的日志审计。

操作员：根据安全保密员的授权分配负责备份恢复管理业务。

巡检员：巡检备份恢复管理业务的信息。

2.4.2.1角色说明

2.4.2.1.1系统管理员

系统管理员，默认用户名admin。

系统管理员负责系统信息、网络配置、磁盘管理等功能。

.. image:: /_static/image/indoc/server23.jpg

租户管理

.. image:: /_static/image/indoc/server24.jpg

创建/编辑租户

.. image:: /_static/image/indoc/server25.jpg

系统用户管理

.. image:: /_static/image/indoc/server26.jpg

新增/编辑系统用户，系统管理员仅负责系统用户的基础信息，权限由安全保密员分配。+

.. image:: /_static/image/indoc/server27.jpg

系统管理员仅可查看自己的日志

.. image:: /_static/image/indoc/server28.jpg

.. image:: /_static/image/indoc/server29.jpg

2.4.2.1.2安全保密员

安全保密员，默认用户名secrecy。

安全保密员主要负责系统用户权限分配、系统用户启用禁用、系统用户日志审计、密码设置。

.. image:: /_static/image/indoc/server30.jpg