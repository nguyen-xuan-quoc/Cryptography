# Lattices và ứng dụng trong mật mã

# Vectors

Vector là một đoạn thẳng có hướng. Đoạn thẳng này biểu thị phương, chiều, độ lớn (chiều dài của vectơ). Ví dụ trong mặt phẳng cho hai điểm phân biệt A và B bất kì ta có thể xác định được vectơ $\vec AB$.

Ví dụ:

Một đường thẳng có hướng từ điểm P(x1, x2) đến điểm Q(y1, y2) là một vectơ có các thành phần sau:  $\overrightarrow{PQ}$ = $\overrightarrow{OS}$ = (s1,s2) = (y1 −x1, y2 −x2 ).

Điểm ban đầu của vectơ $\overrightarrow{OP}$ = (x1, x2) là gốc O = (0,0) và điểm cuối là P = (x1,x2)

Hãy biểu diễn các vectơ $\overrightarrow{PQ}$ và $\overrightarrow{PQ}$ có ba điểm P(0, 1), Q(2, 2) và R(1.5, 1.5)

![](https://i.imgur.com/KocnqaS.png)


Chúng ta có thể dễ dàng làm điều đó bằng cách làm theo định nghĩa:

$\overrightarrow{PQ}$ = (2 −0, 2 −1) = (2, 1)

$\overrightarrow{RQ}$ = (2 −1.5, 2 −1.5) = (0.5, 0.5)

Vận dụng trong sagemath:

```python
sage: OP = vector([0,1])
sage: OQ = vector([2,2])
sage: OR = vector([1.5,1.5])
sage: PQ = OQ - OP
sage: RQ = OQ - OR
sage: PQ
(2, 1)
sage: RQ
(0.500000000000000, 0.500000000000000)
```

Chúng ta có thể nhận ra: $\overrightarrow{PQ}$ = (2, 1) có nghĩa là nếu chúng ta bắt đầu di chuyển từ điểm P và di chuyển sang phải (theo chiều dương trục x) 2 lần và đi lên (theo chiều dương trục y) 1 lần, thì chúng ta sẽ đến điểm Q. Theo cách hiểu tương tự, $\overrightarrow{RQ}$ = (0.5, 0.5 ) có nghĩa là nếu chúng ta bắt đầu di chuyển từ điểm R và di chuyển 0,5 về bên phải và 0,5 lên trên, chúng ta sẽ đến điểm Q.

Với bất kì vector:

$$
x =  \begin{bmatrix}
   x1 \\
   y1 
\end{bmatrix} và \ y =  \begin{bmatrix}
   x2 \\
   y2 
\end{bmatrix} thuộc \ R^2  
$$

và một số nguyên k, tổng của x + y và tích k.x sẽ được tính như sau:

$$
x + y =  \begin{bmatrix}
   x1+x2 \\
   y1+y2 
\end{bmatrix} và \ kx =  \begin{bmatrix}
   k.x1 \\
   k.y1 
\end{bmatrix} thuộc \ R^2  
$$

**Định nghĩa**: Vectơ 0 là vectơ có tất cả các thành phần của nó bằng 0 (điểm đầu của nó được lấy làm gốc tọa độ)

Một n-bộ số (thực) có thứ tự (x1, x2 , ..., xn) được gọi là một vectơ n chiều và có thể được viết là:

$$
x = (x1,x2,x3,...,xn) =  \begin{bmatrix}
   x1 \\
   y2 \\ 
   x3 \\
   ... \\
   xn \\
\end{bmatrix}
$$

Ta gọi x1, x2, … là các phần tử của x.

Phép cộng và phép nhân một số vô hướng với vector nhiều chiều tương tự như cách thực hiện với vector 2 chiều ở trên.

***Ví dụ minh họa***: 

Time for the flag! Given a three dimensional vector space defined over the reals, where `v = (2,6,3)`, `w = (1,0,0)` and `u = (7,7,2)`, calculate `3*(2*v - w) ∙ 2*u`

### Solution:

```python
sage: v = vector((2,6,3))
....: w = vector((1,0,0))
....: u = vector((7,7,2))
sage: (3*(2*v - w)).dot_product(2*u)
702
```

**Định nghĩa:** Cho các vector `v1, v2, …, vk` thuộc không gian $R^n$ và các số bất kì `c1, c2, …, ck`

$$
x = c1.v1 + c2.v2 + ... + ck.vk
$$

được gọi là tổ hợp tuyến tính của các vector `v1, v2, …, vk`

**Định nghĩa:** Cho một vectơ x = (x1, x2, ..., xn) trong $R^n$

$$
\vert v\vert=\sqrt{(x1)^2+(x2)^2+...+(xn)^2}
$$

được gọi là chuẩn (hay độ dài) của vector x.

Ví dụ:

```python
sage: v = vector([3,6,2])
sage: v.norm() # square root of (9 + 36 + 4)
7
```

**Định nghĩa:** Phép nhân vô hướng giữa hai vector [x1, y1, z1] và [x2, y2, z2] được thực hiện như sau: 

$$
[x_1,y_1,z_1]\cdot[x_2,y_2,z_2]=x_1x_2+y_1y_2+z_1z_2
$$

áp dụng tương tự với các vector nhiều hướng hoặc ít hướng hơn.

**Định nghĩa:** Nếu x · y = 0 thì x trực giao với y. Nếu x là bội số vô hướng của y thì x song song với y.

Ví dụ về phép nhân vô hướng giữa 2 vector:

```python
sage: x = vector([5,4,1,3])
sage: y = vector([6,1,2,3])
sage: x*y
45
sage: x.dot_product(y)
45
```

Đối với một vectơ tùy ý, khác vector 0,  v ∈ $R^n$, u = (1 / ‖v‖) · v là một vector đơn vị. Trong Rn, vector đơn vị có dạng:

$e1 = (1,0,0,...,0),e2 = (0,1,0,...,0),...,en = (0,0,0,...,1)$

được gọi là vector đơn vị chuẩn hoặc vector tọa độ.

Định nghĩa trước cho phép chúng ta đưa ra nhận xét sau: Nếu x = (x1,x2,...,xn) là một vectơ tùy ý thuộc không gian $R^n$, sử dụng các vectơ đơn vị chuẩn, chúng ta có thể biểu diễn x như sau:

$$
x = x1.e1 + x2.e2 + ... + xn.en
$$

# Không gian vectors

Một vectơ được định nghĩa qua trường F là một tập V cùng với 2 toán tử thỏa mãn 8 tiên đề dưới đây. Theo đó, *V* × *V* kí hiệu cho phép nhân Cartesian của V với chính nó, và → kí hiệu cho một ánh xạ từ một nhóm đến một nhóm khác

- Toán tử đầu tiên, được gọi là **phép cộng vectơ** hoặc đơn giản là phép cộng +: *V* × *V* → *V,* lấy 2 vectơ bất kì **v** và **w** và đánh dấu một vectơ thứ 3 được viết là **v** + **w,** được gọi là tổng của các vectơ.
- Toán tử thứ 2 được gọi là **[phép nhân vô hướng](https://vi.wikipedia.org/wiki/Ph%C3%A9p_nh%C3%A2n_v%C3%B4_h%C6%B0%E1%BB%9Bng)**: *F* × *V* → *V,* lấy một vô hướng *a* bất kì và một vectơ **v**, cho ta một vectơ khác *a***v**
    1. Phép cộng vectơ có tính kết hợp:
        - Với mọi u, v, w ∈ V, ta có u + (v + w) = (u + v) + w.
    2. Phép cộng vectơ có tính giao hoán:
        - Với mọi v, w ∈ V, ta có v + w = w + v.
    3. Phép cộng vectơ có phần tử trung hòa:
        - Có một phần tử 0 ∈ V, gọi là vectơ không, sao cho v + 0 = v với mọi v ∈ V.
    4. Phép cộng vectơ có phần tử đối:
        - Với mọi v ∈ V, có một phần tử w ∈ V, gọi là phần ngược của v, sao cho v + w = 0.
    5. Phép nhân vô hướng phân phối với phép cộng vectơ:
        - Với mọi a ∈ F và v, w ∈ V, ta có a (v + w) = a v + a w.
    6. Phép nhân vô hướng phân phối với phép cộng vô hướng:
        - Với mọi a, b ∈ F và v ∈ V, ta có (a + b) v = a v + b v.
    7. Phép nhân vô hướng tương thích với phép nhân trong trường các số vô hướng:
        - Với mọi a, b ∈ F và v ∈ V, ta có a (b v) = (ab) v.
    8. Phần tử đơn vị của trường F có tính chất của phần tử đơn vị với phép nhân vô hướng: Với mọi v ∈ V, ta có 1 v = v, 1 ký hiệu đơn vị của phép nhân trong F.
    9. Với mọi x; y ∈ V, ta có x + y ∈ V
    10. Với mọi x ∈ V và a ∈ V, ta có a.x ∈ V

Ví dụ: Hãy xác định tập hợp M của tất cả các ma trận 2 × 2 với các mục nhập là số thực. Ta có

$$
\begin{bmatrix}
   a1&a2 \\
   a3&a4 
\end{bmatrix} +\ \begin{bmatrix}
   b1&b2 \\
   b3&b4
\end{bmatrix} = \begin{bmatrix}
   a1+b1&a2+b2 \\
   a3+b3&a4+b4
\end{bmatrix}
$$

và 

$$
k\begin{bmatrix}
   a1&a2 \\
   a3&a4 
\end{bmatrix} = \begin{bmatrix}
   ka1&ka2 \\
   ka3&ka4 
\end{bmatrix}
$$

Ta có thể dễ dàng nhận thấy đây là 1 không gian vector, trong đó vector 0 là 

$\begin{bmatrix}
0&0 \\
0&0
\end{bmatrix}$

Cách chứng minh tương tự cũng sẽ giúp ta chứng minh tập hợp các đa thức với hệ số thực là một không gian vector.

## Size and Basis

Ta nói tập hợp các vectơ v1, v2, ..., vk ∈ không gian V độc lập tuyến tính nếu nghiệm duy nhất của phương trình:

```python
a1*v1 + a2*v2 + ... + ak*vk = 0
```

là a1 = a2 = ... = ak = 0.

Cơ sở là một tập hợp các vectơ độc lập tuyến tính v1, v2, ..., vn ∈ V sao cho bất kỳ vectơ w ∈ V nào cũng có thể được viết dưới dạng:

```python
w = a1*v1 + a2*v2 + ... + ak*vn
```

Số phần tử trong cơ sở cũng chính là số chiều của không gian vectơ.

Chúng ta xác định kích thước của một vectơ, ký hiệu là ||v||, sử dụng tích bên trong của vectơ với chính nó: ||v||^2 = v ∙ v.

Một cơ sở là trực giao nếu với một cơ sở vectơ v1, v2, ..., vn ∈ V, tích trong giữa hai vectơ khác nhau bất kỳ bằng 0: vi ∙ vj = 0, i ≠ j.

Một cơ sở là trực giao nếu nó trực giao và ||vi|| = 1, với mọi i..

# **Lattices**

Cho tập các vectơ độc lập tuyến tính v1, v2, ..., vn ∈ Rm, mạng L sinh bởi v1, v2, ..., vn là tập các vectơ độc lập tuyến tính v1, v2, ..., vn với hệ số nguyên.

```python
L = {a1*v1 + a2*v2 + ... + ak*vk : a1, a2, ..., an ∈ Z}.
```

Cơ sở của mạng L là bất kỳ tập hợp các vectơ độc lập tuyến tính nào tạo ra L. Việc lựa chọn cơ sở không phải là duy nhất. Trong hình bên dưới, chúng tôi hiển thị một mạng hai chiều với hai vectơ cơ sở khác nhau được cho bởi u1, u2 và v1, v2

![https://cryptohack.org/static/img/lattice.svg](https://cryptohack.org/static/img/lattice.svg)

Sử dụng một cơ sở, chúng ta có thể đạt đến bất kỳ điểm nào trong mạng với bội số nguyên của các vectơ cơ sở. Các vectơ cơ sở cũng xác định miền cơ bản:

```python
F(v1,...,vn) = {t1 v1 + t2 v2 + ... + tn vn : 0 ≤ ti < 1}.
```

Như một ví dụ hai chiều, miền cơ bản là hình bình hành với các cạnh u1 và u2.

Ta có thể tính thể tích của miền cơ bản từ các vectơ cơ sở. Ví dụ, chúng ta hãy lấy một mạng hai chiều với các vectơ cơ sở v = (2,5), u = (3,1). Tạo ma trận A với các hàng tương ứng với các vectơ cơ sở:

$$
A = \begin{bmatrix}
2&5 \\
3&1
\end{bmatrix}
$$

Thể tích của miền cơ bản là độ lớn của định thức của 

A: Vol(F) = |det(A)| = |2.1 *- 5.*3| = |-13| = 13

Cách thể xây dựng một lattice bằng SageMath:

```python
sage: M = matrix(ZZ, [[1,2], [-1,1]])
sage: M
[ 1  2]
[-1  1]
```

Bây giờ, chúng ta có thể dễ dàng kiểm tra xem một điểm đã cho (z1,z2) có thuộc mạng hay không. Nếu (z1, z2) ∈ L thì nó thuộc khoảng (span) của L.

```python
sage: vector([1,1]) in span(M)
False
sage: vector([1,2]) in span(M) # true because [1,2]=1*x+0*y
True
sage: vector([-1,2]) in span(M)
False
sage: vector([-101,5]) in span(M)
True
```

Nếu chúng ta muốn xem tổ hợp tuyến tính chính xác tạo ra điểm (z1,z2) thì sao?

Điều này có nghĩa là chúng tôi muốn biết làm thế nào điểm z này được xây dựng duy nhất bởi các vectơ cơ sở đã cho  x = [1, 2] và y = [-1, 1] (Vẫn dùng lattice ở trên)

```python
sage: M.solve_left(vector([-101,5]))
(-32, 69)
```

Ta có thể kiểm tra lại:

```python
sage: -32*M[0] + 69*M[1]
(-101, 5)
```

## Gaussian Reduction

Nếu bạn nhìn đủ kỹ, các lattice bắt đầu xuất hiện ở mọi nơi trong mật mã. Đôi khi chúng xuất hiện thông qua việc thao túng hệ thống mật mã, phá vỡ các tham số không được tạo đủ an toàn. Ví dụ nổi tiếng nhất về điều này là cuộc tấn công của Coppersmith chống lại mật mã RSA.

Lattice cũng có thể được sử dụng để xây dựng các giao thức mật mã, có tính bảo mật dựa trên hai vấn đề "khó" cơ bản: 

Bài toán vectơ ngắn nhất - The **Shortest Vector Problem** (SVP): tìm vectơ khác 0 ngắn nhất trong mạng L. Nói cách khác, tìm vectơ khác 0 trong v ∈ L sao cho |v| là nhỏ nhất.

Bài toán vectơ gần nhất (CVP) - The **Closest Vector Problem**: Cho vectơ w ∈ $R^m$ không thuộc L, tìm vectơ v ∈ L gần w nhất, tức là tìm vectơ v ∈ L sao cho |v - w| được giảm thiểu.

SVP khó đối với một mạng chung, nhưng đối với các trường hợp đủ đơn giản, có các thuật toán hiệu quả để tính toán giải pháp hoặc phép tính gần đúng cho SVP. Khi kích thước của mạng là bốn hoặc ít hơn, chúng ta có thể tính toán chính xác điều này trong thời gian đa thức; đối với các kích thước cao hơn, chúng ta phải giải quyết một xấp xỉ.

Gauss đã phát triển thuật toán của mình để tìm cơ sở tối ưu cho mạng hai chiều với cơ sở tùy ý. Hơn nữa, đầu ra v1 của thuật toán là một vectơ khác 0 ngắn nhất trong L, và do đó giải được SVP.

Đối với các chiều cao hơn, có một thuật toán giảm lattice cơ bản được gọi là thuật toán LLL, được đặt tên theo Lenstra, Lenstra và Lovász. Thuật toán LLL (được tích hợp sẵn trong sage nên blog này sẽ không mô tả chi tiết) chạy trong thời gian đa thức. Tuy nhiên, bây giờ, hãy thử trong Lattice trong hai chiều.

**Algorithm for Gaussian Lattice Reduction**

```python
Loop
   (a) If ||v2|| < ||v1||, swap v1, v2
   (b) Compute m = ⌊ v1∙v2 / v1∙v1 ⌉
   (c) If m = 0, return v1, v2
   (d) v2 = v2 - m*v1
Continue Loop
```

Ví dụ: Challenge Cryptohack/Mathematics/Gaussian Reduction:

For the flag, take the two vectors `v = (846835985, 9834798552), u = (87502093, 123094980)` and by applying Gauss's algorithm, find the optimal basis. The flag is the inner product of the new basis vectors.

### Solution:

```python
from sage.all import *

v = vector((846835985, 9834798552))
u = vector((87502093, 123094980))

'''
Loop
   (a) If ||v2|| < ||v1||, swap v1, v2
   (b) Compute m = ⌊ v1∙v2 / v1∙v1 ⌉
   (c) If m = 0, return v1, v2
   (d) v2 = v2 - m*v1
Continue Loop
'''

def Gaussian_Reduction(v,u):
    while True:
        if v.norm() < u.norm():
            v,u = u,v
        m =  v.dot_product(u)// v.dot_product(v)
        if m == 0:
            return v,u
        u = u - m*v

a,b = Gaussian_Reduction(v,u)
print(a*b)
```

# **Merkle-Hellman knapsack cryptosystem**

Định nghĩa: Tập hợp các số tự nhiên khác không khác nhau được gọi là **knapsack**. Ngoài ra, nếu tập hợp này có thể được sắp xếp trong một danh sách tăng dần sao cho mọi số đều lớn hơn tổng của tất cả các số trước đó, thì chúng ta sẽ gọi danh sách này là **superincreasing knapsack.**

Merkle-Hellman knapsack cryptosystem là một hệ thống mật mã bất đối xứng khác, thú vị về mặt lý thuyết vì về cơ bản nó cho phép gửi thông tin nhạy cảm qua một kênh không an toàn

Merkle-Hellman knapsack cryptosystem bao gồm 2 knapsack key:

- Public key: chỉ dùng để mã hóa. Nó được gọi là ‘hard snapsack’
- Private key: chỉ dùng cho giải mã. Bao gồm 1 superincreasing knapsack, một nhân tử và một modulus. Nhân tử và modulus có thể được sử dụng để chuyển đổi từ superincreasing knapsack sang hard knapsack

Cơ chế sinh khóa được thể hiện qua các bước sau:

- Đầu tiên, chúng ta phải tạo ra một superincreasing knapsack: W = [w1, w1,…, wn]
- Chọn 1 số nguyên q sao cho q lớn hơn tổng của tất cả các phần tử thuộc W.
    - q được đinh nghĩa là modulus của hệ thống mật mã.
- Chọn q số nguyên r sao cho
    - r thuộc [1, q]
    - gcd(r, q) = 1
    - ⇒ r đươc định nghĩa là nhân tử của hệ thống mật mã
- Private key của hệ thống mật mã là bộ số (W, r, q)
- Ta tạo ra chuỗi số B = (b1, b2, …, bn) với bi = r.wi mod q
    - B là public key của hệ mật.

Nếu chúng ta muốn mã hóa một tin nhắn m, trước tiên chúng ta lấy biểu diễn bit của nó 

M = m1m2m3…mn (mi là các bit 0 hoặc 1). Để đảm bảo tính đúng đắn của thuật toán, Superincreasing knapsack K của chúng ta phải có ít nhất n phần tử →  W = [w1,w2,...,wn,...]. Theo quy trình tạo khóa, chúng tôi tạo public key tương ứng H, tức là H = [h1,h2,...,hn] với q và r thích hợp. 

```python
c = m1.h1 + m2.h2 + ... + mn.hn
```

Để giải mã một thông điệp đã được mã hóa bằng thuật toán Merkle-Hellman, các bước cần thực hiện như sau:

1. Lấy chuỗi khóa riêng tư từ người gửi thông điệp và chuyển đổi nó thành chuỗi khóa công khai bằng cách sử dụng thuật toán Merkle-Hellman.
2. Nhận được chuỗi khóa công khai và chuỗi thông điệp đã được mã hóa từ người gửi thông điệp.
3. Sử dụng chuỗi khóa công khai để giải mã chuỗi thông điệp đã được mã hóa. Để làm điều này, sử dụng thuật toán nhân phân tích có tên là "sải cách nhị phân" để tìm ra các yếu tố của các số hạng trong chuỗi khóa riêng tư.
4. Sau khi tìm ra các yếu tố của các số hạng trong chuỗi khóa riêng tư, sử dụng các yếu tố này để tính toán lại chuỗi thông điệp đã được mã hóa và tìm ra thông điệp ban đầu.

![](https://i.imgur.com/1xtpamo.png)


# **Lattice-based cryptanalysis**

Mã hóa tin nhắn bằng cách sử dụng hard knapsack có độ dài ít nhất bằng độ dài của tin nhắn sẽ an toàn hơn nhiều nhưng vẫn dễ bị tấn công. Chúng tôi sẽ chứng minh lỗ hổng này bằng cách sử dụng một lattice được thiết kế đặc biệt.

Ta có public key H và ciphertext C, chúng ta có thể biểu diễn mỗi phần tử của H dưới dạng một vectơ trong mạng |H| chiều. Chúng ta cần |H| thứ nguyên để đảm bảo chúng tạo thành cơ sở của lattice L. Để đảm bảo chúng độc lập tuyến tính, chúng ta chỉ cần tăng chuyển vị H thành ma trận đơn vị với thứ nguyên |H| − 1.

Giả sử rằng Eve chặn được một tin nhắn giữa Alice và Bob, tin nhắn này được mã hóa bằng hệ thống mật mã Merkle-Hellman. Vì mọi người đều có quyền truy cập vào khóa công khai của hệ thống mật mã nên Eve cũng sở hữu nó. Tin nhắn C bị chặn là:

```python
C = [318668,317632,226697,388930,357448,297811, 344670,219717,388930,307414,220516,281175]
```

Public key tương ứng là:

```python
H = [106507,31482,107518,60659,80717,81516,117973,87697]
```

Để khôi phục tin nhắn, Eve phải giải mã từng phần tử trong C. Ví dụ: hãy bắt đầu với 318668. Trước tiên, chúng ta cần khởi tạo C và H.

```python
sage: H = [106507,31482,107518,60659,80717,81516,117973,87697]
sage: c = 318668
sage: I = identity_matrix(8)
sage: I
[1 0 0 0 0 0 0 0]
[0 1 0 0 0 0 0 0]
[0 0 1 0 0 0 0 0]
[0 0 0 1 0 0 0 0]
[0 0 0 0 1 0 0 0]
[0 0 0 0 0 1 0 0]
[0 0 0 0 0 0 1 0]
[0 0 0 0 0 0 0 1]
```

thêm một hàng với các phần tử 0:

```python
sage: I = I.insert_row(8, [0 for x in range(8)])
sage: I
[1 0 0 0 0 0 0 0]
[0 1 0 0 0 0 0 0]
[0 0 1 0 0 0 0 0]
[0 0 0 1 0 0 0 0]
[0 0 0 0 1 0 0 0]
[0 0 0 0 0 1 0 0]
[0 0 0 0 0 0 1 0]
[0 0 0 0 0 0 0 1]
[0 0 0 0 0 0 0 0]
```

Tiếp theo thêm cột cuối cùng với hoán vị của H và -c

```python
sage: L_helper = [[x] for x in H] # vector of vectors
sage: L_helper.append([-c])
sage: L = I.augment(matrix(L_helper))
sage: L
[      1       0       0       0       0       0       0       0  106507]
[      0       1       0       0       0       0       0       0   31482]
[      0       0       1       0       0       0       0       0  107518]
[      0       0       0       1       0       0       0       0   60659]
[      0       0       0       0       1       0       0       0   80717]
[      0       0       0       0       0       1       0       0   81516]
[      0       0       0       0       0       0       1       0  117973]
[      0       0       0       0       0       0       0       1   87697]
[      0       0       0       0       0       0       0       0 -318668]
```

Bây giờ ta sử dụng thuật toán LLL với ma trận đã tạo để tìm ra cơ sỏ ngắn nhất của Lattice.

```python
sage: L.LLL()
[ 0  1  0  0  0  1  1  1  0]
[-1  1  0  1 -1 -2 -2  2  1]
[ 3  1  2 -1  1  1 -1  1  1]
[ 1 -1 -2 -1 -3 -1  1  1  1]
[ 2 -2 -1  1  0  2 -3  1  1]
[ 0  0  3 -4 -2  1  0  0  0]
[-1  3 -1  3  0  0 -1 -3  2]
[ 0 -1  1  4  0  0  0  0  4]
[-2 -1 -2 -3  1 -1  2  1  3]
```

Ta thấy hàng đầu tiên chỉ chứa 0 và 1 → Là cơ sở mà ta cần tìm và cũng là biểu diễn nhị phân của plaintext.

```python
sage: bs = "".join([str(x) for x in L.LLL()[0][:-1]])
sage: bs
'01000111'
sage: chr(int(bs,2))
'G'
```

Như vâyj ta đã tìm được kí tự đầu tiên của plaintext. Thực hiện tương tự phương pháp trên ta sẽ tìm được plaintext hoàn chỉnh.

### Giaỉ thích

Ma trận trận ta tạo ra có dạng như sau:

```python
[      1       0       0       0       0       0       0       0  106507]
[      0       1       0       0       0       0       0       0   31482]
[      0       0       1       0       0       0       0       0  107518]
[      0       0       0       1       0       0       0       0   60659]
[      0       0       0       0       1       0       0       0   80717]
[      0       0       0       0       0       1       0       0   81516]
[      0       0       0       0       0       0       1       0  117973]
[      0       0       0       0       0       0       0       1   87697]
[      0       0       0       0       0       0       0       0 -318668]
```

Gọi `B = [b1, b2,…, b8]` là biểu diễn nhị phân của kí tự đầu tiên thuộc plaintext.(B chỉ gồm 2 phần tử 0 và 1).

Tạo lattice L với có dạng

```python
L = {a1*v1 + a2*v2 + ... + a9*v9 : a1, a2, ..., an ∈ Z}
```

Với `v1, v2,…` là các hàng của ma trân trên.

Vector ta cần tìm sẽ có dạng.

```python
b1*v1 + b2*v2 + ... + b8*v8 + v9 
= (b1, b2, ..., b8, 0)
```

Do `bi` chỉ là các phần tử 1 hoặc 0 nên đây là một trong những vector ngắn nhất của Lattice L.

VD: challenge Cryptohack/Lattices/Backpack Cryptography

source code:

```python
import random
from collections import namedtuple
import gmpy2
from Crypto.Util.number import isPrime, bytes_to_long, inverse, long_to_bytes

FLAG = b'crypto{??????????????????????????}'
PrivateKey = namedtuple("PrivateKey", ['b', 'r', 'q'])

def gen_private_key(size):
    s = 10000
    b = []
    for _ in range(size):
        ai = random.randint(s + 1, 2 * s)
        assert ai > sum(b)
        b.append(ai)
        s += ai
    while True:
        q = random.randint(2 * s, 32 * s)
        if isPrime(q):
            break
    r = random.randint(s, q)
    assert q > sum(b)
    assert gmpy2.gcd(q,r) == 1
    return PrivateKey(b, r, q)

def gen_public_key(private_key: PrivateKey):
    a = []
    for x in private_key.b:
        a.append((private_key.r * x) % private_key.q)
    return a

def encrypt(msg, public_key):
    assert len(msg) * 8 <= len(public_key)
    ct = 0
    msg = bytes_to_long(msg)
    for bi in public_key:
        ct += (msg & 1) * bi
        msg >>= 1
    return ct

def decrypt(ct, private_key: PrivateKey):
    ct = inverse(private_key.r, private_key.q) * ct % private_key.q
    msg = 0
    for i in range(len(private_key.b) - 1, -1, -1):
         if ct >= private_key.b[i]:
             msg |= 1 << i
             ct -= private_key.b[i]
    return long_to_bytes(msg)

private_key = gen_private_key(len(FLAG) * 8)
public_key = gen_public_key(private_key)
encrypted = encrypt(FLAG, public_key)
decrypted = decrypt(encrypted, private_key)
assert decrypted == FLAG

print(f'Public key: {public_key}')
print(f'Encrypted Flag: {encrypted}')
```

out put:

```python
Public key: [260288377891444370372615148009023640057294926547602419331406531383682223097787288755377467188515435381259752760746, 322734358011758862401399370931929863052553602421714393280581187496537763321855751120439457234561080720455397490349, 88092359256403564783665281993130133541226601877969436905267415353041909757324746080398461245281826552421872983184,....]
Encrypted Flag: 45690752833299626276860565848930183308016946786375859806294346622745082512511847698896914843023558560509878243217521
```

### Solution

Gọi $x = (x1, x2,… xn)$ là biểu diễn nhị phân của plaintext.

```python
Encode_flag = x1*Public_key1+x2*Public_key2+....+xn*Public_keyn
```

Public key: $M = (m1,m2,…mn)$

Ở challenge này tha vì sử dụng Lattice có dạng như trên thì ta có thể dùng 1 lattice khác với các cơ sở như sau:

```python
v1 = (2, 0, ..., 0, m1)
v2 = (2, 0, ..., 0, m2)
.
.
.
vn = (2, 0, ..., 0, mn)
vn+1 = (1,1, ..., 1, Encode_flag )
```

Latitice L có dạng như sau:

```python
L = {a1*v1 + a2*v2 + ... + a9*v9 : a1, a2, ..., an ∈ Z}
```

Vetor ta cần tìm có dạng

```python
t = x1.v1 + x2.v2 + ... + xn.vn - vn+1
-> t = (2x1 - 1, 2x2 - 1, ..., 2xn - 1, 0)
```

Vì `x1,x2,…,xn` chỉ là `0` hoặc `1` nên các phần tử của chỉ là `1` hoặc `-1` riêng phần tử cuối cùng là `0`.

Như vậy, sau khi thu được vector cần tìm ta chỉ cần giữ nguyên phần tử 1 và chuyển phần tử -1 thành 0, sau đó bỏ đi phần tử cuối cùng là thu được flag dưới dạng nhị phân:

full code:

```python
from sage.all import *
from Crypto.Util.number import long_to_bytes

#Public_key = in file ouput.txt

Encrypted_Flag =  45690752833299626276860565848930183308016946786375859806294346622745082512511847698896914843023558560509878243217521
# print(len(Public_key)) 272

I = identity_matrix(272)
I = 2*I
I = I.insert_row(272, [ZZ(1) for x in range(272)])
Public_key.append(Encrypted_Flag)
E = [[ZZ(x)] for x in Public_key]
L = I.augment(Matrix(E))
R = L.LLL()
for i in R:
    if len(set(i[:-1])) == 2:
        F = i

Flag = long_to_bytes(int(''.join(str(i) for i in [1 if i == -1 else 0 for i in F][::-1]),2))
print(Flag)
#crypto{my_kn4ps4ck_1s_l1ghtw31ght}
```
