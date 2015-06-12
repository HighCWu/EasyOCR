# EasyOCR Manual

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

2. Add `easyocr-2.2.0-RELEASE.jar`

3. Call API



##  EasyOCR API：


Built EasyOCR two main API:

1. `ImageClean`: CAPTCHA cleanup class, complete a variety of CAPTCHA cleanup and output. Support for image cleanup (built several predefined picture cleanup mode selection switch can be flexible), deformation and rotation of three scenarios, and the scene at the same time support the application, to improve text recognition rate.

2. `EasyOCR`: OCR text recognition Pictures core classes, complete the call to OCR engine. Internally With `ImageClean` complete automatic cleaning, identify integration work.

 ### [API](API-en.md 'English API')

##  EasyOCR Examples：
![demo_eurotext.png](images/demo_eurotext.png)  

![img_INTERFERENCE_LINE.png](images/img_INTERFERENCE_LINE.png)  

![img_NORMAL.jpg](images/img_NORMAL.jpg) 

```JAVA
EasyOCR e=new EasyOCR();
//Direct identification picture content
System.out.println(e.discern("images/demo_eurotext.png")); 
//Direct identification CAPTCHA picture content
System.out.println(e.discernAndAutoCleanImage("images/img_INTERFERENCE_LINE.png",ImageType.CAPTCHA_INTERFERENCE_LINE)); 
//CAPTCHA, through: general cleaning, automatic integration scenarios deformation processing, identifying content
System.out.println(e.discernAndAutoCleanImage("images/img_NORMAL.jpg", ImageType.CAPTCHA_NORMAL, 1.6, 0.7));
		
```

Tip: For verification code image suitable deformation helps to improve the recognition rate. Under special circumstances need to adjust the ratio can be observed by multiple analysis to get the right ratio.
```JAVA
for(double imageWidthRatio=0.8;imageWidthRatio<=2;imageWidthRatio+=0.1){
	for (double imageHeightRatio = 0.8;imageHeightRatio<=2.8;imageHeightRatio+=0.1) {
		System.out.println(e.discernAndAutoCleanImage("images/d.jpg",ImageType.CAPTCHA_NORMAL,imageWidthRatio,imageHeightRatio));
	}
}
```


##  EasyOCR Chinese Identification Example:
Tesseract default recognition language is English, tesseractOptions property can be modified by identifying the type of language.
```JAVA
EasyOCR e=new EasyOCR();
// Set recognize command-line arguments for the Chinese (the default is English)
e.setTesseractOptions(EasyOCR.OPTION_LANG_CHI_SIM);

System.out.println(e.discern("C:\\novel.png"));
```

## End

[Comments](http://www.easyproject.cn/easyocr/en/index.jsp#about 'Comments')

If you have more comments, suggestions or ideas, please contact me.

Email：<inthinkcolor@gmail.com>

<p>
<form action="https://www.paypal.com/cgi-bin/webscr" method="post" target="_blank">
<input type="hidden" name="cmd" value="_xclick">
<input type="hidden" name="business" value="inthinkcolor@gmail.com">
<input type="hidden" name="item_name" value="EasyProject development Donation">
<input type="hidden" name="no_note" value="1">
<input type="hidden" name="tax" value="0">
<input type="image" src="http://www.easyproject.cn/images/paypaldonation5.jpg"  title="PayPal donation"  border="0" name="submit" alt="Make payments with PayPal - it's fast, free and secure!">
</form>
</P>


[http://www.easyproject.cn](http://www.easyproject.cn "EasyProject Home")