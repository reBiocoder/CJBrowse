JBrowse中文文档
======================

安装
====================

一个最通常的情况是,下载JBrowse之后,将整个项目文件夹放在web服务器的目录下(在ubuntu中通常是/var/www/),JBrowse就是一个静态站点,
它通过index.html中的js进行数据处理,不需要后端处理.

安装JBrowse之前需要做什么
*********************

有一些前提条件可以帮助您进行JBrowse设置，包括

* unix操作系统系列-- MacOSX, Linux, 或者 WSL on Windows(在windows10的应用商店中下载ubuntu,使用cmd输入命令 ``bash`` 即可进入linux环境)
*  Web服务器-JBrowse是一组静态文件，可与Apache或Nginx一起使用
* 命令行技能-熟悉命令行将帮助您更好的使用本教程
* Sudo访问-sudo是没有必要的，除非您需要它来修改Web服务器文件，例如 在/ var / www中,修改权限

如果您不具备所有这些条件，请考虑使用JBrowse Desktop，因为它不需要命令行，并且易于在所有操作系统上使用：）

导入JBrowse插件
*********************

如果您使用的是JBrowse插件，则还需要安装Node.js版本6或更高版本。请按照https://nodejs.org/en/download/package-manager/#debian-and-ubuntu-based-linux-distributions
中的步骤在ubuntu上安装node.js

下载JBrowse的发行版本
*********************

::

    curl -L -O https://github.com/GMOD/jbrowse/releases/download/1.16.7-release/JBrowse-1.16.7.zip
    unzip JBrowse-1.16.7.zip
    sudo mv JBrowse-1.16.7 /var/www/html/jbrowse
    cd /var/www/html
    sudo chown `whoami` jbrowse
    cd jbrowse
    ./setup.sh

.. note::
    文档假设你已经安装了web服务器,例如apache.
    在ubuntu上安装apache使用命令 ``apt install apache2`` 会在/var中自动生成www文件夹,里面会有index.html初始文件.
    访问ubuntu服务器ip即可查看该页面.将JBrowse项目文件夹放入/var/www中,配置apache,apache具体配置自行搜索

如果将JBrowse作为插件需要备用JBrowse设置
*********************

JBrowse现在在构建时捆绑了插件，因此，如果您使用插件或自己修改jbrowse源代码，则必须下载源代码https://github.com/GMOD/jbrowse

::

    git clone https://github.com/gmod/jbrowse
    cd jbrowse
    git checkout 1.16.7-release # or version of your choice
    ./setup.sh
    npm run start # starts a express.js dev server on port 8082, alternatively move the entire jbrowse dir to /var/www/html as above



.. note::
    使用npm run watch自动获取对您所做的代码所做的更改（但是，如果添加或删除文件，则需要重新启动）
    对于中国的用户,如果使用自定义配置,推荐使用npm镜像

::

    npm config set registry http://r.cnpmjs.org
    npm config set puppeteer_download_host=http://cnpmjs.org/mirrors
    export ELECTRON_MIRROR="http://cnpmjs.org/mirrors/electron/"

祝贺!
*********************

您应该看到一条消息，“Congratulations, JBrowse is on the web”，并带有指向“ Volvox example data”的链接。
如果您没有看到此消息，则可能是错过了设置步骤



