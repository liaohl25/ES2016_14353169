### Description

> The distributed operation layer (DOL) is a framework that enables the (semi-) automatic mapping of applications onto the multiprocessor SHAPES architecture platform. The DOL consists of basically three parts:

>* **DOL Application Programming Interface**: The DOL defines a set of computation and communication routines that enable the programming of distributed, parallel applications for the SHAPES platform. Using these routines, application programmers can write programs without having detailed knowledge about the underlying architecture. In fact, these routines are subject to further refinement in the hardware dependent software (HdS) layer.

>* **DOL Functional Simulation**: To provide programmers a possibility to test their applications, a functional simulation framework has been developed. Besides functional verification of applications, this framework is used to obtain performance parameters at the application level.

>* **DOL Mapping Optimization**: The goal of the DOL mapping optimization is to compute a set of optimal mappings of an application onto the SHAPES architecture platform. In a first step, XML based specification formats have been defined that allow to describe the application and the architecture at an abstract level. Still, all the information necessary to obtain accurate performance estimates is contained.


### How to install
* 安装一些必要的环境

<pre>
$	sudo apt-get update  
$	sudo apt-get install ant
$ 	sudo apt-get install openjdk-7-jdk 
$	sudo apt-get install unzip
</pre>


* 下载文件

下载systemc-2.3.1.tgz和dol_ethz.zip，下面语句是使用VMware虚拟机下载，也可以从主机下载拷贝到虚拟机中去： <http://jingyan.baidu.com/article/c33e3f48a5c153ea15cbb5b2.html>
<pre>
$	sudo wget http://www.accellera.org/images/downloads/standards/systemc/systemc-2.3.1.tgz
$	sudo wget http://www.tik.ee.ethz.ch/~shapes/downloads/dol_ethz.zip
</pre>

* 解压文件

	1、新建DOL的文件夹

	`$	mkdir dol`

	2、将dolethz.zip解压到 dol文件夹中

	`$	unzip dol_ethz.zip -d dol`

	3、解压systemc

	`$	tar -zxvf systemc-2.3.1.tgz`

* 编译systemc

	1、解压systemc后进入systemc-2.3.1的目录下
	
	`$	cd systemc-2.3.1`

	2、新建一个临时文件夹objdir

	`$	mkdir objdir`

	3、进入该文件夹objdir

	`$	cd objdir`
	
	4、运行configure(能根据系统的环境设置一下参数，用于编译)

	`$	../configure CXX=g++ --disable-async-updates`

	下图为运行configure之后的截图：

	<img src="https://raw.githubusercontent.com/liaohl25/MarkdownImg/master/res/5.jpg" width="500" alt="configure"/>

	5、编译

	`$	sudo make install`

	6、编译完后文件进入目录，如下

	<pre>$ cd systemc-2.3.1        
	$ ls</pre>
	得到结果为：

	<img src="https://raw.githubusercontent.com/liaohl25/MarkdownImg/master/res/1.PNG" width="500" alt="configure"/>

	7、记录当前的工作路径(会输出当前所在路径，记下来，待会有用)

	`$	pwd`
	
	结果为：

	<img src="https://raw.githubusercontent.com/liaohl25/MarkdownImg/master/res/2.PNG" width="400" alt="configure"/>

	表示我当前的工作路径为  **/home/lhl14353169/systemc-2.3.1**

* 编译dol

	1、进入刚刚dol的文件夹

	`$	cd ../dol`
	
	2、修改build_zip.xml文件

	找到下面这段话，就是说上面编译的systemc位置在哪里：
	<pre>
	property name="systemc.inc" value="YYY/include"
	property name="systemc.lib" value="YYY/lib-linux/libsystemc.a"
	</pre>
	
	把YYY改成上页pwd的结果（注意，对于  64位 系统的机器，lib-linux要改成lib-linux64）

	3、编译dol
		
	`$	ant -f build_zip.xml all`
	
	如果成功会显示build successful，如下图：

	<img src="https://raw.githubusercontent.com/liaohl25/MarkdownImg/master/res/3.PNG" width="500" alt="configure"/> 
<br />
* 运行例子

	1、进入build/bin/mian路径下
	
	 `$	cd build/bin/main`

	2、然后运行第一个例子
	
	`$	ant -f runexample.xml -Dnumber=1`
	
	成功结果如下图：

	<img src="https://raw.githubusercontent.com/liaohl25/MarkdownImg/master/res/4.PNG" width="350" alt="configure"/>

	至此，整个配置dol过程结束。

### Experimental experience
之前有浏览过过Github上的一些代码，但是完全没有了解过Github和Git，学习Git过程中，虽然有很浅显易懂的教程，但还是感觉有很多看不懂的地方，还是要结合操作来学习。在这次实验中，首先在Github上创建好仓库，然后在虚拟机上创建本地库，然后将仓库和本地库连接起来。

关联好仓库和本地库后，编辑dol配置笔记。除了Github和Git，也没有使用过markdown,markdown操作起来还是比较容易上手的。由于之前dol配置之前顺利地完成了，但是都没有截图，所以写安装笔记的时候又去重复了一遍操作，但有一些中间过程的图就没办法截了。花了比较多的时间插入图片，上网查了一些资料，最后采用先把图片放在Github的仓库，然后复制链接粘贴到markdown里边。应该还有其他更加简便的方法，自己还没有研究好怎么使用。编辑好以后把文档放入本地库，然后上传到Github仓库中。虽然实验内容看起来不多，但是整个实验完成得挺艰辛的。

后续：原本以为写好了文档，然后可以按照网上的教程把文档push到Github上就好了，但事实证明我想得太简单了。整个push的过程，先是config失败了，好不容易发现是自己少打了两个空格……解决以后又出现类似下面的一系列错误  

<img src="https://raw.githubusercontent.com/liaohl25/MarkdownImg/master/res/error2.PNG" width="470" alt="configure"/>
<br/>
<img src="https://raw.githubusercontent.com/liaohl25/MarkdownImg/master/res/error.PNG" width="550" alt="configure"/>

查了解决方案所以尝试各种解决的语句，然后又冒出各种错误，一路摸索，磕磕碰碰，git也重装了很多次。最后面还是在廖雪峰老师的教程找到解决方法。针对上图中后面那种错误，老师是这样说的：

> 推送失败，因为你的小伙伴的最新提交和你试图推送的提交有冲突，解决办法也很简单，Git已经提示我们，先用git pull把最新的提交从origin/dev抓下来，然后，在本地合并，解决冲突，再推送

按照老师的[解决方案](http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/0013760174128707b935b0be6fc4fc6ace66c4f15618f8d000)，依次先执行下面两句：
<pre>
$	git branch
$	git pull
</pre>
pull成功以后，会出现冲突，然后去[解决冲突](http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/001375840202368c74be33fbd884e71b570f2cc3c0d1dcf000)。后面执行语句：

`$ git log --graph --pretty=oneline --abbrev-commit`

看到了分支的合并情况：

<img src="https://raw.githubusercontent.com/liaohl25/MarkdownImg/master/res/%E6%8D%95%E8%8E%B7.PNG" width="300" alt="configure"/>

现在大概明白了原来是自己同时push不同版本的文档，虽然我也不知道自己怎么做到的…应该是不熟悉操作，然后就胡乱执行语句。因为之前没有开始操作，很多老师博客里的东西看不懂，就还没看完，现在一定会找时间学习的。最后，放上一张成功push的截图，太不容易了。

<img src="https://raw.githubusercontent.com/liaohl25/MarkdownImg/master/res/push.PNG" width="500" alt="configure"/>
