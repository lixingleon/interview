add open to work in the profile pic

get recommendation in linkedin 

add resume to linkedin

test scores in linkedin (GRE, IELTS)   ✔

add 职业头衔 in linkedin ✔

![image-20211005145409182](C:\Users\Admin\AppData\Roaming\Typora\typora-user-images\image-20211005145409182.png)



resume:

- heading

add linkedin link 

- Education

add coursework 

put GPA at the right side. remove locations.

use abbrv. Month

- Technical Skills

add certificates.

use tools/IDE instead of Other

- work experience

add context( for example: what does the product do, what is your responsibility)



- projects

what problems you want to address

focus on features and outcomes instead of tech stacks



add leadership&Involvement  section in resume





# 在Springboot用apache commons包的ServerFileUpload.parseRequest接收文件上传踩坑经历

1. 发现bug

   1. 前端正常收到200 response，但Filename 是null。说明后端虽然没有报错，但也没有接收到文件。

2. 查bug

   1. 确定前端包含正确数据
      1. 用 request.getParameterMap() 解析打印出request中的所有参数，发现文件信息都在里面。说明前端没有问题。
   2. 定位后端问题位置
      1. 在方法中用print statement的方式，判断哪里出了问题。最终问题定位到ServerFileUpload.parseRequest 得到的返回值为空。
      2. 在StackOverflow上找到问题答案。需要禁用spring框架自带的MultipartResolver。因为request只能被接收一次，该resolver先把request 接收了，我自己定义的ServerFileUpload.parseRequest就不能再接收request了。所以方法的返回值就是空。

3. 解决方案

   1. 在spring的配置文件：application.properties中禁用multipart：

      ```java
      spring.servlet.multipart.enabled=false
          
      ```

      https://stackoverflow.com/questions/32782026/springboot-large-streaming-file-upload-using-apache-commons-fileupload

4. 原理分析

   1. MultipartResolver其实是spring框架的一个接口，它可以帮助我们上传文件。它有两个实现，分别是Apache Commons FileUpload的CommonsMultipartResolver和Servlet 3.0 的StandardServletMultipartResolver

      ![image-20211114191521359](C:\Users\Admin\AppData\Roaming\Typora\typora-user-images\image-20211114191521359.png)

      链接：https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/multipart/MultipartResolver.html

   2. 虽然官网说没有default resolver implementation used，但从我这个bug来看spring默认使用的就是第二种resolver，这篇博客也有相同观点https://www.codenong.com/cs109624474/。

5. 挖坑

   1. 下次用spring自带的servlet3.0实现一下文件上传，比较二者优劣。