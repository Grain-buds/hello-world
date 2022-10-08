Maven 记录控制台输出到文件:  
mvn -X install  > test.log

Maven如何修改本地仓库与中央仓库：
https://blog.csdn.net/tuoni123/article/details/79613043

Maven3种打包方式之一maven-assembly-plugin的使用  
https://blog.csdn.net/qq_32736999/article/details/93395246


maven打包以及module相互依赖问题
https://blog.csdn.net/m0_48983233/article/details/124528417

聚合工程只是用来帮助其他模块构建的工具，本身并没有实质的内容。具体每个工程代码的编写还是在生成的工程中去写。对于在父工程中导的依赖工程也可享有 ，子module可继承父工程依赖。

jar：工程的默认打包方式，打包成jar用作jar包使用。存放一些其他工程都会使用的类，工具类。我们可以在其他工程的pom文件中去引用它。

war：将会打包成war，发布在服务器上，如网站或服务。用户可以通过浏览器直接访问，或者是通过发布服务被别的工程调用。

https://blog.csdn.net/weixin_42541479/article/details/122573086

如果父项目pom中使用的是dependencies,则子项目pom会自动使用pom中的jar包

如果父项目pom使用的是dependencyManagement则子项目pom不会自动使用父pom中的jar包，如果需要使用，就要给出groupId和artifactId，无需给出version


https://blog.csdn.net/qq_39750658/article/details/109764668