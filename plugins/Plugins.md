# Plugins
_____________

It has been developed for the specified processing scenarios verification code plug-ins.

## 1. plugins

| plugin | ImageClean Enum | required |
| ---- | ---- | 
| **easyocr-linkbold**-plugin-3.0.0-RELEASE.jar  | `LinkBoldImageType.LINK_BOLD ` | 1. Create new file: `%Tesseract-OCR%\tessdata\configs\lettersAndNumbers` <br/> 2. Character defining, write content to file: `tessedit_char_whitelist 0123456789ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz`  <br/> 3. Set tesseractOptions: ` lettersAndNumbers` <br/> `EasyOCR ec=new EasyOCR();` <br/>	`ec.setTesseractOptions("lettersAndNumbers");`  |


## 2. Plugins Description
 - #### easyocr-linkbord Plugin
 
   - **Image Type**
   ![linkbord CAPTCHA](example_image/linkbold/plugin_linkbord1.png) ![linkbord CAPTCHA](example_image/linkbold/plugin_linkbord2.png) ![linkbord CAPTCHA](example_image/linkbold/plugin_linkbord3.png)
   
   - **ImageClean Enum**
   `LinkBoldImageType.LINK_BOLD`

   - **Required**
    Character defining only numbers and letters.
    1. Create new file: 
    `%Tesseract-OCR%\tessdata\configs\lettersAndNumbers` 
    2.  Character defining, write content: `tessedit_char_whitelist 0123456789ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz`  
    3.  Set tesseractOptions: ` lettersAndNumbers` 
    `EasyOCR ec=new EasyOCR();` 
   	`ec.setTesseractOptions("lettersAndNumbers");`  

   - **Demo**
   ```JAVA
   EasyOCR ec=new EasyOCR();
   ec.setTesseractOptions("lettersAndNumbers");
   String res=ec.discernAndAutoCleanImage("images/link_bold_text/test/dk3d.png", LinkBoldImageType.LINK_BOLD).replace(" ", "");
   System.out.println(res);
   ```

   - **Recognition rate**
    Total recognition: 31
    Success: 10
    Failed: 21
    Recognition rate: 32.25806451612903%
  
   - **Sample source**
   s\*\*\*\*\*888@gmail.com
