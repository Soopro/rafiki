#Soopro主题文件制作

##响应式html制作

使用bootstrap制作响应式网页，过程就不在这里进行赘述了。



##jinja2 template制作

jinja2 template是主题最终呈现的样式，整个制作过程中可以在本地进行调试，调试方法将在template制作的最后进行描述。

###外部文件使用主题路径

在所有的外部文件的路径之前增加{{theme_url}}，以便pyco能够识别该文件的位置，例：
      <img src="{{theme_url}}uploads/pic.jpg">

###"include文件"制作

这一步是将html中的一部分共用内容，比如<head>、<header>、<footer>等独立出来，以方便进行引用，其过程如下：
  * 1 新建html文件，命名之前加下划线"_",如"_head.html"
  * 2 将html文件中的整个<head>部分剪切，粘贴至"_head.html"中，在"_head.html"文件的最顶部写上统计数据的命令"{{sa}}"
  * 3 将所有html文件中的<head>部分删去，在<head>的位置写上"{% include '_head.html' %}"
  * 4 如法炮制其他共用内容
  

###"g"文件调用

在每一个template文件的最顶部引入"{% import "g" as g with context %}"

###Markdown 文件制作

pyco目录下有一个content文件，是用来模拟平台数据的，首先你得知道自己正在制作的template文件中哪些内容是可编辑的，你可以在template中调用Markdown文件中的属性，但是其中有一些属性是必不可少的，比如"Template"以及循环元素中的"Type"等：
  * 1  如果是在导航栏中的列表页面等主页面，本身不需要进入循环，那么，在"content"目录下直接新建该template的同名md文件，这里以index.html(无论如何，请把首页命名为"index"，因为这是平台默认的)为例，index只需要一个"Template"属性，因为这是必要的，而index其他内容都可以通过循环显示出来
  * 2  md文件中可以有其他数据，你编辑后可以直接在template文件中进行调用，例如，你可以编辑"Myname:Vilian"，那么，你可以在对应的template文件中某一处输入{{meta.myname}}来调用这个数据
  * 3 请按正确的书写格式进行编辑，例如：
      /*
      Title:index
      Nav:首页
      Priority:3
      Template:index
      Sever: 自1965年以来,我们已经提供了最美丽的和美味的季节性糕点。
      Contact: Phone&#58 310-377-7846<br>E-Mail: mayersbakery@aol.com
      */
另外还可以调用本主题的固定数据，方法是{{site_meta.title}}，但是最好在footer或者header等共用的部分才使用，具体请查看主题文档。


###循环

当然不能让用户编辑好了产品页面后一个个的再去填写列表，我们可以让产品页面按照我们设定好的模式自动循环出来，这里引入一个叫content_type的属性，我们只需要取出相应的type中产品的重要属性做成列表，显示在我们的列表页面或者首页就可以了。
  * 1 首先你需要在content目录下新建一个文件夹，命名最好是相应template的名字，因为这样的话，用户编辑网站时，在这个type里面新建一个页面就直接默认同名template了
  * 2 接下来就是在首页写上列表的循环了，相信你在制作html文件的时候，已经做好了相应的排版，这个时候，你可以把列表中的重复部分都删掉，只留一个，然后为这段程序增加一段for循环：
      {% set item_count = 0 %}
      {% for item in pages if item.type== "event" and item_count < 4 %}
      {% set item_count = item_count + 1 %}
      <div class="col-md-3 col-sm-6 event-chd">
        <div>
          <div class="event-img" style="background-image: url({{item.featured_img | thumbnail}})"></div>
          <h3>{{item.title}}</h3>
          <p>{{item.description}}</p>
          <div class="button">
            <a href="{{item.url}}">
              READ MORE
            </a>
          </div>
        </div>
      </div>
      {% endfor %}
      这样，就会在首页上显示最多4个这样的DOM了，其中"| thumbnall"的作用是将图片进行压缩后使用。


###type、taxonomy、term的关系

在制作过程中，这三者的关系着实让我绕了好一阵子，最后终于理清了思路：
  * 1 type:上一节已经讲到过type的使用，这里不单独进行描述了
  * 2 taxonomy&term:pyco中已经默认有两个属性"category"和"tags"，也就是有两个分别叫"category"和"tags"的taxonomy属性，你可以再分别为他们增加子属性，也就是term了，具体写法请参照content中的"site.json"文件，如果是category中的term，那么你可以在平台的编辑器中进行选择，以此作为某个循环的标记；tags中不需要增加子属性，在平台编辑器中，如果输入不同的属性，用逗号隔开，编辑器能自动将不同属性分开，用在哪里，你可以自行想象
  * 3 那么，现在他们的关系是：taxonomy中的属性可以映射在任何type属性中，他们是相互独立的，而term是taxonomy的子属性
  

###导航菜单的循环

相信你的导航菜单应该是放在header文件中的吧，没有也没关系，反正我们要对它动手了，这里引入一个menu属性，其中有一个默认属性"primary"，首先你应该在site.json文件中按照格式以及自己的内容编辑好，接下来就很简单了，又是一个for循环，例如：
<ul class="nav navbar-nav text-center">
  {% for item in menu.primary %}
  <li>
    <a href="{{item.url | url}}">
      {{item.title}}
    </a>
  </li>
  {% endfor %}
</ul>


###本地调试

jinja2 template制作过程中，当然是需要不断进行调试的，具体做法为：打开终端——打开pyco目录——运行pyco.py文件——获取本地ip——在浏览器中进入"localhost:ip"。


##tpl文件制作

只有需要编辑的模板才需要编辑tpl文件，tpl文件的制作难度并不高，但是由于无法进行本地调试，其过程如下：

###编辑
请在可编辑的标签上加上如下属性：
sup-editor-meta ng-model="meta.title"               短文本编辑
sup-angular-wysiwyg ng-model="content"              富文本编辑（一个文件中只能出现一处）
sup-editor-media ng-model="meta.featured_img"       图片编辑
sup-editor-media="bg" ng-model="meta.featured_img"  背景图片编辑


###循环

tpl文件中的循环与jinja2中的并不一样，它是绑定在标签上的，且一般分为两段：循环开始之前的标签和循环中的最外层标签，以导航条为例：
<ul class="nav navbar-nav" sup-editor-menu-query menu="primary" results="nav">
  <li ng-repeat="item in query.nav">
    <a href="#">
      {{item.title}}
    </a>
  </li>
</ul>

而在内容中的循环还需要调用一些其他数据，例如：
<div class="row blog" sup-editor-content-query
   fields="{type:'post'}"
   length="{{theme_meta.options.perpage}}"
   sortby="{{theme_meta.options.sortby}}"
   results="works">
  <div class="blog-list col-sm-10 col-sm-offset-1" ng-repeat="item in query.works" ng-if="item.title && item.featured_img && item.description   && item.author && item.category && item.date">
    <div class="row">
      <h4>{{item.title}}</h4>
      <div class="blog-event-img col-sm-5" style="background-image: url({{item.featured_img}})"></div>
      <div class="blog-event-right col-sm-7">
        <p class="blog-event-text">
          {{item.description}}
        </p>
        <div class="button">
          <a href="#">READ MORE</a>
        </div>
      </div>
      <div class="blog-event-info col-xs-11">
        <div class="col-sm-4 col-xs-6">
          <img src="{{theme_url}}images/blog_icon_person.png">
          <span>{{item.author}}</span>
        </div>
        <div class="col-sm-4 col-xs-6">
          <img src="{{theme_url}}images/blog_icon_tags.png">
          <span>{{item.category}}</span>
        </div>
        <div class="col-sm-4 col-xs-12">
          <span>{{item.date}}</span>
        </div>
      </div>
    </div>
  </div>
</div>


###tpl总结

tpl可以制作成与jinja模板一样的外观进行编辑，这种情况下请务必使编辑界面与网页效果一致；也可以制作成类似wordpress的表格填写进行编辑，这样的话可以在"<form>"中绑一个类"sup-editor-form",这样可以去掉多余的键入icon