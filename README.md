# Autosar Classic

## 1.Khái niệm

AUTOSAR là một tiêu chuẩn toàn cầu cho phát triển phần mềm ô tô với mục tiêu là chuẩn hóa kiến trúc phần mềm cho các hệ thống điều khiển điện tử (ECU) trong ô tô, nhằm tăng tính khả chuyển, khả mở rộng và giảm chi phí phát triển.

AUTOSAR được chia làm 2 loại:

- **Autosar Classic**: Dành cho các hệ thống điều khiển điện tử (ECU) truyền thống với các yêu cầu thời gian thực khắt khe.
- **Autosar Adaptive**: Dành cho các ứng dụng có tính năng linh hoạt, yêu cầu khả năng tính toán cao hơn, như lái xe tự động và kết nối. Nó hỗ trợ khả năng cập nhật phần mềm theo thời gian thực và phát triển dựa trên các tiêu chuẩn hiện đại như POSIX.

## 2.Kiến trúc phân lớp

![image](https://github.com/user-attachments/assets/bc2cfbe7-140f-4063-a433-e0eb6ce28961)


### Application layer

Application layer (lớp ứng dụng) là một thành phần quan trọng trong kiến ​​trúc phần mềm tổng thể của các hệ thống ô tô, bao gồm nhiều thành phần phần mềm (Software Component - SWC) thực hiện tập hợp các tác vụ để hoàn thành chức năng của xe.

<br>

Mỗi SWC chứa nhiều chương trình, logic ứng dụng của hệ thống và không tương tác trực tiếp với phần cứng.

<br>

SWC giao tiếp với phần cứng thông qua RTE.

<br>

### Runtime Enviroment (RTE)

RTE đóng vai trò quan trọng trong việc kết nối Software Components (SWC) và Basic Software (BSW) thông qua một kiến trúc trừu tượng.

Khi SWC cần truy cập phần cứng, chúng không tương tác trực tiếp mà thông qua RTE. RTE sẽ xử lý việc ánh xạ này tới các thành phần BSW, bao gồm các lớp như Microcontroller Abstraction Layer (MCAL) và ECU Abstraction Layer.

Ví dụ: 

- 1 SWC cần gửi dữ liệu qua giao thức CAN: SWC sẽ gửi yêu cầu này qua cổng của mình, RTE sẽ ánh xạ tới lớp CAN Driver trong MCAL của BSW.
- SWC cần đọc cảm biến: RTE sẽ ánh xạ yêu cầu này tới lớp Driver của cảm biến trong MCAL.

<br>

### Basic software (BSW)

Basic Software (BSW) là một trong ba thành phần chính của kiến trúc AUTOSAR, đóng vai trò nền tảng để hỗ trợ phần mềm ứng dụng (SWC) hoạt động trên phần cứng. BSW cung cấp các dịch vụ cơ bản như quản lý phần cứng, giao tiếp, chẩn đoán, và các dịch vụ hệ thống.

BSW được chia thành 3 lớp chính:

- **Service Layer** (Lớp dịch vụ).
- **ECU Abstraction Layer** (Lớp trừu tượng hóa ECU).
- **Microcontroller Abstraction Layer - MCAL** (lớp trừu tượng hóa vi điều khiển)

<br>

#### Service Layer

Đây là lớp cao nhất trong BSW, cung cấp các dịch vụ hệ thống và tiện ích cho các phần mềm ứng dụng (SWC) và các lớp khác của BSW. Các dịch vụ này bao gồm quản lý thời gian thực, chẩn đoán, quản lý lỗi, quản lý nguồn, v.v.

- **OS (Operating System)**: Cung cấp các chức năng của hệ điều hành thời gian thực, bao gồm quản lý task, quản lý tài nguyên và đồng bộ hóa.
- **Memory Services**: Quản lý bộ nhớ không chỉ đọc/ghi mà còn các dịch vụ liên quan đến bảo mật dữ liệu, như Flash EEPROM.
- **Diagnostic Services**: Quản lý và xử lý chẩn đoán hệ thống, bao gồm chẩn đoán giao tiếp và xử lý lỗi.

<br>

#### ECU Abstraction Layer

Lớp này cung cấp một giao diện trừu tượng cho tất cả các thiết bị ngoại vi và phần cứng cụ thể của ECU. Nó ẩn đi sự khác biệt về phần cứng của các thiết bị ngoại vi khác nhau và cung cấp một giao diện tiêu chuẩn cho các lớp bên trên (Service Layer và SWC).

- **I/O Hardware Abstraction (IoHwAb)**: đóng vai trò giao tiếp SWC và phần cứng, giúp trừu tượng hóa việc truy cập các thiết bị ngoại vi. Nó sẽ chuyển đổi các yêu cầu từ SWC thành các lệnh mà phần cứng có thể hiểu, giúp ứng dụng có thể dễ dàng tương thích với nhiều loại phần cứng khác nhau.
- **Communication Hardware Abstraction**: Hỗ trợ giao tiếp với các mạng truyền thông khác nhau, ví dụ như các giao thức truyền thông nội bộ ECU hoặc mạng xe.
- **Memory Hardware Abstraction**: Cung cấp giao diện để truy cập các loại bộ nhớ khác nhau mà không quan tâm đến cách thức thực hiện cụ thể.

<br>

#### MCAL

Đây là lớp thấp nhất trong BSW, cung cấp giao diện trừu tượng để tương tác trực tiếp với các thành phần phần cứng của vi điều khiển, chẳng hạn như bộ xử lý trung tâm (CPU), các thiết bị ngoại vi tích hợp (như ADC, PWM, UART), và các bộ định thời (timer).

- **Microcontroller Drivers**: Điều khiển các tính năng cụ thể của vi điều khiển như bộ định thời (timer), bộ watchdog.
- **Memory Drivers**: Hỗ trợ giao tiếp, cấu hình với các bộ nhớ khác nhau như RAM, EEPROM, Flash, v.v.
- **Crypto Drivers**: Cung cấp các chức năng mã hóa hoặc giải mã.
- **Wireless Communication Drivers**: Hỗ trợ các giao thức truyền thông không dây như Bluetooth, WiFi.
- **Communication Drivers**: Hỗ trợ giao tiếp với CAN, LIN, SPI, Ethernet, v.v.
- **I/O Drivers**: Cung cấp cấu hình về ADC, PWM, ICU, v.v, .


### B. Quá trình compile (Compiler Process)

Quá trình compile sẽ bao gồm các bước sau:

- ****Preprocessing**** (Tiền xử lý):

    - Quá trình sẽ chuyển các file (.c;.cpp;.h) sang file .i, với cú pháp để thực hiện quá trình trong terminal (IDE VScode) như sau: ``` gcc -E file.c -o file.i ```
    - Quá trình này bao gồm các công việc:

        + **Include Header**: Tìm kiếm và chèn mã nguồn

        ```cpp
        Ex.h (Ví dụ cho file.h)
        #ifndef EX_H
        #define EX_H
        printf("Đây là file.h");
        #endif
        
        file.c (Ví dụ cho file.c)
        #include "Ex.h"
        printf("Đây là dòng code dưới dòng code trong file.h");
        
        file.i (Quá trình preprocessing sẽ biên dịch từ file.c sang file.i như sau)
        #ifdef EX_H
        #define EX_H
        printf("Đây là file.h");
        #endif
        printf("Đây là dòng code dưới dòng code trong file.h");
        ```

        + **Delete Comment**: Xóa đi các dòng comment

        ```cpp
        file.c
        // Dòng này sẽ bị xóa
        printf("Dòng code này thì không bị xóa");
        
        file.i (Khi này file.i sẽ không còn dòng commnent nữa)
        printf("Dòng code này thì không bị xóa");
        ```

        + **Expand Macro**: Thay thế các macro, chỉ có tác dụng thay thế như thay thế văn bản

        ```cpp
        file.c
        #define LED 17
        #define BUZZER 16
        
        digitalWrite(LED, HIGH);
        digitalWrite(BUZZER, LOW);
        
        file.i
        digitalWrite(17, HIGH);
        digitalWrite(16, LOW);
        ```
