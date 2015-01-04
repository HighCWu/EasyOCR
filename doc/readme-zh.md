# EasyOCR 使用手册

---------------


EasyOCR 是一个使用Java语言实现的 OCR 识别组件，其工作基于 Tesseract-OCR 开源引擎。借助几个简单的API，即能使用Java语言调用 Tesseract-OCR 引擎完成图片内容识别工作。

**EasyOCR 最主要特点是能几种常见 CAPTCHA 验证码图片提供自动一体化的识别功能（自动化完成图片清理、识别 CAPTCHA 验证码图片内容的工作）。**

> CAPTCHA：**C**ompletely **A**utomated **P**ublic **T**uring Test to Tell **C**omputers and **H**umans **A**part

**主要特点：**

- 专门针对常用验证码图片的清理、识别一体化实现，支持对图片的清理、形变和旋转三种场景，内置多种常见类型的验证码图片清理选项
- API极简，一个方法，一行代码即可完成
- 由于基于Tesseract-OCR引擎，所以引擎支持的多种语言都可以识别。

**汉字识别说明：**

tesseract-ocr 是一个相对精准的开源免费 OCR 引擎。但 OCR 引擎对汉字识别相对较复杂，尤其是在文字较小、字体设计不清晰、段落文字、文字间距较小等场景下识别会不太理想（这是普遍问题，对比 ABBYY 等顶尖商业引擎，Tesseract OCR 也并不逊色），但如果能提前对待识别图片文字进行处理和优化（具体文字情况，需要具体的转换处理方法，如图片清洗、间距调整、宽高比例调整......等等），则可大幅提高识别率。


##  EasyOCR 使用步骤：

1. 必须首先在服务器下载并安装[Tesseract-OCR（项目主页）](https://code.google.com/p/tesseract-ocr/ "Tesserat-OCR Homepage")。在PATH环境变量中添加Tesseract-OCR的执行目录（可选，推荐设置）。

2. 加入`easyocr-1.0.0-RELEASE.jar`

3. 调用API



##  EasyOCR API：


EasyOCR 内置两套主要的API：

1. `ImageClean`：验证码图片清理类，完成各种验证码图片的清理工作并输出。支持图片清理（内置几种预定义的图片清理模式可以灵活切换选择）、形变和旋转三种场景，并支持场景的同时应用，来提高文字识别率。

2. `EasyOCR`：识别图片文字的OCR核心类，完成对OCR引擎的调用。内部可借助`ImageClean`完成自动清理，识别的一体化工作。



##  EasyOCR 使用实例：
![demo_eurotext.png](images/demo_eurotext.png)  
![img_NORMAL.jpg](images/img_NORMAL.jpg) 

```JAVA
EasyOCR e=new EasyOCR();
//直接识别图片内容
System.out.println(e.discern("images/demo_eurotext.png")); 
//验证码图片，经过：普通清理、形变场景自动一体化处理后，识别内容
System.out.println(e.discernAndAutoCleanImage("images/img_NORMAL.jpg", ImageType.CAPTCHA_NORMAL, 1.6, 0.7));
		
```




## 结束



如果您有更好意见，建议或想法，请联系我。


联系、反馈、定制、培训 Email：<inthinkcolor@gmail.com>

[http://www.easyproject.cn](http://www.easyproject.cn "EasyProject Home")