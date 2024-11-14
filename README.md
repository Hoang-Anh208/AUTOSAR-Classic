# Autosar Classic

## 1.Khái niệm

AUTOSAR là một tiêu chuẩn toàn cầu cho phát triển phần mềm ô tô với mục tiêu là chuẩn hóa kiến trúc phần mềm cho các hệ thống điều khiển điện tử (ECU) trong ô tô, nhằm tăng tính khả chuyển, khả mở rộng và giảm chi phí phát triển.

AUTOSAR được chia làm 2 loại:

- **Autosar Classic**: Dành cho các hệ thống điều khiển điện tử (ECU) truyền thống với các yêu cầu thời gian thực khắt khe.
- **Autosar Adaptive**: Dành cho các ứng dụng có tính năng linh hoạt, yêu cầu khả năng tính toán cao hơn, như lái xe tự động và kết nối. Nó hỗ trợ khả năng cập nhật phần mềm theo thời gian thực và phát triển dựa trên các tiêu chuẩn hiện đại như POSIX.

## 2.Kiến trúc phân lớp

![image](https://github.com/user-attachments/assets/bc2cfbe7-140f-4063-a433-e0eb6ce28961)


### Application layer

Application layer (lớp ứng dụng) là một thành phần quan trọng trong kiến ​​trúc phần mềm tổng thể của các hệ thống ô tô, bao gồm nhiều thành phần phần mềm (Software Component - SWC) thực hiện tập hợp các tác vụ để hoàn thành chức năng của xe. Mỗi SWC chứa nhiều chương trình có thể chạy được. Các thành phần phần mềm (SWC) này được kết nối với sự trợ giúp của các cổng (Ports). Các cổng này cung cấp khả năng giao tiếp giữa hai SWC cũng như với BSW.

![image](https://github.com/user-attachments/assets/4af64466-7c55-4c37-8ea8-a792fbb07e5d)


### Runtime Enviroment (RTE)

RTE đóng vai trò quan trọng trong việc kết nối Software Components (SWC) và Basic Software (BSW) thông qua một kiến trúc trừu tượng.

Khi SWC cần truy cập phần cứng, chúng không tương tác trực tiếp mà thông qua RTE. RTE sẽ xử lý việc ánh xạ này tới các thành phần BSW, bao gồm các lớp như Microcontroller Abstraction Layer (MCAL) và ECU Abstraction Layer.

Ví dụ: 

- 1 SWC cần gửi dữ liệu qua giao thức CAN: SWC sẽ gửi yêu cầu này qua cổng của mình, RTE sẽ ánh xạ tới lớp CAN Driver trong MCAL của BSW.
- SWC cần đọc cảm biến: RTE sẽ ánh xạ yêu cầu này tới lớp Driver của cảm biến trong MCAL.



### Basic software (BSW)

Basic Software (BSW) là một trong ba thành phần chính của kiến trúc AUTOSAR, đóng vai trò nền tảng để hỗ trợ phần mềm ứng dụng (SWC) hoạt động trên phần cứng. BSW cung cấp các dịch vụ cơ bản như quản lý phần cứng, giao tiếp, chẩn đoán, và các dịch vụ hệ thống.

BSW được chia thành 3 lớp chính:

- **Service Layer** (Lớp dịch vụ).
- **ECU Abstraction Layer** (Lớp trừu tượng hóa ECU).
- **Microcontroller Abstraction Layer - MCAL** (lớp trừu tượng hóa vi điều khiển)

#### Service Layer

#### ECU Abstraction Layer

#### MCAL


