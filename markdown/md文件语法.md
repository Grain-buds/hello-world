#标题  
一级标题 #   
二级标题 ##  
三级标题 ###   
四级标题 ####  
五级标题 #####  
六级标题 ###### 

#无序列表
无序列表用 - + * 任何一种都可以

#有序列表
数字加点



#分割线
三个或者三个以上的 - 或者 * 都可以


#表格  
表头|表头|表头  
---|:--:|---:  
内容|内容|内容  
内容|内容|内容  

第二行分割表头和内容。  
有一个 - 就行，为了对齐，多加了几个文字默认居左  
两边加：表示文字居中  
右边加：表示文字居右  
注：原生的语法两边都要用 | 包起来。此处省略

如：  
表头|表头2|4     
--|:--:|--:  
流|打包|的撒

#图片
如果图片与.md文件在同一目录下，那么相对路径这样表示

--- '![avatar](buildWebsites.jpg)'
其中avatar表示图片未正常加载时所显示的内容，buildWebsites.jpg为文件名  
https://blog.csdn.net/weixin_42474261/article/details/81540053?utm_medium=distribute.pc_aggpage_search_result.none-task-blog-2~aggregatepage~first_rank_ecpm_v1~rank_v31_ecpm-3-81540053-null-null.pc_agg_new_rank&utm_term=md%E4%BD%BF%E7%94%A8%E5%9B%BE%E7%89%87%E7%9B%B8%E5%AF%B9%E8%B7%AF%E5%BE%84&spm=1000.2123.3001.4430  

或者![alt](xxx.JPG)

#代码片段
码片段的前后用“```”标记。``` 不是三个单引号，而是数字1左边，Tab键上面的键。

要实现语法高亮那么只要在 ``` 之后加上你的编程语言即可（忽略大小写


# 字体
加粗： 要加粗的文字左右分别用两个*号包起来

# 脚注
脚注（footnote）

实现方式如下：
hello[^hello] 

测试

[^hello]:hi


#参考
https://www.cnblogs.com/mrwhite2020/p/12906440.html