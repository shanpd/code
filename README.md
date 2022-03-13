<a href="/01_git/01_git.pdf">01_git</a><br/>
<a href="/01_git/02_gitbook.pdf">02_gitbook</a><br/>
<a href="/01_git/03_markdown.pdf">03_markdown</a><br/>

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

