- [\[JAVA\] - BUỔI 10: NHẬP XUẤT FILE, UNIT TEST](#java---buổi-10-nhập-xuất-file-unit-test)
  - [Xử lí File trong Java](#xử-lí-file-trong-java)
    - [Các loại luồng dữ liệu](#các-loại-luồng-dữ-liệu)
    - [Các thao tác xử lý dữ liệu](#các-thao-tác-xử-lý-dữ-liệu)
      - [Xử lý nhập xuất dữ liệu sử dụng luồng byte](#xử-lý-nhập-xuất-dữ-liệu-sử-dụng-luồng-byte)
      - [Xử lý nhập xuất dữ liệu bằng luồng character](#xử-lý-nhập-xuất-dữ-liệu-bằng-luồng-character)
  - [Assertions](#assertions)
    - [Định nghĩa và cú pháp](#định-nghĩa-và-cú-pháp)
    - [Kích hoạt Assertion](#kích-hoạt-assertion)
    - [Tại sao lại cần sử dụng Assertion trong Java](#tại-sao-lại-cần-sử-dụng-assertion-trong-java)
  - [Unit test](#unit-test)
    - [Unit Test trong Java](#unit-test-trong-java)

# [JAVA] - BUỔI 10: NHẬP XUẤT FILE, UNIT TEST
## Xử lí File trong Java
Đọc và ghi file trong java là các hoạt động nhập/xuất dữ liệu (nhập dữ liệu từ bàn phím, đọc dữ liệu từ file, ghi dữ liệu lên màn hình, ghi ra file, ghi ra đĩa, ghi ra máy in…) đều được gọi là luồng (stream).
### Các loại luồng dữ liệu

![](https://giasutinhoc.vn/wp-content/uploads/2016/02/doc-va-ghi-file-trong-java-1.jpg)

Có 2 kiểu luồng trong Java:

![](https://giasutinhoc.vn/wp-content/uploads/2016/02/doc-va-ghi-file-trong-java-2.jpg)

Kiến trúc Input Stream (Luồng nhập dữ liệu)

![](https://giasutinhoc.vn/wp-content/uploads/2016/02/doc-va-ghi-file-trong-java-3.jpg)

Kiến trúc Output Stream (Luồng xuất dữ liệu)

![](https://giasutinhoc.vn/wp-content/uploads/2016/02/doc-va-ghi-file-trong-java-4.jpg)

![](https://giasutinhoc.vn/wp-content/uploads/2016/02/doc-va-ghi-file-trong-java-5.jpg)

### Các thao tác xử lý dữ liệu

* Bước 1: Tạo đối tượng luồng và liên kết với nguồn dữ liệu.
* Bước 2: Thao tác dữ liệu (đọc hoặc ghi hoặc cả hai).
* Bước 3: Đóng luồng.
#### Xử lý nhập xuất dữ liệu sử dụng luồng byte
Sử dụng luồng byte trong các trường hợp như nhập xuất kiểu dữ liệu nguyên thủy (như kiểu int, float, double, boolean), nhập xuất kiểu dữ liệu kiểu đối tượng (object)

Đọc và ghi dữ liệu nhị phân (binary data)
![](https://giasutinhoc.vn/wp-content/uploads/2016/02/doc-va-ghi-file-trong-java-6.jpg)

Một số phương thức xử lý dữ liệu nhị phân của class DataInputStream

![](https://giasutinhoc.vn/wp-content/uploads/2016/02/doc-va-ghi-file-trong-java-7.jpg)

Một số phương thức xử lý dữ liệu nhị phân của class DataOutputStream

![](https://giasutinhoc.vn/wp-content/uploads/2016/02/doc-va-ghi-file-trong-java-8.jpg)

Ví dụ 1: Ghi dữ liệu vào D:/input.txt với DataOutputStream
```java
import java.io.DataOutputStream;
import java.io.FileOutputStream;
import java.io.IOException;

class DataOutputExample {
    public static void main(String[] args) {
        try {
            //Bước 1: Tạo đối tượng luồng và liên kết nguồn dữ liệu
            FileOutputStream fos = new FileOutputStream("D:/input.txt");
            DataOutputStream dos = new DataOutputStream(fos);
            //Bước 2: Ghi dữ liệu
            dos.writeInt(100);
            dos.writeDouble(9.5);
            //Bước 3: Đóng luồng
            fos.close();
            dos.close();
            System.out.println("Done!");
        } catch (IOException ex) {
            ex.printStackTrace();
        }
    }
}
```
Ví dụ 2: Đọc dữ liệu chứa trong tập tin D:/input.txt với DataInputStream
```java
import java.io.DataInputStream;
import java.io.FileInputStream;
import java.io.IOException;

class DataInputExample {
    public static void main(String[] args) {
        try {
            //Bước 1: Tạo đối tượng luồng và liên kết nguồn dữ liệu
            FileInputStream fis = new FileInputStream("D:/input.txt");
            DataInputStream dis = new DataInputStream(fis);
            //Bước 2: Đọc dữ liệu
            int n = dis.readInt();
            double m = dis.readDouble();
            //Bước 3: Đóng luồng
            fis.close();
            dis.close();
            //Hiển thị nội dung đọc từ file
            System.out.println("Số nguyên: " + n);
            System.out.println("Số thực: " + m);
        } catch (IOException ex) {
            ex.printStackTrace();
        }
    }
}
```
#### Xử lý nhập xuất dữ liệu bằng luồng character

Luồng byte rất mạnh mẽ và linh hoạt. Tuy nhiên nếu muốn lưu trữ file chứa văn bản Unicode thì luồng character là lựa chọn tốt nhất vì ưu điểm của luồng character là nó thao tác trực tiếp trên ký tự Unicode.

Tất cả các luồng character đều được kế thừa từ 2 lớp Reader và Writer

Ví dụ 1: Ghi dữ liệu với FileWriter
```java
import java.io.File;
import java.io.FileWriter;
import java.io.IOException;

public class FileWriterExample {
    public static void main(String[] args) {
        try {
            //Bước 1: Tạo đối tượng luồng và liên kết nguồn dữ liệu
            File f = new File("D:/input.txt");
            FileWriter fw = new FileWriter(f);
            //Bước 2: Ghi dữ liệu
            fw.write("Ghi dữ liệu bằng luồng character");
            //Bước 3: Đóng luồng
            fw.close();
        } catch (IOException ex) {
            System.out.println("Loi ghi file: " + ex);
        }
    }
}
```
Ví dụ 2: Đọc dữ liệu với FileReader

```java
import java.io.BufferedReader;
import java.io.File;
import java.io.FileReader;
public class FileReaderExample {
    public static void main(String[] args) {
        try {
            //Bước 1: Tạo đối tượng luồng và liên kết nguồn dữ liệu
            File f = new File("D:/input.txt");
            FileReader fr = new FileReader(f);
            //Bước 2: Đọc dữ liệu
            BufferedReader br = new BufferedReader(fr);
            String line;
            while ((line = br.readLine()) != null){
                System.out.println(line);
            }
            //Bước 3: Đóng luồng
            fr.close();
            br.close();
        } catch (Exception ex) {
            System.out.println("Loi doc file: "+ex);
        }
    }
}
```
## Assertions
### Định nghĩa và cú pháp
Câu lệnh Assertion trong Java giúp phát hiện lỗi bằng cách kiểm tra đoạn mã mà người lập trình cho là đúng.

Một câu lệnh xác nhận được thực hiện bằng cách sử dụng từ khóa assert.

Cú pháp:
```java
assert điều_kiện;
```
Ở đây, điều kiện là một biểu thức boolean mà ta giả sử là đúng khi chương trình thực thi.
Dạng khác của câu lệnh Assertion
```java
assert điều_kiện : biểu_thức;
```
Trong dạng câu lệnh Assertion này, một biểu thức được chuyển tới hàm tạo của đối tượng AssertionError. Biểu thức này có một giá trị được hiển thị dưới dạng thông báo chi tiết về lỗi nếu điều kiện sai. Thông báo chi tiết được sử dụng để nắm bắt và truyền thông tin về lỗi xác nhận để giúp gỡ lỗi.
### Kích hoạt Assertion
Theo mặc định, các Assertion sẽ bị tắt và bị bỏ qua trong thời gian chạy.

Để bật nó, ta sử dụng câu lệnh sau trên cửa sổ dòng lệnh:
```java
java -ea:arguments
```
Hoặc
```java
java -enableassertions:arguments
```
Khi các Assertion được kích hoạt và điều kiện là đúng, chương trình sẽ thực thi bình thường.
Nhưng nếu điều kiện là false, JVM sẽ đưa ra một lỗi AssertionError và chương trình sẽ dừng ngay lập tức.

Ví dụ:
```java
class Main {
    public static void main(String args[]) {
        int[] a = {1,2,3};
        assert a.length == 6;
        System.out.println("Chuong trinh hoat dong binh thuong");
    }
}
```

### Tại sao lại cần sử dụng Assertion trong Java
Assertion được sử dụng bất kỳ khi nào kỹ sư phần mềm muốn kiểm tra tính đúng sai của một vấn đề trong lập trình Java.

* Để đảm bảo rằng một mã không thể truy cập thực sự không thể truy cập được
* Để đảm bảo rằng các giá trị giả định được viết trong các nhận xét là đúng
* Để đảm bảo rằng trường hợp chuyển đổi mặc định không được thực thi
* Kiểm tra trạng thái của đối tượng
* Sử dụng tại điểm, bắt đầu của phương thức
## Unit test
Unit Test có nghĩa là kiểm thử đơn vị, một bước trong kiểm thử phần mềm. Với Unit Test, chỉ có những đơn vị hay những thành phần riêng lẻ của phần mềm được kiểm thử. Mục đích là để xác định rằng mỗi đơn vị của phần mềm đều hoạt động đúng như kỳ vọng. 

Tầm quan trọng của việc viết Unit test:

* Giúp sửa bug sớm trong chu trình phát triển sản phẩm và tiết kiệm chi phí
* Giúp các lập trình viên hiểu được nền tảng mã kiểm thử và cho phép họ đưa ra các thay đổi nhanh chóng
* Có thể được sử dụng như các ghi chép về dự án, nếu hiệu quả
* Tái sử dụng code. Kết hợp cả code của bạn và kiểm thử của bạn cho dự án mới. Thay đổi code cho đến khi kiểm thử chạy được
### Unit Test trong Java
JUnit là một khung kiểm tra nguồn mở để kiểm thử đơn vị dành cho ngôn ngữ lập trình Java. Nó đóng một vai trò quan trọng trong sự phát triển theo hướng kiểm thử. JUnit là một “thành viên” của gia đình khung kiểm thử cho kiểm thử đơn vị xUnit. 

**Các tính năng của JUnit:**
* Unit là một khung kiểm tra nguồn mở, được sử dụng để viết và chạy test.
* Cung cấp annotation để định dạng các test method
* Cung cấp assertion để kiểm thử các kết quả mong đợi
* Cung cấp các trình chạy để chạy test
* Cho phép bạn viết code nhanh hơn, cải thiện chất lượng
* JUnit khá đơn giản. Nó ít phức tạp hơn và tốn ít thời gian hơn
* JUnit có thể được chạy tự động và tự kiểm tra kết quả, cung cấp phản hồi nhanh chóng. Không cần thiết phải xem xét thủ công các báo cáo về kết quả kiểm thử. 
* Các bài test JUnit có thể được tổ chức thành các bộ kiểm thử chứa các trường hợp kiểm thử, thậm chí là chứa các bộ kiểm thử khác. 
* JUnit thể hiện tiến độ kiểm thử trên một thanh. Nếu thanh có màu xanh, bài test đang chạy êm ả. Ngược lại, nếu thanh chuyển đỏ, tức là bài test thất bại.