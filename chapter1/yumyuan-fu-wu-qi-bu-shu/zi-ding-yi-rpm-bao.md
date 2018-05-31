###一、安装软件包

    yum -y install rpm-build
    yum -y install rpm-devel
    yum -y install rpmdevtools
    
###二、rpmbuild环境

**1、生成环境**

    rpmdev-setuptree    
 
     
    [root@nfs-node2 rpmbuild]# pwd
    /root/rpmbuild  
    
    #环境目录 
    [root@nfs-node2 rpmbuild]# tree
    .
    ├── BUILD
    ├── RPMS
    ├── SOURCES
    ├── SPECS
    └── SRPMS
    
    5 directories, 0 files
    
**2、目录介绍**

    默认位置 	                宏代码 	        名称 	        用途
    ~/rpmbuild/SPECS          %_specdir 	Spec 文件目录 	保存 RPM 包配置（.spec）文件
    ~/rpmbuild/SOURCES        %_sourcedir 	源代码目录 	保存源码包（如 .tar 包）和所有 patch 补丁
    ~/rpmbuild/BUILD 	        %_builddir 	构建目录 	源码包被解压至此，并在该目录的子目录完成编译
    ~/rpmbuild/BUILDROOT       %_buildrootdir 	最终安装目录 	保存 %install 阶段安装的文件
    ~/rpmbuild/RPMS 	        %_rpmdir 	标准 RPM 包目录 	生成/保存二进制 RPM 包
    ~/rpmbuild/SRPMS 	        %_srcrpmdir 	源代码 RPM 包目录 	生成/保存源码 RPM 包(SRPM)

**3、打包流程**

>1> 首先，需要把源代码放到%_sourcedir中；
 2> 然后，进行编译，编译的过程是在%_builddir中完成的，所以需要先把源代码复制到这个目录下边，一般情况下，源代码是压缩包格式，那么就解压过来即可；
3> 第三步，进行“安装”，这里有点类似于预先组装软件包，把软件包应该包含的内容（比如二进制文件、配置文件、man文档等）复制到%_buildrootdir中，并按照实际安装后的目录结构组装，比如二进制命令可能会放在/usr/bin下，那么就在%_buildrootdir下也按照同样的目录结构放置；
4> 然后，需要配置一些必要的工作，比如在实际安装前的准备啦，安装后的清理啦，以及在卸载前后要做的工作啦等等，这样也都是通过配置在SPEC文件中来告诉rpmbuild命令；
5> 还有一步可选操作，那就是检查软件是否正常运行；
6> 最后，生成的RPM包放置到%_rpmdir，源码包放置到%_srpmdir下。

**4、阶段介绍**

    阶段 	读取的目录 	写入的目录 	具体动作
    %prep 	%_sourcedir 	%_builddir 	读取位于 %_sourcedir 目录的源代码和 patch 。之后，解压源代码至 %_builddir 的子目录并应用所有 patch。
    %build 	%_builddir 	%_builddir 	编译位于 %_builddir 构建目录下的文件。通过执行类似 ./configure && make 的命令实现。
    %install 	%_builddir 	%_buildrootdir 	读取位于 %_builddir 构建目录下的文件并将其安装至 %_buildrootdir 目录。这些文件就是用户安装 RPM 后，最终得到的文件。注意一个奇怪的地方: 最终安装目录 不是 构建目录。通过执行类似 make install 的命令实现。
    %check 	%_builddir 	%_builddir 	检查软件是否正常运行。通过执行类似 make test 的命令实现。很多软件包都不需要此步。
    bin 	%_buildrootdir 	%_rpmdir 	读取位于 %_buildrootdir 最终安装目录下的文件，以便最终在 %_rpmdir 目录下创建 RPM 包。在该目录下，不同架构的 RPM 包会分别保存至不同子目录， noarch 目录保存适用于所有架构的 RPM 包。这些 RPM 文件就是用户最终安装的 RPM 包。
    src 	%_sourcedir 	%_srcrpmdir 	创建源码 RPM 包（简称 SRPM，以.src.rpm 作为后缀名），并保存至 %_srcrpmdir 目录。SRPM 包通常用于审核和升级软件包。
    
###三、实例
**1、制作nginx **

    略
**2、制作php**

    略   
**3、制作mysql**
    
    略



