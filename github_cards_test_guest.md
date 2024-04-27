## 2024/4/27 15:25:18:
	- #math
	  #que 深度学中 2维矩阵乘法, 举个例子
	- #ans
	  当你在深度学习中使用 2 维矩阵乘法时，通常是在进行神经网络的前向传播或反向传播过程中。这个操作在线性代数中被称为矩阵乘法或者矩阵点积。
	  
	  假设你有两个矩阵 A 和 B，它们的形状分别为 (m, n) 和 (n, p)。要将这两个矩阵相乘，确保 A 的列数与 B 的行数相同，结果矩阵的形状将是 (m, p)。
	  
	  下面是一个简单的例子：
	  
	  假设我们有以下两个矩阵：
	  
	  \[ A = \begin{bmatrix} 1 & 2 \\ 3 & 4 \end{bmatrix} \]
	  
	  \[ B = \begin{bmatrix} 5 & 6 \\ 7 & 8 \end{bmatrix} \]
	  
	  要计算它们的乘积，我们按照矩阵乘法的规则：
	  
	  \[ C = A \times B \]
	  
	  \[ C_{ij} = \sum_{k=1}^{n} A_{ik} \times B_{kj} \]
	  
	  其中，\( C_{ij} \) 是结果矩阵 C 的第 i 行第 j 列的元素，\( A_{ik} \) 是矩阵 A 的第 i 行第 k 列的元素，\( B_{kj} \) 是矩阵 B 的第 k 行第 j 列的元素。
	  
	  现在我们计算每个元素的值：
	  
	  \[ C_{11} = (1 \times 5) + (2 \times 7) = 5 + 14 = 19 \]
	  
	  \[ C_{12} = (1 \times 6) + (2 \times 8) = 6 + 16 = 22 \]
	  
	  \[ C_{21} = (3 \times 5) + (4 \times 7) = 15 + 28 = 43 \]
	  
	  \[ C_{22} = (3 \times 6) + (4 \times 8) = 18 + 32 = 50 \]
	  
	  所以，结果矩阵 C 为：
	  
	  \[ C = \begin{bmatrix} 19 & 22 \\ 43 & 50 \end{bmatrix} \]
	  
	  这就是矩阵 A 和 B 的乘积。
- ## 2024/4/27 13:30:03:
	- #code #向量数据库
	  #que chroma 向量数据库
	- #ans
	  chroma 使用 sqlite3 存储数据，因此它是内存数据库
	  
	  https://docs.trychroma.com/embeddings#sentence-transformers
	  
	  向量数据库选型： https://guangzhengli.com/blog/zh/vector-database/#%E5%90%91%E9%87%8F%E6%95%B0%E6%8D%AE%E5%BA%93%E9%80%89%E5%9E%8B
- ## 2024/4/27 13:27:44:
	- #twitter
	  #que 吴恩达 twitter
	- #ans
	  https://twitter.com/AndrewYNg
	  https://www.deeplearning.ai/the-batch/issue-245/
- ## 2024/4/18 00:11:56:
  你好，
- ## 2024/3/24 11:00:05:
	- #web
	  #que memoflow demo
	- #ans
	  注册
	  http://150.109.23.141:6060/v1/diary-log/register
	  
	  客户端， 用户名，密码都是 guest
	  http://150.109.23.141:6060/v1/diary-log/login
	  
	  GitHub 同步文件：
	  https://github.com/qyzhizi/MemoFlowGuest/blob/main/github_cards_test_guest.md
- ## 2024/3/24 10:56:54:
	- #code
	  #que memoflow 添加 latex 支持
	- #ans
	   公式使用 `$...$` 或者 `$$...$$` 来标识， 然后点击右侧下拉按钮 `latexView` 即可展示， 注意标识前面要有空格或者换行，否者不会解析为公式
	  例如：
	  Here is a LaTeX equation: $$\int_{-\infty}^{\infty} e^{-x^2} \, dx = \sqrt{\pi}$$
	  $$\int_{-\infty}^{\infty} e^{-x^2} \, dx = \sqrt{\pi}$$  
	  $$\int_{a}^{b} x^2 \,dx = \frac{1}{3}x^3 \Big|_{a}^{b}$$
	  $\int_{a}^{b} x^2 \,dx = \frac{1}{3}x^3 \Big|_{a}^{b}$
	  
	  commit:
	  https://github.com/qyzhizi/MemoFlow/commit/5204630cb9c884b9303e74e748e352181f7dc790