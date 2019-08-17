# Taisite-Platform

## Ⅰ. 泰斯特平台简介

「泰斯特平台」是一个由「同时拥有丰富 软件测试 & 机器学习 经验的软件爱好者」开发的自动化测试平台。
 
 平台遵循「前后端分离开发」思想，技术栈为：「Python + Vue + Mongodb」，后端开发使用的是轻量级 Web 框架 Flask，
前端 UI 框架则采用的是易上手的 ElementUi。
    
 
 
 平台致力于将人工智能技术与软件测试有效结合，让平台在保障测试精确性要求的同时更具智能化、泛化能力。
 致力于成为让使用者拥有 **最佳使用体验** 的自动化测试解决方案。目标是成为 **最贴心、最智能、颜值最高**的开源测试平台。
    
## Ⅱ. 泰斯特平台特点 （os：和其他测试平台有什么区别？）

 1.平台遵循「小而精」的策略，最大化所有功能的开发、使用性价比，可帮助小型测试团队快速搭建起易于上手 / 维护的
 自动化测试体系。
 
 2.平台遵循「颜值即正义」原则，操作界面展示如下：
 
 ![操作界面展示](https://github.com/amazingTest/Taisite-Platform/blob/master/images/操作界面展示.png)
 
 3.平台拥有极佳的定时任务体验，启动定时任务后可随时停用 / 任意编辑任务内容且立即生效，同时拥有丰富的告警策略，
 页面展示如下：
 
 ![定时任务配置](https://github.com/amazingTest/Taisite-Platform/blob/master/images/定时任务配置.png)
 
 4.平台拥有丰富、详细、集中、实用的测试报告内容，展示如下：
 
 ![测试报告展示](https://github.com/amazingTest/Taisite-Platform/blob/master/images/测试报告展示.png)
 
 5.平台支持用例导入 / 导出功能，统一使用 Excel 格式，易于批量生成 / 修改用例。
 
 ![数据导入展示](https://github.com/amazingTest/Taisite-Platform/blob/master/images/数据导入展示.png)
 
 6.平台拥有丰富的测试结果校验体系，同时支持**文本相似度**校验方法，
 具体内容可参考[这篇博文](https://juejin.im/post/5cfe1dd96fb9a07ed7407321)
 
 7.......
 
 ***泰斯特平台还有许许多多令人惊喜的小特色等着你去探索 & 挖掘***
 

## Ⅱ. 泰斯特平台功能图解
    
    
    

## 

## 泰斯特平台部署

### windows 环境下部署
    
#### 1. 安装 python 3.6 环境

#### 2. 启动自然语言模型

##### 2.1 [点击下载压缩包](https://storage.googleapis.com/bert_models/2018_11_03/chinese_L-12_H-768_A-12.zip)

##### 2.2 解压压缩包

##### 2.3 安装 python 依赖包

    pip install tensorflow==1.11  -i https://pypi.tuna.tsinghua.edu.cn/simple
    pip install bert-serving-server -i https://pypi.tuna.tsinghua.edu.cn/simple
    
##### 2.4 启动模型
    
    // 当前目录切换至模型文件夹目录后执行
    bert-serving-start -model_dir ./chinese_L-12_H-768_A-12/ -num_worker=1
    
启动成功后输出如下：

![NLP模型启动成功输出](https://github.com/amazingTest/Taisite-Platform/blob/master/images/NLP模型启动成功输出.png)
 
#### 3. 部署 Mongo 数据库

#### 4. 设置系统环境变量
    
	AUTOTEST_PLATFORM_NLP_SERVER=127.0.0.1
    AUTOTEST_PLATFORM_MONGO_HOST=${MONGO_HOST}
    AUTOTEST_PLATFORM_MONGO_PORT=${MONGO_PORT}
    AUTOTEST_PLATFORM_MONGO_USERNAME=${USERNAME}
    AUTOTEST_PLATFORM_MONGO_PASSWORD=${PASSWORD}
    AUTOTEST_PLATFORM_MONGO_DEFAULT_DBNAME=${DBNAME}
	
设置完成后需重启电脑生效，可通过下列命令进行测试输出（CMD切换至项目根目录下）

	python ./backend/config.py
	
若配置成功则可看到配置的值

#### 5. 打包前端dist文件

##### 5.1 安装Vue环境，下载node.js并配置环境，下载npm包管理器

##### 5.2 cmd进入frontend目录下，配置 cnpm:
        
    npm install -g cnpm --registry=https://registry.npm.taobao.org    
        
##### 5.3 cmd进入frontend目录下，执行安装依赖包命令:

    cnpm install
    
##### 5.4 cmd进入frontend目录下，执行打包命令:

    cnpm run build

若成功打包则会在项目根目录下生成 dist 文件夹
   
#### 6. 启动后端

##### 6.1 安装后端依赖包
    
    // 切换至项目根目录下执行
    pip install -r ./backend/requirements.txt -i https://pypi.tuna.tsinghua.edu.cn/simple
    // 创建管理员帐号( 注意查看输出，记录下帐号密码 )
    python ./backend/createAdminUser.py
    // 启动后端 ( 默认5050端口 )
    python ./backend/run.py
    
#### 7. 访问项目

现在就可以访问 [http://127.0.0.1:5050/login](http://127.0.0.1:5050/login) 进行登录
    
### Linux 环境下 Docker 容器化部署 

[Docker 安装指南](https://www.runoob.com/docker/ubuntu-docker-install.html)

#### 1. 自然语言模型部署

	sudo -i
	docker pull shaoyuyishiwo/bertserver
	docker run --name autotest-platform-bertserver -d shaoyuyishiwo/bertserver 

#### 2. Mongo 数据库部署 (若已有现成数据库可用则可跳过此步)
  
##### 2.1 启动数据库 & 数据挂载至宿主机
    
    sudo -i
    docker pull mongo 
    docker run  --name autotest-platform-mongo -p 27017:27017 -v /data/db:/data/db -v /data/configdb:/data/configdb ``-d mongo
  
##### 2.2 创建数据库帐号

	docker exec -it autotest-platform-mongo /bin/bash
	
    mongo
    
	> use admin
  
	switched to db admin
  
    > db.createUser({user:"${USERNAME}",pwd:"${PASSWORD}",roles:["root"]})
  
	Successfully added user: { "user" : "admin", "roles" : [ "root" ] }

##### 2.3 数据库内存扩容(建议)
  
    > db.adminCommand({setParameter:1, internalQueryExecMaxBlockingSortBytes:335544320})
    
    { "was" : 33554432, "ok" : 1 }
 

#### 3. 环境变量配置
  
    // 编辑 /etc/profile 文件
    sudo -i
    vi /etc/profile
  
  若出现警告则选择 (E)dit anyway (输入 E)
  
  ##### 3.1 文本末端插入下列数据 (输入 i 则变为 insert 状态)
  
    export AUTOTEST_PLATFORM_NLP_HOST=${BERT_IPADRESS}
    export AUTOTEST_PLATFORM_MONGO_HOST=${MONGO_HOST}
    export AUTOTEST_PLATFORM_MONGO_PORT=${MONGO_PORT}
    export AUTOTEST_PLATFORM_MONGO_USERNAME=${USERNAME}
    export AUTOTEST_PLATFORM_MONGO_PASSWORD=${PASSWORD}
    export AUTOTEST_PLATFORM_MONGO_DEFAULT_DBNAME=${DBNAME}
	
  变量为动态值，部署者自行根据实际情况输入，${DBNAME} 值可任意自定义（数据库表名），
  其中 ${BERT_IPADRESS} 和 ${MONGO_HOST} 值可通过下列命令查询：
  
    docker inspect autotest-platform-bertserver
    docker inspect autotest-platform-mongo // 若使用了上面的步骤部署数据库
  
  输出如下图所示：
  
  ![控制台输出1](https://github.com/amazingTest/Taisite-Platform/blob/master/images/控制台输出1.png)
  
  ##### 3.2 插入完毕后点击 ESC 按钮、输入 :wq 后单击回车保存
  
  ##### 3.3 执行下列命令后环境变量立即生效
 
    source /etc/profile

#### 4. 启动项目
    
    //克隆项目
    git clone https://github.com/amazingTest/Taisite-Platform.git
    
    //执行部署文件
    sh deploy ${PORT} 
    
  其中 ${PORT} 变量填写项目访问端口即可，项目启动的同时也创建了管理员帐号，如下图所示：
  
  ![控制台输出2](https://github.com/amazingTest/Taisite-Platform/blob/master/images/控制台输出2.png)
  
#### 5. 访问项目

浏览器访问部署服务器地址的 ${PORT} 端口后使用「4.启动项目」中创建的帐号密码登陆即可

![平台登录界面](https://github.com/amazingTest/Taisite-Platform/blob/master/images/平台登录界面.png)


## Ⅱ. 泰斯特平台使用教程

    主流程使用可参考[这篇博文](https://juejin.im/post/5cd0117be51d456e537ef3bd)
    详细的教程后续补上:)




    
    
