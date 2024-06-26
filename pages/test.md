## 2024/6/22 17:57:07:
  `)dslfjsd`
- ## 2024/6/22 17:53:55:
	- #code
	  #que what's the meaning of `struct tag`  of Struct  in golang
	- #ans
	  A struct tag in Go is a string literal that can be attached to a struct field to provide metadata or instructions for other packages that work with the struct. It is enclosed in backticks (`) and follows the field type declaration.
	  
	  @@ Struct Tag Syntax
	  
	  The general syntax for a struct tag is:
	  
	  ```go
	  type StructName struct {
	      FieldName Type `key1:"value1" key2:"value2"`
	  }
	  ```
		- `key1` and `key2` are the tag keys, which are strings that identify the purpose of the tag value.
		- `"value1"` and `"value2"` are the corresponding tag values associated with each key.
		  
		  Multiple tags can be added to a single field by separating them with a space.[1][2]
		  
		  @@ Common Use Cases
		  
		  Struct tags are widely used in the Go standard library and third-party packages for various purposes, such as:
		  
		  1. **JSON/XML Encoding**: The `encoding/json` and `encoding/xml` packages use struct tags to customize the mapping between struct fields and their JSON/XML representations.[2][3]
		  
		  2. **Database Mapping**: ORMs (Object-Relational Mapping) libraries like `gorm` and `xorm` use struct tags to map struct fields to database columns.[1]
		  
		  3. **Form Binding**: Web frameworks like `gin` and `echo` use struct tags to bind form data to struct fields.
		  
		  4. **Validation**: Validation packages like `validator` use struct tags to define validation rules for struct fields.
		  
		  @@ Accessing Struct Tags
		  
		  The `reflect` package in Go provides a way to access struct tags at runtime. The `StructTag` type represents a struct field's tag string and provides methods like `Get(key string)` to retrieve the value associated with a specific key.[4]
		  
		  Struct tags are a powerful feature in Go that allows developers to add metadata to struct fields, enabling better integration with various libraries and frameworks.
		  
		  Citations:
		  [1] https://go.dev/wiki/Well-known-struct-tags
		  [2] https://www.digitalocean.com/community/tutorials/how-to-use-struct-tags-in-go
		  [3] https://qiita.com/ea_gitro/items/f6f9feadc24b428d1b24
		  [4] https://stackoverflow.com/questions/23507033/get-struct-field-tag-using-go-reflect-package
		  [5] https://www.mongodb.com/docs/drivers/go/current/usage-examples/struct-tagging/
		  
		  `sque` `json:"email"` meanning in real struct 
		  Struct Definition
		  ```go
		  type Response struct {
		      Name  string `json:"name"`
		      Age   int    `json:"age"`
		      Email string `json:"email"`
		  }
		  ```
		  The `json:"email"` part in the struct definition is known as a **struct tag**. Struct tags in Go provide metadata about the fields of the struct, which can be used by various packages for different purposes. In this case, the tag is used by the `encoding/json` package to specify the JSON key name that corresponds to the struct field when the struct is marshaled (converted) to JSON or unmarshaled (converted back) from JSON.
		  
		  Here’s a breakdown of the usage of `json:"email"`:
		  
		  @@@ Purpose of JSON Struct Tags
		  
		  1. **Custom JSON Key Names**: 
		     - The struct tag `json:"email"` indicates that the `Email` field should be represented as `"email"` in the JSON output.
		     - This allows the JSON keys to differ from the Go struct field names, which is useful if the JSON format requires different naming conventions (e.g., lowercase keys, snake_case, etc.).
		  
		  2. **Serialization and Deserialization**:
		     - When marshaling the struct to JSON, the `encoding/json` package uses the tag to determine the key name for the JSON object.
		     - When unmarshaling JSON into a struct, the `encoding/json` package uses the tag to map JSON keys to the appropriate struct fields.
		  
		  @@@ Example
		  
		  Consider the following struct definition:
		  ```go
		  type Response struct {
		      Name  string `json:"name"`
		      Age   int    `json:"age"`
		      Email string `json:"email"`
		  }
		  ```
		  
		  @@@ Marshalling Example
		  
		  When an instance of `Response` is marshaled to JSON:
		  ```go
		  resp := Response{
		      Name:  "Alice",
		      Age:   30,
		      Email: "alice@example.com",
		  }
		  
		  bs, _ := json.MarshalIndent(resp, "", "    ")
		  fmt.Println(string(bs))
		  ```
		  
		  The output will be a JSON string with keys as specified by the tags:
		  ```json
		  {
		      "name": "Alice",
		      "age": 30,
		      "email": "alice@example.com"
		  }
		  ```
		  
		  @@@ Unmarshalling Example
		  
		  When a JSON object is unmarshaled into a `Response` struct:
		  ```go
		  jsonStr := `{"name": "Bob", "age": 25, "email": "bob@example.com"}`
		  var resp Response
		  json.Unmarshal([]byte(jsonStr), &resp)
		  ```
		  
		  The `resp` struct will be populated with:
		  ```go
		  Response{
		      Name:  "Bob",
		      Age:   25,
		      Email: "bob@example.com",
		  }
		  ```
		  
		  @@@ Summary
		- **Struct Tag**: `json:"email"`
		- **Purpose**: To specify that the `Email` field in the Go struct should be represented as `"email"` in the JSON object.
		- **Benefits**: Ensures consistency between the struct fields and JSON keys, allows customization of JSON output, and aids in parsing JSON input correctly.
- ## 2024/6/22 15:43:57:
  
  (`package main`
  `package main`
  (`package main` (`package main`
- ## 2024/6/22 15:40:50:
  (`) dslfjld(`)
  `package main`)
  (`package main`)
  (`package main`
  `package main`
  (`package main` (`package main`
- ## 2024/6/22 15:26:13:
  @@ 2024/6/21 10:52:07:
	- #code 
	  #que go 语言 包的访问规则
	- #ans
	  不同目录中文件是不可以直接互相访问?
	  您的理解是正确的。在Go语言中,不同目录中的文件通常不能直接互相访问,除非它们属于同一个包。这是Go语言包管理和可见性规则的一部分。以下是一些关键点:
	  
	  1. 同一个包内的文件:
	     - 如果文件在同一个目录下,并且都属于同一个包(例如都声明为`package main`),它们可以直接互相访问定义的类型、变量和函数,无需导入。
	  
	  2. 不同包的文件:
	     - 如果文件在不同的目录下,或者属于不同的包,它们不能直接互相访问。
	     - 要访问其他包中的内容,需要使用`import`语句导入该包。
	  
	  3. 可见性规则:
	     - Go使用大小写来控制可见性。大写开头的标识符(如`Person`)是可导出的,可以被其他包访问。
	     - 小写开头的标识符(如`person`)是包私有的,只能在同一个包内访问。
	  
	  4. 项目结构:
	     - 通常,Go项目会将相关的功能组织在同一个包(目录)中。
	     - 不同的功能模块可以放在不同的包(目录)中,通过导入来使用。
	  
	  5. 导入路径:
	     - 导入其他包时,使用的是相对于项目根目录(通常是`$GOPATH/src`或模块根目录)的路径。
	  
	  例如,假设有以下项目结构:
	  
	  ```
	  myproject/
	      main.go
	      utils/
	          helper.go
	  ```
	  
	  在这种情况下:
		- `main.go`和`helper.go`不能直接互相访问,因为它们在不同的目录中。
		- 如果`main.go`需要使用`helper.go`中定义的内容,需要在`main.go`中导入`utils`包:
		  
		  ```go
		  import "myproject/utils"
		  ```
		  
		  这种设计促进了代码的模块化和封装,有助于管理大型项目的复杂性。
		  
		  Citations:
		  [1] https://github.com/golang/go/issues/44691
		  [2] https://pkg.go.dev/io/fs
		  [3] https://stackoverflow.com/questions/45429210/how-do-i-check-a-files-permissions-in-linux-using-go
		  [4] https://pkg.go.dev/os
		  [5] https://www.maths.cam.ac.uk/computing/linux/unixinfo/perms
		  
		  #sque 同属于 package main , 并且是同级文件夹
		  #ans
		  根据您的描述,这种情况确实是同属于package main,并且是同级文件夹的情况。在Go中,这是一种常见且有效的代码组织方式。以下是对这种情况的解释:
		  
		  1. 同属于package main:
		     在Go中,一个可执行程序的入口点必须是package main。通常,我们会在同一个目录下创建多个.go文件,它们都声明为package main[1][2].
		  
		  2. 同级文件夹:
		     这些文件位于同一个目录中。Go的包系统是基于目录的,而不是基于单个文件的[2][3].
		  
		  3. 编译行为:
		     当你运行go build或go run时,Go编译器会自动将同一目录下所有声明为package main的.go文件编译在一起[1][3].
		  
		  4. 代码组织:
		     这种方式允许你将大型的main包分割成多个文件,以提高代码的可读性和可维护性[2].
		  
		  5. 导入规则:
		     在这些文件之间,你不需要使用import语句。所有在同一个包(在这种情况下是main包)中定义的类型、函数和变量都可以直接使用[4].
		  
		  例如,你可能有以下文件结构:
		  
		  ```
		  myproject/
		      main.go
		      utils.go
		      handlers.go
		  ```
		  
		  这里的main.go、utils.go和handlers.go都声明为package main,它们可以直接相互访问彼此定义的函数和变量,无需导入。
		  
		  这种组织方式非常适合中小型项目。对于较大的项目,你可能需要考虑将某些功能拆分到单独的包中,以提高代码的模块化程度。
		  
		  Citations:
		  [1] https://www.reddit.com/r/golang/comments/18471go/declare_package_main_in_files_other_than_main_file/
		  [2] https://golangbot.com/go-packages/
		  [3] https://stackoverflow.com/questions/20427890/does-it-make-sense-to-have-two-packages-in-the-same-directory
		  [4] https://github.com/golang/go/issues/32504
		  [5] https://go.dev/ref/mod
		  
		  #sque Person 在同级的另一个文件
		  #ans
		  在Go中,如果你想在同一个包的不同文件中使用某个类型(如Person结构体),你不需要显式导入该类型,因为Go会自动把同一包中的所有类型、变量和函数视为可见的。
		  
		  举个例子,假设你有两个文件:
		  
		  person.go:
		  
		  ```go
		  package main
		  
		  type Person struct {
		      Name string
		      Age  int
		  }
		  ```
		  
		  main.go:
		  
		  ```go
		  package main
		  
		  import "fmt"
		  
		  func main() {
		      p := Person{
		          Name: "Alice",
		          Age:  30,
		      }
		      fmt.Printf("%+v\n", p)
		  }
		  ```
		  
		  在main.go中,你可以直接使用Person类型来创建实例,而不需要导入它,因为Person是在同一个main包中定义的。Go会在编译时自动把main包中的所有源文件合并处理。
		  
		  这种设计使得代码组织更加简洁,减少了不必要的导入,提高了可读性和可维护性。你只需要导入不同包中定义的类型。
		  
		  所以在Go中,通常的做法是按照逻辑将相关的类型、变量和函数放在同一个文件中,而不是为每个类型单独创建一个文件,除非该类型足够复杂,单独的文件能增加可读性。
		  
		  Citations:
		  [1] https://icarus.cs.weber.edu/~dab/cs1410/textbook/5.Structures/header.html
		  [2] https://courses.cs.washington.edu/courses/cse374/20au/lectures/12-structs-in-c/12-structs-in-c.pdf
		  [3] https://www.reddit.com/r/golang/comments/ydxzyi/is_it_ok_to_have_a_separate_file_for_each_type/
		  [4] https://stackoverflow.com/questions/3041797/how-to-use-a-defined-struct-from-another-source-file
		  [5] https://www.skillvertex.com/blog/read-write-structure-from-to-a-file-in-c/
- ## 2024/5/17 05:36:09:
	- #code
	  #que go what's the meaning of `bs, _ := json.MarshalIndent(resp, "", "    ")`
	- #ans
	  The line of code `bs, _ := json.MarshalIndent(resp, "", "    ")` is written in the Go programming language (Golang). Here's a breakdown of what it does:
	  
	  1. **`json.MarshalIndent`**: This function is part of the `encoding/json` package in Go. It is used to convert a Go object into a JSON string with indentation for easier readability.
	  
	  2. **`resp`**: This is the Go object that you want to convert to JSON. It could be any data structure that is supported by Go's JSON marshalling (e.g., struct, map, slice).
	  
	  3. **`""` (the second parameter)**: This parameter specifies the prefix to use for each JSON line. An empty string means no prefix.
	  
	  4. **`"    "` (the third parameter)**: This parameter specifies the indentation to use for each level of the JSON hierarchy. Here, it is set to four spaces.
	  
	  5. **`bs, _ := ...`**: This part assigns the result of `json.MarshalIndent` to two variables:
	     - `bs` is a byte slice (`[]byte`) that contains the JSON-encoded data.
	     - The underscore (`_`) is used to ignore the error value returned by `json.MarshalIndent`. This is common in Go when you are confident that there won't be an error or you simply want to ignore it.
	  
	  Putting it all together, this line of code converts the `resp` object into a pretty-printed JSON byte slice with an indentation of four spaces, ignoring any errors that might occur during the conversion.
	  
	  Here's an example of how this might look in a complete program:
	  
	  ```go
	  package main
	  
	  import (
	      "encoding/json"
	      "fmt"
	  )
	  
	  type Response struct {
	      Name  string `json:"name"`
	      Age   int    `json:"age"`
	      Email string `json:"email"`
	  }
	  
	  func main() {
	      resp := Response{
	          Name:  "Alice",
	          Age:   30,
	          Email: "alice@example.com",
	      }
	  
	      bs, _ := json.MarshalIndent(resp, "", "    ")
	      fmt.Println(string(bs))
	  }
	  ```
	  
	  In this example, `resp` is an instance of the `Response` struct. When `json.MarshalIndent` is called, it converts `resp` to a JSON-formatted byte slice with indented formatting. The resulting JSON string will look something like this:
	  
	  ```json
	  {
	      "name": "Alice",
	      "age": 30,
	      "email": "alice@example.com"
	  }
	  ```
	  
	  This JSON string is then printed to the console.
- ## 2024/5/17 04:50:16:
  (`djfsldfjslf`)
- ## 2024/5/17 04:50:04:
  `(dljd)`
- ## 2024/5/17 04:47:50:
  A struct tag in Go is a string literal that can be attached to a struct field to provide metadata or instructions for other packages that work with the struct. It is enclosed in backticks (`) and follows the field type declaration.
  
  @@ Struct Tag Syntax
  
  The general syntax for a struct tag is:
  
  ```go
  type StructName struct {
      FieldName Type `key1:"value1" key2:"value2"`
  }
  ```
	- `key1` and `key2` are the tag keys, which are strings that identify the purpose of the tag value.
	- `"value1"` and `"value2"` are the corresponding tag values associated with each key.
	  
	  Multiple tags can be added to a single field by separating them with a space.[1][2]
	  
	  @@ Common Use Cases
	  
	  Struct tags are widely used in the Go standard library and third-party packages for various purposes, such as:
	  
	  1. **JSON/XML Encoding**: The `encoding/json` and `encoding/xml` packages use struct tags to customize the mapping between struct fields and their JSON/XML representations.[2][3]
	  
	  2. **Database Mapping**: ORMs (Object-Relational Mapping) libraries like `gorm` and `xorm` use struct tags to map struct fields to database columns.[1]
	  
	  3. **Form Binding**: Web frameworks like `gin` and `echo` use struct tags to bind form data to struct fields.
	  
	  4. **Validation**: Validation packages like `validator` use struct tags to define validation rules for struct fields.
	  
	  @@ Accessing Struct Tags
	  
	  The `reflect` package in Go provides a way to access struct tags at runtime. The `StructTag` type represents a struct field's tag string and provides methods like `Get(key string)` to retrieve the value associated with a specific key.[4]
	  
	  Struct tags are a powerful feature in Go that allows developers to add metadata to struct fields, enabling better integration with various libraries and frameworks.
	  
	  Citations:
	  [1] https://go.dev/wiki/Well-known-struct-tags
	  [2] https://www.digitalocean.com/community/tutorials/how-to-use-struct-tags-in-go
	  [3] https://qiita.com/ea_gitro/items/f6f9feadc24b428d1b24
	  [4] https://stackoverflow.com/questions/23507033/get-struct-field-tag-using-go-reflect-package
	  [5] https://www.mongodb.com/docs/drivers/go/current/usage-examples/struct-tagging/
- ## 2024/5/16 17:59:44:
  ```
  sldfjslfjsdf
  sldfjslfjs;f
  ```
- ## 2024/5/13 04:22:00:
	- #url
	  #que url test
	- #ans
	  http://150.109.23.141
	  http://150.109.23.141/
	  http://150.109.23.141:6060
	  http://150.109.23.141:6060/
	  http://150.109.23.141:6060/v1/diary-log
	  https://github.com
	  https://github.com/qyzhizi
	  https://github.com/qyzhizi/python_logical/blob/main/2024-04-19-html-top-sticky/test.html#L17
	  (https://arxiv.org/pdf/1603.07285.pdf)
	  https://zh-v2.d2l.ai/chapter_computer-vision/transposed-conv.html
- ## 2024/5/13 04:13:31:
  http://150.109.23.141:6060/v1/diary-log
- ## 2024/5/10 22:58:02:
	- #ai #transformer
	  #que transformer Positional Encoding
	- #ans
	  Since our model contains no recurrence and no convolution, in order for the model to make use of the
	  order of the sequence, we must inject some information about the relative or absolute position of the
	  tokens in the sequence. To this end, we add "positional encodings" to the input embeddings at the
	  bottoms of the encoder and decoder stacks. The positional encodings have the same dimension dmodel
	  as the embeddings, so that the two can be summed. There are many choices of positional encodings,
	  learned and fixed [9].
	  In this work, we use sine and cosine functions of different frequencies:
	  $$PE_{(pos,2i)} = sin(pos/10000^{2i/d_{model}} )$$
	  $$PE_{(pos,2i+1)} = cos(pos/10000^{2i/d_{model}} )$$
	  where pos is the position and i is the dimension. That is, each dimension of the positional encoding
	  corresponds to a sinusoid. The wavelengths form a geometric progression from 2π to 10000 ·2π. We
	  chose this function because we hypothesized it would allow the model to easily learn to attend by
	  relative positions, since for any fixed offset k, PEpos+k can be represented as a linear function of
	  PEpos.
	  We also experimented with using learned positional embeddings [9] instead, and found that the two
	  versions produced nearly identical results (see Table 3 row (E)). We chose the sinusoidal version
	  because it may allow the model to extrapolate to sequence lengths longer than the ones encountered
	  during training
- ## 2024/5/10 22:28:02:
  $$slfjslfjs;$$   
  djfsljfdslfj;sfjjjjjjjjjjjjjjjjjjjjjjjjjjjjjjjjjjjjjjjjjjjjjjjjjjjjjjjjj
- ## 2024/5/10 07:19:03:
  `sdlfjsdlfj` `<sfjsdlfjsdl`
- ## 2024/5/10 03:37:45:
	- #code
	  #que 正则匹配 获得格式化内容
	- #ans
	  `2024-05-10更新后`
	  ```js
	  function matchPattern(input, Pattern, PatternType) {
	      let match;
	      let Matches = [];
	      let lastIndex = 0; // 用于跟踪上一个匹配项的结束位置
	  
	      // 搜集所有行间代码块及其位置
	      while ((match = Pattern.exec(input)) !== null) {
	          // 检查并添加前一个代码块后和当前代码块前的非代码块文本
	          if (match.index > lastIndex) {
	              Matches.push({
	                  type: 'text',
	                  content: input.substring(lastIndex, match.index),
	              });
	          }
	  
	          Matches.push({
	              type: PatternType,
	              content: match[1],
	          });
	  
	          // 更新lastIndex为当前代码块的结束位置
	          lastIndex = match.index + match[0].length;
	      }
	  
	      // 检查最后一个代码块后是否还有文本
	      if (lastIndex < input.length) {
	          Matches.push({
	              type: 'text',
	              content: input.substring(lastIndex),
	          });
	      }
	      return Matches;
	  }
	  
	  
	  function subMatchPattern(Matches, urlPattern, urlPatternType){
	      let tempMatches = [...Matches];
	      for(let i = 0, addItems = 0; i < tempMatches.length; i++){
	          let item  = tempMatches[i];
	          if (item.type == 'text'){
	              let PatternMatches = matchPattern(item.content, urlPattern, urlPatternType);
	              Matches.splice(i + addItems, 1, ...PatternMatches)
	              addItems = addItems + PatternMatches.length - 1
	          }
	      }
	  }
	  
	  
	  function mulMatchPattern(input, codeBlockLinesPattern, inlinePattern, otherPatterns){
	      let Matches = matchPattern(input, codeBlockLinesPattern.regex, codeBlockLinesPattern.type);
	      subMatchPattern(Matches, inlinePattern.regex, inlinePattern.type)
	      otherPatterns.forEach(element => {
	          subMatchPattern(Matches, element.regex, element.type)
	      }); 
	      return Matches
	  }
	    
	  const input = "sldfjsldfjsd;```111111111111111111```904https://abc.qyzhi.cn 65 $$a = b + c$$ 80,4`aaaaaa`0934https://x.com/deguang_li/status/1788026004990296432 85034`bbbbb``ccccc````2222222222222222````ddddd`05468045";
	  var Matches = [];
	  codeBlockLinesPattern = {regex:/```(?!`)([\s\S]+?)```/g, type: 'codeBlockBetweenLines'}
	  inlinePattern = {regex:/`([^`]+)`/g, type: 'inLinecodeBlock'}
	  otherPatterns = [
	      {regex:/(https?:\/\/(?:www\.)?[-a-zA-Z0-9@:%._\+~#=]{1,256}\.[a-z]{2,6}\b(?:[-a-zA-Z0-9@:%_\+.~#?&/=]*))/g, type: 'url'},
	      {regex:/(?:\s|\r?\n)*?\$\$([\s\S]*?)\$\$(?:\s|\r?\n)*?/g, type: 'MulLineslatex'}
	  ]
	  Matches = mulMatchPattern(input, codeBlockLinesPattern, inlinePattern, otherPatterns)
	  console.log(Matches)
	  
	  
	  ```
	  ********************************分割线****************************
	  `old` :
	  ```js
	  function matchPattern(input, Pattern, PatternType) {
	      // const blockPattern = /```(?!`)([\s\S]+?)```/g;
	      let match;
	      let Matches = [];
	      let lastIndex = 0; // 用于跟踪上一个匹配项的结束位置
	  
	      // 搜集所有行间代码块及其位置
	      while ((match = Pattern.exec(input)) !== null) {
	          // 检查并添加前一个代码块后和当前代码块前的非代码块文本
	          if (match.index > lastIndex) {
	              Matches.push({
	                  type: 'text',
	                  content: input.substring(lastIndex, match.index),
	                  // originStart: lastIndex,
	                  // originEnd: match.index - 1,
	              });
	          }
	  
	          Matches.push({
	              type: PatternType,
	              content: match[1],
	              // originStart: match.index,
	              // originEnd: match.index + match[0].length - 1,
	          });
	  
	          // 更新lastIndex为当前代码块的结束位置
	          lastIndex = match.index + match[0].length;
	      }
	  
	      // 检查最后一个代码块后是否还有文本
	      if (lastIndex < input.length) {
	          Matches.push({
	              type: 'text',
	              content: input.substring(lastIndex),
	              // originStart: lastIndex,
	              // originEnd: input.length - 1,
	          });
	      }
	  
	      return Matches;
	  }
	  
	  function subMatchPattern(Matches, urlPattern, urlPatternType){
	      let tempMatches = [...Matches];
	      for(let i = 0, addItems = 0; i < tempMatches.length; i++){
	          let item  = tempMatches[i];
	          if (item.type == 'text'){
	              let PatternMatches = matchPattern(item.content, urlPattern, urlPatternType);
	              // Matches[i] = inlineMatches
	              
	              Matches.splice(i + addItems, 1, ...PatternMatches)
	              addItems = addItems + PatternMatches.length - 1
	          }
	      }
	      return Matches 
	  }
	  
	  // function mulMatchPattern(input, Pattern){
	  //     latexPattern = Pattern.latex;
	  //     type = Pattern.type;
	  
	  // }
	    
	  const input = "sldfjsldfjsd;```111111111111111111```904https://abc.qyzhi.cn 65 $$a = b + c$$ 80,4`aaaaaa`0934https://x.com/deguang_li/status/1788026004990296432 85034`bbbbb``ccccc````2222222222222222````ddddd`05468045";
	  var Matches = [];
	  
	  const codeBlockLinesPattern = /```(?!`)([\s\S]+?)```/g;
	  const blockPatternType = 'codeBlockBetweenLines';
	  Matches = matchPattern(input, codeBlockLinesPattern, blockPatternType);
	  console.log(Matches)
	  
	  console.log("******************************************************")
	  const inlinePattern = /`([^`]+)`/g;
	  const inlinePatternType = 'inLinecodeBlock';
	  Matches = subMatchPattern(Matches, inlinePattern, inlinePatternType)
	  console.log(Matches)
	  
	  console.log("******************************************************")
	  const urlPattern = /(https?:\/\/(?:www\.)?[-a-zA-Z0-9@:%._\+~#=]{1,256}\.[a-z]{2,6}\b(?:[-a-zA-Z0-9@:%_\+.~#?&/=]*))/g;
	  const urlPatternType = 'url';
	  Matches = subMatchPattern(Matches, urlPattern, urlPatternType)
	  console.log(Matches)
	  
	  
	  console.log("******************************************************")
	  const latexPattern = /(?:\s|\r?\n)*?\$\$([\s\S]*?)\$\$(?:\s|\r?\n)*?/g;
	  const latexPatternType = 'MulLineslatex';
	  Matches = subMatchPattern(Matches, latexPattern, latexPatternType)
	  console.log(Matches)
	  
	  ```
- ## 2024/5/10 07:01:54:
  ********************************分割线****************************
  `old` :
- ## 2024/5/10 06:19:01:
	- #code 
	  #que js 函数传参 有引用传递吗?
	- #ans
	  `aaaaaaaaaaaaa`
	  ```
	    sdjfslfjs
	    sldfjslfjsd;
	  ```
	  https://abc.qyzhizi.cn lsdjfsl $a = b = c $
	  $$a = b + c$$
	  让我们通过一个例子来说明这个概念：
	  ```
	  `sdjfslfjslf` `jj`  sldjflsfj `
	  ```
	  
	  ```javascript
	  function modifyArray(arr) {
	    arr.push(4);
	  }
	  
	  const myArray = [1, 2, 3];
	  modifyArray(myArray);
	  console.log(myArray); // 输出 [1, 2, 3, 4]
	  ```
	  dlsjfslfj
- ## 2024/5/9 17:03:27:
  chatGPT
  https://chat.openai.com/share/eab1707a-9e45-4723-8474-8c10d811be56
  ```
  function identifyElements(inputString) {
      const codeBlockRegex = /```(.*?)```/gs; // 匹配代码块
      const inlineCodeRegex = /(?<!``)`([^`]+)`/g; // 匹配行内代码
      const urlRegex = /(https?:\/\/[^\s]+)/g; // 匹配URL
    
      let codeBlockList = [];
      let inlineBlockList = [];
      let urlList = [];
      let textList = [];
    
      // 匹配代码块
      let match;
      while ((match = codeBlockRegex.exec(inputString)) !== null) {
          console.log(match[0].length, match[1].length)
          codeBlockList.push([match.index, match.index + match[0].length - 1]);
      }
      console.log(codeBlockList)
    
      // 从代码块之外匹配行内代码
      // let codeBlockPositions = codeBlockList.flat();
      // console.log(codeBlockList)
      codeBlockList.forEach(pos => {
          const startPos = pos[0];
          const endPos = pos[1];
          // 在这里处理每个代码块
          console.log(`Code block start position: ${startPos}`);
          console.log(`Code block end position: ${endPos}`);
      });
  
      let lastMatchEndIndex = 0;
      while ((match = inlineCodeRegex.exec(inputString)) !== null) {
          if (!codeBlockList.some(pos => match.index >= pos[0] && match.index <= pos[1]) && match.index > lastMatchEndIndex) {
  
              console.log(match.index, match.index + match[0].length - 1)
              inlineBlockList.push([match.index, match.index + match[0].length - 1]);
              lastMatchEndIndex = match.index + match[0].length - 1;
          }
      }
    
      // 匹配URL
      while ((match = urlRegex.exec(inputString)) !== null) {
          urlList.push([match.index, match.index + match[0].length - 1]);
      }
    
      // 提取文本
      let lastIndex = 0;
      for (let i = 0; i < codeBlockList.length || i < inlineBlockList.length || i < urlList.length; i++) {
          let minIndex = Math.min(
              codeBlockList[i] ? codeBlockList[i][0] : Infinity,
              inlineBlockList[i] ? inlineBlockList[i][0] : Infinity,
              urlList[i] ? urlList[i][0] : Infinity
          );
    
          textList.push([lastIndex, minIndex - 1]);
          lastIndex = minIndex;
      }
    
      textList.push([lastIndex, inputString.length - 1]);
    
      return {
          codeBlockList,
          inlineBlockList,
          urlList,
          textList
      };
  }
    
  function getContent(inputString, positions) {
      return positions.map(pos => inputString.substring(pos[0], pos[1] + 1));
  }
    
  const inputString = "```12对了师`傅js`ljfslfdjs3```串方法,https://www.fukonglzw.cn:5051/ 用于`dslfjs``sdfjsdldfjs`查找";
  console.log(inputString)
  const elements = identifyElements(inputString);
  console.log("Code Block Content:", getContent(inputString, elements.codeBlockList));
  console.log("Inline Code Content:", getContent(inputString, elements.inlineBlockList));
  console.log("URL Content:", getContent(inputString, elements.urlList));
  console.log("Text Content:", getContent(inputString, elements.textList));
  ```
- ## 2024/5/9 16:47:07:
  `@@` sdjfslfjsd;f
- ## 2024/5/9 16:14:31:
    #正则匹配
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