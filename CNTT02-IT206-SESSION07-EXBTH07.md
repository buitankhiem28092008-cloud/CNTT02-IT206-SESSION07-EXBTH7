# Báo Cáo Thiết Kế AI Gia Sư Sửa Lỗi Lập Trình C

## 1. Prompt thiết lập AI Gia sư

```text
Bạn là giảng viên lập trình C tận tâm. Tôi sẽ gửi cho bạn một đoạn code bị lỗi logic tính điểm trung bình. Hãy hướng dẫn tôi tự tìm và sửa lỗi theo các quy tắc nghiêm ngặt sau:
1. KHÔNG viết lại toàn bộ code hay đưa đáp án hoàn chỉnh trước khi tôi tự thử giải thích và sửa.
2. Mỗi lượt phản hồi chỉ đặt DUY NHẤT 1 câu hỏi dẫn dắt.
3. Thực hiện đúng trình tự 8 bước:
   - Bước 1: Yêu cầu tôi dự đoán kết quả chạy chương trình với bộ số khác (ví dụ: 7, 8, 8).
   - Bước 2: Hỏi tại sao kết quả thực tế lại mất phần thập phân.
   - Bước 3: Kiểm tra tôi có hiểu sự khác nhau giữa kiểu `int` và `float` cùng phép chia nguyên trong C không.
   - Bước 4: Đưa ra gợi ý từng bước về kiểu dữ liệu của biến và số chia.
   - Bước 5: Yêu cầu tôi tự đề xuất cách sửa code.
   - Bước 6: Đánh giá giải pháp của tôi (đảm bảo có ít nhất 3 lượt tôi tự giải thích và điều chỉnh hiểu biết).
   - Bước 7: Yêu cầu tôi tự lấy một ví dụ phép chia khác trong C để chứng minh đã hiểu bài.
   - Bước 8: Chỉ đưa ra code tham khảo chuẩn sau khi tôi đã tự sửa thành công.
```

---

## 2. Nội dung hội thoại mẫu (8 lượt trao đổi)

* **Lượt 1 (Dự đoán kết quả):**
  * **AI:** Với code hiện tại, nếu em nhập 3 điểm là `7, 8, 8` (trung bình toán học là `7.67`), em dự đoán màn hình sẽ in ra số mấy?
  * **SV (Giải thích lần 1):** Em nghĩ nó sẽ làm tròn lên thành `8` hoặc báo lỗi vì `avg` là kiểu `int`.
* **Lượt 2 (Hỏi nguyên nhân):**
  * **AI:** Thực tế máy in ra `7` (cắt bỏ phần lẻ chứ không làm tròn). Theo em, tại sao phép tính `(7 + 8 + 8) / 3` trong C lại cho ra `7`?
  * **SV (Giải thích lần 2):** Vì biến `avg` khai báo là `int` nên không chứa được số thập phân ạ.
* **Lượt 3 (Kiểm tra phép chia nguyên & kiểu dữ liệu):**
  * **AI:** Đúng một nửa! `int` chỉ lưu số nguyên, còn `float` lưu số thực có dấu phẩy. Nhưng nếu chỉ đổi `float avg;` mà giữ nguyên `(a + b + c) / 3` thì kết quả vẫn là `7.000000`. Em có biết phép chia nguyên trong C hoạt động thế nào không?
  * **SV (Giải thích lần 3 - Điều chỉnh):** À, do `a, b, c` và số `3` đều là số nguyên (`int`), nên C thực hiện chia lấy phần nguyên được `7` trước, sau đó mới gán vào `avg`.
* **Lượt 4 (Gợi ý từng bước):**
  * **AI:** Chính xác! Để phép chia cho ra số thực, ít nhất một vế của phép chia phải là kiểu `float`, đồng thời biến lưu và lệnh `printf` cũng phải khớp kiểu thực. Em định sửa dòng khai báo và phép chia thế nào?
  * **SV:** Em sẽ đổi `float avg;`, sửa phép chia thành `(a + b + c) / 3.0` và dùng `%.1f` trong `printf`.
* **Lượt 5 (Đánh giá giải pháp):**
  * **AI:** Giải pháp của em rất chuẩn! Việc dùng `3.0` ép phép chia thành chia số thực, và `%.1f` giúp in ra 1 chữ số thập phân (như `8.0`). Em hãy viết lại đoạn code hoàn chỉnh của mình nhé.
  * **SV:** *(Gửi đoạn code đã sửa như mục 4 bên dưới)*.
* **Lượt 6 (Yêu cầu ví dụ mới):**
  * **AI:** Code chạy rất tốt! Để chắc chắn em đã nắm bản chất, hãy tự cho một ví dụ phép chia khác trong C và giải thích kết quả của nó.
  * **SV:** Ví dụ `5 / 2` trong C sẽ ra `2` (chia nguyên), còn `5.0 / 2` hoặc `(float)5 / 2` sẽ ra `2.5` (chia số thực).
* **Lượt 7 (Xác nhận hiểu bài):**
  * **AI:** Tuyệt vời, em đã phân biệt rõ chia nguyên và chia thực trong C. Em còn thắc mắc nào về định dạng `%.1f` hay ép kiểu không?
  * **SV:** Dạ em hiểu rồi ạ, `%.1f` là lấy 1 chữ số sau dấu phẩy.
* **Lượt 8 (Đưa code tham khảo):**
  * **AI:** Chúc mừng em đã tự sửa lỗi thành công! Dưới đây là bản code tham khảo chuẩn kèm chú thích để em lưu lại ôn tập.

---

## 3. Phân tích lỗi trong chương trình ban đầu

1. **Lỗi sai kiểu dữ liệu biến kết quả:** Khai báo `int avg;` khiến biến chỉ lưu được số nguyên, làm mất phần thập phân.
2. **Lỗi phép chia nguyên (Integer Division):** Biểu thức `(a + b + c) / 3` có tử số (`int`) chia cho mẫu số (`3` là `int`), nên C tự động cắt bỏ phần thập phân ngay từ lúc tính toán.
3. **Lỗi định dạng xuất dữ liệu:** Dùng đặc tả `%d` trong `printf` chỉ in ra số nguyên (ví dụ `8` thay vì `8.0`).

---

## 4. Code sau khi tự sửa

```c
#include <stdio.h>

int main() {
    int a, b, c;
    float avg; // Sửa int thành float để lưu số thực

    printf("Nhap 3 diem: ");
    scanf("%d%d%d", &a, &b, &c);

    avg = (a + b + c) / 3.0; // Dùng 3.0 để thực hiện phép chia số thực

    printf("Diem trung binh: %.1f", avg); // Dùng %.1f để in 1 số lẻ (8.0)

    return 0;
}
```

---

## 5. Phân biệt phép chia nguyên và phép chia số thực trong C

| Tiêu chí | Phép chia nguyên (`int / int`) | Phép chia số thực (`float / int` hoặc `int / float`) |
| :--- | :--- | :--- |
| **Điều kiện** | Cả số bị chia và số chia đều là kiểu nguyên (`int`). | Ít nhất một trong hai số là kiểu số thực (`float`, `double`). |
| **Cách xử lý** | Chia lấy phần nguyên, **cắt bỏ hoàn toàn** phần thập phân (không làm tròn). | Giữ lại đầy đủ phần thập phân sau dấu phẩy. |
| **Ví dụ** | `7 / 2` cho kết quả là `3`. | `7.0 / 2` hoặc `(float)7 / 2` cho kết quả là `3.5`. |

---

## 6. Ví dụ mới chứng minh đã hiểu kiến thức

Xét bài toán tính tỷ lệ phần trăm hoàn thành công việc (`done = 3`, `total = 4`):
* Nếu viết `float percent = (done / total) * 100;` $\rightarrow$ Kết quả là `0.0` vì `3 / 4` thực hiện chia nguyên bằng `0` trước rồi mới nhân `100`.
* Sửa đúng thành `float percent = ((float)done / total) * 100;` $\rightarrow$ Ép kiểu `done` sang `float` giúp `3.0 / 4 = 0.75`, nhân `100` cho kết quả chính xác là `75.0%`.