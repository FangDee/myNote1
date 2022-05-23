# web知识点整理

## 1.文件上传和下载

### 1.1 文件上传

~~~java
使用前提
** 要有一个form标签 method=post请求
** form标签的编码的enctype属性值必须为：multipart/form-data
   它表示提交的数据，以多段（每一个表单项一个数据段）的形式进项拼接，然后以二进制流的形式发送给服务器！
** form标签中使用<input typut="file">添加上传文件
** 编写服务器代码接收，处理上传的数据。
~~~

~~~xml
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
     <form action="" method="post" enctype="multipart/form-data" >
         用户名：<input type="text" name="username"><br> --一个表单项
         头像：<input type="file" value="photo"><br> --一个表单项
         <input type="submit" value="上传">
     </form>
</body>
</html>
~~~

![v](E:\截图图片\A3\Snipaste_2020-06-17_23-26-15.png)

服务器代码

~~~java
由于是以流的形式发送到服务器，故需要以流的形式来接收数据
public class UpLoadServlet extends HttpServlet {
    /**
     * 文件上传处理逻辑
     * @param request
     * @param response
     * @throws javax.servlet.ServletException
     * @throws IOException
     */
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws javax.servlet.ServletException, IOException {
        System.out.println("文件上传过来了！");
        ServletInputStream inputStream = request.getInputStream();
        byte[] buffer = new byte[102400];
        int number = inputStream.read(buffer);
        System.out.println(new String(buffer,0,number));
    }
}
~~~

**解析文件**

~~~java
解析上传的文件实际上已经有很多的API啦,用于对收到的数据进行解析
如：commons-fileupload.jar
~~~

![](E:\截图图片\A3\Snipaste_2020-06-18_00-23-15.png)

~~~java
解析步骤：
    1.导包：两个包都要导入
    2.ServletFileUpload类：用于解析上传的文件
      boolean ServletFileUpload.isMultipartContent(HttpServletRequest request):判断当前上传的数据格式是否是多段的格式，如果不是，解析不了
      public List<FileItem> parseRequest(HttpServletRequest request):解析上传的数据
    3.FileItem类：表示每一个表单项
      boolean FileItem.isFormField() ：判断当前这个表单项，是否是普通的表单项。还是上传的文件类型。 true 表示普通类型的表单项 false 表示上传的文件类型 
      String FileItem.getFieldName()： 获取表单项的 name 属性值
      String FileItem.getString()： 获取当前表单项的值。 
      String FileItem.getName()： 获取上传的文件名 
      void FileItem.write( file )： 将上传的文件写到 参数 file 所指向抽硬盘位置 。
~~~

~~~java
package com.atguigu.com;

import org.apache.commons.fileupload.FileItem;
import org.apache.commons.fileupload.FileItemFactory;
import org.apache.commons.fileupload.disk.DiskFileItemFactory;
import org.apache.commons.fileupload.servlet.ServletFileUpload;

import java.io.File;
import java.io.IOException;
import java.util.List;
import javax.servlet.ServletException;
import javax.servlet.ServletInputStream;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

public class UpLoadServlet extends HttpServlet {
    /**
     * 文件上传处理逻辑
     * @param req
     * @param response
     * @throws javax.servlet.ServletException
     * @throws IOException
     */
    protected void doPost(HttpServletRequest req, HttpServletResponse response) throws javax.servlet.ServletException, IOException {
        System.out.println("文件上传过来了！");
        //判断上传的数据是否是多端的数据，只有是多端的数据才可以解析，才是文件上传的
        if(ServletFileUpload.isMultipartContent(req)){
            ServletFileUpload sf = new ServletFileUpload();
            // 创建 FileItemFactory 工厂实现类
            FileItemFactory fileItemFactory = new DiskFileItemFactory();
            // 创建用于解析上传数据的工具类 ServletFileUpload 类
            ServletFileUpload servletFileUpload = new ServletFileUpload(fileItemFactory);
            try {
                // 解析上传的数据，得到每一个表单项 FileItem
                 List<FileItem> list = servletFileUpload.parseRequest(req);
                // 循环判断，每一个表单项，是普通类型，还是上传的文件
                 for (FileItem fileItem : list) {
                    if (fileItem.isFormField()) {
                    // 普通表单项
                        System.out.println("表单项的 name 属性值：" + fileItem.getFieldName());
                    // 参数 UTF-8.解决乱码问题
                        System.out.println("表单项的 value 属性值：" + fileItem.getString("UTF-8"));
                    } else {
                        // 上传的文件
                        System.out.println("表单项的 name 属性值：" + fileItem.getFieldName());
                        System.out.println("上传的文件名：" + fileItem.getName());
                        fileItem.write(new File("e:\\" + fileItem.getName()));
                    }
                }
            } catch (Exception e) {
                e.printStackTrace();
            }
                    }

        }

    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        super.doGet(req, resp);
    }
}

~~~

## 2.js的一些方法tips

### 2.1 Math.pow(x,y)方法

~~~java
js的Math.pow(x,y)方法：
    pow()方法可返回 x 的 y 次幂的值。
    x：必需，底数，必须是数字。
    y：必需。幂数，必须是数字。
说明：
   如果结果是虚数或负数，则该方法将返回 NaN。如果由于指数过大而引起浮点溢出，则该方法将返回 Infinity
~~~





