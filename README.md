<a href="/01_git/01_git.pdf">01_git</a><br/>

```
git add .
git commit -m 'all'
git push origin master
```

<a href="/01_git/02_gitbook.pdf">02_gitbook</a><br/>
<a href="/01_git/03_markdown.pdf">03_markdown</a><br/>[04_gitbook常用插件](https://juejin.cn/post/6844903865146441741)<br/>

* 插件book.json

```
{
    "plugins": [
        "back-to-top-button",
        "chapter-fold",
        "code",
        "splitter",
        "-lunr",
        "-search",
        "search-pro",
        "insert-logo",
        "custom-favicon",
        "pageview-count",
        "tbfed-pagefooter",
        "popup",
        "-sharing",
        "sharing-plus"
    ],
    "pluginsConfig": {
        "insert-logo": {
            "url": "https://file.smallzhiyun.com/%E4%B9%A6.png",
            "style": "background: none; max-height: 30px; min-height: 30px"
        },
        "favicon": "./icon/book.ico",
        "tbfed-pagefooter": {
            "copyright": "Copyright &copy dsx2016.com 2019",
            "modify_label": "该文章修订时间：",
            "modify_format": "YYYY-MM-DD HH:mm:ss"
        },
        "sharing": {
            "douban": true,
            "facebook": true,
            "google": true,
            "pocket": true,
            "qq": true,
            "qzone": true,
            "twitter": true,
            "weibo": true,
            "all": [
                "douban",
                "facebook",
                "google",
                "instapaper",
                "linkedin",
                "twitter",
                "weibo",
                "messenger",
                "qq",
                "qzone",
                "viber",
                "whatsapp"
            ]
        }
    }
}
```



### gitbookUtil

```
import com.sun.scenario.effect.impl.sw.sse.SSEBlend_SRC_OUTPeer;

import java.io.File;
import java.io.IOException;

public class FileDemo {
    public static void main(String[] args) throws IOException {

        String dirPath = "C:\\Users\\shanpd\\Desktop\\book\\_book";

        //String dirPath = "C:\\Users\\shanpd\\Desktop\\book\\01_git";
        File dir = new File(dirPath);
        getFileList(dir);

    }

    //获取文件夹下面的所有文件
    public static void getFileList(File dir) throws IOException {

        File[] files = dir.listFiles();
        for (int i = 0;i < files.length; i++){
            if (files[i].isDirectory()){
                getFileList(files[i]);
            }
            if (files[i].getName().contains(".pdf")){
                String name = files[i].getAbsolutePath().split("\\\\_book")[1].replace("\\", "/");
                System.out.println("<a href=" + name +  ">"+ files[i].getName() + "</a><br/>");
            }
        }
    }
}

```

