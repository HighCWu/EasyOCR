# EasyOCR 使用手册

---------------


EasyOCR is implemented using the Java language OCR recognition component, its work is based on the open source Tesseract-OCR engine. With a few simple API, the Java language can be used to call Tesseract-OCR engine to complete the picture content identification work.

** EasyOCR main feature is to some common CAPTCHA CAPTCHA recognition feature provides automatic integration (automatic cleanup to complete the picture, the image content recognition CAPTCHA code work). **

> CAPTCHA：**C**ompletely **A**utomated **P**ublic **T**uring Test to Tell **C**omputers and **H**umans **A**part

** Features: **

- Specific code used to clean up the image, identify integration implementation, support for image cleanup, deformation and rotation three scenarios, built many common types of CAPTCHA cleanup options
- API minimalist, a method, a line of code to complete
- Because based Tesseract-OCR engine, so the engine supports multiple languages are recognized.

** Character Recognition Description: **

tesseract-ocr is a relatively accurate free open source OCR engine. Character Recognition OCR engine but relatively complex, especially in smaller text, font design is not clear, paragraph text, text spacing smaller under less than ideal, such as Scene Recognition will (this is a common problem, compared to other top business ABBYY engine, Tesseract OCR is not inferior), but if we can treat early recognition processing and optimization pictograph (specific text, it is necessary to convert the specific processing method such as picture cleaning, pitch adjustment, the aspect ratio adjustment ...... etc. etc.), can greatly improve the recognition rate.


##  EasyOCR Use these steps:

1. You must first download and install [Tesseract-OCR (project home)] (https://code.google.com/p/tesseract-ocr/ "Tesserat-OCR Homepage") in the server. Add Tesseract-OCR perform directory in the PATH environment variable (optional, but recommended setting).

2. Add `easyocr-1.0.0-RELEASE.jar`

3. Call API



##  EasyOCR API：


Built EasyOCR two main API:

1. `ImageClean`: CAPTCHA cleanup class, complete a variety of CAPTCHA cleanup and output. Support for image cleanup (built several predefined picture cleanup mode selection switch can be flexible), deformation and rotation of three scenarios, and the scene at the same time support the application, to improve text recognition rate.

2. `EasyOCR`: OCR text recognition Pictures core classes, complete the call to OCR engine. Internally With `ImageClean` complete automatic cleaning, identify integration work.



##  EasyOCR Examples：
![demo_eurotext.png](images/demo_eurotext.png)  
![img_NORMAL.jpg](images/img_NORMAL.jpg) 

```JAVA
EasyOCR e=new EasyOCR();
//Direct identification picture content
System.out.println(e.discern("images/demo_eurotext.png")); 
//CAPTCHA, through: general cleaning, automatic integration scenarios deformation processing, identifying content
System.out.println(e.discernAndAutoCleanImage("images/img_NORMAL.jpg", ImageType.CAPTCHA_NORMAL, 1.6, 0.7));
		
```




## End



If you have more comments, suggestions or ideas, please contact me.

Email：<inthinkcolor@gmail.com>

[http://www.easyproject.cn](http://www.easyproject.cn "EasyProject Home")