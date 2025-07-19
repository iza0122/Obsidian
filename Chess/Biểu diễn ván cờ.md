Ta sẽ chọn <font color="#ffff00">Bitboard</font> để biểu diễn ván cờ cho trò chơi.
# 1. Ý tưởng 💡
Một bàn cờ vua tiêu chuẩn 8x8 có 64 ô cờ. Mỗi ô cờ chỉ có 2 trạng thái: Có quân cờ hoặc không có quân cờ. Với trạng thái này, ta có thể biểu diễn nó thông qua nhị phân. Chúng ta sẽ dùng 1 dãy 64 bits tượng trưng cho 64 ô cờ.
- 0 không có quân.
- 1 có quân.
<font color="#ffff00">Một dãy như vậy  được gọi là 1 bitboard</font>
Trùng hợp thay, ta có kiểu 🔢 **`unsigned long long`** (ULL) sử dụng 64 bits để lưu giá trị. Và ta sẽ sử dụng kiểu dữ liệu này xuyên suốt trò chơi. Tuy nhiên 1 bitboard chỉ có thể biểu diễn cho 1 loại quân (có phân biệt màu).

Cách giải quyết là ta sẽ dùng 1 mảng ULL với một chỉ số tương ứng với một loại quân riêng biệt. Trong cờ vua, có 6 loại quân khác nhau tương ứng với 12 loại quân nếu phân biệt màu. Vậy 1 mảng kích thước 12 phần tử ULL là đủ để biểu diễn một bàn cờ.

## 2. Minh họa 🖼️
Một bàn cờ tiêu chuẩn sẽ được xếp như sau:
<div class="chess-board">
8 ♜ ♞ ♝ ♛ ♚ ♝ ♞ ♜
7 ♟ ♟ ♟ ♟ ♟ ♟ ♟ ♟
6 
5 
4 
3 
2 ♙ ♙ ♙ ♙ ♙ ♙ ♙ ♙
1 ♖ ♘ ♗ ♕ ♔ ♗ ♘ ♖
</div>

Hiển nhiên chỉ với trạng thái có hay không có, làm sao chúng ta phân loại được các quân khác nhau?. Do đó ta sẽ tách bàn cờ này thành các lớp đơn giản hơn.

Những con tốt trắng sẽ được biểu diễn dưới một dãy 64 bits như sau:
```
 0000000000000000000000000000000000000000000000001111111100000000
```
Khá là khó nhìn phải không, ta sẽ định dạng lại một chút:

```
0 0 0 0 0 0 0 0
0 0 0 0 0 0 0 0
0 0 0 0 0 0 0 0
0 0 0 0 0 0 0 0
0 0 0 0 0 0 0 0
0 0 0 0 0 0 0 0
1 1 1 1 1 1 1 1
0 0 0 0 0 0 0 0
```

Tương tự với quân xe trắng:
```
0 0 0 0 0 0 0 0
0 0 0 0 0 0 0 0
0 0 0 0 0 0 0 0
0 0 0 0 0 0 0 0
0 0 0 0 0 0 0 0
0 0 0 0 0 0 0 0
0 0 0 0 0 0 0 0
1 0 0 0 0 0 0 1
```


Tương tự với các quân còn lại.

Bây giờ bạn chồng các bitboards đó lại với nhau, bạn sẽ nhận được bàn cờ hoàn chỉnh.

![[Idea.png]]
# 3. [Bitwise Boolean](https://www.chessprogramming.org/General_Setwise_Operations#Bitwise_Boolean) 🔢
Dưới đây là 1 số phép toán:
- Intersection (Giao)
- Union (Hợp)
- Complement Set (Phần bù)
- Relative Complement (Hiệu 2 tập hợp)
- Implication (Kéo theo)
- Exclusive Or (XOR)
- Equivalence (Tương đương)
- Majority (Đa số)
- Greater One Sets (Nếu có 1 hoặc nhiều hơn 1 giá trị true, trả về true. Ngược lại trả về false)
# 4. Shifting bitboard 
Hiểu đơn giản là cách di chuyển quân cờ trên bitboard
<font color="#ffff00">Lưu ý:</font> ta sẽ dùng Little-Endian Rank-File Mapping

![[Little-Endian Rank-File Mapping.png]]

Trong cách này:
- **Ô A1 (góc dưới trái)** là **bit 0**
- Đếm **từ trái qua phải**, rồi **từ dưới lên trên**
- Bit cao nhất (**bit 63**) là ô **H8 (góc trên phải)**

<font color="#ffff00">Note:</font> 
- Dãy 64 bits khi đó được đánh số như sau: 63 62 61 ... 2 1 0.
- Vì chỉ số vuông được mã hóa dưới dạng lũy thừa của 2 trong một bitboard, nên phép nhân hoặc chia lũy thừa của 2 tương đương với việc cộng hoặc trừ chỉ số vuông.

Để di chuyển, ta chỉ cần (+) hoặc (-) với một lượng:

---
      NW        N        NE
         +7    +8    +9
             \  |  /
      W  -1 <-  0  -> +1  E
             /  |  \
         -9    -8    -7
      SW        S        SE

---

*Đừng quên phải xử lí những trường hợp đi ra khỏi bàn cờ!*

