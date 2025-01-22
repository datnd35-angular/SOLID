# SOLID 

Mục đích đảm bảo hệ thống phần mềm trở nên dễ bảo trì, dễ mở rộng và giảm thiểu sự phụ thuộc lẫn nhau, cho dù bạn đang làm việc với lớp, module hay bất kỳ thành phần phần mềm nào.

SOLID chủ yếu liên quan đến việc thiết kế và tổ chức **lớp** (class) và **đối tượng** (objects), nhưng mục tiêu của SOLID không chỉ xoay quanh việc thiết kế lớp, mà là để đảm bảo hệ thống phần mềm trở nên dễ bảo trì, dễ mở rộng và giảm thiểu sự phụ thuộc lẫn nhau, cho dù bạn đang làm việc với lớp, module hay bất kỳ thành phần phần mềm nào.

## Cụ thể về từng nguyên tắc trong SOLID:

### **Single Principle (SRP)**: 

**1. Có hai nội dung chính trong nguyên lý này:**
  
  - Hạn chế sửa đổi: Không nên chỉnh sửa source code của một module hoặc một class có sẵn, vì sẽ làm ảnh hướng đến tính đúng đắn của chương trình.
  
  - Ưu tiên mở rộng: Khi cần thêm tính năng mới, nên kế thừa và mở rộng các module/class có sẵn thành các module con lớn hơn. Các module/class con vừa có các đặc tính của lớp cha (đã được kiểm chứng đúng đắn), vừa được bổ sung tính năng mới phù hợp với yêu cầu.
  
  ![image](https://github.com/user-attachments/assets/6409dfaf-aab8-4095-972e-7b7c05a94c6a)
  
  Máy ảnh có bộ phận đèn flash nhưng công suất yếu, người dùng muốn đèn công suất cao hơn... Thay vì tháo rỡ và sửa chữa các bộ phận của máy ảnh, chúng ta có thể ráp thêm một cái flash rời.
  
**2. Ví Dụ**

Giả sử bạn đang phát triển một hệ thống tính lương cho nhân viên của một công ty. Hệ thống này có một lớp `SalaryCalculator` dùng để tính lương cho các nhân viên. Ban đầu, chỉ có hai loại nhân viên: nhân viên toàn thời gian và nhân viên bán thời gian.

**Thiết Kế Ban Đầu (Không Tuân Thủ OCP):**

```java
public class SalaryCalculator {
    public double calculateSalary(Employee employee) {
        if (employee instanceof FullTimeEmployee) {
            FullTimeEmployee fullTimeEmployee = (FullTimeEmployee) employee;
            return fullTimeEmployee.getMonthlySalary();
        } else if (employee instanceof PartTimeEmployee) {
            PartTimeEmployee partTimeEmployee = (PartTimeEmployee) employee;
            return partTimeEmployee.getHourlyRate() * partTimeEmployee.getWorkedHours();
        }
        return 0;
    }
}
```

Trong thiết kế này:
- `SalaryCalculator` cần phải sửa đổi mỗi khi có một loại nhân viên mới (ví dụ: nhân viên hợp đồng, nhân viên tạm thời, v.v.).
- Việc thêm loại nhân viên mới yêu cầu thay đổi trong lớp `SalaryCalculator`, vi phạm nguyên lý Mở - Đóng vì lớp này không "đóng" với sự thay đổi.

**Refactor Để Tuân Thủ OCP:**

```java
// Giao diện chung cho các loại nhân viên
public interface Employee {
    double calculateSalary();
}

// Nhân viên toàn thời gian
public class FullTimeEmployee implements Employee {
    private double monthlySalary;

    public FullTimeEmployee(double monthlySalary) {
        this.monthlySalary = monthlySalary;
    }

    @Override
    public double calculateSalary() {
        return monthlySalary;
    }
}

// Nhân viên bán thời gian
public class PartTimeEmployee implements Employee {
    private double hourlyRate;
    private int workedHours;

    public PartTimeEmployee(double hourlyRate, int workedHours) {
        this.hourlyRate = hourlyRate;
        this.workedHours = workedHours;
    }

    @Override
    public double calculateSalary() {
        return hourlyRate * workedHours;
    }
}

// Lớp tính lương không cần thay đổi khi thêm loại nhân viên mới
public class SalaryCalculator {
    public double calculateSalary(Employee employee) {
        return employee.calculateSalary();
    }
}
```
**Lưu ý**
- Bản chất của nó là chi nhỏ ra,  tránh liên quan tới nhau, để dễ kiểm soát và thực thi phù hợp với từng mục đích của từng đối tượng và có thể sử dụng lại.
- Mình có thể liên tưởng về nhiều `class extend một class chung`, hay nhiều `class của nhiều đối tượng khác nhau implement cùng một interface chung`.

3. **Open/Closed Principle (OCP)**: Nguyên tắc này áp dụng cho cả lớp và module, giúp hệ thống có thể mở rộng dễ dàng mà không cần thay đổi mã hiện tại. Việc tạo ra các lớp con hoặc module mới để mở rộng chức năng, thay vì thay đổi các lớp hiện tại, là một phần quan trọng của OCP.

4. **Liskov Substitution Principle (LSP)**: Nguyên tắc này giúp đảm bảo rằng các lớp con có thể thay thế lớp cha mà không gây ra lỗi hoặc thay đổi hành vi của hệ thống, điều này đặc biệt liên quan đến kế thừa và đa hình.

5. **Interface Segregation Principle (ISP)**: Nguyên tắc này chủ yếu áp dụng cho **giao diện** (interface), giúp tránh việc có các giao diện quá phức tạp và khó sử dụng cho các lớp không cần đến tất cả các phương thức trong đó.

6. **Dependency Inversion Principle (DIP)**: Nguyên tắc này áp dụng cho cách mà các lớp và module phụ thuộc vào nhau. Thay vì các lớp phụ thuộc vào các chi tiết cụ thể (concrete implementations), chúng nên phụ thuộc vào các trừu tượng (abstracts) như interface hoặc abstract class.

### Tóm lại:
Mặc dù SOLID tập trung vào việc thiết kế lớp và đối tượng, nhưng các nguyên tắc này cũng có thể áp dụng cho các **module**, **giao diện** và các thành phần phần mềm khác. Mục đích của SOLID là giúp tạo ra một hệ thống phần mềm dễ duy trì, mở rộng và dễ hiểu, không chỉ liên quan đến lớp mà còn cả cách thức các lớp tương tác với nhau.
