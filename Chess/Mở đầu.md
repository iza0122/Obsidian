- Tài liệu tham khảo: https://archive.gamedev.net/archive/reference/articles/article1014.html
## 1. Cốt lõi
Máy tính không như con người, để mà máy tính có thể "chơi cờ", máy tính phải cần những thành phần sau để hoạt động:
- Biểu diễn bàn cờ và quân cờ
- Quy tắc tạo ra những nước đi hợp lệ
- Kĩ thuật để máy tính ra nước đi
- Một cái máy chơi cờ (engine) để đưa ra các nước đi mạnh
- Giao diện người dùng

# Các tính năng chính cần có
## 1. Biểu diễn ván cờ
Ta có 2 lựa chọn sau:
	- Dùng mảng 2 chiều 8x8.
	- Dùng [Bitboard](https://en.wikipedia.org/wiki/Bitboard).

[[Biểu diễn ván cờ|Part 2]] sẽ nói về vấn đề này.

## 2. Tạo lập những nước đi
Cờ vua là một trò chơi có đa dạng quân cờ, do đó cũng sẽ có nhiều cách thức các quân di chuyển và hoạt động. Ví dụ như: Quân xe di chuyển theo chiều ngang & dọc, quân tượng di chuyển theo đường chéo, tốt phong cấp, nhập thành, ... .

Đây là vấn đề yêu cầu khối lượng tính toán rất cao và phức tạp. Chúng ta cần một ==**cấu trúc dữ liệu**== và ==thuật toán== phù hợp để tối ưu hóa công việc tính toán.

*Part 3 sẽ nói về vấn đề này.*

## 3. Tìm kiếm những nước đi tốt.
Với việc tạo lập những nước đi, máy tính chỉ có thể liệt kê tất cả các nước cờ khả thi nhưng không thể lựa chọn nước đi tốt nhất trong số đó.
Do đó ta cần 1 cách thức nào đó để máy tính có thể chọn những nước đi tốt nhất.
Có một số thuật toán để phục vụ cho việc này:
 - Thuật toán Minimax
 - Alpha-Beta Pruning (Phiên bản tối ưu của minimax)
 - Iterative Deepening
Ta sẽ sử dụng giải thuật Alpha-Beta Pruning vì nó dễ tiếp cận với beginner.

## 4. Đánh giá ván cờ
Chương trình sẽ cần chức năng này để nhận biết người chơi nào đang chiếm lợi thế. (Thắng/Hòa/Thua).


