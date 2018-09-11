---
title: Word通过模版生成文档
date: 2018-04-04 21:54:00
tags: [Java,Office]
---
> 目的是通过使用预先设置好的模版文档，通过使用调度任务产生的数据（包括文本、图片、列表、报表等），回填生成完整的Word文档，通常可用于需要定时生成文档的场景。

### 基本思路：

使用开源项目Word模版生成文档项目[poi-tl](https://github.com/Sayi/poi-tl)，把后台组织好的数据，包括文本、列表、表格和图片，填到相应的模版里，生成一份完整的文档。

### 使用说明：

`poi-tl`是一个jar包，使用时只需在项目pom文件内引入依赖即可：

```xml
<dependency>
    <groupId>com.deepoove</groupId>
    <artifactId>poi-tl</artifactId>
    <version>1.2.0</version>
</dependency>
```

该组件对模版文档内的语法结构要求都是以 {{ 开始，以 }} 结束，具体如下：

```
# 普通文本：{{template}}，渲染类为String或者TextRenderData
# 图片：{{@template}}，渲染类为PictureRenderData
# 表格：{{#template}}，渲染类为TableRenderData
# 列表：{{*template}}，渲染类为NumbericRenderData
```

另外，关于文档样式的问题，默认会使用模版占位符的样式，如果需要动态变化样式，可以在构造渲染类的时候注入样式。

### Demo：

具体使用代码如下，其中Map里的所有Key都是模版文档里的占位符：

```java
public static void main(String[] args) throws IOException {
    Map<String, Object> renderData = new HashMap<String, Object>() {{
        // 文本
        put("date", "2018-03-19"); // 直接用String写入文本
        put("title", new TextRenderData("Here is the Title.")); // 通过渲染类写入文本
        put("author", new TextRenderData("66ccFF", "Elvis")); // 添加样式，另外还可以修改渲染类的Style属性配置完整的样式

        // 通过文件系统路径方式加载图片
        put("image1", new PictureRenderData(700, 300, "/image1.png"));
        // 通过Http方式加载图片
        String url = "xxx"; // 请求的URL
        byte[] imageByHttp = getImageByHttp(url, "[{...}]"); // param参数为JSON字符串，url参数根据实际需要修改(此处使用的是post方法的请求)
        put("image2", new PictureRenderData(700, 300, "image.png", imageByHttp)); // 由于path参数会有验证不能为空且必须为合法格式，但其实通过字节数组的方式不需要读文件，所以填定义一个合法文件名即可

        // 表格
        put("Table", new TableRenderData(new ArrayList<RenderData>() {{
            add(new TextRenderData("d0d0d0", "column1"));
            add(new TextRenderData("111111", "column2"));
            add(new TextRenderData("d0d0d0", "column3"));
        }}, new ArrayList<Object>() {{
            add("row1;r1c2;");
            add("row2;;r2c3");
            add("row3;r3c2;r3c3");
        }}, "no datas", 10600));

        // 列表
        put("List", new NumbericRenderData(FMT_DECIMAL, new ArrayList<TextRenderData>() {{
            add(new TextRenderData("Deeply in love with the things you love, just deepoove."));
            add(new TextRenderData("df2d4f", "Deeply in love with the things you love, just deepoove."));
        }}));
    }};

    // 读取模版并生成文档
    XWPFTemplate template = XWPFTemplate.compile("/xx/template.docx").render(renderData);

    // 输出到文件系统
    FileOutputStream out = new FileOutputStream("/xx/output.docx");
    template.write(out);
    template.close();
    out.close();
}
```

上面有使用Http请求的方法来获取图片，其实`poi-tl`包里提供了Http请求的`Get`的工具方法，但上面我们给出了`Post`方法的使用方法，下面是`Post`请求的工具方法：

```java
public static byte[] getImageByHttp(String urlStr, String param) throws IOException {
    try {
        URL url = new URL(urlStr);
        URLConnection conn = url.openConnection();
        conn.setRequestProperty("Content-Type", "application/png;charset=UTF-8");
        conn.setDoOutput(true);
        conn.setDoInput(true);
        PrintWriter out = new PrintWriter(new OutputStreamWriter(conn.getOutputStream(), "utf-8"));
        out.print(param);
        out.flush();

        int len;
        byte[] data = new byte[1024];
        InputStream inputStream = conn.getInputStream();
        ByteArrayOutputStream outputStream = new ByteArrayOutputStream();
        while ((len = inputStream.read(data)) != -1) {
            outputStream.write(data, 0, len);
        }
        byte[] result = outputStream.toByteArray();

        out.close();
        inputStream.close();
        outputStream.close();
        return result;
    } catch (Exception e) {
        e.printStackTrace();
        throw e;
    }
}
```

### 题外话：

使用`poi-tl`基本上只能生成一些结构比较基本的文档，当需要相对复杂的图或者表的时候是处理不来的，这种情况下的一种解决方法是把需要的图或者表转成图片然后插入到文档中。至于如何生成图和表，可以在`PhantomJS`的环境下利用诸如`Echars`、`HighCharts`这些图表插件生成。