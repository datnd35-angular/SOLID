# SOLID 

Mục đích đảm bảo hệ thống phần mềm trở nên dễ bảo trì, dễ mở rộng và giảm thiểu sự phụ thuộc lẫn nhau, cho dù bạn đang làm việc với lớp, module hay bất kỳ thành phần phần mềm nào.

SOLID chủ yếu liên quan đến việc thiết kế và tổ chức **lớp** (class) và **đối tượng** (objects), nhưng mục tiêu của SOLID không chỉ xoay quanh việc thiết kế lớp, mà là để đảm bảo hệ thống phần mềm trở nên dễ bảo trì, dễ mở rộng và giảm thiểu sự phụ thuộc lẫn nhau, cho dù bạn đang làm việc với lớp, module hay bất kỳ thành phần phần mềm nào.

=> Tư duy phát triển phần mềm.

## Cụ thể về từng nguyên tắc trong SOLID:

### **I. Single Principle (SRP)**: 
**1. Ví Dụ Dụng Cụ Đa Năng**:

![image](https://github.com/user-attachments/assets/04ae161e-4159-425e-b569-17bb81754248)

  - Dụng cụ đa năng kết hợp nhiều chức năng như kìm, búa, cưa, thước, v.v., nhưng có nhược điểm về hiệu năng: các chức năng như cưa, búa quá bé, thước ngắn, và không có đầu kìm để vặn.
  - Khi thực hiện công việc cụ thể như gõ đinh, thay vì dùng búa, bạn phải dùng dụng cụ đa năng với nhiều công cụ kết hợp, gây cồng kềnh.
  - Nếu một phần trong dụng cụ đa năng bị hỏng, bạn phải tháo toàn bộ và sửa chữa, gây khó khăn trong việc duy trì và mở rộng.

- **Kết Luận**: Dụng cụ đa năng quá cồng kềnh, phức tạp, và khó duy trì, mở rộng.

**2. Áp Dụng trong Lập Trình**:
  - Giống như dụng cụ đa năng, một class có quá nhiều chức năng sẽ trở nên cồng kềnh và khó duy trì.
  - Khi ứng dụng phát triển và yêu cầu thay đổi, class quá phức tạp sẽ khó thay đổi, tốn thời gian sửa chữa và có thể ảnh hưởng đến các module khác.
  - **Giải Pháp**: Để dễ bảo trì và mở rộng, mỗi class nên chỉ chịu trách nhiệm một tính năng duy nhất. Nếu có đoạn code không thuộc trách nhiệm của class, hãy tách nó ra thành một class riêng biệt.

### **II. Open/Closed Principle (OCP)**: 

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
- Bản chất của nó là chi nhỏ ra,  tránh liên quan tới nhau, để dễ kiểm soát và thực thi phù hợp với từng mục đích của từng đối tượng.
- Mình có thể liên tưởng về nhiều `class extend một class chung`, hay nhiều `class của nhiều đối tượng khác nhau implement cùng một interface chung`.

### **III. Liskov  Principle (LSP Nguyên tắc thay thế Liskov)**: 

**1. Nội dung chính trong nguyên lý này:**

> **Một lớp con phải có khả năng thay thế lớp cha mà không làm thay đổi tính đúng đắn của chương trình.**  

Điều này có nghĩa là:
- Nếu bạn có một đoạn mã hoạt động đúng khi sử dụng một đối tượng của lớp cha, thì nó cũng phải hoạt động đúng khi bạn thay thế đối tượng đó bằng một đối tượng của lớp con.
- Các lớp con không được thay đổi hành vi của lớp cha theo cách vi phạm các giả định mà người dùng lớp cha đã đặt ra.

**2. Ý nghĩa**
LSP đảm bảo rằng khi sử dụng kế thừa trong lập trình hướng đối tượng:
1. **Tính đúng đắn của hệ thống**: Các lớp con phải duy trì logic mà lớp cha đã định nghĩa.
2. **Khả năng mở rộng**: Lớp con có thể mở rộng chức năng nhưng không được làm phá vỡ chức năng hiện tại.
3. **Tăng tính tái sử dụng**: Người dùng chỉ cần làm việc với các interface hoặc lớp cha mà không cần quan tâm đến các chi tiết cụ thể của lớp con.


**3. Ví dụ**
**Vi phạm LSP**
Giả sử bạn có lớp `Rectangle` và lớp con `Square`:

```typescript
class Rectangle {
  constructor(protected width: number, protected height: number) {}

  setWidth(width: number): void {
    this.width = width;
  }

  setHeight(height: number): void {
    this.height = height;
  }

  getArea(): number {
    return this.width * this.height;
  }
}

class Square extends Rectangle {
  setWidth(width: number): void {
    this.width = this.height = width; // Đảm bảo width và height bằng nhau
  }

  setHeight(height: number): void {
    this.width = this.height = height; // Đảm bảo width và height bằng nhau
  }
}
```

**Vấn đề**: 
- `Square` thay đổi hành vi của lớp `Rectangle`.
- Nếu một đoạn mã sử dụng `Rectangle` mong đợi hành vi của hình chữ nhật (chiều rộng và chiều cao có thể khác nhau), thì việc sử dụng `Square` sẽ làm sai lệch kết quả.

**Cách sửa**
Thay vì kế thừa, bạn có thể sử dụng một interface hoặc một lớp cơ sở trừu tượng chung:

```typescript
interface Shape {
  getArea(): number;
}

class Rectangle implements Shape {
  constructor(private width: number, private height: number) {}

  getArea(): number {
    return this.width * this.height;
  }
}

class Square implements Shape {
  constructor(private side: number) {}

  getArea(): number {
    return this.side * this.side;
  }
}
```

**3. Ứng dụng khác**
- **Database Design** Nếu bạn thay đổi cấu trúc bảng hoặc bổ sung bảng, các query hoặc view cũ không được bị phá vỡ.
- **Microservices** Nếu một microservice được cập nhật để thêm một feature mới, nó không được phá vỡ cách các microservices khác tương tác với nó.

### **IV. Interface  Principle (ISP Nguyên tắc phân tách giao diện)**

**1. Nội dụng dung chính nguyên lý này**
> **"Một giao diện, class, hay thập chí là một khí cạnh phần mềm nào đó không nên ép các lớp triển khai các phương thức hay cách tính năng, modules mà chúng không sử dụng."**

**2. Ví dụ**

#### Không tuân thủ ISP:
Giả sử bạn có một giao diện `Animal` với các phương thức `eat()`, `sleep()`, và `fly()`:

```typescript
interface Animal {
    eat(): void;
    sleep(): void;
    fly(): void;
}

class Dog implements Animal {
    eat() {
        console.log("Dog is eating");
    }
    
    sleep() {
        console.log("Dog is sleeping");
    }
    
    fly() {
        // Lớp Dog không biết bay, nhưng vẫn phải triển khai phương thức này
        throw new Error("Dogs cannot fly");
    }
}

class Bird implements Animal {
    eat() {
        console.log("Bird is eating");
    }
    
    sleep() {
        console.log("Bird is sleeping");
    }
    
    fly() {
        console.log("Bird is flying");
    }
}
```

Trong ví dụ trên, lớp `Dog` không có khả năng bay, nhưng vẫn phải triển khai phương thức `fly()` mặc dù nó không sử dụng. Điều này vi phạm ISP.

#### Tuân thủ ISP:

```typescript
interface Eater {
    eat(): void;
}

interface Sleeper {
    sleep(): void;
}

interface Flyer {
    fly(): void;
}

class Dog implements Eater, Sleeper {
    eat() {
        console.log("Dog is eating");
    }
    
    sleep() {
        console.log("Dog is sleeping");
    }
}

class Bird implements Eater, Sleeper, Flyer {
    eat() {
        console.log("Bird is eating");
    }
    
    sleep() {
        console.log("Bird is sleeping");
    }
    
    fly() {
        console.log("Bird is flying");
    }
}
```

**3. Ứng dụng khác**


- **API Design (Thiết kế API)** Khi thiết kế API, ISP giúp đảm bảo rằng các endpoints không cung cấp các chức năng không liên quan hoặc không cần thiết.
  
    Tách thành các endpoint nhỏ hơn:
    - `/users/{id}/profile` – Trả về thông tin cơ bản của người dùng.
    - `/users/{id}/transactions` – Trả về lịch sử giao dịch.
    - `/users/{id}/activities` – Trả về lịch sử hoạt động.

- **Microservices Architecture** Trong kiến trúc microservices, ISP đảm bảo rằng mỗi service chỉ chịu trách nhiệm cho một nhóm chức năng cụ thể và không ép buộc các service khác tương tác với những chức năng không cần thiết.

    Giả sử có một service quản lý người dùng và dịch vụ thanh toán:
    
    - Nếu service người dùng cần gọi đến service thanh toán để lấy dữ liệu không cần thiết (như thông tin chi tiết hóa đơn thay vì chỉ trạng thái thanh toán), điều này vi phạm ISP.
    
    **Sửa chữa:**
    Tách thành các service nhỏ hơn:
    - **UserService** – Quản lý thông tin người dùng.
    - **PaymentService** – Quản lý thanh toán.
    - **BillingService** – Quản lý hóa đơn.
      
- **Database Design** Trong thiết kế cơ sở dữ liệu, ISP có thể được áp dụng thông qua việc phân chia các bảng dữ liệu hoặc quan hệ để đảm bảo tính tách biệt.

    Một bảng `Users` lưu trữ thông tin người dùng và bao gồm cả dữ liệu không liên quan như lịch sử giao dịch hoặc cài đặt hệ thống.
    
    **Sửa chữa:**
    Tách bảng thành:
    - `Users` – Lưu thông tin cơ bản người dùng.
    - `Transactions` – Lưu thông tin giao dịch.
    - `Settings` – Lưu thông tin cài đặt.

**4. Lưu ý**
- Có thể ta thấy Interface  Principle khá giống với Single Principle nhưng bản chất của chúng là giống nhau. Single Principle nó tập trung vào mục đích nhất định ví dụ: chúng chia ra Services Payment hoăck Service Customer, v.v, mà không quan tâm đến phương thức trong service đó như get, update, delete, add liệu có cái nào không dùng ko. Interface  Principle thì nó quan tâm đến các phương thức này liệu có dùng không, có nên chia nhỏ ra không mà ko.

### **V. Dependency Inversion Principle (DIP Nguyên tắc đảo ngược phụ thuộc)**

**1. Có hai nội dung chính trong nguyên lý này:**

- Nhấn mạnh rằng các `module cấp cao (high-level modules)` không nên phụ thuộc vào các `module cấp thấp (low-level modules)`. Cả hai nên phụ thuộc vào `abstraction (tức là interface hoặc abstract class)`.
- `abstraction không nên phụ thuộc vào chi tiết cụ thể (details)`, mà ngược lại, `chi tiết cụ thể nên phụ thuộc vào abstraction`.

**2. Ví dụ**

Khi xây dựng các module giao tiếp, chẳng hạn như gửi email, tin nhắn SMS, hoặc thông báo đẩy, bạn có thể áp dụng DIP để giảm phụ thuộc vào các dịch vụ cụ thể.

```
interface Notifier {
  sendNotification(message: string): void;
}

class EmailNotifier implements Notifier {
  sendNotification(message: string): void {
    console.log(`Sending email: ${message}`);
  }
}

class SMSNotifier implements Notifier {
  sendNotification(message: string): void {
    console.log(`Sending SMS: ${message}`);
  }
}

class NotificationService {
  constructor(private notifier: Notifier) {}

  notify(message: string): void {
    this.notifier.sendNotification(message);
  }
}
```
- **High-level modules:** là `NotificationService` và nó ko nên phụ thuộc vào **low-level modules** như `EmailNotifier hay SMSNotifier`  mà nó nên phụ thuộc vào `abstraction(Notifier)`.
- **Abstraction (Notifier)** không nên phụ thuộc vào chi tiết cụ thể nghĩa là ko có `implement` nào trong `Notifier` nó ko cần biết `message` củ thể là gì. Mà ngược lại `message` phải xét xem trong `bstraction` này mình có được sử dụng hay không.

**3. Ứng dụng khác**
- **evOps và CI/CD** Sử dụng abstraction để quản lý các môi trường (staging, production) thay vì viết các pipeline cụ thể cho từng môi trường.
- **Tích hợp bên thứ ba (Third-Party Integration)** Khi sử dụng dịch vụ thanh toán (PayPal, Stripe), bạn có thể tạo một `abstraction PaymentGateway` thay vì tích hợp trực tiếp với từng dịch vụ cụ thể.

**Lưu ý**
- DIP Không phụ thuộc trực tiếp vào các chi tiết cụ thể.
- DIP tập trung vào abstraction để tạo ra hệ thống linh hoạt và dễ bảo trì.
- DIP thường được thực thi thông qua các pattern như `Dependency Injection` và `Factory Pattern`.
