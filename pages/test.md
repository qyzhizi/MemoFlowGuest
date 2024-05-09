## 2024/5/9 16:14:53:
  #code 
  ＃que sldjfslf
	- #ans
	  在正则表达
- ## 2024/5/9 16:14:31:
    ＃正则匹配
- ## 2024/5/9 16:14:20:
  ＃正则匹配
  ＃que 正则匹配
	- #ans
	  在正则表达
- ## 2024/5/9 16:13:18:
  ＃正则匹配
  ＃que 正则匹配 在 `[xxx]`中的字符需要转义吗？
	- #ans
	  在正则表达
- ## 2024/5/9 16:07:34:
	- #正则匹配 #code
	  #que url memoflow 正则表达式
	- #ans
	  ****************************************`new`*******************************
	  `update`: 2024-05-09
	  ```
	  // var urlRegex = /https?:\/\/(?:www\.)?[^\s(<]+/g;
	      var urlRegex = /https?:\/\/(?:www\.)?[-a-zA-Z0-9@:%._\+~#=]{2,256}\.[a-z]{2,6}\b(?:[-a-zA-Z0-9@:%_\+.~#?&/=]*)/g;
	  ```
	  这个正则表达式可以匹配包含协议（http:// 或 https://）、域名（包括 www）、路径和查询参数的 URL。它会忽略空格和尖括号，而且不会匹配左括号 "("。
	  
	  请注意，尽管这个正则表达式相对准确，但并不是绝对完美的。URL 的格式和规则非常复杂，可能会有一些特殊情况无法完全覆盖。对于更复杂的 URL 匹配需求，可能需要使用更高级的 URL 解析库或工具。
	  `(?:www\.)?` 是一个非捕获分组，用于匹配可选的 "www." 子域名部分。这个正则表达式中的 `?:` 表示该分组不会被捕获，不会作为匹配结果的一部分返回。
	  
	  具体来说，`(?:www\.)?` 表示匹配零次或一次 "www." 字符串。也就是说，它可以匹配包含 "www." 子域名和不包含 "www." 子域名的 URL。
	  
	  例如，它可以匹配以下两种 URL：
	  
	    - `http://example.com`：不包含 "www." 子域名的 URL
	    - `http://www.example.com`：包含 "www." 子域名的 URL
	  
	  通过使用 `(?:www\.)?`，我们可以灵活地匹配这两种情况，而不需要将 "www." 子域名部分作为一个单独的捕获组返回。
	  
	  `[-a-zA-Z0-9@:%._\+~#=]{2,256}` 是一个字符集范围，用于匹配 URL 中的域名部分。
	  
	  具体来说，它包含以下字符：
	  ```
	    - `-`：表示连字符（hyphen）
	    - `a-z`：表示小写字母 a 到 z
	    - `A-Z`：表示大写字母 A 到 Z
	    - `0-9`：表示数字 0 到 9
	    - `@`：表示字符 "@"
	    - `:`：表示字符 ":"
	    - `%`：表示字符 "%"
	    - `.`：表示字符 "."
	    - `_`：表示字符 "_"
	    - `\+`：表示字符 "+"
	    - `~`：表示字符 "~"
	    - `#`：表示字符 "#"
	    - `=`：表示字符 "="
	  ```
	  
	  这个字符集范围用于匹配域名中允许的字符。范围内的字符可以出现 2 到 256 次，即域名的长度可以在 2 到 256 个字符之间。
	  
	  请注意，这个字符集范围并不是绝对准确的，因为 URL 的规则和允许的字符可能会因特定的标准或要求而有所不同。这个范围只是一个常见的示例，可能无法覆盖所有情况。在实际使用中，应根据特定的需求和标准进行适当的调整和验证。
	  
	  `\b` 是一个特殊的正则表达式元字符，表示单词边界。
	  
	  单词边界指的是单词字符（字母、数字、下划线）与非单词字符之间的位置。具体来说，`\b` 匹配以下三种情况之间的位置：
	  
	  1. 单词字符与非单词字符之间的位置，例如单词和空格之间的位置。
	  2. 字符串的开头和第一个单词字符之间的位置。
	  3. 字符串的结尾和最后一个单词字符之间的位置。
	  
	  `\b` 可以用于确保正则表达式只匹配完整的单词，而不是单词的一部分。
	  
	  例如，考虑以下字符串："Hello, world!"。如果我们使用正则表达式 `\bworld\b` 进行匹配，它只会匹配字符串中的 "world" 这个完整的单词，而不会匹配 "Hello, world!" 中的 "world"。
	  
	  需要注意的是，`\b` 是一个零宽度断言，不会消费任何字符。它只匹配位置，而不匹配实际的字符。
	  
	  ****************************************`old`*******************************
	  末尾 匹配任何不是空白字符（`\s`）或"<"的一个或多个字符, 添加 "<" 是由于避免匹配到html 元素的开头, 例如: `<span> ... </span>`, 否则会破坏元素结构, 这主要是我 直接解析 html 导致的后遗症, 应该直接解析 txt,得到结构化的数据, 然后再统一转换为 html 元素.
	  
	  ```
	  var urlRegex = /(?<=^|\s)https?:\/\/[^\s<]+/g;
	  ```
	  这段代码是一个正则表达式，用于匹配文本中的网址。正则表达式是一种强大的工具，用于在字符串中搜索、匹配和替换符合特定模式的文本。这个特定的正则表达式旨在找出文本中所有的HTTP或HTTPS链接。让我们逐部分解析这个表达式：
	    - `(?<=^|\s)`：这是一个正向后视断言，用于匹配一个位置，这个位置后面紧跟着的是表达式`^`（字符串的开始）或`\s`（任何空白字符，比如空格、制表符等）。这意味着匹配的URL必须位于字符串的开始或者一个空白字符后面，但这部分本身不会被包含在匹配结果中。
	    - `https?`：这部分匹配字符串"http"后面跟随0个或1个"s"，因此它可以匹配"http"或"https"。
	    - `:\/\/`：这部分匹配字符串"://"，这是URL中协议和后续部分之间的分隔符。
	    - `[^\s<]+`：这是一个字符集，匹配任何不是空白字符（`\s`）或"<"的一个或多个字符。`^`在方括号的开头表示否定，所以这里匹配的是所有非空白且不是"<"的字符序列。这部分旨在匹配URL的主体部分，直到遇到空白字符或"<"为止。
	    - `/g`：这是一个修饰符，表示全局搜索。正则表达式会应用于整个字符串，找到所有匹配的项，而不是在找到第一个匹配项后就停止。
	  
	  总的来说，这个正则表达式用于从文本中提取完整的HTTP或HTTPS链接，这些链接必须位于字符串的开头或者空白字符后面，并且直到遇到另一个空白字符或"<"为止。例如，在文本`"Visit our site at https://www.example.com for more information."`中，它会匹配`"https://www.example.com"`。
- ## 2024/5/9 15:34:50:
	- #tools #windows
	  #que Windows，微软官方出的 PowerToys，小工具集合，还是开源的。尤其是里面的 Text Extractor 用作 OCR 很方便。
	- #ans
	  下载地址：https://learn.microsoft.com/zh-cn/windows/powertoys/
	  源代码：https://github.com/microsoft/PowerToys
	  
	  当前 PowerToy 实用工具
	  当前可用的实用工具包括：
	  Always On Top
	  📷Always On Top 使你能够使用快捷键 (⊞ Win+Ctrl+T) 将窗口固定在其他窗口之上。
	  
	  https://x.com/dotey/status/1788384863373939051
- ## 2024/5/4 20:49:00:
	- #code #memoflow
	  #que 一个网页在手机端浏览器打开, 发送到手机桌面, 当以app形式打开时, 如何设置隐藏地址栏?
	- #ans
	  要在手机端浏览器打开的网页发送到手机桌面，并以类似应用程序（App）形式打开时隐藏地址栏，可以通过在网页的 HTML `<head>` 部分添加特定的元标签（meta tags）来实现。具体步骤如下：
	  
	  1. **添加网站样式**：在网站的 HTML `<head>` 部分，添加以下元标签来自定义应用名称、图标，并设置全屏显示，从而在打开时不显示地址栏。
	  
	  ```html
	  <head>
	      <meta charset="UTF-8">
	      <!-- 自定义应用名称 -->
	      <meta name="application-name" content="您的应用名称">
	      <!-- 自定义应用图标可用 -->
	      <link rel="apple-touch-icon-precomposed" sizes="120x120" href="您的图标地址">
	      <!-- 全屏设置 -->
	      <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no, minimal-ui"/>
	      <!-- 网站开启对 web app 程序的支持 具体表现为去除浏览器地址栏和底部导航栏 -->
	      <meta name="apple-mobile-web-app-capable" content="yes">
	      <!-- 用来定义顶部状态栏的形式 -->
	      <meta name="apple-mobile-web-app-status-bar-style" content="black-translucent"/>
	  </head>
	  ```
	  
	  2. **添加到桌面图标**：用户使用 Safari 浏览器打开网站后，点击添加到主屏幕。这样，当用户通过主屏幕上的图标打开网页时，它将以全屏模式显示，类似于一个原生应用程序，地址栏将被隐藏。
	  
	  3. **打开桌面图标**：用户点击桌面上的图标后，网页将直接以全屏模式展示，提供类似于 App 的体验，而不会显示地址栏。
	  
	  通过设置 `<meta name="apple-mobile-web-app-capable" content="yes">` 元标签，可以使得网页在作为 Web App 添加到主屏幕后，以全屏模式打开，从而隐藏地址栏。同时，`<meta name="apple-mobile-web-app-status-bar-style" content="black-translucent"/>` 允许定义顶部状态栏的样式，进一步增强类似 App 的体验[3]。
	  
	  Citations:
	  [1] https://github.com/meterscao/tick
	  [2] https://juejin.cn/s/safari%E6%80%8E%E4%B9%88%E9%9A%90%E8%97%8F%E5%9C%B0%E5%9D%80%E6%A0%8F
	  [3] https://blog.csdn.net/weixin_44786530/article/details/129728215
	  [4] https://support.apple.com/zh-cn/guide/shortcuts/apd735880972/ios
	  [5] https://support.apple.com/zh-sg/guide/iphone/iphb3100d149/ios
	  [6] https://sspai.com/post/86288
	  
	  #sque `<meta name="apple-mobile-web-app-status-bar-style" content="black-translucent"/>` 是什么意思?
	  #ans 
	  Based on the search results provided, the answer to the question is:
	  
	  The `<meta name="apple-mobile-web-app-status-bar-style" content="black-translucent"/>` meta tag sets the style of the status bar for a web application on iOS devices when the web page is added to the home screen and opened in full-screen mode.
	  
	  Specifically:
	  
	    - The `black-translucent` value sets the status bar to have a black and translucent background, with white text and symbols.[1][2]
	    - This setting causes the web content to be displayed on the entire screen, partially obscured by the status bar.[1][2]
	    - The `black-translucent` option is one of the only three supported values for this meta tag, the others being `default` and `black`.[2][5]
	    - To use this meta tag, the `<meta name="apple-mobile-web-app-capable" content="yes">` meta tag must also be set to enable full-screen mode.[1][2][5]
	  
	  So in summary, the `<meta name="apple-mobile-web-app-status-bar-style" content="black-translucent"/>` meta tag allows you to customize the appearance of the status bar when your web page is added to the iOS home screen and opened in full-screen mode.[1][2][3][4][5]
	  
	  Citations:
	  [1] https://stackoverflow.com/questions/39749015/apple-mobile-web-app-status-bar-style-in-ios-10
	  [2] https://github.com/vaadin/flow/issues/5576
	  [3] https://iphtechnologies.com/apple-mobile-web-app-status-bar-style/
	  [4] https://codeburst.io/progressive-web-apps-customize-status-bar-23c4b2de590f?gi=06fcde5332b6
	  [5] https://www.drupal.org/project/webapp/issues/1858800
	  
	  `Memos` 的实现
	  ``` 
	  <link rel="manifest" href="/site.webmanifest">
	  {
	    "name": "Memos",
	    "short_name": "",
	    "icons": [
	      { "src": "/android-chrome-192x192.png", "sizes": "192x192", "type": "image/png" },
	      { "src": "/android-chrome-512x512.png", "sizes": "512x512", "type": "image/png" }
	    ],
	    "theme_color": "#ffffff",
	    "background_color": "#ffffff",
	    "display": "standalone"
	  }
	  ```
	  
	  `总结:`
	  要实现在手机端（如iOS和Android设备）将网页以应用程序形式打开，并隐藏地址栏，可以通过以下几种方法实现：
	  
	  @@@ 1. 使用Web App Manifest（适用于Android）
	  
	  对于支持 Progressive Web Apps (PWA) 的浏览器（如Chrome），你可以通过添加一个名为`manifest.json`的Web App Manifest文件到你的网站中，来指示浏览器你的网站是一个PWA。在这个文件中，你可以指定一些应用元数据（如应用名称、图标、启动画面等），以及控制应用启动时的显示模式。
	  
	  例如，要隐藏地址栏，可以在`manifest.json`文件中设置`"display": "fullscreen"`或`"display": "standalone"`，这将使应用以全屏或独立窗口模式启动，不显示地址栏：
	  
	  ```json
	  {
	    "short_name": "AppName",
	    "name": "Application Name",
	    "icons": [
	      {
	        "src": "icon/lowres.webp",
	        "sizes": "48x48",
	        "type": "image/webp"
	      },
	      {
	        "src": "icon/hd_hi.ico",
	        "sizes": "72x72 96x96 128x128 256x256",
	        "type": "image/x-icon"
	      }
	    ],
	    "start_url": "index.html",
	    "background_color": "#FFFFFF",
	    "display": "standalone",
	    "scope": "/",
	    "theme_color": "#FFFFFF"
	  }
	  ```
	  
	  然后，在网页的`<head>`标签中引用这个manifest文件：
	  
	  ```html
	  <link rel="manifest" href="/manifest.json">
	  ```
	  
	  @@@ 2. 使用iOS的Web应用程序元标签
	  
	  对于iOS设备，Safari允许通过添加特定的`<meta>`标签到你的网页中来控制网页作为Web应用程序打开时的一些行为，包括隐藏地址栏。要实现这一点，可以在网页的`<head>`部分添加如下标签：
	  
	  ```html
	  <meta name="apple-mobile-web-app-capable" content="yes">
	  <meta name="apple-mobile-web-app-status-bar-style" content="black">
	  ```
	  
	  `apple-mobile-web-app-capable`设置为`yes`会告诉Safari这个网站是一个Web应用程序，应该以全屏模式运行，不显示Safari的浏览器UI。`apple-mobile-web-app-status-bar-style`控制状态栏的样式。
	  
	  @@@ 注意事项
	    - 当用户通过这些方式添加的Web应用到主屏幕时，应用会在没有浏览器UI的情况下运行，但这并不意味着它们是真正的原生应用。
	    - 这些方法依赖于用户手动将网页添加到主屏幕。你可以通过在网站上添加指引，鼓励用户进行这一操作。
	    - 功能的支持程度可能因浏览器和操作系统的版本不同而有所差异。
	  
	  通过上述方法，你可以为用户提供类似于原生应用的体验，同时隐藏地址栏，使得用户界面更加干净、专注于内容。
- ## 2024/5/4 20:21:20:
  ```jdljfsldjfsdl```
- ## 2024/5/4 19:43:19:
	- #正则匹配 #code
	  #que memoflow 标签 正则匹配 tag 加入 utf-8 中文 匹配
	- #ans
	  ```
	  const tagRegex = /(?<!#)#(?![#])[/\w\u4e00-\u9fff]+(?=\x20|\n)/g;
	  ```
	  更新:
	  ```
	  const tagRegex = /(?<=(\x20|^))(?<!#)#(?![#])[/\w\u4e00-\u9fff]+(?=\x20|\n|$)/g;
	  ```
	  `[/\w\u4e00-\u9fff]+`不能改为空白字符外的任意字符`.*`, 也会破坏html 文档结构, 这主要是我 直接解析 html 导致的后遗症, 应该直接解析 txt,得到结构化的数据, 然后再统一转换为 html 元素.
	  `[/\w\u4e00-\u9fff]` 其中 `/` 表示一个独立的字符, 因为我的tag 会包含 `/`
	  `\w`表示英文单词
	  `\u4e00-\u9fff`表示中文字符集
- ## 2024/5/4 19:39:41:
  #jdlfj
- ## 2024/5/4 19:34:29:
  ` #jdlfj $djfsdlfj$`
- ## 2024/5/4 17:55:21:
  #sldjfs
  https://github.com/qyzhizi/python_logical/blob/main/2024-04-19-html-top-sticky/test.html#L17 #sldjs
- ## 2024/5/4 17:40:19:
  `$aa$`
  `#lsdjs`
- ## 2024/5/4 17:04:41:
  dfjslfjsdlfjsl;fjds;
- ## 2024/5/4 17:02:58:
  dfjslfjsdlfjsl;fjds;
- ## 2024/5/4 15:42:07:
  ldfjsldjs
  ```
  11111111
  ```
  `abc`
  ```
  22222222222
  ```
  `efg`
- ## 2024/5/4 14:19:44:
  ```
  11111111
  ```
  `abc`
  ```
  22222222222
  ```
  `efg`
  
  #sque `<meta name="apple-mobile-web-app-status-bar-style" content="black-translucent"/>` 是什么意思?
	- #ans
	  sjfsl
	  
	  #asldjsdfl #sdjfsldfj #lsjflsdjf
	  #lsjfsdljf
- ## 2024/5/3 13:32:36:
  ```
  $fjslfs$
  ``` gfgfdgdg
- ## 2024/5/3 13:32:14:
  ```
  sldfjsldfjsdl
  ```
- ## 2024/5/3 12:46:28:
  
  ```
  const tagRegex = /(?<!#)#(?![#])[/\w\u4e00-\u9fff]+(?=\x20|\n)/g;
  ```
  但fsdfjsl
- ## 2024/5/3 12:16:58:
  @@ 2024/4/19 08:42:54:
	- #code
	  #que sticky top 的使用方法
	- #ans
	  https://github.com/qyzhizi/python_logical/blob/main/2024-04-19-html-top-sticky/test.html#L17
	  ```css
	  position: sticky;
	  top: 10px;
	  left: 800px;
	  ```
	  sdlfjsdlfjsldf
- ## 2024/5/3 11:29:53:
	- #正则匹配 #code
	  #que `/(?<!#)#.*?(?=\x20|\n)/g;`修改表达式, 我希望开头只会有一个
- ## 2024/5/3 10:32:10:
	- #code
	  #que 点击 `#cancel-account-info-edit` 按钮第一次可以正常工作，但之后点击就没有任何效果
	- #ans
	  如果你遇到的问题是点击 `#cancel-account-info-edit` 按钮第一次可以正常工作，但之后点击就没有任何效果，这通常是因为事件绑定的问题。这种情况经常发生在动态加载的内容或者是在某些操作之后动态改变了DOM元素的情况下。
	  
	  在jQuery中，对于动态添加到页面的元素，直接使用 `.click()` 或者 `.on('click', handler)` 方式绑定事件处理函数可能不会生效，因为当执行这些绑定代码时，目标元素可能还不存在于DOM中。解决这个问题的一种方法是使用事件委托（Event Delegation）。事件委托是一种技术，它利用了事件冒泡的原理，将事件监听器添加到一个父元素上，而不是直接添加到目标子元素上。这样，无论何时添加子元素，只要事件冒泡到父元素，就会触发事件处理函数。
	  
	  针对你的情况，如果 `#cancel-account-info-edit` 是后来动态添加到DOM中的，或者其父元素是动态变化的，你可以尝试使用事件委托。下面是一个使用事件委托的例子，这里假设 `body` 是该按钮的一个静态父元素（你可以根据实际情况选择更合适的静态父元素）：
	  
	  ```javascript
	  $('body').on('click', '#cancel-account-info-edit', function() {
	      $('.dialog-wrapper.update-account-dialog.showup').removeClass('showup').addClass('showoff');
	  });
	  
	  ```
	  这段代码的意思是，不直接在 `#cancel-account-info-edit` 上绑定点击事件，而是在 `body` 上绑定。只要点击事件在 `#cancel-account-info-edit` 上发生并冒泡到 `body`，就会触发这个处理函数。这样，无论 `#cancel-account-info-edit` 何时被添加到DOM中，这个点击事件处理函数都能正常工作。
	  
	  如果这个方法解决了你的问题，那么原因就是事件绑定没有正确处理动态添加的元素。如果问题依旧，可能需要进一步检查其他潜在的问题，比如是否有其他脚本错误干扰了事件的正常触发等。
- ## 2024/5/3 09:37:08:
  `body` ```1``` `#cancel-account-info-edit`
- ## 2024/5/3 06:46:30:
  `$...$` 或者 `$$...$$` ` #code ` gfg
- ## 2024/5/3 03:25:22:
  `sldfjs`
- ## 2024/5/3 00:01:43:
  ```
  `sfsjlf`
   #agb 
  $djslf$
  https://github.com/qyzhizi/MemoFlow/commit/5204630cb9c884b9303e74e748e352181f7dc790
  $$
  \begin{equation}
  \int_{a}^{b} x^2 \,dx = \frac{1}{3}x^3 \Bigg|_{a}^{b}
  \end{equation}
  $$
  ```
- ## 2024/5/2 23:21:16:
  const codeRegex = /```(\w*)([\s\S]*?)```/gi;