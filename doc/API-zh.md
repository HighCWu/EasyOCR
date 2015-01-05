# EasyOCR API

---------------


EasyOCR 内置两套主要的API：

1. `ImageClean`：验证码图片清理类，完成各种验证码图片的清理工作并输出。支持图片清理（内置几种预定义的图片清理模式可以灵活切换选择）、形变和旋转三种场景，并支持场景的同时应用。

2. `EasyOCR`：识别图片文字的OCR核心类，完成对OCR引擎的调用。内部可借助`ImageClean`完成自动清理，识别的一体化工作。


## 1. ImageClean:

ImageClean主要是为了完成验证码的清理工作，支持图片清理、形变和旋转三种场景。图片清理选项内置多种清理方法，通过ImageType枚举可选择预定义的图片清理模式。

- ### ImageType 图片类型枚举（预定义的验证码图片清理模式）：

 - `CAPTCHA_NORMAL` ：普通验证码图片
   ![普通验证码](images/img_NORMAL.jpg)   ![普通验证码](images/img_NORMAL2.png)

 - `CAPTCHA_SPOT` ： 点状验证码图片
   ![点状验证码](images/img_SPOT.gif)  ![点状验证码](images/img_SPOT2.gif)  

 - `CAPTCHA_WHITE_CHAR` ： 白色文字，纯色背景验证码图片
   ![白色文字验证码](images/img_WHITE_CHAR.jpg)  ![白色文字验证码](images/img_WHITE_CHAR2.jpg) 
 
 - `CAPTCHA_HOLLOW_CHAR` ： 空心文字验证码图片
   ![空心文字验证码](images/img_HOLLOW.jpg)  ![空心文字验证码](images/img_HOLLOW2.jpg) 
 
 - `CLEAR` ： 无特殊干扰的普通图片清晰化，提高识别率
   ![普通图片清晰化](images/img_CLEAR.jpg)
  
 - `NONE` ： 在识别时，不对图片做任何清理

*由于验证码图片种类繁多，而且具体场景要具体处理，并无完全通用的方案。EasyOCR内置的清理类型仅针对有限情况，面对具体场景还需通过有针对性处理，如通过其他方式对图片进行预处理，或对ORC引擎进行特别训练实现。*

1. **构造方法：**

    ```JAVA
    /**
     * 构造图片清理对象
     * 
     * @param imageType  验证码图片类型，默认为 ImageType.CAPTCHA_NORMAL
     * @param imageWidthRatio 图片宽度形变比例，默认为 1
     * @param imageHeightRatio 图片宽度形变比例，默认为 1
     * @param degrees 图片顺时针旋转角度，默认为 0
     */
     ImageClean([ImageType imageType] [, double imageWidthRatio] [, double  imageHeightRatio] [, int degrees]);
    ```
2. **核心方法：**
 
 - 图片清理
 ```JAVA
   /**
	 * 图片清理
	 * 
	 * @param from  需要清理的图片，支持String字符串路径、File文件对象、FileInputStream输入流等多种重载
	 * @param to 清理后的图片，支持String字符串路径、File文件对象等多种重载。
	 * @param imageWidthRatio 直接指定图片宽度度放大比例，某些图片形变后识别率可以提高，默认为 1
	 * @param imageHeightRatio 直接指定图片高度放大比例，某些图片形变后识别率可以提高，默认为 1
	 * @param degrees 直接指定图片顺时针旋转角度，默认为 0
	 * @return 处理结果
	 */
  public boolean cleanImage(from, to [, imageWidthRatio] [, imageHeightRatio] [, degrees]);
  ```

2. **其他方法：**

 以下方法可以直接设置ImageClean实例的默认属性值，在进行清理操作时，无需每次直接传入参数。

 - 默认图片清理类型
 ```JAVA
  void setImageType(ImageType imageType)
 ```

 - 图片宽度和高度形变比例，某些图片形变后识别率可以提高，1为原始比例，不形变
 ```JAVA
  void setImageRatio(double imageWidthRatio, double imageHeightRatio)
 ```

 - 图片宽度形变比例，1为原始比例
 ```JAVA
  void setImageWidthRatio(double imageWidthRatio)
 ```

 - 图片高度形变比例，1为原始比例
 ```JAVA
  void setImageHeightRatio(double imageHeightRatio)
 ```

 - 图片顺时针旋转角度，0为默认值，不旋转
 ```JAVA
  void setDegrees(int degrees)
 ```

## 2. EasyOCR:

识别图片文字的OCR核心类，完成对OCR引擎的调用。内部可借助`ImageClean`完成自动清理、形变、旋转和识别的一体化工作。


1. **构造方法：**

    ```JAVA
    /**
     * 构造图片清理对象
     * 
     * @param tesseractPath  tesseract执行命令的位置，如果配置了path环境变量，则可以不用设置
     * @param tesseractOptions tesseract命令执行参数
     */
     EasyOCR([String tesseractPath] [, String tesseractOptions]);
    ```

2. **tesseractOptions 中英文语言可选常量：**
   
 Tesseract-OCR 默认可直接识别英文。tesseractOptions属性可以指定tesseract命令行参数，用来控制识别的语言（需要提前安装好语言包），细节参数等。
 EasyOCR内置了中文和英文基本参数常量可供选择。

 ```JAVA
    /**
	 * 英文识别命令执行参数
	 */
	public static final String OPTION_LANG_ENG = "-l eng";
	/**
	 * 中文识别命令执行参数
	 */
	public static final String OPTION_LANG_CHI_SIM = "-l chi_sim";
```
3. **核心方法：**

 - 直接识别图片
  ```JAVA
	/**
	 * 识别图片内容，并返回读取到的内容字符串
	 * 
	 * @param fromImage 要识别的图片
	 * @return 读取到的代码
	 */
	public String discern(String fromImage)

	/**
	 * 将识别的图片上的内容输出到指定文件。不指定toText参数时，默认输出文件名为：图片名.txt
	 * 
	 * @param fromImage 要识别的图片，
	 * @param toText 保存识别内容的文件名，默认会自动添加.txt后缀       
	 * @return 生成是否完成
	 */
	public boolean discernToFile(String fromImage [, String toText]) 

    /**
	 * 将识别的图片上的内容输出到指定文件，并返回读取到的内容字符串。不指定toText参数时，默认输出文件名为：图片名.txt
	 * 
	 * @param fromImage 要识别的图片
	 * @param toText  同时保存进的的文件，默认会自动添加.txt后缀    
	 * @return 读取到的代码
	 */
	public String discernToFileAndGet(String fromImage [, String toText]) 
```

 - 清理并识别图片
  ```JAVA
	/**
	 * 按图片类型、形变比例、顺时针旋转角度等场景，清理识别图片内容，并返回读取到的内容字符串
	 * 
	 * @param fromImage 原始图片
	 * @param imageType 图片类型枚举，默认为ImageType.NONE
	 * @param autoCleanImageWidthRatio 直接指定图片宽度度放大比例，某些图片形变后识别率可以提高
	 * @param autoCleanImageHeightRatio 直接指定图片高度放大比例，某些图片形变后识别率可以提高
	 * @param autoCleanDegrees 图片自动清理时的顺时针旋转角度               
	 * @return 识别内容
	 */
	public String discernAndAutoCleanImage(String fromImage [, ImageType imageType] [, double autoCleanImageWidthRatio] [, double  autoCleanImageHeightRatio] [, int autoCleanDegrees]) 

	/**
	 * 按图片类型、形变比例、顺时针旋转角度等场景，清理识别图片内容，输出到指定文件，默认文件名为：图片名.txt
	 * 
	 * @param fromImage 要识别的图片
	 * @param toFile 保存内容的文件，不指定toText参数时，默认输出文件名为：图片名.txt
	 * @param imageType  图片类型， ImageType.NONE
	 * @param autoCleanImageWidthRatio  直接指定图片宽度度放大比例，某些图片形变后识别率可以提高
	 * @param autoCleanImageHeightRatio 直接指定图片高度放大比例，某些图片形变后识别率可以提高
	 * @param autoCleanDegrees 图片自动清理时的顺时针旋转角度     
	 * @return 生成是否完成
	 */
	public boolean discernToFileAndAutoCleanImage(String fromImage [,String toFile] [, ImageType imageType] [, double autoCleanImageWidthRatio] [, double autoCleanImageHeightRatio] [,int autoCleanDegrees])

	/**
	 * 按图片类型、形变比例、顺时针旋转角度等场景，清理识别图片内容，输出到指定文件，并返回读取到的内容字符串，输出文件无需后缀名.txt
	 * 
	 * @param fromImage 要识别的图片
	 * @param toFile 同时保存内容的的文件，不指定toText参数时，默认输出文件名为：图片名.txt
	 * @param imageType 图片类型
	 * @param autoCleanImageWidthRatio 直接指定图片宽度度放大比例，某些图片形变后识别率可以提高
	 * @param autoCleanImageHeightRatio 直接指定图片高度放大比例，某些图片形变后识别率可以提高
	 * @param autoCleanDegrees 图片自动清理时的顺时针旋转角度     
	 * @return 读取到的代码
	 */
	public String discernToFileAndGetAndAutoCleanImage(String fromImage [, String toFile] [, ImageType imageType] [, double autoCleanImageWidthRatio] [, double autoCleanImageHeightRatio] [,int autoCleanDegrees])
```

4. **其他方法：**

以下方法可以直接设置EasyOCR实例的默认属性值，在进行识别操作时，调用清理并识别图片方法时，无需每次直接传入参数。

 - tesseract执行命令的位置，如果配置了path环境变量，则可以不用设置
 ```JAVA
 void setTesseractPath(String tesseractPath)
 ```

 - tesseract命令执行参数，用来调整运行参数。可从TesseractOCR.OPTION_LANG_...常量选择基本语言
 ```JAVA
 void setTesseractOptions(String tesseractOptions)
 ```

 - 设置图片清理类型
 ```JAVA
  void setAutoCleanImageType(ImageType autoCleanImageType) 
 ```

 - 图片宽度和高度形变比例，某些图片形变后识别率可以提高，1为原始比例，不形变
 ```JAVA
  void setAutoCleanImageRatio(double autoCleanImageWidthRatio, double autoCleanImageHeightRatio)
 ```

 - 图片宽度形变比例，1为原始比例
 ```JAVA
  void setAutoCleanImageWidthRatio(double autoCleanImageWidthRatio)
 ```
 - 图片宽度形变比例，1为原始比例
 ```JAVA
  void setAutoCleanImageHeightRatio(double autoCleanImageHeightRatio)
 ```

 - 图片顺时针旋转角度，0为默认值，不旋转
 ```JAVA
  void setDegrees(int autoCleanDegrees)
 ```


## 结束

[留言评论](http://www.easyproject.cn/easyocr/zh-cn/index.jsp#about '留言评论')

如果您有更好意见，建议或想法，请联系我。


联系、反馈、定制、培训 Email：<inthinkcolor@gmail.com>


[http://www.easyproject.cn](http://www.easyproject.cn "EasyProject Home")