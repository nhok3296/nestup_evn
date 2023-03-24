CÔNG CỤ THEO DÕI ĐIỆN NĂNG TIÊU THỤ TỪ EVN VIỆT NAM DÀNH CHO HOMEASSISTANT VIẾT BẰNG NGÔN NGỮ PYTHON

I. MÔ TẢ:

Từ việc sử dụng các phương thức có sẵn của module AIOHTTP thông qua những giao thức HTTP(S) cơ bản, công cụ cho phép theo dõi dữ liệu điện năng tiêu thụ từ EVN trực tiếp trên UI Home Assistant, hiện tại đã hỗ trợ cho tất cả vùng miền tại Việt Nam cùng với chi nhánh EVN tương ứng.
a) Các tính năng cơ bản:
- Thiết lập và theo dõi nhiều mã khách hàng cùng một lúc, trên cùng một máy chủ HA.
- Cài đặt và chỉnh sửa trực tiếp bằng UI (thông qua HACS), quản lí các thông số điện năng thông qua các thiết bị theo dõi tập trung.
- Hỗ trợ cho tất cả chi nhánh EVN toàn quốc (bao gồm 5 tổng công ty và hơn 400 chi nhánh lớn nhỏ).
- Tự động xác định máy chủ EVN.
- Tương thích với tất cả platform HA: Core, Supervisors, Hass OS.
b) Công cụ hỗ trợ theo dõi các thông số sau thông qua các sensors:
- Sản lượng ngày mới nhất và ngày trước đó, cùng với sản lượng tháng hiện tại (tạm chốt).
- Số tiền được (tạm) tính từ 3 sản lượng phía trên.
- Tình trạng hóa đơn nợ và số tiền nợ (nếu có).
- Chỉ số mới nhất và chỉ số cũ từ ngày đầu kì (ngày bắt đầu hóa đơn tháng).
- Ngày cập nhật dữ liệu mới nhất cùng với ngày đầu kì.
- Xem thêm ý nghĩa của các sensors phía dưới để hiểu rõ chức năng/ hạn chế của từng thông số trên.
 
c) Các điểm hạn chế:
- Chưa hoàn toàn hỗ trợ thêm sensor vào Energy (đang thử nghiệm).
- Chưa hoàn toàn hỗ trợ đối tượng khác khác ngoài các hộ sinh hoạt tiêu thụ điện 2 pha thông thường.
- Sensors về tiền điện ngày/ tháng chỉ mang tính chất tham khảo, được tính một cách thủ công theo giá bán lẻ bên dưới, vì vậy khả năng sai số là rất cao.
- Các thông tin không được cập nhật tức thì từ dữ liệu mới nhất của EVN, mà luôn được cập nhật theo chu kì cố định.
   
 
II. LƯU Ý TRƯỚC KHI CÀI ĐẶT:
1. Phiên bản Home Assistant: tối thiểu 2022.7.0
2. Công tơ điện EVN
- Công cụ chỉ hỗ trợ cho loại công tơ điện tử đo xa ghi theo ngày:
- Không phải tất cả công tơ điện tử đều hỗ trợ đọc chỉ số từ xa (đo xa).
- Không phải tất cả công tơ điện tử đo xa đều hỗ trợ ghi theo ngày.
- Để đảm bảo công tơ nhà bạn đủ điều kiện để sử dụng công cụ, xin hãy truy cập vào link đăng nhập phía dưới (xin chọn tương ứng với khu vực EVN của bạn).
- Nếu như bạn có thể theo dõi được sản lượng theo ngày trên website hoặc app chính thức của EVN, thì công tơ nhà bạn thích hợp để sử dụng công cụ này.
3. Mã khách hàng và tài khoản EVN
- Hiện tại tất cả chi nhánh, vùng miền đều cần phải có tài khoản EVN tương ứng với mã khách hàng để sử dụng công cụ.
- Tài khoản EVN hợp lệ sẽ bao gồm:
1. Tên tài khoản / Username (thông thường sẽ là mã khách hàng hoặc số điện thoại)
2. Mật khẩu / Password.
- Mã khách hàng của bạn phải thỏa những điều kiện sau:
1. Chứa từ 11 tới 13 kí tự.
2. Bắt đầu bằng chữ 'P'.
Chú ý: Kiểm tra ở bảng phía dưới, liên hệ với TTCSKH (Trung tâm Chăm sóc Khách Hàng) để xin thông tin đăng nhập nếu chưa có:
Chi nhánh EVN	Khu vực	Đăng nhập	TTCSKH
EVNHANOI	Thủ đô Hà Nội	Link
Link

EVNHCMC	Thành phố Hồ Chí Minh	Link
Link

EVNNPC	Khu vực miền Bắc	Link
Link

EVNCPC	Khu vực miền Trung	Link
Link

EVNSPC	Khu vực miền Nam	Link
Link








III. HƯỚNG DẪN CÀI ĐẶT:
- Cài đặt thủ công thông qua Samba/ SFTP
Giải nén thư mục custom_components/nestup_evn vào thư mục custom_components trong Homeassistant của bạn.
Thư mục custom_components phụ thuộc vào thư mục cài đặt HomeAssistant của bạn.
Thông thường, thư mục cài đặt HomeAssistant sẽ là ~/homeassistant/.
Nói cách khác, thư mục cài đặt HomeAssistant là thư mục chứa file configuration.yaml.
Sau khi cài đặt đúng, đường dẫn sẽ có dạng:
1.	└── ...  
2.	└── configuration.yaml  
3.	└── secrets.yaml  
4.	└── custom_components  
5.	    └── nestup_evn  
6.	        └── __init__.py  
7.	        └── sensor.py  
8.	        └── nestup_evn.py  
9.	        └── ...  
 Chú ý: nếu thư mục custom_components không tồn tại, bạn phải tự tạo nó.
 
IV. THIẾT LẬP VÀ CHỈNH SỬA:
a) Cách thiết lập công cụ:
1. Tìm công cụ EVN Data Fetcher trong những công cụ đã tải về.
- Settings > Devices and Services > Integrations > Add Integrations > Tìm EVN Data Fetcher
 
2. Điền Mã khách hàng.
- Yêu cầu: Mã khách hàng của bạn phải thỏa những điều kiện sau:
+ Chứa từ 11 tới 13 kí tự.
+ Bắt đầu bằng chữ 'P'.
 
3. Điền Tài khoản EVN và chọn Ngày bắt đầu hóa đơn.
- Chú ý: ngày bắt đầu hóa đơn là ngày đầu tiên trong hóa đơn điện hàng tháng (xem hóa đơn các kì trước để chắc chắn).
 
4. Hoàn thành, bây giờ bạn có thể thấy thiết bị theo dõi của mình ở phần Devices.
 
b) Chỉnh sửa thông số:
- Hiện tại, sau khi đã thiết lập, các thông số của công cụ chưa thể chỉnh sửa.
- Tuy nhiên, những thông số này "có vẻ như" sẽ không cần phải chỉnh sửa, hiện tại đã được gán mặc định như bên dưới.
1. Ý nghĩa của các sensor được tạo sẵn:
- Do thiếu sự đồng bộ về các khái niệm chỉ số điện năng giữa các chi nhánh và tổng công ty EVN, các sensors sẽ được thống nhất như bên dưới:
+ Ngày tạm chốt: là ngày đã có đầy đủ các thông tin về điện năng tiêu thụ - (theo lý thuyết) được tính từ 00:00 đến 23:59 của ngày đó (khác đối với EVNCPC).
+ Ngày đầu kì: là ngày đầu tiên trong hóa đơn điện tiêu thụ hàng tháng (xem hóa đơn của các kì trước để biết).
+ Chỉ số tạm chốt: là chỉ số được lấy khi kết thúc ngày tạm chốt.
+ Chỉ số đầu kì: là chỉ số được lấy khi bắt đầu ngày đầu kì.
+ 2 sensors Sản lượng ngày: tích hợp tính năng Dynamic Name*, là sản lượng điện tiêu thụ được tính (theo lý thuyết) từ 00:00 đến 23:59 của ngày hôm đó (khác đối với EVNCPC).
+ Sản lượng tháng: là sản lượng điện tiêu thụ được tính (theo lý thuyết) từ 00:00 của ngày đầu kì đến 23:59 của ngày tạm chốt (khác đối với EVNCPC).
- Để thuận tiện hơn trong việc theo dõi điện tiêu thụ hàng ngày (ví dụ ước lượng số tiền điện mình sử dụng trong ngày). 2 sensors bên dưới chỉ mang tính chất tham khảo, không được lấy trực tiếp từ dữ liệu EVN, mà được tính theo giá bán lẻ bên dưới nên khả năng sai số là rất cao*
+ Các sensors tiền điện ngày: được tính từ các sensors sản lượng ngày.
+ Tiền điện tháng: được tính từ sản lượng tháng.
2. Giá bán lẻ điện EVN:
- Mặc dù có nhiều loại biểu giá tùy vào mục đích sử dụng điện, nhưng dự án này sẽ mặc định tính giá tiền điện theo biểu giá bán lẻ của nhóm đối tượng Sinh Hoạt.
- Truy cập link này để xem biểu giá bán lẻ tiền điện của EVN.
3. Chu kì cập nhật dữ liệu mới từ EVN:
- 6 tiếng là chu kì mặc định giữa các lần cập nhật dữ liệu điện năng tiêu thụ từ EVN.
- Việc cập nhật dữ liệu theo chu kì, không cố định 1 mốc thời gian cụ thể, lí do là:
- Để các sensors luôn cập nhật được dữ liệu mới nhất từ EVN.
- Thời điểm cập nhật dữ liệu điện năng của hơn 400 chi nhánh EVN toàn quốc là không cố định.
