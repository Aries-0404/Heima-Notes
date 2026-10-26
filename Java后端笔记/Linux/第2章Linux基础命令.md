# 第2章Linux基础命令

## 1、Linux目录结构

![image-20240621153955483](%E7%AC%AC2%E7%AB%A0Linux%E5%9F%BA%E7%A1%80%E5%91%BD%E4%BB%A4.assets/image-20240621153955483.png)

## notes：Linux路径描述方式

![image-20240621154035328](%E7%AC%AC2%E7%AB%A0Linux%E5%9F%BA%E7%A1%80%E5%91%BD%E4%BB%A4.assets/image-20240621154035328.png)



## 2、Linux命令基础格式

![image-20240621154245994](%E7%AC%AC2%E7%AB%A0Linux%E5%9F%BA%E7%A1%80%E5%91%BD%E4%BB%A4.assets/image-20240621154245994.png)

## notes：Home目录

==每个用户登录时的默认工作目录==

![image-20240621154952731](%E7%AC%AC2%E7%AB%A0Linux%E5%9F%BA%E7%A1%80%E5%91%BD%E4%BB%A4.assets/image-20240621154952731.png)

![image-20240621155412583](%E7%AC%AC2%E7%AB%A0Linux%E5%9F%BA%E7%A1%80%E5%91%BD%E4%BB%A4.assets/image-20240621155412583.png)





## 3、常见的Linux命令

### （1）ls命令（展示文件目录的命令）

![image-20240621154917185](%E7%AC%AC2%E7%AB%A0Linux%E5%9F%BA%E7%A1%80%E5%91%BD%E4%BB%A4.assets/image-20240621154917185.png)

#### notes：ls命令的 `-a -l -h`选项

![image-20240621220223576](%E7%AC%AC2%E7%AB%A0Linux%E5%9F%BA%E7%A1%80%E5%91%BD%E4%BB%A4.assets/image-20240621220223576.png)

![image-20240621220358064](%E7%AC%AC2%E7%AB%A0Linux%E5%9F%BA%E7%A1%80%E5%91%BD%E4%BB%A4.assets/image-20240621220358064.png)

![image-20240621220459453](%E7%AC%AC2%E7%AB%A0Linux%E5%9F%BA%E7%A1%80%E5%91%BD%E4%BB%A4.assets/image-20240621220459453.png)

![image-20240621220525648](%E7%AC%AC2%E7%AB%A0Linux%E5%9F%BA%E7%A1%80%E5%91%BD%E4%BB%A4.assets/image-20240621220525648.png)

#### notes：如何判断一个目录是文件夹还是文件？

除了颜色不同外，当用ls-l命令查看的时候如果前面是用D开头代表是文件夹否则是文件

![image-20240621164955126](%E7%AC%AC2%E7%AB%A0Linux%E5%9F%BA%E7%A1%80%E5%91%BD%E4%BB%A4.assets/image-20240621164955126.png)

 

 

### （2）cd命令（切换工作目录的命令）

![image-20240621220608725](%E7%AC%AC2%E7%AB%A0Linux%E5%9F%BA%E7%A1%80%E5%91%BD%E4%BB%A4.assets/image-20240621220608725.png)

### （3）pwd命令（查看当前工作目录的命令）

![image-20240621220643715](%E7%AC%AC2%E7%AB%A0Linux%E5%9F%BA%E7%A1%80%E5%91%BD%E4%BB%A4.assets/image-20240621220643715.png)



## notes：绝对路径、相对路径、和特殊路径

![image-20240621220844087](%E7%AC%AC2%E7%AB%A0Linux%E5%9F%BA%E7%A1%80%E5%91%BD%E4%BB%A4.assets/image-20240621220844087.png)

![image-20240621220859969](%E7%AC%AC2%E7%AB%A0Linux%E5%9F%BA%E7%A1%80%E5%91%BD%E4%BB%A4.assets/image-20240621220859969.png)

![image-20240621221003646](%E7%AC%AC2%E7%AB%A0Linux%E5%9F%BA%E7%A1%80%E5%91%BD%E4%BB%A4.assets/image-20240621221003646.png)



### （4）mkdir命令（创建文件夹的命令）

![image-20240621221115603](%E7%AC%AC2%E7%AB%A0Linux%E5%9F%BA%E7%A1%80%E5%91%BD%E4%BB%A4.assets/image-20240621221115603.png)

#### notes：mkdir命令的`-p`选项

![image-20240621221219170](%E7%AC%AC2%E7%AB%A0Linux%E5%9F%BA%E7%A1%80%E5%91%BD%E4%BB%A4.assets/image-20240621221219170.png)



### （5）文件操作命令

#### ①touch命令（创建文件的命令）

![image-20240621221512147](%E7%AC%AC2%E7%AB%A0Linux%E5%9F%BA%E7%A1%80%E5%91%BD%E4%BB%A4.assets/image-20240621221512147.png)

#### ②cat命令（查看文件内容的命令）

![image-20240621221541877](%E7%AC%AC2%E7%AB%A0Linux%E5%9F%BA%E7%A1%80%E5%91%BD%E4%BB%A4.assets/image-20240621221541877.png)

#### ③more命令（查看文件内容的命令）

![image-20240621221631448](%E7%AC%AC2%E7%AB%A0Linux%E5%9F%BA%E7%A1%80%E5%91%BD%E4%BB%A4.assets/image-20240621221631448.png)

#### ④cp命令（复制文件/文件夹的命令）

![image-20240621221843294](%E7%AC%AC2%E7%AB%A0Linux%E5%9F%BA%E7%A1%80%E5%91%BD%E4%BB%A4.assets/image-20240621221843294.png)

#### notes：cp命令的`-r`选项

![image-20240621221944987](%E7%AC%AC2%E7%AB%A0Linux%E5%9F%BA%E7%A1%80%E5%91%BD%E4%BB%A4.assets/image-20240621221944987.png)

#### ⑤mv命令（移动文件/文件夹的命令）

![image-20240621222128658](%E7%AC%AC2%E7%AB%A0Linux%E5%9F%BA%E7%A1%80%E5%91%BD%E4%BB%A4.assets/image-20240621222128658.png)

#### ⑥rm命令（删除文件/文件夹的命令）

![image-20240621222316181](%E7%AC%AC2%E7%AB%A0Linux%E5%9F%BA%E7%A1%80%E5%91%BD%E4%BB%A4.assets/image-20240621222316181.png)

#### notes：rm命令的`-r -f`选项

![image-20240621222414064](%E7%AC%AC2%E7%AB%A0Linux%E5%9F%BA%E7%A1%80%E5%91%BD%E4%BB%A4.assets/image-20240621222414064.png)

#### notes：rm命令的通配符（用于模糊匹配）

![image-20240621222514191](%E7%AC%AC2%E7%AB%A0Linux%E5%9F%BA%E7%A1%80%E5%91%BD%E4%BB%A4.assets/image-20240621222514191.png)



### （6）查找命令

#### ①which命令（查找linux命令可执行文件位置的命令）

![image-20240621222716239](%E7%AC%AC2%E7%AB%A0Linux%E5%9F%BA%E7%A1%80%E5%91%BD%E4%BB%A4.assets/image-20240621222716239.png)

#### ②find命令（查找文件的命令）

##### 2.1`find -name`按文件名查找

![image-20240621222832688](%E7%AC%AC2%E7%AB%A0Linux%E5%9F%BA%E7%A1%80%E5%91%BD%E4%BB%A4.assets/image-20240621222832688.png)

#### notes：find命令的通配符（用于模糊查询）

![image-20240621222916733](%E7%AC%AC2%E7%AB%A0Linux%E5%9F%BA%E7%A1%80%E5%91%BD%E4%BB%A4.assets/image-20240621222916733.png)

##### 2.2`find -size`按文件大小查找

![image-20240621223014356](%E7%AC%AC2%E7%AB%A0Linux%E5%9F%BA%E7%A1%80%E5%91%BD%E4%BB%A4.assets/image-20240621223014356.png)

### （7）grep命令（从文件中通过关键字过滤文件行）

![image-20240621223451133](%E7%AC%AC2%E7%AB%A0Linux%E5%9F%BA%E7%A1%80%E5%91%BD%E4%BB%A4.assets/image-20240621223451133.png)

#### notes：grep命令的`-n`选项

![image-20240621223526992](%E7%AC%AC2%E7%AB%A0Linux%E5%9F%BA%E7%A1%80%E5%91%BD%E4%BB%A4.assets/image-20240621223526992-1718980528005-1.png)

### （8）wc命令（统计文件的行数、单词数量、字节数、字符数等参数）

![image-20240621223608249](%E7%AC%AC2%E7%AB%A0Linux%E5%9F%BA%E7%A1%80%E5%91%BD%E4%BB%A4.assets/image-20240621223608249.png)

#### notes：wc命令的`-c -m -l -w`选项

![image-20240621223701257](%E7%AC%AC2%E7%AB%A0Linux%E5%9F%BA%E7%A1%80%E5%91%BD%E4%BB%A4.assets/image-20240621223701257.png)

### （9）管道符（将管道符左边命令的结果，作为右边命令的输入）

![image-20240621223843614](%E7%AC%AC2%E7%AB%A0Linux%E5%9F%BA%E7%A1%80%E5%91%BD%E4%BB%A4.assets/image-20240621223843614.png)

![image-20240621224108250](%E7%AC%AC2%E7%AB%A0Linux%E5%9F%BA%E7%A1%80%E5%91%BD%E4%BB%A4.assets/image-20240621224108250.png)



### （10）echo命令（打印命令）

![image-20240704221645086](%E7%AC%AC2%E7%AB%A0Linux%E5%9F%BA%E7%A1%80%E5%91%BD%E4%BB%A4.assets/image-20240704221645086.png)

==echo命令的作用类似于print命令==



#### notes：有关反引号`的作用（就是tab健上面的那个健）

![image-20240704221730797](%E7%AC%AC2%E7%AB%A0Linux%E5%9F%BA%E7%A1%80%E5%91%BD%E4%BB%A4.assets/image-20240704221730797.png)





### （11）tail命令（查看文件尾部内容的命令）

![image-20240704222056587](%E7%AC%AC2%E7%AB%A0Linux%E5%9F%BA%E7%A1%80%E5%91%BD%E4%BB%A4.assets/image-20240704222056587.png)

#### notes：tail命令的`-f -num`选项

![image-20240704222530445](%E7%AC%AC2%E7%AB%A0Linux%E5%9F%BA%E7%A1%80%E5%91%BD%E4%BB%A4.assets/image-20240704222530445.png)

![image-20240704222600049](%E7%AC%AC2%E7%AB%A0Linux%E5%9F%BA%E7%A1%80%E5%91%BD%E4%BB%A4.assets/image-20240704222600049.png)







### （12）重定向符（将重定向符左侧命令的结果写入到右侧文件中）

![image-20240704222010603](%E7%AC%AC2%E7%AB%A0Linux%E5%9F%BA%E7%A1%80%E5%91%BD%E4%BB%A4.assets/image-20240704222010603.png)





### （13）vim命令（用来编辑文本的命令）

![image-20240704223302177](%E7%AC%AC2%E7%AB%A0Linux%E5%9F%BA%E7%A1%80%E5%91%BD%E4%BB%A4.assets/image-20240704223302177.png)

![image-20240704223148570](%E7%AC%AC2%E7%AB%A0Linux%E5%9F%BA%E7%A1%80%E5%91%BD%E4%BB%A4.assets/image-20240704223148570.png)

#### notes：vim编辑器三种模式之间的转换：

==首先通过`vim 文件路径名`会进入命令模式==

命令模式下按住`i 键`进入输入模式

输入模式下按住`esc 键`进入命令模式

命令模式下按住`: 键`进入底线命令模式

![image-20240704223756486](%E7%AC%AC2%E7%AB%A0Linux%E5%9F%BA%E7%A1%80%E5%91%BD%E4%BB%A4.assets/image-20240704223756486.png)



#### notes：命令模式下查找关键字的功能

在命令模式下输入`/ 键`就进入了查找关键字的功能，接着输入关键字就可以在当前编辑的文件中搜索关键字，然后按n可以在当前文件中搜索下一个关键字、按N可以在当前文件中搜索上一个关键字



