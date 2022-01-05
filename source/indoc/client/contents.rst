3.客户端安装、卸载
==================
3.1.客户端安装

客户端软件安装之前，请准备好以下工作：

1）获取所需平台的客户端软件，客户端安装好后，只能备份文件；

2）如需备份数据库及其它应用，还需要安装相应的应用代理软件；

3）客户端服务器开放5102，5103、5107端口（一般情况下，客户端安装程序会自动开启需要使用的端口）；

4）需要使用系统管理员帐户登陆来安装客户端软件。

3.1.1.Windows客户端安装

Windows客户端提供32位和64位的安装程序，文件名一般如下图所示：

.. image:: /_static/image/indoc/client01.jpg

客户端安装部署原则：

1）如果需要备份的应用（如Oracle、SQL Server）是32位版本，即安装32位的客户端程序；

2）如果需要备份的应用（如Oracle、SQL Server）是64位版本，即安装64位的客户端程序；

以管理员身份运行安装程序，直至页面提示：

.. image:: /_static/image/indoc/client02.jpg

选择安装你需要备份的应用代理程序，然后执行“下一步”。

———————————————————————————————————————————————————————

提示：应用代理程序不要选择错误，也不能多选，否则可能会造成备份失败。如只需要备份文件，就不用选择代理程序。

———————————————————————————————————————————————————————

.. image:: /_static/image/indoc/client03.jpg

最后，按提示选择安装VC++依赖包，点击完成！

4.1.2.Linux客户端安装

Linux/Unix提供多个发行版本的客户端安装包，软件包安装的方式几乎完全一致，这里以最常用的Ubuntu和CentOS系统为例来说明安装过程。

安装时，需要根据客户端CPU架构、操作系统版本及位数选择相应的客户端安装包。Linux平台一般需要安装两个包：

1）客户端软件（仅能备份文件）

2）应用代理软件

Ubuntu:
以root用户登陆客户端系统，执行以下命令，安装客户端软件：

dpkg -i backup-client-ubuntu12_1.5_amd64.deb

.. image:: /_static/image/indoc/client04.jpg

CentOS:

以root用户登陆客户端系统，执行以下命令，安装客户端软件：

rpm -ivh Backup_Client-V8.0_20211125-el7.x86_64.rpm

.. image:: /_static/image/indoc/client05.jpg

应用代理安装:

以root用户登陆客户端系统，执行以下命令安装代理软件（以下以安装Oracle代理为例，其它应用代理安全类似）：

rpm -ivh Backup_Agent-Oracle-V1.5_20211125-All.noarch.rpm

.. image:: /_static/image/indoc/client06.jpg

4.2.客户端卸载

4.2.1.Windows客户端卸载

在 “控制面板” 进入 “程序和功能” 界面右键点击客户端进行卸载。

.. image:: /_static/image/indoc/client07.jpg

4.2.2.Linux客户端卸载

Ubuntu:

以root用户登陆客户端系统，执行以下命令，查询安装的客户端

dpkg --get-selections | grep back

.. image:: /_static/image/indoc/client08.jpg

以root用户登陆客户端系统，执行以下命令，卸载客户端软件：

dpkg -r backup-client-ubuntu12

.. image:: /_static/image/indoc/client09.jpg

CentOS:

注：卸载客户端前，需要先卸载已安装的代理软件

以root用户登陆客户端系统，执行以下命令查询安装的客户端和代理软件：

rpm -qa|grep Backup

.. image:: /_static/image/indoc/client10.jpg

以root用户登陆客户端系统，执行以下命令卸载代理软件

rpm -e Backup_Agent-Oracle-V1.5_20211125-All.noarch

.. image:: /_static/image/indoc/client11.jpg

以root用户登陆客户端系统，执行以下命令卸载客户端

rpm -e Backup_Client-V8.0_20211125-el7.x86_64

.. image:: /_static/image/indoc/client12.png