# [应用程序本地化](http://blog.csdn.net/q199109106q/article/details/8564615)

### 简介
  * 使用本地化功能，可以轻松地将应用程序翻译成多种语言，甚至可以翻译成同一语言的多种方言
  * 如果要添加本地化功能，需要为每种支持的语言创建一个子目录，称为”本地化文件夹”，通常使用`.lproj`作为拓展名
  * 当本地化的应用程序需要载入某一资源时，如图像、属性列表、nib文件，应用程序会检查用户的语言和地区，并查找相匹配的本地化文件夹。如果找到了相应的文件夹，就会载入这个文件夹中的资源
  
### 本地化前的准备
其实就是先创建好中文的本地化文件夹（zh-Hans.lproj），让应用程序支持中文语言环境
Main.storyboard 和 XIB可能有所不同，但是都是大同小异

> YRLocationDemo --> PROLECT -->Localization -->Info -->+ -->Chinese(zh-Hans)

进行勾选需要本地化的文件 | 勾选的含义
----|----
**Main.storyboard** | 都选代本地化.storyboard文件，如果有XIB文件的话，勾选上的意思数一样的
**InfoPlist.strings** | 勾选上代表本地化Info.plist文件

选择Finish后

+ 硬盘上多了一个中文的本地化文件夹`zh-Hans.lproj`，至于`en.lproj`文件夹是英文的本地化文件夹，创建项目时默认就有的.
+ 项目中的`Main.storyboard`、`InfoPlist.strings`文件左边也多了个可以展开的三角形，展开可以发现分别都有2个版本的文件:

Main.storyboard | InfoPlist.strings
----|----
Main.storyboard(English)对应en.lproj文件夹中的Main.storyboard文件 |InfoPlist.strings(English)对应en.lproj文件夹中的InfoPlist.strings文件
Main.storyboard(Chinese)对应zh-Hans.lproj文件夹中的Main.storyboard文件 | InfoPlist.strings(Chinese)对应zh-Hans.lproj文件夹中的InfoPlist.strings文件

### Main.storyboard 或者 XIB的本地化
打开Main.storyboard(Chinese)文件，修改里面的文字信息(这里不修改图片)


### 应用程序名称本地化(Info.plist本地化)
`知识背景：Info.plist中有个叫CFBundleDisplayName的key决定了应用程序的名称`

+ 为Info.plist添加一个key-value，让应用程序支持名称本地化，Info.plist就会去InfoPlist.strings加载CFBundleDisplayName对应的字符串

	Information Property List --> + --> Application has localized display name --> YES
	
+ 在InfoPlist.strings(English)文件中加入：

	`CFBundleDisplayName` = "应用需要显示的英文名";
	
+ 在InfoPlist.strings(Chinese)文件中加入：

	`CFBundleDisplayName` = "应用需要显示的中文名";
	

### 图片本地化

+ 单击选中home.png --> 然后查看右上角的视图 --> 找图片的信息 --> Localization --> English --> localize
	
	选择图片支持中文，在localization选中Chinese选项，和上面进行过本地化的一样也会在磁盘上和工程中对同一个文件出现两个类型的文件
+ 这时候只需要将zh-Hans.lproj文件下的图片换成需要显示的中文环境下的图片即可`工程中照常使用图片`,会自动进行环境的适配

### 字符串的本地化

+ 创建一个字符串资源文件
	iOS --> Resource --> String.File  和创建Setting.Bundle在同一个地方
	
+ 文件名最好是`Localizable.strings`，如果使用其他文件名，使用字符串时的调用会有些区别

+ 为Localizable.strings添加多语言支持(跟上面图片本地化类似)

+ 在Localizable.strings(English)文件加入需要进行本地化的字符串

	Tip = "Tip";
	
	OK = "OK";
	
+ 在Localizable.strings(Chinese)文件加入对应的本地化字符串
	
	Tip = "提醒";
	
	OK = "好的";

+ 在代码中使用本地化字符串 `如果没有对字符串进行本地化 或者 找不到key对应的值，NSLocalizedString将直接返回key这个字符串`

	在代码中使用`NSLocalizedString(key, comment)`来读取本地化字符串
	
	key是Localizable.strings文件中等号左边的字符串，comment纯粹是注释
	
	- NSString *testString = NSLocalizedString(@"test", nil);
	
    - NSString *okString = NSLocalizedString(@"OK", nil);
	
	
+ 如果你的字符串资源文件名不是Localizable.strings,如yr.strings

	那么你就得使用`NSLocalizedStringFromTable(key, tbl, comment)`来读取本地化字符串
	
	- NSString *yrString = NSLocalizedStringFromTable(@"test", @"yr", nil);
