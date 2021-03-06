1.
```c
void triangle_judge(int a, int b, int c) {
	int c1 = a + b > c;
	int c2 = a + c > b;
	int c3 = b + c > a;
	int c4 = a == b;
	int c5 = a == c;
	int c6 = b == c;
	if (!c1 || !c2 || ! c3) {
		printf("Not Triangle\n");
		return;
	}
	if (c4 && c5) {
		printf("Equilateral Triangle\n");
		return;
	}
	if (c4 || c5 || c6) {
		printf("Isosceles Triangle\n");
		return;
	}
	printf("Trivial Triangle\n");
}
```
此题要注意逻辑判断的先后次序；应设计多组测试用例，以覆盖所有的条件分支语句。


2.
```c
void print_hex(int a) {
	static char ntoc[] = {'0', '1', '2', '3', '4',
	                      '5', '6', '7', '8', '9',
	                      'A', 'B', 'C', 'D', 'E', 'F'
	                     };
	if (a == 0)
		return;
	print_hex(a / 16);
	printf("%c", ntoc[a % 16]);
}
```
此题我使用了递归的方法，代码实现较为简洁。你也可以使用循环（即迭代）的方法来实现。

此题我用static关键字来定义了一个静态数组。static关键字与静态变量是C语言的重要内容。你可以阅读教材或查阅网上的资料来了解它的使用方法，以下提供了几份讲解供参考：

https://baike.baidu.com/item/static/9598919?fr=aladdin#2
http://blog.csdn.net/pengpengblog/article/details/52874580
http://www.cnblogs.com/getyoulove/p/3656184.html


3.
```c
int is_pow2(int n) {
	return n & (n - 1) ? -1 : 0;
}
```
若n为2的幂次方，则n的二进制形式为1000…（忽略前导的0），而n-1的二进制形式为0111…（忽略前导的0），故n & (n - 1)必为0。


4.
```c
int count1_in_bin(int n) {
	int cnt = 0;
	for ( ; n; n >>= 1)
		if (n & 1)
			++cnt;
	return cnt;
}
```
逐步取出n的二进制表示中的每一位，对为1的位进行计数即可。使用这种方法，for循环要迭代执行sizeof(int) * 8 - x次，其中x为n的二进制表示中前导的0的个数。例如，当n为13时，n的二进制表示为1101（忽略前导的0），则要迭代执行4次。

下面提供一个更高效的方法：
```c
int quick_count1_in_bin(int n) {
	int cnt = 0;
	for ( ; n; n = n & (n - 1))
		++cnt;
	return cnt;
}
```
这种方法汲取了第3题的思想。在每次迭代中，n被赋值为n & (n - 1)，即，直接消除了n的二进制表示中最右边的1。举例来说，当n为45时，n的二进制表示为101101（忽略前导的0）。在第一次迭代后，n的值更新为了101100；在第二次迭代后，n的值更新为了101000；在第三次迭代后，n的值更新为了100000；在第四次迭代后，n的值更新为了0，循环结束。使用这种方法，for循环只需迭代执行x次，其中，x为n的二进制表示中1的个数。


5.
```c
int fac_bit_count(int n) {
	double res = 1.0;
	int i;
	for (i = 2; i <= n; ++i)
		res += log10(i);
	return (int)res;
}
```
若先求出n的阶乘，再计算其位数，这种方法显然是不可行的，因为当n略大时，n!会超出int所能表示的范围，即时用更宽的数据类型，如long int和long long也无济于事。因此要换个思路。

我们知道，整数n的位数，其实就是⌊𝑙𝑔(n)⌋+1，其中⌊⌋为取下整。而n!=1∗2∗…∗n，则n!的位数为⌊𝑙𝑔(n!)⌋+1。


6.
```c
int is_prime(int n) {
	int i;
	if (n < 2)
		return -1;
	if (n == 2)
		return 0;
	if ((n & 1) == 0)
		return -1;
	for (i = 3; i * i <= n; i += 2) {
		if (n % i == 0)
			return -1;
	}
	return 0;
}
```
注意程序的健壮性，要充分考虑各种可能的输入数据。

先判断n是否为偶数，之后可以让迭代的步长为2，这样做更高效。

主函数部分略。


7.

递归版：
```c
int GCD_recursive(int m, int n) {
	return n == 0 ? m : GCD_recursive(n, m % n);
}
```
循环（迭代）版：
```c
int GCD_iterative(int m, int n) {
	int r;
	for (r = m % n; r; r = m % n) {
		m = n;
		n = r;
	}
	return n;
}
```

8.
```c
int LCM(int m, int n) {
	return m / GCD_recursive(m, n) * n; // c1
}
```
对于m，n，有m * n = GCD(m, n) * LCM(m, n)。

注意，c1处若写为：
```c
return m * n / GCD_recursive(m, n);
```
则m * n有溢出的风险。


9.
```c
int is_int_pal(int n) {
	int m = n, rn = 0;
	while (m != 0) {
		rn = rn * 10 + m % 10;
		m /= 10;
	}
	return rn == n ? 0 : -1;
}
```
将n翻转为rn，再与n进行比较即可。


10.
```c
int is_str_pal(const char* str) {
	if (str == NULL) /* assert also do */
		return -1;
	int i, j;
	for (i = 0, j = (int)strlen(str) - 1; i < j; ++i, --j)
		if (str[i] != str[j])
			return -1;
	return 0;
}
```
注意程序的健壮性，要充分考虑各种可能的输入数据。即，有必要首先指针str是否为空。也可以使用assert断言来判断，即：
```c
assert(str != NULL);
```
关于assert断言的讲解，可参见[百度百科](https://baike.baidu.com/item/assert/10931289?fr=aladdin)


11.
```c
void reverse(char* str) {
	if (str == NULL)
		return;
	int i = 0, j = strlen(str) - 1;
	char temp;
	for ( ; i < j; ++i, --j) {
		temp = str[i];
		str[i] = str[j];
		str[j] = temp;
	}
}
```

12.
```c
void matrix_product(int mat1[][K], int mat2[][N], int product[][N]) {
	int i, j, k;
	memset(product, 0, sizeof(int) * M * N); // zero product
	for (i = 0; i < M; ++i)
		for (j = 0; j < N; ++j)
			for (k = 0; k < K; ++k)
				product[i][j] += mat1[i][k] * mat2[k][j];
}
```
我用到了memset函数，来将product矩阵清零。关于memset的讲解，可参见[百度百科](https://baike.baidu.com/item/memset/4747579?fr=aladdin)


13.
```c
void matrix_transpose(int mat[][N], int transposed[][M]) {
	int i, j;
	for (i = 0; i < N; ++i)
		for (j = 0; j < M; ++j)
			transposed[i][j] = mat[j][i];
}
```
