#### Lab3 DOL实例分析&编程
***
##### 实验任务

1. 修改example1，使其输出3次方数

2. 修改example2，让3个square模块变成2个

##### 实验结果
1. 修改example1

修改example1前的运行结果：
<img src="https://raw.githubusercontent.com/liaohl25/MarkdownImg/master/dol/1.1.PNG" width="300" alt="configure"/>

修改example1后的运行结果：
<img src="https://raw.githubusercontent.com/liaohl25/MarkdownImg/master/dol/1.2.PNG" width="300" alt="configure"/>

可以看到，修改前example1输出的结果是2次方数，修改后变为输出3次方数。

dot 图：

<img src="https://raw.githubusercontent.com/liaohl25/MarkdownImg/master/dol/1.3.PNG" width="400" alt="configure"/>

<br />
2. 修改example2

修改example2前的运行结果：
<img src="https://raw.githubusercontent.com/liaohl25/MarkdownImg/master/dol/2.1.PNG" width="300" alt="configure"/>

修改example2后的运行结果：
<img src="https://raw.githubusercontent.com/liaohl25/MarkdownImg/master/dol/2.2.PNG" width="300" alt="configure"/>

修改前example2输出的结果是 8 次方数，修改后变为输出 4 次方数。square模块的内容是进行一次平方运算，那么原来3个模块输出结果应当为 2^3 = 8 次，修改后2个模块输出结果为 2^2 = 4 次，结果符合理论值。

dot 图：

<img src="https://raw.githubusercontent.com/liaohl25/MarkdownImg/master/dol/2.5.PNG" width="600" alt="configure"/>

##### 修改过程
1. 修改example1

对数据处理的模块是 square，它将从生产者模块输入的数据进行一定的处理之后，输出到消费者模块。我们先来看square.c中对数据处理的代码段：
<pre>
if (p->local->index < p->local->len) {
	DOL_read((void*)PORT_IN, &i, sizeof(float), p);
	i = i*i;
	DOL_write((void*)PORT_OUT, &i, sizeof(float), p);
	p->local->index;
} 
</pre>

实现乘方的就是中间那句代码：
<pre>
	i = i*i;
</pre>

所以，等式右边再乘上一个 i 就可以实现数据的三次方计算了，也即：
<pre>
	i = i*i*i;
</pre>

修改完代码后查看结果需要重新编译、运行，在这之前需要把上一次编译运行生成的文件删除，否则重新运行的结果还是代码未修改前的结果。做法是，进入 build/bin/main 路径，然后把 example1 文件夹删除。

<br />
2. 修改example2

example2 的 square 模块的数量是在 xml 文件中的 iterator 控制的，先来看原来的代码段：

	  <variable value="3" name="N"/>
	  <!-- instantiate resources -->
	   ……
	  <iterator variable="i" range="N">
	    <process name="square">
 	     <append function="i"/>
	      <port type="input" name="0"/>
	      <port type="output" name="1"/>
	      <source type="c" location="square.c"/>
 	  	</process>
	  </iterator>
		……


N 即为 iterator 的迭代次数，通过 iterator 可以生成的 3 个 square 模块。所以我们要令 example2 生成 2 个 iterator ，只需要把 N 的值修改成 2 即可：

	<variable value="2" name="N"/>


与 example1 同理，重新编译运行需要先删除文件夹 example2 。

##### 实验感想

总的来说，这次实验的内容很好理解，工作量不大，主要是通过 2 个example 来熟悉 dol 的代码和结构。每个 dol 实例包括三个部分，生产者、平方模块、消费者，模块间通过通道连接，使用 iterator 来控制平方模块个数。每个 example 的 xml 文件里定义模块与模块之间的连接。

实验过程中被卡住的地方就是重新编译、运行的那个过程了，刚开始直接调用语句来编译、运行，输出的结果是未修改代码前的。通过对比其他的 example ，发现 example 运行过后会生成一些文件，把这些文件删除以后，再编译、运行就可以看到修改代码以后的效果了。