#Auto_Feeding_Machine


Đây là thành phần cốt lõi của dự án AquaNova, chịu trách nhiệm thu thập dữ liệu cảm biến, xử lý logic điều khiển tại chỗ và quản lý cơ cấu chấp hành cho hệ thống hồ cá thông minh.

Kiến trúc phần cứng (Hardware Architecture)
Hệ thống sử dụng cấu trúc xử lý song song với hai vi điều khiển chính:

Bộ xử lý trung tâm (MCU1 - STM32F103C8T6): Chịu trách nhiệm chính trong việc đọc dữ liệu ADC từ cảm biến, điều khiển màn hình LCD và quản lý động cơ Servo.

Module truyền thông (MCU2 - ESP32-C3 Mini): Đóng vai trò làm cầu nối (Gateway) để truyền nhận dữ liệu qua Wi-Fi và xử lý giao tiếp UART với STM32.


Các khối chức năng chính:
Khối cảm biến (Perception):

Nhiệt độ: Cảm biến DS18B20 (chống nước, sai số ±0.5°C).

Độ đục: Turbidity Sensor (MJKDZ) đo độ trong của nước qua tín hiệu Analog.

Mức thức ăn: Cảm biến siêu âm HC-SR04 đo khoảng cách bằng hiện tượng phản xạ sóng âm.

Khối chấp hành (Actuators):

Động cơ Servo SG90: Điều khiển tấm gạt cơ khí để định lượng thức ăn.


Hệ thống hiển thị: Màn hình LCD 16x2 giao tiếp qua I2C để hiển thị thông số thời gian thực.

Khối thời gian thực: Module RTC DS3231 giúp duy trì lịch trình cho ăn ngay cả khi mất kết nối Internet.

Giao thức kết nối nội bộ
Hệ thống nhúng AquaNova sử dụng các chuẩn giao tiếp công nghiệp để kết nối các thành phần:

UART: Truyền dữ liệu giữa STM32 và ESP32 với tốc độ baud cấu hình sẵn (thường là 9600 hoặc 115200).

I2C: Kết nối STM32 với màn hình LCD và module RTC DS3231.

1-Wire: Sử dụng để giao tiếp với cảm biến nhiệt độ DS18B20.

PWM: Điều khiển góc quay của Servo SG90 để thực hiện thao tác cho ăn.


Thiết kế cơ khí & In 3D
Phần nhúng còn bao gồm thiết kế cơ khí cho bộ cấp liệu tự động được thực hiện trên Autodesk Inventor:

Cơ chế: Tận dụng trọng lực với phễu chứa hình nón.

Tấm gạt (Van chặn): Gắn trực tiếp vào trục Servo; khi Servo quay 45°, khe hở sẽ mở ra để thức ăn rơi xuống.


Vật liệu: In 3D bằng nhựa PLA trên máy in Bambu.


 Luồng xử lý Firmware
STM32 (Lập trình qua KeilC/STM32CubeMX):

Khởi tạo các ngoại vi (GPIO, ADC, I2C, UART, PWM).

Đọc giá trị nhiệt độ từ DS18B20 và giá trị Analog từ cảm biến độ đục.

Tính toán lượng thức ăn còn lại dựa trên khoảng cách từ HC-SR04.

Đóng gói dữ liệu thành chuỗi (String) và gửi sang ESP32 qua UART.

Lắng nghe lệnh từ UART để điều khiển Servo hoặc bật/tắt đèn.

ESP32 (Lập trình qua Arduino IDE):

Quản lý kết nối Wi-Fi và duy trì trạng thái kết nối.

Nhận chuỗi dữ liệu từ STM32 qua UART, phân tách (parse) dữ liệu.

Thực hiện chức năng cầu nối (UART-to-Network bridge) để chuyển tiếp dữ liệu.

 Thông số kỹ thuật hệ thống nhúng

Điện áp hoạt động: 5V DC (nguồn chính) và 3.3V DC (cho các module cảm biến).

Thời gian khởi động: 8–10 giây.

Độ trễ truyền dữ liệu nội bộ (UART): Khoảng 2–4 ms.

Tần suất cập nhật cảm biến: Mặc định 10 phút/lần (có thể cấu hình lại).



