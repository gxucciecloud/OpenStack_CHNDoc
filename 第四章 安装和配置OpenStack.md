# **第四章 安装和配置OpenStack** #
## 目录 ##

- 概述.......................................................................................................... 30
- 安装OpenStack命令行客户端.................................................................... 31
- 设置使用OpenStack RC文件的环境变量 ....................................................33
- 创建openrc.sh 文件 .................................................................................. 34

　下面的部分包含关于使用OpenStack 客户端工作的信息。<br>
　回顾：在前面的章节，我们使用了**keystone**客户端。<br><br>
　你必须安装客户端工具来完成安装过程的剩余部分。<br><br>
　配置你的客户端安装在你的桌面上而不是服务器上来让你有和你的用户相同的体验。

## 概述 ##
　你可以使用OpenStack 命令行客户端来运行简单的命令去做API调用。你可以运行这些来自命令行的指令或者运用脚本来自动执行任务。如果你提供了OpenStack的凭证，那么你可以在任何一台电脑上运行这些指令。<br>
　内部呢，每个客户端命令都是运行嵌入了API请求的cURL指令。OpenStack 的API都是那些使用了HTTP协议的RESTful APIS,这些协议包括方法，网址，媒体类型和响应代码。<br>
　这些开源的Python客户端运行在linux或者Mac OS X 系统上，非常简单学习和使用。每一个OpenStack服务都有它自己的命令行客户端。在某些客户端命令上，你可以指定一个调试参数来显示这个命令的底层API请求。这是一个好方法来快速熟悉
OpenStack的API调用。<br>
　下面的表格列出了每个OpenStack服务的命令行客户端，包含了它的包名和描述。<br><br>
**表4.1. OpenStack服务和客户端**<br>
<table>
<tbody>
<tr><td>服务</td><td>客户端</td><td>包</td><td>描述</td></tr>
<tr><td>块存储</td><th>cinder</th><td>python-cinderclient</td><td>创建和管理卷</td></tr>
<tr><td>计算</td><th>nova</th><td>python-novaclient</td><td>创建和管理图片，实例和口味</td></tr>
<tr><td>数据库服务</td><th>trove</th><td>python-troveclient</td><td>创建和管理数据库</td></tr>
<tr><td>身份认证</td><th>keystone</th><td>python-keystoneclient</td><td>创建和管理用户，租用用户，角色，端点，认证凭据</td></tr>
<tr><td>镜像服务</td><th>glance</th><td>python-glanceclient</td><td>创建管理镜像</td></tr>
<tr><td>网络</td><th>neutron</th><td>python-neutronclient</td><td>为客户服务器配置网络，这个客户端以前叫做quantum</td></tr>
<tr><td>对象存储</td><th>swift</th><td>python-swiftclient</td><td>收集统计数据，列表项，更新元数据并上传下载，和删除由对象存储服务存储的文件。获得访问对象存储设备的专案处理。</td></tr>
<tr><td>业务流程</td><th>heat</th><td>python-heatclient</td><td>从模板启动堆栈查看运行堆栈的细节，包括事件和资源，以及上传和跟新堆栈</td></tr>
<tr><td>遥测</td><th>ceilometer</th><td>pythonceilometerclient</td><td>从OpenStack创建和收集测量数据</td></tr>
</tbody>
</table>
一个普通的OpenStack客户端就是这样开发的。<br><br>
## 安装OpenStack命令行客户端 ##
首先为每个OpenStack客户端安装必备软件和Python包。
### 安装必备软件 ###
下表列出了你需要用来运行命令行客户端的软件，并提供了可能需要的安装说明。<br><br>
**Table 4.2.必备软件**<br>
<table>
<tbody>
<tr><td>必备软件</td><td>客户端</td></tr>
<tr><td>Python2.6或者更早</td><td>目前这些客户端不支持Python3</td></tr>
<tr><td>设置工具包</td><td>在MAC OS X系统上是默认安装。<br><br>许多linux发行版本都提供软件包使得设置工具包非常简单安装。从你的安装包管理器搜索setuptools来发现一个安装包。如果你找不到，可以直接从下面这个网址下载：<a href="http://pypi.python.org/pypi/setuptools">http://pypi.python.org/pypi/setuptools</a>。<br><br>在微软windows系统下安装配置工具包的推荐方式是根据在配置工具包的<a href="https://pypi.python.org/pypi/setuptools#windows">网址</a>提供的文档进行。另一种方式是使用由Christoph Gohlke整理的非官方安装包（<a href="http://www.lfd.uci.edu/~gohlke/pythonlibs/#setuptools">http://www.lfd.uci.edu/~gohlke/pythonlibs/#setuptools</a>）。</td></tr>
<tr><td>PIP 包</td><td>使用pip在Linux，MAC OS X，windows系统上来安装这些客户端。保证你能从Python安装包索引得到最新的客户端版本，并且后面更新或者删除安装包。<br><br>通过安装包管理器为你的系统安装pip：<br><strong>mac os.</strong><br><code># easy_install pip</code><br><br><strong>microsoft windows.</strong><br>确保C:\Python27\Scripts目录已经确定写入PATH环境变量里，并且使用设置工具包里使用easy_install命令:<br><code>C:\>easy_install pip</code><br>另一种方法是选择由Christoph Gohlke提供的非官方安装包（<a href="http://www.lfd.uci.edu/~gohlke/pythonlibs/#pip">http://www.lfd.uci.edu/~gohlke/pythonlibs/#pip</a>）。<br><br><strong>Ubuntu and Debian</strong>.<br><code># apt-get install python-pip</code><br><br><strong>Red Hat Enterprise Linux, CentOS, or Fedora.</strong><br>在RDO有一个可得到的版本包能让你使用yum来安装客户端或者可以使用安装pip来管理客户端安装。<br><br><code># yum install python-pip</code><br><br><strong>openSUSE 12.2 and earlier.</strong>在Open Build Service有一个可打包版本能够让你使用rpm或者zypper来安装这些客户端，或者可以安装pip来安装管理这些客户端：<br><br><code># zypper install python-pip</code><br><br><strong>openSUSE 12.3 and later.</strong>一个可打包版本能让你使用rpm或者zypper来安装这些客户端。可见下部分<a href="#ct1">“安装客户端”</a></td></tr>
</tbody>
</table>

## 安装客户端 ##
 <a name="ct1" id="ct1"></a>
　当跟随本节的指示，使用客户端的小写名字来代替项目来完成安装，例如**nova**。重复每个客户端。以下是有效值：<br>
  
- ceilometer - **Telemetry API**
- cinder - **Block Storage API and extensions**
- heat - **Orchestration API**
- keystone - **Identity service API and extensions**
- neutron - **Networking API**
- nova - **Compute API and extensions**
- swift - **Object Storage API**
- trove - **Database Service API**<br>

下面的例子展示了使用pip来安装nova客户端的命令。

	# pip install python-novaclient
### 使用pip来安装 ###
在Linux，Mac OS X或者Microsoft Windows系统上使用pip来安装OpenStack客户端。这个非常简单去使用并且确保从[Python Package Index](https://pypi.python.org/pypi)得到最新的客户端版本。当然，pip能够使你更新或者删除一个包。<br><br>
分别使用以下指令来安装每个客户端。

- **对于Mac OS X或者Linux:**<br><br>
`# pip install python-PROJECTclient`<br><br>
- **对于微软windows系统：**

	`C:\>pip install python-PROJECTclient`

### 从安装包安装 ###
　RDO和openSUSE由客户端安装包无需使用pip来安装。<br>
　在Red Hat Enterprise Linux，CentOS，或者Fedora系统上，可以使用从[RDO](https://openstack.redhat.com/Main_Page)可得到安装包用yum来安装这些客户端：

	# yum install python-PROJECTclient
　对于openSUSE，可以使用rpm或者zypper来安装从[the Open Build Service](https://build.opensuse.org/package/show?package=python-novaclient&project=Cloud:OpenStack:Master)得到可安装版本：

	# zypper install python-PROJECT`
### 更新或者删除客户端 ###
　为了更新一个客户端，要早pip install命令里添加--upgrade选项：

	# pip install --upgrade python-PROJECTclient
　删除一个客户端，可以运行下面这个pip uninstall 命令：

	# pip uninstall python-PROJECTclient
## 使用OpenStack RC 文件来设置环境变量 ##
　　为了为这些OpenStack命令行客户端设置必须的环境变量，你必须创建一个叫做OpenStack rc文件或者叫做openrc.sh文件的环境文件。这些特殊项目的环境文件包含了所有OpenStack服务使用所需的凭证。<br><br>
　　当你打开这个源文件，环境变量就已经为你当前shell设置好了。这些变量能使这些OpenStack客户端命令能与那些运行在云里的OpenStack服务进行联系。<br><br>
**注意**<br>
　　确定环境变量使用一个环境文件在微软windows系统上不是一个常见做法。环境变量通常在系统属性对话框中的高级选项中进行设置。
### 创建和生成OpenStack RC文件 ###
1.在文本编辑器中，创建一个叫做*project*-openrc.sh的文件，并且添加下面的认证信息：<br><br>
   下面的例子展示了一个叫做admin项目的信息，当系统的使用者名字也叫做admin，并且标示位于控制器的主机。

	export OS_USERNAME=admin
	export OS_PASSWORD=ADMIN_PASS
	export OS_TENANT_NAME=admin
	export OS_AUTH_URL=http://controller:35357/v2.0
2.在任何你想要运行OpenStack命令的shell上，为每个不同项目释放*PROJECT*-openrc.sh文件。在下面的例子里，你为admin项目释放admin-openrc.sh文件：

	source admin-openrc.sh
### 覆盖环境变量值 ###
　　当你运行OpenStack客户端命令时，你可以使用在help末尾列出的不同客户端命令输出选项来覆盖一些环境变量设置。例如。你可以通过在keystone指令中特殊一个密码来覆盖设置在PROJECT-openrc.sh文件中的OS_PASSWORD。如下：

    $ keystone --os-password *PASSWORD* service-list
　　其中*PASSWORD*就是你的密码。
### 创建openrc.sh文件 ###
　　如同在小节“创建和生成OpenStack RC文件”中所解释的，使用小节“定义使用者，租用用户和角色”的凭据来创建下面的*PROJECT*-openrc.sh文件：<br>


-为管理者用户创建的admin-openrc.sh文件。<br><br>
-为普通用户创建的demo-openrc.sh文件：	

	export OS_USERNAME=demo
    export OS_PASSWORD=DEMO_PASS
    export OS_TENANT_NAME=demo
    export OS_AUTH_URL=http://controller:35357/v2.0

