# Vấn đề hình thoi (the Diamond problem)

Trong Java, vấn đề hình thoi có liên quan đến đa kế thừa. Đa kế thừa là một đặc điểm của khái niệm hướng đối tượng, trong đó một lớp có thể kế thừa các thuộc tính của nhiều hơn một lớp cha. Tính năng tạo ra một vấn đề khi tồn tại các phương thức có cùng tên và chữ ký trong cả super-class và sub-class. Khi chúng ta gọi phương thức, trình biên dịch bị nhầm lẫn và không thể xác định phương thức lớp nào được gọi và thậm chí khi gọi phương thức lớp nào được ưu tiên.

Ví dụ: 
```java
    class A {
        public void display() {
            System.out.println("class A display() method called");
        }
    }

    class B extends A {
        @Override
        public void display() {
            System.out.println("class B display() method called");
        }
    }

    class C extends A {
        @Override
        public void display() {
            System.out.println("class C display() method called");
        }
    }

    // not supported in Java
    public class D extends B,C
    {
        public static void main(String args[]) {
            D d = new D();
            // creates ambiguity which display() method to call
            d.display();
        }
    }
```
- Lớp B và lớp C kế thừa lớp A. Phương thức display () của lớp A bị lớp B và lớp C ghi đè.
- Lớp D kế thừa lớp B và lớp C (không hợp lệ trong Java). Giả sử rằng chúng ta cần gọi phương thức display () bằng cách sử dụng đối tượng của lớp D, trong trường hợp này trình biên dịch Java không biết phương thức display () nào cần gọi. Do đó, nó tạo ra sự mơ hồ. Java được thiết kế theo tiêu chí đơn giản, nên nó không cho phép một lớp được thừa kế từ nhiều hơn một lớp cha.
Vậy ta phải giải quyết bài toán như thế nào với Java?


# Interface

> *Ta nói đến khái niệm interface với ý nghĩa là một cấu trúc lập trình của Java được định nghĩa với từ khóa interface (tương tự như cấu trúc lớp được định nghĩa với từ khóa class).*

Giải pháp mà Java cung cấp là interface. Cấu trúc interface này cho phép ta giải quyết bài toán đa thừa kế, cho ta hưởng phần lớn các ích lợi mang tính đa hình mà đa thừa kế mang lại, nhưng tránh cho ta các rắc rối nhập nhằng ngữ nghĩa như đã giới thiệu trong mục trước. 

Nguy cơ nhập nhằng ngữ nghĩa được tránh bằng cách rất đơn giản: 
> **phương thức nào cũng phải trừu tượng! Theo đó, lớp con buộc phải cài đặt các phương thức. Nhờ vậy, khi chương trình chạy, máy ảo Java không phải bối rối lựa chọn giữa hai phiên bản mà một đối tượng được thừa kế.**

Giống như một lớp thuần túy trừu tượng bao gồm toàn các phương thức trừu tượng và không có biến thực thể. Nhưng về cú pháp thì interface có khác lớp trừu tượng một chút. Để định nghĩa một interface, ta dùng từ khóa interface thay vì class như đối với lớp:
```java
    public interface Pet {...}
```

Đối với một lớp trừu tượng, ta cần tạo lớp con cụ thể. Còn đối với một interface, ta tạo lớp cài đặt các phương thức trừu tượng mà interface đó đã quy định. Lớp đó được gọi là lớp cài đặt interface mà ta đang nói đến. Để khai báo rằng một lớp cài đặt một interface, ta dùng từ khóa implements thay vì extends, theo sau là tên của interface. Một lớp có thể cài đặt một vài interface và đồng thời là lớp con của một lớp khác. Chẳng hạn lớp Dog vừa là lớp con của Canine, vừa là lớp cài đặt interface Pet:
```java
    class Dog extends Canine implements Pet {...}
```

Như vậy ta có thể dùng cấu trúc interface để thực hiện một thứ gần giống đa thừa kế. Nó không hẳn là đa thừa kế ở chỗ: khác với lớp trừu tượng, ta không thể đặt mã cài đặt tại các interface.

Khi các phương thức tại interface đều trừu tượng, và do đó không thể tái sử dụng, ta được ích lợi gì ở đây? 
> *Câu trả lời là đa hình và đa hình. Khi ta dùng một interface thay cho các lớp riêng biệt làm tham số và giá trị trả về của phương thức, ta có thể truyền lớp bất kì nào cài đặt interface đó vào vị trí của tham số hay giá trị trả về đó. ***Không chỉ có vậy, các lớp nằm trên các cây thừa kế khác nhau có thể cùng cài đặt một interface.****