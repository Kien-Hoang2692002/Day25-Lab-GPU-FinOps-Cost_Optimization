# GPU FinOps & Tối ưu hóa chi phí - Báo cáo

## Thông tin sinh viên
- **Tên**: Hoàng Văn Kiên
- **MSSV**: 2A202600077
- **Ngày**: 13 tháng 5 năm 2026
- **Tiêu đề bài lab**: Bài lab thực hành GPU FinOps & Tối ưu hóa chi phí

## Giới thiệu

Mục tiêu của bài lab này là để có được kinh nghiệm thực hành với các kỹ thuật GPU FinOps và tối ưu hóa chi phí. FinOps (Tài chính và Vận hành) cho các tải công việc GPU liên quan đến giám sát, quản lý và tối ưu hóa mức tiêu thụ tài nguyên GPU để tối đa hóa hiệu suất trong khi giảm thiểu chi phí. Bài lab này cung cấp trải nghiệm thực tế về nhiều khía cạnh của GPU FinOps bao gồm giám sát cụm máy chủ, quản lý tải công việc, theo dõi chi phí, quản lý phiên bản spot và tự động mở rộng quy mô.

Kiến trúc bao gồm nhiều dịch vụ chạy trong các container Docker mô phỏng môi trường cụm GPU thực tế:
- Dịch vụ cổng truy cập (cổng 8000) - Điểm truy nhập duy nhất cho notebook
- Trình quản lý nút GPU (cổng 8001) - Mô phỏng cụm GPU đa nút
- API thanh toán (cổng 8002) - Mô phỏng thanh toán điện toán đám mây
- Trình quản lý Spot (cổng 8003) - Đấu giá và ngắt kết nối phiên bản spot
- Trình tự động mở rộng (cổng 8004) - Tự động mở rộng GPU kiểu KEDA
- Trình theo dõi chi phí (cổng 8005) - Phân bổ chi phí kiểu OpenCost

## Phân tích từng phần của bài lab

### 1. Giám sát cụm GPU
Trong giai đoạn giám sát cụm, tôi đã quan sát trạng thái ban đầu của cụm GPU mô phỏng. Cụm bao gồm nhiều nút GPU với các chỉ số công suất và sử dụng khác nhau. Những quan sát chính bao gồm:
- Trạng thái ban đầu của nút và khả năng sẵn sàng tài nguyên
- Mô hình sử dụng bộ nhớ GPU
- Chỉ số tiêu thụ điện năng
- Chỉ báo sức khỏe tổng thể của cụm

Công cụ giám sát cụm cung cấp khả năng nhìn thấy mức sử dụng tài nguyên GPU theo thời gian thực, điều này rất quan trọng để quản lý chi phí hiệu quả. Hiểu rõ mức độ sử dụng hiện tại giúp đưa ra quyết định sáng suốt về vị trí tải công việc và mở rộng tài nguyên.

### 2. Gửi tải công việc & Theo dõi chi phí
Trong phần này, tôi đã gửi nhiều tải công việc đến cụm và giám sát việc thực thi cũng như chi phí liên quan. Những phát hiện chính bao gồm:
- Các tải công việc khác nhau có yêu cầu tài nguyên và thời gian thực thi khác nhau
- Mô hình tích lũy chi phí khác nhau dựa trên mức sử dụng tài nguyên
- Mối quan hệ giữa thời gian chạy và chi phí được thể hiện rõ ràng
- Hồ sơ thanh toán cho thấy sự phân bổ chi phí cho từng tải công việc

Bài tập này nhấn mạnh tầm quan trọng của việc lập lịch tải công việc hiệu quả để tối ưu hóa chi phí trong khi đáp ứng các yêu cầu về hiệu suất.

### 3. Quản lý phiên bản Spot
Phiên bản Spot cung cấp tiết kiệm chi phí đáng kể so với phiên bản theo nhu cầu. Trong phần này, tôi khám phá:
- Giá spot hiện tại so với giá theo nhu cầu
- Yêu cầu phiên bản spot và giám sát tính sẵn có của chúng
- Mô phỏng các sự kiện ngắt kết nối spot và tác động của chúng
- Tính toán tiềm năng tiết kiệm từ việc sử dụng spot

Tính năng quản lý spot cho thấy cách các tổ chức có thể tận dụng tài nguyên tính toán gián đoạn để giảm đáng kể chi phí GPU, đặc biệt đối với các tải công việc chịu lỗi.

### 4. Tự động mở rộng (kiểu KEDA)
Khả năng tự động mở rộng được kiểm tra bằng cách sử dụng các chính sách tương tự như KEDA (Tự động mở rộng theo sự kiện Kubernetes). Những khía cạnh chính bao gồm:
- Cấu hình các chính sách tự động mở rộng dựa trên các chỉ số
- Giám sát các sự kiện mở rộng được kích hoạt bởi nhu cầu tải công việc
- Đánh giá hiệu quả của các quyết định mở rộng
- Điều chỉnh chính sách để tối ưu hóa việc sử dụng tài nguyên

Cơ chế tự động mở rộng chứng tỏ giá trị trong việc tự động điều chỉnh công suất cụm dựa trên nhu cầu, ngăn ngừa việc cấp phát quá mức trong thời gian nhu cầu thấp và đảm bảo tài nguyên đầy đủ trong thời gian cao điểm.

### 5. Phân tích chi phí & Phát hiện lãng phí
Phần này tập trung vào việc xác định những bất hiệu quả về chi phí và cơ hội tối ưu hóa:
- Lấy các bản chụp chi phí định kỳ để theo dõi mô hình chi tiêu
- Phân tích các báo cáo lãng phí để xác định tài nguyên chưa sử dụng hết
- Xem xét các khuyến nghị tối ưu hóa từ hệ thống
- Kiểm tra chế độ xem bảng điều khiển tổng hợp

Các công cụ phân tích chi phí tiết lộ những hiểu biết quan trọng về sự lãng phí tài nguyên và cung cấp các khuyến nghị có thể thực hiện để cải thiện.

### 6. Trực quan hóa
Các thành phần trực quan hóa giúp diễn giải dữ liệu thu thập được một cách hiệu quả:
- Biểu đồ phân bổ chi phí thể hiện phân phối qua các loại khác nhau
- Biểu đồ chuỗi thời gian theo dõi xu hướng chi phí theo thời gian
- Trực quan hóa mối quan hệ hiệu suất và chi phí

Những trực quan hóa này rất quan trọng để truyền đạt những hiểu biết FinOps cho các bên liên quan và đưa ra quyết định dựa trên dữ liệu.

### 7. Quy trình hoàn chỉnh FinOps
Quy trình làm việc cuối đến cuối tích hợp tất cả các thành phần trước đó thành một quy trình tổng thể:
- Giám sát trạng thái cụm và việc sử dụng tài nguyên
- Gửi và theo dõi tải công việc
- Quản lý phiên bản spot để tiết kiệm chi phí
- Áp dụng các chính sách tự động mở rộng
- Thực hiện phân tích và tối ưu hóa chi phí
- Triển khai các khuyến nghị
- Xác minh các cải tiến

Cách tiếp cận toàn diện này cho thấy cách các thực hành FinOps có thể được áp dụng một cách hệ thống để quản lý hiệu quả chi phí GPU.

### 8. Tải công việc GPU thực tế trên Kaggle/Colab
Chạy các tải công việc GPU thực tế trên Kaggle/Colab cung cấp kinh nghiệm thực tế với:
- Cài đặt và cấu hình môi trường hỗ trợ GPU
- Huấn luyện mô hình sử dụng cả FP32 và Độ chính xác hỗn hợp tự động (AMP)
- So sánh sự khác biệt về hiệu suất và chi phí giữa các chế độ độ chính xác
- Thu thập dữ liệu đo đạc GPU trong quá trình huấn luyện
- Báo cáo chi phí GPU thực tế trở lại cổng truy cập

Phần này lấp đầy khoảng cách giữa các tải công việc mô phỏng và thực tế, cho thấy cách các kỹ thuật tối ưu hóa được áp dụng vào phần cứng thực tế.

### 8.5 Tối ưu hóa chi phí GPU nâng cao
Phân tích nâng cao bao gồm:
- Phân tích hiệu quả mở rộng đa GPU
- Dự báo chi phí dự án với khoảng tin cậy
- Phân tích cơ hội tối ưu hóa ưu tiên
- Bảng điều khiển chi phí tích hợp với nhiều chế độ xem
- Bài tập thử thách yêu cầu chiến lược tối ưu hóa tùy chỉnh

Những kỹ thuật nâng cao này giúp các tổ chức mở rộng quy mô các thực hành FinOps của họ trên các triển khai cơ sở hạ tầng GPU lớn hơn.

## Những điều học được và Nhận thức

### Chiến lược tối ưu hóa chi phí
1. **Sử dụng phiên bản Spot**: Việc tận dụng các phiên bản spot có thể mang lại tiết kiệm chi phí từ 70-90% cho các tải công việc phù hợp. Việc triển khai các cơ chế chịu lỗi là rất quan trọng để xử lý hiệu quả các lần ngắt kết nối.

2. **Định cỡ tài nguyên phù hợp**: Định cỡ tài nguyên GPU phù hợp với yêu cầu tải công việc ngăn ngừa việc cấp phát quá mức và giảm chi phí không cần thiết.

3. **Triển khai tự động mở rộng**: Việc mở rộng tài nguyên linh hoạt dựa trên nhu cầu thực tế giúp tối ưu hóa chi phí trong khi duy trì các SLA hiệu suất.

4. **Lập lịch tải công việc**: Việc lập lịch chiến lược cho tải công việc trong giờ thấp điểm hoặc khi tài nguyên sẵn có nhiều hơn có thể dẫn đến tiết kiệm chi phí.

### Những phương pháp tốt đã nhận diện
1. **Giám sát liên tục**: Việc giám sát định kỳ các chỉ số sử dụng GPU và chi phí cho phép thực hiện hành động tối ưu hóa kịp thời.

2. **Cân bằng giữa hiệu suất và chi phí**: Việc hiểu rõ sự cân bằng giữa yêu cầu hiệu suất và các hạn chế chi phí là rất quan trọng đối với FinOps GPU hiệu quả.

3. **Ra quyết định dựa trên dữ liệu**: Sử dụng các công cụ phân tích và báo cáo để thông báo các quyết định tối ưu hóa dẫn đến quản lý chi phí hiệu quả hơn.

4. **Xem xét và điều chỉnh định kỳ**: FinOps là một quá trình liên tục đòi hỏi phải xem xét và điều chỉnh liên tục các chính sách và chiến lược.

## Kết luận và Đề xuất

Bài lab này đã cung cấp kiến thức toàn diện về các nguyên tắc và thực hành GPU FinOps. Kinh nghiệm thực hành với việc giám sát, theo dõi chi phí, quản lý spot và tự động mở rộng giúp củng cố các khái niệm lý thuyết với ứng dụng thực tế.

Những điểm cần lưu ý bao gồm:
- Tiết kiệm chi phí đáng kể có thể đạt được thông qua việc sử dụng chiến lược các phiên bản spot
- Tầm quan trọng của các công cụ giám sát trong việc xác định các cơ hội tối ưu hóa
- Giá trị của việc mở rộng tự động trong việc phù hợp cung cấp tài nguyên với nhu cầu
- Hiệu quả của các công cụ trực quan hóa trong việc truyền đạt các hiểu biết FinOps

Để triển khai GPU FinOps trong các tình huống thực tế, tôi đề xuất:
1. Thiết lập các chỉ số cơ bản cho việc sử dụng và chi phí GPU hiện tại
2. Triển khai các giải pháp giám sát để theo dõi mức sử dụng và chi tiêu
3. Từ từ giới thiệu các phiên bản spot cho các tải công việc phù hợp với khả năng chịu lỗi thích hợp
4. Thiết lập các chính sách mở rộng tự động dựa trên yêu cầu kinh doanh
5. Thường xuyên xem xét và tối ưu hóa phân bổ tài nguyên dựa trên phân tích
6. Đào tạo nhóm về các phương pháp sử dụng tài nguyên có ý thức về chi phí

Bài lab này hiệu quả chứng minh tiềm năng tiết kiệm chi phí đáng kể trong hoạt động GPU thông qua các thực hành FinOps hệ thống trong khi vẫn duy trì các yêu cầu về hiệu suất.