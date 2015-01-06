# EasyOCR API

---------------


Built EasyOCR two main API:

1. `ImageClean`: CAPTCHA cleanup class, complete a variety of CAPTCHA cleanup and output. Support for image cleanup (built several predefined picture cleanup mode selection switch can be flexible), deformation and rotation of three scenarios, and support the scene simultaneously.

2. `EasyOCR`: OCR text recognition Pictures core classes, complete the call to OCR engine. Internally With `ImageClean` complete automatic cleaning, identify integration work.


## 1. ImageClean:

ImageClean mainly in order to complete the cleanup code, the support picture cleaning, deformation and rotation in three scenes. Image cleanup options built a variety of clean-up methods, selectable by a predefined enumeration ImageType picture cleanup mode.

- ### ImageType image type enumeration (predefined CAPTCHA cleanup mode):

  - `CAPTCHA_NORMAL`: ordinary CAPTCHA
    ![Common code](images/img_NORMAL.jpg) ![Common code](images/img_NORMAL2.png)

  - `CAPTCHA_SPOT`: dot CAPTCHA
    ![Dot code](images/img_SPOT.gif) ![Dot code](images/img_SPOT2.gif)

  - `CAPTCHA_WHITE_CHAR`: white text on a solid color background CAPTCHA
    ![White text code](images/img_WHITE_CHAR.jpg) ![White text code](images/img_WHITE_CHAR2.jpg)
 
  - `CAPTCHA_HOLLOW_CHAR`: hollow text CAPTCHA
    ![Hollow character code](images/img_HOLLOW.jpg) ![Hollow character code](images/img_HOLLOW2.jpg)
 
  - `CLEAR`: No special interference ordinary picture clarity, improve the recognition rate
    ![General picture clarity](images/img_CLEAR.jpg)
  
  - `NONE`: In recognition, the picture does not do any cleanup

*Because CAPTCHA variety, and specific scenarios to be specifically addressed, there is no completely generic program. EasyOCR built-clean type only for limited circumstances, face particular scene needs through targeted treatment, such as by other means the picture pretreatment, or special training to achieve ORC engine.*

1. **Constructor:**

    ```JAVA
    /**
      * Clean up the object constructor Pictures
      *
      *@param ImageType CAPTCHA type, the default is ImageType.CAPTCHA_NORMAL
      *@param ImageWidthRatio deformation ratio of image width, default is 1
      *@param ImageHeightRatio deformation ratio of image width, default is 1
      *@param Degrees clockwise rotation angle of the picture, the default is 0
      */
     ImageClean([ImageType imageType] [, double imageWidthRatio] [, double  imageHeightRatio] [, int degrees]);
    ```
2. **Core methods:**
 
 - Image Cleanup
 ```JAVA
    /**
	 * Image Cleanup
	 * 
     *@param From the need to clean up the images, Support File String path; http://, ftp:// at the beginning of the URL string; File file object; InputStream input stream, a variety of sources overload
     *@param To clean up after the images, support for String String path, File file objects, and other overload.
     *@param ImageWidthRatio directly specify the degree of magnification image width, after some pictures deformation recognition rate can be improved, the default is 1
     *@param ImageHeightRatio directly specified height of the image magnification, some pictures after deformation recognition rate can be improved, the default is 1
     *@param Degrees clockwise angle pictures directly specified, the default is 0
     *@return Results
	 */
  public boolean cleanImage(from, to [, imageWidthRatio] [, imageHeightRatio] [, degrees]);
  ```

2. **Other methods:**

 The following methods can be directly set the default property values ImageClean instance, during the clean-up operation, without directly into each parameter.

 - Default image cleanup type
  ```JAVA
   void setImageType (ImageType imageType)
  ```

  - The ratio of image width and height of the deformation, deformation after some picture recognition rate can be improved, one of the original proportions, no deformation
  ```JAVA
   void setImageRatio (double imageWidthRatio, double imageHeightRatio)
  ```

  - The ratio of image width distortion, 1 of the original proportions
  ```JAVA
   void setImageWidthRatio (double imageWidthRatio)
  ```

  - Image Height deformation ratio of 1 for the original proportions
  ```JAVA
   void setImageHeightRatio (double imageHeightRatio)
  ```

  - Picture clockwise rotation angle, 0 is the default value, does not rotate
  ```JAVA
   void setDegrees (int degrees)
  ```

## 2. EasyOCR:

OCR text recognition Pictures core classes to complete the call to OCR engine. Internally With `ImageClean` complete automatic cleaning, deformation, rotation and identify integration work.


1. **Constructor:**

    ```JAVA
    /**
     * Pictures clean up the object structure
     * 
     * @param TesseractPath tesseract execute a command position, if you configure the path environment variable, you can not set
     * @param TesseractOptions tesseract command parameters
     */
     EasyOCR([String tesseractPath] [, String tesseractOptions]);
    ```

2. **tesseractOptions English language optional constants:**
   Tesseract-OCR can directly identify the default English. tesseractOptions property tesseract command line parameters can be specified to control the identified language (need to install language packs in advance), the details of parameters.
  EasyOCR built in Chinese and English, the basic parameters of the constants available.

 ```JAVA
    /**
	 * English recognition command parameters
	 */
	public static final String OPTION_LANG_ENG = "-l eng";
	/**
	 * Chinese recognize the command execution parameters
	 */
	public static final String OPTION_LANG_CHI_SIM = "-l chi_sim";
```
3. **Core methods:**

 - Direct identification pictures
  ```JAVA
	/**
	 * Identification picture content, and return to read the contents of the string
	 * 
	 * @param fromImage To identify the picture, Support File String path; http://, ftp:// at the beginning of the URL string; File file object; InputStream input stream, a variety of sources overload
	 * @return Code read
	 */
	public String discern(fromImage)

	/**
	 * The content identification picture on output to the specified file. toFile parameter is not specified, The default output to System.getProperty ("java.io.tmpdir") directory, file name: FileName.txt
	 * 
	 * @param fromImage To identify the picture, Support File String path; http://, ftp:// at the beginning of the URL string; File file object; InputStream input stream, a variety of sources overload
	 * @param toFile save the contents of the file, the default will automatically add the suffix .txt      
	 * @return Generation is complete
	 */
	public boolean discernToFile(fromImage [, String toFile]) 

    /**
	 * The content-aware image output to the specified file and read the contents of the string is returned. toFile parameter is not specified, The default output to System.getProperty ("java.io.tmpdir") directory, file name: FileName.txt
	 * 
	 * @param fromImage To identify the picture, Support File String path; http://, ftp:// at the beginning of the URL string; File file object; InputStream input stream, a variety of sources overload
	 * @param toFile  save the contents of the file, the default will automatically add the suffix .txt      
	 * @return Code read
	 */
	public String discernToFileAndGet(fromImage [, String toFile]) 
```

 - Clean and identify pictures
  ```JAVA
	/**
    * Press the picture type, strain ratio clockwise angle scene cleanup identification picture content, and return to read the contents of the string
    *
    * @param FromImage to identify pictures, Support File String path; http://, ftp:// at the beginning of the URL string; File file object; InputStream input stream, a variety of sources overload
    * @param ImageType picture type enumeration, the default is ImageType.NONE
    * @param AutoCleanImageWidthRatio directly specify the degree of magnification image width, after some pictures deformation recognition rate can be improved
    * @param AutoCleanImageHeightRatio directly specified height of the image magnification, after some pictures deformation recognition rate can be improved
    * @param AutoCleanDegrees picture clockwise angle when automatic cleaning
    * @return Identify the content
    */
	public String discernAndAutoCleanImage(fromImage [, ImageType imageType] [, double autoCleanImageWidthRatio] [, double  autoCleanImageHeightRatio] [, int autoCleanDegrees]) 

    /**
    * Press the picture type, strain ratio clockwise angle scene cleanup identify the image content, the output to the specified file, the default file name: Photo of .txt
    * toFile parameter is not specified, The default output to System.getProperty ("java.io.tmpdir") directory, file name: FromImageName.txt
	*
    * @param FromImage to identify pictures, Support File String path; http://, ftp:// at the beginning of the URL string; File file object; InputStream input stream, a variety of sources overload
    * @param ToFile save the contents of the file, the default will automatically add the suffix .txt 
    * @param ImageType image type, ImageType.NONE
    * @param AutoCleanImageWidthRatio directly specify the degree of magnification image width, after some pictures deformation recognition rate can be improved
    * @param AutoCleanImageHeightRatio directly specified height of the image magnification, after some pictures deformation recognition rate can be improved
    * @param AutoCleanDegrees picture clockwise angle when automatic cleaning
    * @return Generation is complete
    */
	public boolean discernToFileAndAutoCleanImage(fromImage [,String toFile] [, ImageType imageType] [, double autoCleanImageWidthRatio] [, double autoCleanImageHeightRatio] [,int autoCleanDegrees])

	/**
    * Press the picture type, strain ratio clockwise angle scene cleanup identification picture content, output to the specified file and returns to read the contents of the string, the output file without .txt extension
    * toFile parameter is not specified, The default output to System.getProperty ("java.io.tmpdir") directory, file name: FromImageName.txt
	*
    * @param FromImage to identify pictures, Support File String path; http://, ftp:// at the beginning of the URL string; File file object; InputStream input stream, a variety of sources overload
    * @param ToFile save the contents of the file, the default will automatically add the suffix .txt
    * @param ImageType image type
    * @param AutoCleanImageWidthRatio directly specify the degree of magnification image width, after some pictures deformation recognition rate can be improved
    * @param AutoCleanImageHeightRatio directly specified height of the image magnification, after some pictures deformation recognition rate can be improved
    * @param AutoCleanDegrees picture clockwise angle when automatic cleaning
    * @return Read code
    */
	public String discernToFileAndGetAndAutoCleanImage(fromImage [, String toFile] [, ImageType imageType] [, double autoCleanImageWidthRatio] [, double autoCleanImageHeightRatio] [,int autoCleanDegrees])
```

4. **Other methods:**
The following methods can be directly set the default property values EasyOCR instance, during the identification operation, clean up and identify pictures when calling method, each directly without passing parameters.

 - Location tesseract execute command if the configuration of the path environment variable, you can not set
 ```JAVA
 void setTesseractPath(String tesseractPath)
 ```

 - tesseract command parameter is used to adjust operating parameters. You can select a language from the basic constants TesseractOCR.OPTION_LANG _...
 ```JAVA
 void setTesseractOptions(String tesseractOptions)
 ```

 - Picture cleanup type
 ```JAVA
  void setAutoCleanImageType(ImageType autoCleanImageType) 
 ```

 - Image width and height ratio of deformation, deformation some pictures after the recognition rate can be improved, one of the original proportions, no deformation
 ```JAVA
  void setAutoCleanImageRatio(double autoCleanImageWidthRatio, double autoCleanImageHeightRatio)
 ```

 - Deformation ratio of image width, 1 of the original proportions
 ```JAVA
  void setAutoCleanImageWidthRatio(double autoCleanImageWidthRatio)
 ```
 - Deformation ratio of image width, 1 of the original proportions
 ```JAVA
  void setAutoCleanImageHeightRatio(double autoCleanImageHeightRatio)
 ```

 - Photos clockwise rotation angle, 0 is the default value, does not rotate
 ```JAVA
  void setDegrees(int autoCleanDegrees)
 ```


## End

[留言评论](http://www.easyproject.cn/easyocr/zh-cn/index.jsp#about '留言评论')

If you have more comments, suggestions or ideas, please contact me.

Email：<inthinkcolor@gmail.com>

[http://www.easyproject.cn](http://www.easyproject.cn "EasyProject Home")