# baitapAndroidStudio

họ và tên : bùi ngọc anh

lớp k58.ktp

mssv : k225510201001

YÊU CẦU:
Viết App sử dụng Android Studio:

1. Tạo app1 sử dụng cơ chế Dữ liệu chuẩn bị trước trong Assets
  
2.  APP2 (android studio): tạo app tương đương với Mit App inventor, app có 3 activity

   Lý thuyết

## 1. AndroidManifest.xml

AndroidManifest.xml là tập tin cấu hình trung tâm của ứng dụng Android. Tập tin này dùng để khai báo các thành phần của ứng dụng như Activity, quyền truy cập (Permission), tên ứng dụng, biểu tượng ứng dụng và các thông tin cấu hình khác.

Ví dụ, nếu ứng dụng cần truy cập Internet để tải dữ liệu du lịch từ máy chủ thì cần khai báo:

```xml
<uses-permission android:name="android.permission.INTERNET"/>
```

Trong ứng dụng Cẩm nang Du lịch Thái Nguyên, AndroidManifest.xml được sử dụng để khai báo các Activity như MainActivity và DetailActivity.

---

## 2. Vòng đời của Activity trong Android

Mỗi Activity trong Android đều trải qua các trạng thái khác nhau trong quá trình hoạt động.

Các phương thức chính:

* onCreate(): khởi tạo Activity.
* onStart(): Activity bắt đầu hiển thị.
* onResume(): Activity sẵn sàng tương tác với người dùng.
* onPause(): Activity tạm dừng.
* onStop(): Activity không còn hiển thị.
* onDestroy(): Activity bị hủy.

Trình tự phổ biến:

onCreate → onStart → onResume

Khi thoát ứng dụng:

onPause → onStop → onDestroy

---

## 3. Tại sao project mới tạo đã có hàm onCreate()?

Khi tạo một project Android mới, Android Studio tự động sinh ra hàm onCreate().

```java
@Override
protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    setContentView(R.layout.activity_main);
}
```

Hàm này được gọi khi Activity được tạo lần đầu tiên. Đây là nơi dùng để khởi tạo giao diện, đọc dữ liệu, gán sự kiện và thực hiện các thao tác chuẩn bị cho màn hình.

---

## 4. Kiểm tra quyền trong Java

Một số quyền thuộc nhóm nguy hiểm (Dangerous Permission) cần được kiểm tra khi chạy ứng dụng.

Ví dụ:

```java
if (ContextCompat.checkSelfPermission(
        this,
        Manifest.permission.ACCESS_FINE_LOCATION)
        != PackageManager.PERMISSION_GRANTED) {

    ActivityCompat.requestPermissions(
            this,
            new String[]{
                    Manifest.permission.ACCESS_FINE_LOCATION
            },
            1);
}
```

Ý nghĩa:

* checkSelfPermission(): kiểm tra đã được cấp quyền hay chưa.
* requestPermissions(): yêu cầu người dùng cấp quyền.

---

## 5. Giao diện XML trong thư mục res/layout

Giao diện Android được mô tả bằng các file XML trong thư mục:

```text
res/layout
```

Ví dụ:

```xml
<TextView
    android:id="@+id/txtTitle"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:text="@string/app_name"/>
```

Android Studio hỗ trợ hai chế độ:

* Code XML.
* Design Preview (kéo thả giao diện).

---

## 6. Tại sao không nên Hardcode văn bản?

Không nên viết:

```xml
android:text="Cẩm nang du lịch Thái Nguyên"
```

Nên lưu trong:

```xml
res/values/strings.xml
```

Ví dụ:

```xml
<string name="app_name">
    Cẩm nang du lịch Thái Nguyên
</string>
```

Sau đó tham chiếu:

```xml
android:text="@string/app_name"
```

Hoặc trong Java:

```java
textView.setText(R.string.app_name);
```

### Ưu điểm

* Dễ bảo trì.
* Hỗ trợ đa ngôn ngữ.
* Hỗ trợ giao diện sáng/tối.
* Dễ tái sử dụng.

---

## 7. Hỗ trợ Language, Location và Theme

Android có thể tự động chọn tài nguyên phù hợp theo:

* Ngôn ngữ (Language).
* Khu vực (Location).
* Chủ đề giao diện (Theme).

Ví dụ:

```text
values/
values-en/
values-ja/
```

Khi người dùng đổi ngôn ngữ thiết bị, ứng dụng sẽ tự động hiển thị đúng ngôn ngữ tương ứng mà không cần sửa mã nguồn.

Điều này giúp ứng dụng có khả năng quốc tế hóa (Internationalization).

---

## 8. Layout chứa các thành phần con

Android cung cấp nhiều Layout để sắp xếp giao diện.

Ví dụ:

### LinearLayout

Sắp xếp theo chiều dọc:

```xml
android:orientation="vertical"
```

Sắp xếp theo chiều ngang:

```xml
android:orientation="horizontal"
```

Canh giữa:

```xml
android:gravity="center"
```

Trong ứng dụng Cẩm nang Du lịch Thái Nguyên, LinearLayout được dùng để bố trí ảnh, tên địa điểm và mô tả.

---

## 9. Tương tác giữa Java và Layout

Khai báo trong XML:

```xml
<TextView
    android:id="@+id/txtName"/>
```

Trong Java:

```java
TextView txtName =
        findViewById(R.id.txtName);

txtName.setText(
        getString(R.string.app_name));
```

Sử dụng getString() giúp văn bản tự động thay đổi theo ngôn ngữ của hệ thống.

---

## 10. Xử lý sự kiện (Event)

### Cách 1: Khai báo trong XML

XML:

```xml
<Button
    android:onClick="showDetail"/>
```

Java:

```java
public void showDetail(View view){
    // xử lý
}
```

### Cách 2: Dùng OnClickListener

```java
Button btn =
        findViewById(R.id.btnOpen);

btn.setOnClickListener(v -> {
    // xử lý
});
```

Cách thứ hai được sử dụng phổ biến hơn vì dễ quản lý và bảo trì.

---

## 11. Thư mục Assets

Assets là thư mục đặc biệt dùng để chứa dữ liệu đi kèm ứng dụng.

Ví dụ:

```text
assets/
 └── du_lich_tn.json
```

Khi biên dịch ứng dụng, toàn bộ dữ liệu trong Assets sẽ được đóng gói vào file APK.

---

## 12. Truy cập dữ liệu trong Assets

Ví dụ đọc file JSON:

```java
InputStream is =
        getAssets().open("du_lich_tn.json");
```

Sau đó sử dụng InputStreamReader và BufferedReader để đọc nội dung.

---

## 13. Lợi ích của Assets

* Hoạt động khi không có Internet.
* Tốc độ truy cập nhanh.
* Dữ liệu luôn đi kèm ứng dụng.
* Không phụ thuộc máy chủ bên ngoài.

---

## 14. Ứng dụng trong đề tài

Ứng dụng Cẩm nang Du lịch Thái Nguyên sử dụng file JSON lưu trong thư mục Assets để lưu thông tin các địa điểm du lịch như:

* Hồ Núi Cốc
* Đồi chè Tân Cương
* ATK Định Hóa
* Hang Phượng Hoàng

Khi khởi động, ứng dụng đọc dữ liệu từ Assets, phân tích JSON và hiển thị danh sách địa điểm cho người dùng. Vì dữ liệu được lưu trực tiếp trong ứng dụng nên người dùng vẫn có thể tra cứu thông tin ngay cả khi không có kết nối Internet.

bài 1 : TẠO APP1: sử dụng cơ chế Dữ liệu chuẩn bị trước trong Assets

tên đề tài : cẩm nang du lịch thái nguyên

Dữ liệu sử dụng

Ứng dụng sử dụng dữ liệu được chuẩn bị trước và lưu trong thư mục Assets dưới dạng tệp JSON.

Mỗi địa điểm du lịch bao gồm các thông tin:

Mã địa điểm. Tên địa điểm. Tỉnh/Thành phố. Mô tả ngắn. Hình ảnh minh họa.

Ứng dụng được xây dựng bằng Android Studio sử dụng ngôn ngữ Java.

Quy trình hoạt động:

Đọc dữ liệu từ tệp JSON trong thư mục Assets. Chuyển dữ liệu JSON thành các đối tượng Java. Hiển thị dữ liệu bằng RecyclerView. Cho phép người dùng tìm kiếm địa điểm theo tên. Cập nhật danh sách kết quả sau khi tìm kiếm.

Ứng dụng hoạt động hoàn toàn offline, không yêu cầu kết nối Internet và đáp ứng đầy đủ yêu cầu sử dụng dữ liệu chuẩn bị trước trong Assets.

tạo projcet 
<img width="1366" height="768" alt="Image" src="https://github.com/user-attachments/assets/d7a8a004-9c68-47a1-91c1-02ad4e8a8df3" />


Tạo thư mục Assets

Chọn New -> Directory -> Assets Folder -> Finish. Sau khi nhấn Finish dưới thư mục main sẽ xuất hiện một thư mục tên là Assets

<img width="1366" height="768" alt="Image" src="https://github.com/user-attachments/assets/bc034103-af52-462a-8ee3-83cfebfe0e92" />

Tạo file dữ liệu DuLich_places.json

hấp chuột phải vào thư mục Assets vừa tạo -> New -> File

<img width="1366" height="768" alt="Image" src="https://github.com/user-attachments/assets/e89e9f30-5a4f-4e06-95c6-ea7d9e28d784" />

Thêm ảnh vào app

Tìm thư mục app -> res -> drawable. Dán ảnh vào trong drawable


<img width="1366" height="768" alt="Image" src="https://github.com/user-attachments/assets/d4254f96-7b7e-438d-8c83-81f749c382ff" />

Thiết kế giao diện XML

Mở file activity_main.xml và chuyển sang chế độ xem Code sau đó sửa lại file activity_main.xml

<img width="1366" height="768" alt="Image" src="https://github.com/user-attachments/assets/97d4f184-6e41-4c73-bd10-b66157345426" />

Thiết kế ô hiển thị file dulich_travel.xml

<img width="1366" height="768" alt="Image" src="https://github.com/user-attachments/assets/2a497e88-c818-4256-9c99-859dbe8d54af" />

Tạo dữ liệu mẫu 

creat file dulich.java

Xóa hết code tự sinh bên trong đi và viết lại code chuẩn package vào

<img width="1366" height="768" alt="Image" src="https://github.com/user-attachments/assets/49d9f3b0-38f9-4971-abb0-b5fdf94c7e9f" />

Tạo bộ nạp dữ liệu lên màn hình

Tạo tiếp một Class Java tên là Duliapp File này đóng vai trò cầu nối, lấy dữ liệu từ file JSON đổ vào file giao diện

Mở file MainActivity.java có sẵn ra và cập nhật lại toàn bộ code để thực hiện việc: Đọc file JSON từ Assets -> Chạy thuật toán tìm kiếm tuyến tính

<img width="1366" height="768" alt="Image" src="https://github.com/user-attachments/assets/572db784-9ee8-4695-a275-8914d08e0ee0" />

<img width="1366" height="768" alt="Image" src="https://github.com/user-attachments/assets/d89ce1f3-4e94-4504-94c6-4fc9ba6bebd6" />

TẠO APP2

Tạo project mới đặt tên giatoan

Cấp quyền Internet trong AndroidManifest.xml

Mở file AndroidManifest.xml và dán dòng lệnh sau nằm trên thẻ <application>

<uses-permission android:name="android.permission.INTERNET" />

<img width="1366" height="768" alt="Image" src="https://github.com/user-attachments/assets/c9e5ab1d-f0e5-4413-ae53-d709dfc96d2a" />

 Tạo các màn hình (Activity2, 3)

 MainActivity sẽ đóng vai trò là Activity 1 (About).

Tạo thêm MathActivity đóng vai trò là Activity 2 (Giải toán).

Tạo thêm WebActivity đóng vai trò là Activity 3 (WebView)

<img width="407" height="249" alt="Image" src="https://github.com/user-attachments/assets/ab4b8c39-f38e-4a82-8d7f-5b96ecf2bcee" />

Thiết kế giao diện XML trong file activity_main.xml

Mở file activity_main.xml trong res/layout/ lên, chuyển sang góc nhìn Code và viết code cấu hình cho giao diện chính:

<img width="1366" height="768" alt="Image" src="https://github.com/user-attachments/assets/29a95209-5dcd-45ed-84cb-8aed6790e9fe" />

MainActivity.java ra và cập nhật lại code 

<img width="1366" height="768" alt="Image" src="https://github.com/user-attachments/assets/1972adde-5801-4e73-87df-7268cd38d49d" />

Thiết kế giao diện giải toán (activity_math.xml)

<img width="1366" height="768" alt="Image" src="https://github.com/user-attachments/assets/021372a2-2a40-46f9-9b36-536639c6e2f9" />

Cài thư viện gọi API (OkHttp) vào dự án

Mở file có tên chính xác là: build.gradle.kts (Module: app).

Cuộn xuống dưới cùng của file, tìm khối lệnh có chữ dependencies { ... }

<img width="1366" height="768" alt="Image" src="https://github.com/user-attachments/assets/0773117d-ff88-4ea4-8150-2dc64c8980bc" />

Mở file MathActivity.java ra, xóa code cũ và viết lại code giải phương trình bậc 2 và gọi API gửi lên server:

<img width="1366" height="768" alt="Image" src="https://github.com/user-attachments/assets/12d002e2-3756-4d42-bc66-4107bb107976" />

 Thiết kế giao diện Webview (activity_web.xml) 

 <img width="1366" height="768" alt="Image" src="https://github.com/user-attachments/assets/c05c7412-f1ef-4977-92db-eabc75e704db" />

 Viết mã nguồn xử lý logic và truyền Url https://k58kmt.tdh.io.vn

 <img width="566" height="48" alt="Image" src="https://github.com/user-attachments/assets/abaaecaf-0274-49a3-97c6-3fb36b791d7f" />

 Chạy  kết quả
 <img width="1739" height="3768" alt="Image" src="https://github.com/user-attachments/assets/075b75b8-1873-475e-90a2-6add30c364e5" />

 <img width="1739" height="3768" alt="Image" src="https://github.com/user-attachments/assets/cdfa313d-06ed-4059-b4a8-f8f85d06a51c" />
