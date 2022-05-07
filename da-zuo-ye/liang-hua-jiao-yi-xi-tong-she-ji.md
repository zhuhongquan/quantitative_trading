# 量化交易系统设计

<mark style="color:orange;">注：以下内容并不完整，仅供参考。</mark>

## 数据库设计

可以采用一些较为主流的数据库，例如MySQL。针对各个功能模块，设计不同的表来存储相应的信息。

* 用户表——存放用户信息\
  字段：用户名、密码、邮箱、电话等个人信息。
* 股票日行情表——存放沪深股票每日的行情信息\
  字段：股票代码、股票名称、交易日期、开盘价、最高价、最低价、收盘价、成交量......\
  注：为了减少大家工作量，大家可以只选择50-100个股票最近一年的数据进行导入。<mark style="color:orange;">该表是日行情数据，只需考虑以天为单位的行情，无需记录每天中任意交易时刻的数据。</mark>
* 用户订单表——存放每个用户的交易订单\
  字段：订单编号、用户id、股票代码、交易时间、交易价格、交易数量......
* 策略表——存放用户自定义策略
* 新闻表——存放爬取的新闻
* ......

## 后端基础教程(Tornado)

### 安装Tornado

pip install tornado

### 创建一个Tornado项目

我给大家提供了一个Tornado项目的模板：

```python
import tornado.web
import tornado.httpserver
import tornado.ioloop
import tornado.options
from tornado.escape import json_decode, json_encode


class BaseHandler(tornado.web.RequestHandler):
    def __init__(self, *argc, **argkw):
        super(BaseHandler, self).__init__(*argc, **argkw)

    # 解决跨域问题
    def set_default_headers(self):
        self.set_header("Access-Control-Allow-Origin", "*")  # 写成*需要Vue前端withCredentials设置为false，否则要写前端域名
        self.set_header("Access-Control-Allow-Methods", "POST, GET, OPTIONS")
        self.set_header("Access-Control-Allow-Headers", "*")
        self.set_header("Access-Control-Max-Age", 1000)
        self.set_header("Content-type", "application/json")

    def get(self):
        self.write('request get')

    def post(self):
        self.write('request post')

    # vue一般需要访问options方法， 如果报错则很难继续，所以只要通过就行了，当然需要其他逻辑就自己控制。
    def options(self):
        # # 返回方法1
        # self.set_status(204)
        # self.finish()
        # # 返回方法2
        self.write(json_encode({"code": "0", "Message": "success"}))


class mainHandler(BaseHandler):
    # 主页
    # @tornado.web.authenticated
    def get(self, *args, **kwargs):
        self.write("Hello!")


class LoginHandler(BaseHandler):
    """
    登录接口
    """
    def post(self):
        # 获取用户登录的用户名
        try:
            # 1.处理axios的数据，Content-Type: application/json， b'{"username":"admin","password":"admin"}'
            data = json.loads(self.request.body)  # json.dumps
            username = data['username']
            password = data['password']
            mysql = MySQL()
            sql_str = "select * from user where username='{}' and password='{}'".format(username, password)
            res = mysql.query(sql_str)
            if len(res) > 0:  # 用户名密码正确
                self.write(json_encode({'code': 0, "msg": "success"}))
            else:
                self.write(json_encode({'code': 1, "msg": "error"}))
            # 2.获取ajax发送过来的请求，'Content-Type': 'application/x-www-form-urlencoded',  b'username=admin&password=admin'
            # username = self.get_argument('username')
            # password = self.get_argument('password')
            print(username, password)
        except Exception as e:
            print("无法解析数据", e)
            self.write(json_encode({'code': 1000, "msg": "请求的参数错误！"}))


class Application(tornado.web.Application):
    def __init__(self):
        handlers = [
            (r'/', mainHandler),
            (r'/login', LoginHandler),
        ]

        settings = {
            'template_path': 'templates',
            'static_path': 'static',
            "cookie_secret": "bZJc2sWbQLKos6GkHn/VB9oXwQt8S0R0kRvJ5/xJ89E=",
            "login_url": "/login"
        }

        tornado.web.Application.__init__(self, handlers, **settings)


if __name__ == '__main__':
    tornado.options.parse_command_line()
    app = tornado.httpserver.HTTPServer(Application())
    app.listen(8000)
    print("Tornado server is ready for service: http://localhost:8000/\r")
    tornado.ioloop.IOLoop.instance().start()

```

## 前端基础教程

先甩一个我觉得讲得比较详细的入门教程：[https://blog.csdn.net/play\_big\_knife/article/details/111715598](https://blog.csdn.net/play\_big\_knife/article/details/111715598)

前端选用Vue，可以采用node+npm+vue/cli(vue脚手架)的方式来快速搭建Vue项目

你可能需要自行补习的知识：

Node.js，npm，vue.js，vue-cli以及非常复杂且多的前端知识。

### 安装node环境

到[官网](https://nodejs.org/zh-cn/)下载Node安装包，一直next安装。

安装完成后命令行输入node -v，即可看到Node版本，检验是否安装成功。

### 安装vue/cli

关于vue/cli的介绍和使用教程，请看vue/cli[官网](https://cli.vuejs.org/zh/guide/)。

使用`npm install -g @vue/cli`安装vue脚手架，`vue -V`查看vue/cli(vue脚手架)版本。

![](<../.gitbook/assets/image (8).png>)

![](<../.gitbook/assets/image (10).png>)

再运行一个命令：`npm i -g @vue/cli-init`，这是用来初始化vue2.X项目的。

你可以把Node.js类比成Python解释器，一个是js的运行环境，一个是Python的运行环境；把npm类比成pip，都是用来安装各自库的工具；vue/cli就是第三方库，我们可以用vue/cli来快速地构建Vue项目。注意，vue/cli不等于Vue。

通过以上几步，我们需要准备的环境和工具都准备好了，接下来就开始使用vue/cli来构建项目。

### 构建Vue项目

首先我们要选择存放项目的位置，然后再用命令行cd到项目的目录中，在这里，我选择在D盘下创建新的目录（D:\VueProjects），用cd 将目录切到该目录下，如下图：

![](<../.gitbook/assets/image (13).png>)

在VueProjects目录下，在命令行中运行命令 `vue init webpack firstApp`（初始化一个完整版的项目）。解释一下这个命令，这个命令的意思是初始化一个项目，其中webpack是构建工具，也就是整个项目是基于webpack的。其中firstApp是整个项目文件夹的名称，这个文件夹会自动生成在你指定的目录中（我的实例中，会在VueProjects目录生成该文件夹）。

输入命令后，会询问我们几个简单的选项，我们根据自己的需要进行填写就可以了。

* Project name：项目名称 ，如果不需要更改直接回车就可以了。注意：这里不能使用大写，所以我把名称改成了first-app
* Project description：项目描述，默认为A Vue.js project，直接回车，不用编写。&#x20;
* Author：作者，如果你有配置git的作者，他会读取。
* Install vue-router? 是否安装vue的路由插件，我们这里需要安装，所以选择Y。
* Use ESLint to lint your code? 是否用ESLint来限制你的代码错误和风格。我们这里不需要输入n（建议），如果你是大型团队开发，最好是进行配置。
* setup unit tests with Karma + Mocha? 是否需要安装单元测试工具Karma+Mocha，我们这里不需要，所以输入n。
* Setup e2e tests with Nightwatch? 是否安装e2e来进行用户行为模拟测试，我们这里不需要，所以输入n。

运行初始化命令的时候会让用户输入几个基本的配置选项，如项目名称、项目描述、作者信息，对于有些不明白或者不想填的信息可以一直按回车去填写就好了，等待一会，就会显示创建项目创建成功，如下图:

![](<../.gitbook/assets/image (15).png>)

接下来，我们去VueProjects目录下去看是否已创建文件：

![](<../.gitbook/assets/image (5).png>)

打开firstApp 项目，项目中的目录如下：

![](<../.gitbook/assets/image (9).png>)

介绍一下目录及其作用：

* build：最终发布的代码的存放位置。
* config：配置路径、端口号等一些信息，我们刚开始学习的时候选择默认配置。
* node\_modules：npm 加载的项目所需要的各种依赖模块。
* src：这里是我们开发的主要目录（源码），基本上要做的事情都在这个目录里面，里面包含了几个目录及文件：
* assets:放置一些图片(会根据图片大小分类进行base64命名还是其他方式命名)，如logo等
* components:目录里放的是一个个的组件文件
* router/index.js：配置路由的地方
* App.vue：项目入口组件（根组件），我们也可以将组件写这里，而不使用components目录。主要作用就是将我们自己定义的组件通过它与页面建立联系进行渲染，这里面的必不可少。
* main.js ：项目的核心文件（整个项目的入口js）引入依赖包、默认页面样式等（项目运行后会在index.html中形成一个app.js文件）。
* static：静态资源目录(会原分不动的对文件进行处理)，如图片、字体等。
* test：初始测试目录，可删除
* .XXXX文件：配置文件。
* index.html：html单页面的入口页面，可以添加一些meta信息或者同统计代码啥的或页面的重置样式等。
* package.json：项目配置信息文件/所依赖的开发包的版本信息及所依赖的插件信息。（大概版本）
* package-lock.json：项目配置信息文件/所依赖的开发包的版本信息及所依赖的插件信息。（具体版本）
* README.md：项目的说明文件。
* webpack.config.js：webpack的配置文件，例：把.vue的文件打包成浏览器能读懂的文件。
* .babelrc:是检测es6语法的配置文件，例：适配哪些浏览器的限制
* .gitignore:上传到服务器忽略哪些文件的配置（比如模拟本地数据mock不让他在get提交/打包上线的时候忽略不使用可在这里配置）
* .postcssrc.js:前缀的配置 （css转化的配置）
* .editorconfig:对代码进行规范，例：root是否进行检测，代码尾部是否换行，缩行前面几个空格...（建议定义这个规范）

这就是整个项目的目录结构，其中，我们<mark style="color:red;">主要在src目录中做修改（模块化开发）</mark>。这个项目现在还只是一个结构框架，整个项目需要的依赖资源都还没有安装。

cd 项目文件夹名，进入项目中，**如果你前面初始化项目时npm install没有成功，请看下面步骤**，成功了请忽略——安装项目所需要的依赖包/插件（在package.json可查看）：执行 npm install (如果npm没有成功，这里可以用cnpm代替npm，**运行别人的代码一定先安装依赖**)。

**若拿到别人的项目或从github上下载的项目，第一步就是要在项目中npm install；下载项目所依赖的插件，然后npm run dev 运行项目。**

> npm install 命令用来安装模块到node\_modules目录
>
> npm install 安装之前，npm install会先检查，node\_modules目录之中是否已经存在指定模块。如果存在，就不再重新安装了，即使远程仓库已经有了一个新版本，也是如此。
>
> npm install --force / npm install -f 如果你希望，一个模块不管是否安装过，npm 都要强制重新安装，可以使用-f或–force参数。

**现在，我们就可以运行整个前端项目了！**

在项目目录中，运行命令 npm run dev （npm run start），会用热加载的方式运行我们的应用，热加载可以让我们在修改完代码后不用手动刷新浏览器就能实时看到修改后的效果。

![](<../.gitbook/assets/image (14).png>)

项目启动后，在浏览器中输入项目启动后的地址：

![](<../.gitbook/assets/image (12).png>)

这样，vue/cli脚本架就搭建成一个vue的项目。

### **安装element-ui**

**element-ui**是一套UI组件库（注意Vue2.0和Vue3.0是不一样的），使用`npm install element-ui --save`命令（还是要在项目文件夹内执行命令）进行安装。

### Vue文件的认识

通过前面的操作，项目搭建成功，element-ui插件也安装完毕了。现在可以用编辑器打开这个项目，认识一下项目中各个文件的相关功能。

这里使用PyCharm做为编辑器，对Vue文件进行编辑。（你也可以使用专业的WebStorm）

PyCharm中点击“File”菜单中的“Settings”项，在打开的对话框中，左边的树形结构有“plugins”插件的选项，然后在右边的“Marketplace”中找到“Vue”的选项。如下图所示。

![](<../.gitbook/assets/image (16) (1).png>)

现在用PyCharm菜单中的“Open”选项打开刚刚创建成功的Vue项目，如下图所示：

![](<../.gitbook/assets/image (18).png>)

图中所示的是vue/cli脚手架创建的Vue项目结构图，关于每个文件或目录的功能，在前面已经介绍过。

下面就写一个登录页面。

### 登录页面

参考文章：[https://www.jianshu.com/p/e8aae5bc49cd](https://www.jianshu.com/p/e8aae5bc49cd)，里面有错误，所以还是看下面的代码

首先，在main.js中导入element-ui

```jsx
import ElementUI from 'element-ui';
import 'element-ui/lib/theme-chalk/index.css';
Vue.use(ElementUI);
```

在assets文件下新建css文件夹，新建global.css

```css
/*全局样式表*/
html,body,#app{
  height: 100%;
  margin: 0;
  padding: 0;
}
```

在main.js 中导入（添加一行）

```dart
import './assets/css/global.css';
```

在components文件夹下新建登录页面Login.vue，将以下内容复制进去

```n4js
<template>
  <div class="login_container">
    <div class="login_box">
      <div class="avatar_box">
        <img src="../assets/logo.png" alt="">
      </div>
      <el-form label-width="0px" class="login_in" ref="loginForm" :model="loginForm" :rules="rules">
        <el-form-item prop="userName">
          <el-input v-model=loginForm.userName type="text" prefix-icon="el-icon-user"></el-input>
        </el-form-item>
        <el-form-item prop="passWord">
          <el-input v-model=loginForm.passWord type="password" prefix-icon="el-icon-thumb"></el-input>
        </el-form-item>
        <el-form-item class="btns">
          <el-button type="primary" @click="submitForm('loginForm')">登录</el-button>
          <el-button>重置</el-button>
        </el-form-item>
      </el-form>
    </div>
  </div>
</template>

<script>
export default {
  name: "Login",
  data() {
    return {
      loginForm: {
        userName: undefined,
        passWord: undefined
      },
      rules: {
        userName: [
          {required: true, message: '请输入用户名', trigger: 'blur'},
          {min: 3, max: 8, message: '长度在3到8个字符', trigger: 'blur'}
        ],
        passWord: [
          {required: true, message: '请输入密码', trigger: 'blur'},
          {min: 5, max: 16, message: '长度在5到16个字符', trigger: 'blur'}
        ]
      }
    }
  },
  methods: {
    
  }
}
</script>

<style scoped>
.login_container {
  background-color: #42b983;
  height: 100%;
}

.login_box {
  width: 450px;
  height: 300px;
  background-color: #fff;
  border-radius: 3px;
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
}

.login_box .avatar_box {
  height: 130px;
  width: 130px;
  border: 1px solid #eee;
  border-radius: 50%;
  box-shadow: 0 0 10px #ddd;
  position: absolute;
  left: 50%;
  transform: translate(-50%, -50%);
  background-color: #fff;
  padding: 10px;
}

.login_box .avatar_box img {
  width: 100%;
  height: 100%;
  border-radius: 50%;
  background-color: #eee;
}

.btns {
  display: flex;
  justify-content: flex-end;

}

.login_in {
  position: absolute;
  bottom: 0;
  width: 100%;
  padding: 0 20px;
  box-sizing: border-box;
}
</style>

```

router文件夹下index.js添加一些内容（<mark style="color:red;">注意是添加</mark>，不是覆盖源文件！）

```jsx
import Login from '../components/Login'

export default new Router({
  mode: "history",
  routes: [
    {
      path: '/',
      name: 'HelloWorld',
      component: HelloWorld
    },
    {
      path: '/login',
      name: 'Login',
      component: Login
    }
  ]
})
```

然后浏览器中输入网址[http://localhost:8080/login](http://localhost:8080/login)，就可以看到登录界面了！

到现在为止，我们只是做了个界面，如何能使得”登录“起作用（也就是发送数据给后端）？

这就涉及到之前讲的get/post请求。

Vue中有几种发送post请求的方法，例如axios，ajax等等，这边把两种方法都写在这里，但是<mark style="color:red;">**建议大家之后使用主流的axios方法**</mark>。

**（1）ajax方法**

我们先安装一个东西——jQuery，它是一个js中能帮助我们发送post请求的工具：[https://www.csdn.net/tags/MtjaggysNzQ0Mi1ibG9n.html](https://www.csdn.net/tags/MtjaggysNzQ0Mi1ibG9n.html)

然后我们就可以在Login.vue中为登录按钮编写方法，实现发送post请求了。

采用jquery的ajax方法，我们需要在项目中安装jquery，看这个教程：[https://www.jianshu.com/p/baee4ff78c27](https://www.jianshu.com/p/baee4ff78c27)

装完之后，我们就在methods里面为按钮编写方法：

```python
  methods: {
    submitForm(formName) {
      let data =  {username: this.loginForm.userName, password: this.loginForm.passWord};

      // ajax方式发送请求
      $.ajax({
        url: 'http://localhost:8000/login',  //接受请求的链接
        type: 'POST',                        //请求方式，还有GET
        data: data,                          //要发送的数据
        dataType: 'json',
        //contentType : 'application/json',
        success: function (data, status, xhr) {
          // Do Anything After get Return data
        },
        Error: function (xhr, error, exception) {
          // handle the error.
          alert(exception.toString());
        }
      });
    }
  }
```

**（2）axios方法：**

第一步：安装axios包，我们依然在这个项目目录中打开终端，输入命令：\
`npm install --save axios`

第二步：在vue的 main.js中导入axios

![](<../.gitbook/assets/image (16).png>)

第三步：设计默认路由前缀

继续在main.js中添加一行，这是因为我们日后请求的路径很多，但他们的前缀都一样，我们可以设置统一的前缀，这边就是我们的后端服务器http://localhost:8000/

```
axios.defaults.baseURL='http://localhost:8000/'
```

第四步：在Login.vue中编写对应方法

注意，这边不能和前面的ajax方法同时存在，只能选择一种。

```javascript
  methods: {
    submitForm(formName) {
      let config = {
        // 关于comnteng-type，看https://www.jb51.net/article/145209.htm
        headers: {
          'Content-Type': 'application/json'  //默认是这个
          // 'Content-Type': 'multipart/form-data'
          // 'Content-Type': 'application/x-www-form-urlencoded'
        }
      }
      let data =  {username: this.loginForm.userName, password: this.loginForm.passWord};

      // axios方式发送请求
      this.$axios.post('/login', data, config).then(response => {
        console.log(response.data)
      }).catch(function (error) { // 请求失败处理
          console.log(error);
        });
    }
  }
```
