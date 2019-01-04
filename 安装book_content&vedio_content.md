[TOC]

# 问题:convert error

这个问题之前也有遇到过,但是不知道是哪个模块导致的,所以现在我一个一个模块的安装,看是哪个模块导致的.

> 1. ![](https://ws1.sinaimg.cn/large/891f7782gy1fyucgqjex0j20ar05h0so.jpg)
> 2. ![](https://ws1.sinaimg.cn/large/891f7782gy1fyucn6klccj209x059mx4.jpg)
> 3. ![](https://ws1.sinaimg.cn/large/891f7782gy1fyucpx5jluj20ny0chwg9.jpg)

## 出现错误:

![](https://ws1.sinaimg.cn/large/891f7782ly1fyue68jnikj20t504smyo.jpg)

> [修改网页这里](https://stackoverflow.com/questions/42928765/convertnot-authorized-aaaa-error-constitute-c-readimage-453)
>
> 1. comment line
>
>    ```xml
>    <!-- <policy domain="coder" rights="none" pattern="MVG" /> -->
>    ```
>
> 2. change line
>
>    ```xml
>    <policy domain="coder" rights="none" pattern="PDF" />
>    ```
>
>    to
>
>    ```xml
>    <policy domain="coder" rights="read|write" pattern="PDF" />
>    ```
>
> 3. add line
>
>    ```xml
>    <policy domain="coder" rights="read|write" pattern="LABEL" />
>    ```

然后:安装浏览pdf的插件

> Install the **Islandora PDF.js** module as usual, see [this](https://drupal.org/documentation/install/modules-themes/modules-7) for further information.
>
> You also need to [Download](http://mozilla.github.io/pdf.js/getting_started/#download) and **install the generic build of PDF.js** and move the directory to the sites/all/libraries/pdfjs directory, or run `drush pdfjs-plugin`.



## 现在安装book collection的集合,很大概率会导致之前的错误

![](https://ws1.sinaimg.cn/large/891f7782gy1fyufmwh7tjj20hw08f74l.jpg)

安装这个五个模块

**而islandora_openseadragon也不好装**

**装完了但是就是一个提示,忘记截图了,晕死**



其中large_iamge需要安装另外的图片处理服务:

### 安装[Djatoka](https://wiki.duraspace.org/display/ISLANDORA/Djatoka)

先不安装这个了,真的是不好安装的呀



## 安装vedio模块

### 安装ffmpeg

> ffmpeg's downloads page includes links to its source code for installation; however, in order for Islandora to support additional codecs, additional libraries will have to be installed and enabled during compilation. For additional information, check [ffmpeg's compilation guide](http://ffmpeg.org/trac/ffmpeg/wiki/CompilationGuide) on how to include additional codecs for your operating system and compile ffmpeg.

> 安装教程,可谓说麻烦至极
>
> [**网址在这里**](https://trac.ffmpeg.org/wiki/CompilationGuide/Ubuntu)

### 安装ffmpeg2Theora

>



## 设置虚拟机

