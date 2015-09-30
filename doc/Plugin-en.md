# EasyOCR Image clean plugin write

---------------

EasyOCR most important feature is the addition to the line of code can be done using OCR recognition, but also for the verification code identification provide cleanup and recognition support integration. However, the emergence of object code verification is to submit the anti-machine and prevent identification, it is a relationship between the spear and shield to identify and counter-identification, this strong weak Peter, you come to me.

Verification code recognition technology, the key point is not that the technology OCR, but that image cleaning, from washing to extract useful picture in picture feature, cleansed of impurities. But verification code itself in order to prevent the OCR, glyphs do a lot of interference, a good cleaning algorithm can reduce interference significantly improve the recognition rate.


Of course, since the characteristics of the code itself, there is no algorithm that can adapt to all types of authentication code, so that recognition accuracy rate of 100% is unlikely, some people are very difficult to distinguish between the code, the machine may not be able do it. So, very often, different authentication code type, different scenarios, need to make different pictures cleanup.

** EasyOCR While it is not exhaustive of all the code and the type of scene, but can be extended through plug-in support, the ability to write on EasyOCR integrated identification verification code cleanup extensions, by writing your own image cleanup handling code, or a combination of different treatments the code to meet the needs of different people deal with and solve problems in different scenarios. **


Finally, it is worth considering that in the OCR recognition technology continues to improve, machine learning and progress, big data analytics faster and faster when the traditional character recognition to the role-based authentication code are shaken, problem type the verification code, Selectable image verification code and so on are no longer only be achieved through OCR.


## Plugin list

- It has been developed for the specified processing scenarios verification code plug-ins.


| plugin | ImageClean Enum | required |
| ----------- | ------------ | ----------- |
| **easyocr-linkbold**-plugin-3.0.0-RELEASE.jar  | `LinkBoldImageType.LINK_BOLD ` | 1. Create new file: `%Tesseract-OCR%\tessdata\configs\lettersAndNumbers` <br/> 2. Character defining, write content to file: `tessedit_char_whitelist 0123456789ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz`  <br/> 3. Set tesseractOptions: ` lettersAndNumbers` <br/> `EasyOCR ec=new EasyOCR();` <br/>	`ec.setTesseractOptions("lettersAndNumbers");`  |

- Maven
```XML
<dependency>
       <groupId>cn.easyproject</groupId>
       <artifactId>easyocr-linkbold-plugin</artifactId>
       <version>3.0.3-RELEASE</version>
</dependency>
```

- Plugin details
[Plugins](../plugins/Plugins.md "Plugins ")


## Plugin the preparation of specifications and procedures

1. Custom image processing method of clean-up (optional, if you do not write your own picture cleanup code, you can not define)

 ```JAVA
/**
 * Custom image cleanup class
 * @author easyproject.cn
 *
 */
public class LinkBoldClean {
	
   	/**
   	 * Image processing method will automatically inject BufferedImage objects and ImageClean
   	 * @param image Picture object to be processed
   	 * @param imageClean ImageClean Object
   	 */
   	public void cleanOperate1(BufferedImage image, ImageClean imageClean){
   		   // Image cleanup code ....
   	}
   	/**
   	 * Image processing method will automatically inject BufferedImage objects and ImageClean
   	 * @param image Picture object to be processed
   	 * @param imageClean ImageClean Object
   	 */
   	public void cleanOperate2(BufferedImage bufferedImage, ImageClean imageClean){
   	   	// Image cleanup code ....
   	}
}
```
Because in order to provide a clearer interface to the caller, in the image ImageClean cleanup associated method setting access to protected, it is not visible externally. If you need to call the protected ImageClean cleaning method, using reflection to realize, for example:
```JAVA
// invoke imageClean object changeToBinarization method
Method m = imageClean.getClass().getDeclaredMethod("changeToBinarization",
					int.class);
m.setAccessible(true);
m.invoke(imageClean, 117);
```

2. Custom enumeration type cleanup
 - **Enum must to implements **`cn.easyproject.easyocr.Type`
 - **Enum Format: **
    - `cleanMethod1`**#**`cleanMethod2`**#**`cleanMethod3`
 - **Clean method:**
 	- Methods for cleaning must add the fully qualified name of the corresponding class format:  `className.method` 
 	- Use the across multiple cleaning method `#` separate, clean-up methods will be performed in order to define
 	- If the cleaning method in `cn.easyproject.easyocr.ImageClean`, you can omit the name part of the fully qualified class
 	- You can use `,` instead of `#`
	
 ```JAVA
public enum LinkBoldImageType implements Type {
		/**
		 * Custom image type enumerations, specifies cleanup method to call 
		 * Enum Format:cleanMethod1#cleanMethod2#cleanMethod3
		 * 1. Methods for cleaning must add the fully qualified name of the corresponding class format:  `className.method`
		 * 2. Use the across multiple cleaning method `#` separate, clean-up methods will be performed in order to define
		 * 3. If the cleaning method in `cn.easyproject.easyocr.ImageClean`, you can omit the name part of the fully qualified class
		 * 4. You can use `,` instead of `#`
		 */
		LINK_BOLD("changeToBinarizationForLinkBold"
		+ "#changeToGrey"
		+ "#cn.easyproject.easyocr.plugin.linkbold.LinkBoldClean.cleanOperate1"
		+ "#cn.easyproject.easyocr.plugin.linkbold.LinkBoldClean.cleanOperate2");

		private final String cleanMethods;

		private LinkBoldImageType(String cleanMethods) {
			this.cleanMethods = cleanMethods;
		}
		/**
		* return enumeration defines cleanup method list
		* @return Methods for cleaning up list
		*/
		public String getCleanMethods() {
			return this.cleanMethods;
		}
}
```

3. test 
```JAVA
	EasyOCR ec=new EasyOCR();
	System.out.println(ec.discernAndAutoCleanImage("images/link_bold_text/test/dk3d.png", LinkBoldImageType.LINK_BOLD));
```


## End

[Comments](http://www.easyproject.cn/easyocr/zh-cn/index.jsp#about 'Comments')

If you have more comments, suggestions or ideas, please contact me.

Email：<inthinkcolor@gmail.com>

[http://www.easyproject.cn](http://www.easyproject.cn "EasyProject Home")