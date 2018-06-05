#Grok Debugger 安装

### 一、Ruby的安装

    #安装依赖包
    yum -y instal openss-devel gcc  gcc-c++
    
    #下载安装ruby
    wget https://ruby.taobao.org/mirrors/ruby/2.1/ruby-2.1.7.tar.gz
    tar xf ruby-2.1.7.tar.gz
    cd ruby-2.1.7
    ./configure --prefix=/usr/local/ruby2.1.7  && make && make install
    
    #配置环境变量
    vim /etc/profile.d/ruby.sh 
    export PATH=/usr/local/ruby2.1.7/bin:$PATH
    
    #重新加载环境变量
    source /etc/profile
    
###二、RubyGems 工具安装
    wget http://rubygems.global.ssl.fastly.net/rubygems/rubygems-2.6.2.tgz
    tar zxf rubygems-2.6.2.tgz
    cd rubygems-2.6.2
    ruby setup.rb

###三、替换gem 源 
    
    gem sources --add https://ruby.taobao.org/ --remove https://rubygems.org/
    gem sources -l
    
###四、Grokbug 安装

    mkdir /usr/local/grokbug
    cd /usr/local/grokbug
    wget https://codeload.github.com/nickethier/grokdebug/zip/master
    unzip master
    mv grokdebug-master/* .
    rm -rf grokdebug-master/
    
    #查看组件
    ruby config.ru 
    
 ###五、替换Google 的jquery源
 
    cd views
    sed -i 's#//ajax.googleapis.com/ajax/libs/jquery/1.8.1/jquery.min.js#//lib.sinaapp.com/js/jquery/1.8.1/jquery.min.js#g' index.haml
    sed -i 's#//ajax.googleapis.com/ajax/libs/jqueryui/1.9.2/jquery-ui.min.js#//lib.sinaapp.com/js/jquery-ui/1.9.2/jquery-ui.min.js#g' index.haml
    sed -i 's#//ajax.googleapis.com/ajax/libs/jquery/1.7.2/jquery.min.js#//lib.sinaapp.com/js/jquery/1.7.2/jquery.min.js#g' patterns.haml
    sed -i 's#//ajax.googleapis.com/ajax/libs/jqueryui/1.9.0/themes/ui-lightness/jquery-ui.css#//lib.sinaapp.com/js/jquery-ui/1.9.0/themes/ui-lightness/jquery-ui.css#g' layout.haml
    sed -i 's#//ajax.googleapis.com/ajax/libs/jquery/1.7.2/jquery.min.js#//lib.sinaapp.com/js/jquery/1.7.2/jquery.min.js#g' discover.haml   

###六、启动服务

    nohup bundle exec unicorn -p 8080 -c ./unicorn &        
    