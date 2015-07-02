# EasyOCR 图片清理插件编写

---------------

EasyOCR最主要的特点，就是除了使用一行代码就能完成OCR识别之外，还针对验证码识别提供一体化的清理和识别支持。但是，验证码出现的目的就是反机器提交和防止识别，所以识别和反识别之间是一对矛与盾的关系，此强彼弱，你来我往。

验证码识别技术中，关键技术点其实并不在于OCR，而是在于图片清理，能从洗图片中提取到有用的图片特征，清洗掉杂质。而验证码本身为了防止OCR，对字形做了很多干扰，一个好的清理算法能减少干扰，显著提高识别率。


当然，由于验证码本身的特征，没有任何算法是可以适应所有验证码类型的，做到100%准确无误的识别率也是不太可能的，有些验证码人都很难区分，机器也不一定能做到。所以，很多时候，不同的验证码类型，不同的场景，需要作出不同的图片清理。

**EasyOCR虽然无法穷举所有验证码场景和类型，但通过可扩展的插件支持，能够编写基于EasyOCR一体化识别的验证码图片清理扩展插件，通过自行编写图片清理的处理代码，或组合不同处理代码，来满足不同人的处理需求，解决不同场景问题。**


最后，值得考虑的是，在OCR识别技术不断提高，机器学习能力不断进步，大数据分析越来越快的的时候，传统以字符识别为主的验证码作用也在动摇，问题型验证码，图像选择型验证码等等都已经不再仅能通过OCR实现了。


## 插件列表

- 已开发的针对指定处理场景的验证码插件: 

| plugin | ImageClean Enum |
| ---- | ---- | 
| easyocr-linkbold-plugin-3.0.0-RELEASE.jar  | `LinkBoldImageType.LINK_BOLD ` |


- 插件详情
[Plugins](../plugins/Plugins.md "Plugins ")

## 插件编写规范和步骤

1. 定义图片清理类和处理方法（可选，如果不需要自行编写图片清理代码，则可以不定义）

 ```JAVA
/**
 * 定义图片清理处理类
 * @author easyproject.cn
 *
 */
public class LinkBoldClean {
	
   	/**
   	 * 图片处理方法，会自动注入BufferedImage和ImageClean对象
   	 * @param image 要处理的图片对象
   	 * @param imageClean ImageClean对象
   	 */
   	public void cleanOperate1(BufferedImage image, ImageClean imageClean){
   		   // 图片清理代码....
   	}
   	/**
   	 * 图片处理方法，会自动注入BufferedImage和ImageClean对象
   	 * @param image 要处理的图片对象
   	 * @param imageClean ImageClean对象
   	 */
   	public void cleanOperate2(BufferedImage bufferedImage, ImageClean imageClean){
   	   	// 图片清理代码....
   	}
}
```
由于为了向调用者提供更清晰的接口，ImageClean中将图片清理相关方法访问权限设置为protected，在外部并不可见。如果需要调用ImageClean中protected的清理方法，请利用反射实现，例如：
```JAVA
// 调用imageClean对象中的changeToBinarization方法
Method m = imageClean.getClass().getDeclaredMethod("changeToBinarization",
					int.class);
m.setAccessible(true);
m.invoke(imageClean, 117);
```

2. 自定义清理枚举类型
 - **枚举类型必须实现**`cn.easyproject.easyocr.Type`**接口**
 - **枚举格式：**
    - `清理方法`**#**`清理方法2`**#**`清理方法3`
 - **清理方法：**
 	- 清理方法必须加上对应类的完全限定名，格式：`类完全限定名.方法`
 	- 多个清理方法之间使用`#`分隔，清理方法会按定义顺序执行
 	- 如果清理方法在`cn.easyproject.easyocr.ImageClean`中，可以省略类完全限定名部分
 	- 可使用`,`代替`#`
	
 ```JAVA
public enum LinkBoldImageType implements Type {
		/**
		 * 定义图片类型枚举项，指定要调用的清理方法 
		 * 格式：清理方法#清理方法2#清理方法3
		 * 1. 清理方法必须加上对应类的完全限定名，格式：类完全限定名.方法
		 * 2. 多个清理方法之间使用#分隔，清理方法会按定义顺序执行
		 * 3. 如果清理方法在cn.easyproject.easyocr.ImageClean中，可以省略类完全限定名部分
		 * 4. 可使用`,`代替`#`
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
		* 返回清理枚举定义的方法调用列表
		* @return 清理方法列表
		*/
		public String getCleanMethods() {
			return this.cleanMethods;
		}
}
```

3. 调用测试 
```JAVA
	EasyOCR ec=new EasyOCR();
	System.out.println(ec.discernAndAutoCleanImage("images/link_bold_text/test/dk3d.png", LinkBoldImageType.LINK_BOLD));
```



## 结束

[留言评论](http://www.easyproject.cn/easyocr/zh-cn/index.jsp#about '留言评论')

如果您有更好意见，建议或想法，请联系我。


联系、反馈、定制、培训 Email：<inthinkcolor@gmail.com>


[http://www.easyproject.cn](http://www.easyproject.cn "EasyProject Home")