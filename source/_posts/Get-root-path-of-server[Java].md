---
title: Java获取服务器根目录
date: 2015-05-10 10:28:00
tags: [Java]
---
获取服务器Tomcat目录下webapps(项目发布的位置)的根路径，对Linux和Windows系统下分别使用斜杠和反斜杠的问题进行了处理。

具体实现如下：
```java
/**
 * Gets the root path of server.
 *
 * @return the root path
 */
public static String getRootPath() {
    String classPath = Thread.currentThread().getContextClassLoader()
      .getResource("").getPath();
    String rootPath = "";

    /** For Windows */
    if ("\\".equals(File.separator)) {
        String path = classPath.substring(1, classPath.indexOf("/WEB-INF/classes"));
        rootPath = path.substring(0, path.lastIndexOf("/"));
        rootPath = rootPath.replace("/", "\\");
    }

    /** For Linux */
    if ("/".equals(File.separator)) {
        String path = classPath.substring(0, classPath.indexOf("/WEB-INF/classes"));
        rootPath = path.substring(0, path.lastIndexOf("/"));
        rootPath = rootPath.replace("\\", "/");
    }
    return rootPath;
}
```