# Manipulating Images — PIL (Pillow) & OpenCV (cv2)

Bộ notebook này hướng dẫn **thao tác ảnh cơ bản (basic image manipulation)** trong Python theo 2 cách:
- **PIL (Pillow)**: thao tác ảnh theo kiểu đối tượng `Image`
- **OpenCV (cv2)**: thao tác ảnh theo kiểu **NumPy array** (phổ biến trong Computer Vision)
File trong repo:
- `2.2.1_basic_image_manipulation_PIL.ipynb`
- `2.2.2_basic_image_manipulation_open_CV.ipynb`

## Công nghệ sử dụng
- **Python 3.x**
- **Jupyter Notebook** (`.ipynb`)
- **Pillow (PIL)** — đọc/ghi ảnh, crop, flip, paste, vẽ text/shape
- **OpenCV (cv2)** — đọc/ghi ảnh, flip/rotate, crop, vẽ text/shape
- **NumPy** — xử lý mảng pixel, slicing
- **Matplotlib** — hiển thị ảnh
- **os** — thao tác đường dẫn (nếu cần)

## Cách hoạt động (Workflow)
Các notebook đều theo quy trình:
### 1) Tải ảnh cho bài lab
Notebook có cell tải ảnh bằng `wget` (ví dụ `lenna.png`, `baboon.png`, `cat.png`, `barbara.png`,...).
> Nếu máy bạn không có `wget` (Windows), hãy tải ảnh thủ công và đặt cùng thư mục với notebook.

### 2) Load ảnh & hiển thị ảnh
**PIL**
```python
from PIL import Image
image = Image.open("cat.png")
```

**OpenCV**
```python
import cv2
image = cv2.imread("cat.png")
```
OpenCV đọc ảnh theo hệ màu **BGR**, nên khi hiển thị bằng Matplotlib cần chuyển sang **RGB**:
```python
plt.imshow(cv2.cvtColor(image, cv2.COLOR_BGR2RGB))
```

## Nội dung chính trong notebook

### A) Copying Images — Tránh “Aliasing” (tham chiếu chung bộ nhớ)
- Nếu gán:
  ```python
  A = baboon
  ```
  thì `A` và `baboon` **trỏ chung 1 vùng nhớ** → sửa 1 cái thì cái kia đổi theo.

- Đúng cách:
  ```python
  B = baboon.copy()
  ```
Tránh lỗi “biến A bị thay đổi khi mình sửa baboon”.

### B) Flipping Images — Lật ảnh
#### PIL
- Lật dọc:
  ```python
  from PIL import ImageOps
  im_flip = ImageOps.flip(image)
  ```
- Lật ngang:
  ```python
  im_mirror = ImageOps.mirror(image)
  ```

#### OpenCV
- Dùng `cv2.flip(image, flipCode)`:
  - `0`: lật dọc
  - `1`: lật ngang
  - `-1`: lật cả dọc + ngang
- Ngoài ra có thể dùng `cv2.rotate()` để xoay nhanh.

### C) Cropping an Image — Cắt ảnh (crop)
Dùng slicing NumPy:
```python
crop = image[upper:lower, left:right, :]
```

Ngoài ra trong PIL có thể crop bằng:
```python
crop_image = image.crop((left, upper, right, lower))
```

### D) Changing Specific Image Pixels — Thay đổi pixel theo vùng
Ví dụ đưa một vùng pixel về màu đen:

**PIL (mảng RGB)**
```python
array_sq[upper:lower, left:right, 1:3] = 0   # tắt kênh G,B (giữ Red)
```

**OpenCV (mảng BGR)**
```python
array_sq[upper:lower, left:right, :] = 0
```

---

### E) Vẽ hình & chèn chữ lên ảnh
#### PIL (ImageDraw)
- Vẽ hình chữ nhật:
  ```python
  from PIL import ImageDraw
  draw = ImageDraw.Draw(image_draw)
  draw.rectangle([left, upper, right, lower], fill="red")
  ```
- Chèn chữ:
  ```python
  draw.text((0,0), "box", fill=(0,0,0))
  ```

#### OpenCV
- Vẽ rectangle:
  ```python
  cv2.rectangle(image_draw, (left, upper), (right, lower), (0,0,255), thickness=5)
  ```
- Chèn chữ:
  ```python
  cv2.putText(image_draw, "Stuff", (10,500), fontFace=4, fontScale=5,
              color=(255,255,255), thickness=2)
  ```

---

### F) Overlay / Paste ảnh lên ảnh (PIL)
- Ghi đè pixel của ảnh A lên ảnh B bằng slicing:
  ```python
  array_lenna[upper:lower,left:right,:] = array_cat[upper:lower,left:right,:]
  ```
- Hoặc dùng `paste()`:
  ```python
  image_lenna.paste(crop_image, box=(left, upper))
  ```

---

## Kết quả (Output)

Khi chạy notebook, bạn sẽ thu được:

Hiểu rõ **copy vs aliasing** (lỗi trỏ chung bộ nhớ)  
Ảnh được **flip / mirror / rotate**  
Ảnh được **crop theo vùng** (trên/dưới/trái/phải)  
Thay đổi pixel theo vùng (tạo vùng màu đen hoặc chỉ giữ 1 kênh màu)  
Ảnh được **vẽ rectangle + chèn chữ**  
(PIL) Dán một phần ảnh lên ảnh khác bằng **paste/overlay**  






