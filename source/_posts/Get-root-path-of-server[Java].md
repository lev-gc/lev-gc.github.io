---
title: Java获取服务器根目录
date: 2015-05-10 10:28:00
tags: [Java]
---
获取路径为服务器webapps(项目发布的目录) 下的根路径，处理了Linux和Windows系统的斜杠、反斜杠问题，
该方法只适用于class文件是以源码部署的(即在WEB-INF/classes目录下)，如果打包成jar包后则不适用，
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
        String path = classPath.substring(1,
                classPath.indexOf("/WEB-INF/classes"));
        rootPath = path.substring(0, path.lastIndexOf("/"));
        rootPath = rootPath.replace("/", "\\");
    }

    /** For Linux */
    if ("/".equals(File.separator)) {
        String path = classPath.substring(0,
                classPath.indexOf("/WEB-INF/classes"));
        rootPath = path.substring(0, path.lastIndexOf("/"));
        rootPath = rootPath.replace("\\", "/");
    }
    return rootPath;
}
```