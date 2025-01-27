##总结:
###☆1.参数绑定:(从请求中接收参数)
	1)默认支持的类型:Request,Response,Session,Model
	2)基本数据类型(包含String)
	3)Pojo类型
	4)Vo类型
	5)Converter自定义转换器
	6)数组
	7)List

###☆2.controller方法返回值(指定返回到哪个页面, 指定返回到页面的数据)
	1)ModelAndView 
		modelAndView.addObject("itemList", list); 指定返回页面的数据
		modelAndView.setViewName("itemList");	  指定返回的页面
	2)String(推荐使用)
		返回普通字符串,就是页面去掉扩展名的名称, 返回给页面数据通过Model来完成
		返回的字符串以forward:开头为请求转发
		返回的字符串以redirect:开头为重定向
	3)返回void(使用它破坏了springMvc的结构,所以不建议使用)
		可以使用request.setAttribut 来给页面返回数据
		可以使用request.getRquestDispatcher().forward()来指定返回的页面
		如果controller返回值为void则不走springMvc的组件,所以要写页面的完整路径名称

	相对路径:相对于当前目录,也就是在当前类的目录下,这时候可以使用相对路径跳转
	绝对路径:从项目名后开始.
	在springMvc中不管是forward还是redirect后面凡是以/开头的为绝对路径,不以/开头的为相对路径
	例如:forward:/items/itemEdit.action 为绝对路径
		forward:itemEdit.action为相对路径


###3.架构级别异常处理:
	主要为了防止项目上线后给用户抛500等异常信息,所以需要在架构级别上整体处理.hold住异常
	首先自定义全局异常处理器实现HandlerExceptionResolver接口
	在spirngMvc.xml中配置生效

###4.上传图片:

	1)在tomcat中配置虚拟图片服务器
	2)导入fileupload的jar包
	3)在springMvc.xml中配置上传组件
	4)在页面上编写上传域,更改form标签的类型
	5)在controller方法中可以使用MultiPartFile接口接收上传的图片
	6)将文件名保存到数据库,将图片保存到磁盘中

###5.Json数据交互:

	需要加入jackson的jar包
	@Requestbody:将页面传到controller中的json格式字符串自动转换成java的pojo对象
	@ResponseBody:将java中pojo对象自动转换成json格式字符串返回给页面

###6.RestFul支持:

	就是对url的命名标准,要求url中只有能名词,没有动词(不严格要求),但是要求url中不能用问号?传参
	传参数:
		页面:${pageContext.request.contextPath }/items/itemEdit/${item.id}
		方法: @RquestMapping("/itemEdit/{id}")
		方法: @PathVariable("id") Integer idd

###7.拦截器:

	作用:拦截请求,一般做登录权限验证时用的比较多
	1)需要编写自定义拦截器类,实现HandlerInterceptor接口
	2)在spirngMvc.xml中配置拦截器生效

###8.登录权限验证:

	1)编写登录的controller, 编写跳转到登录页面的方法,  编写登录验证方法
	2)编写登录页面
	3)编写拦截器

###运行过程:

	1)访问随意一个页面,拦截器会拦截请求,会验证session中是否有登录信息
		如果已登录,放行
		如果未登录,跳转到登录页面
	2)在登录页面中输入用户名,密码,点击登录按钮,拦截器会拦截请求,如果是登录路径放行
		在controller方法中判断用户名密码是否正确,如果正确则将登录信息放入session

