# HAPPY SOCIAL AI 5.0 — Content Factory / MVP 0.1

Ứng dụng web nội bộ chạy trên Windows, dữ liệu UTF-8, giờ Việt Nam. Đây là MVP độc lập, không phải bản Cloud production đã triển khai. Không có dữ liệu Facebook thực tế đi kèm. Tên/chủ đề 5 kênh là cấu hình tạm; xác minh Page ID qua Meta trước khi đăng.

## 1. Cài đặt Windows
1. Giải nén ZIP vào thư mục riêng (ví dụ D:\HappySocialAI).
2. Cài Python 3.11 trở lên từ https://www.python.org/downloads/ (bật Python Launcher).
3. Cài FFmpeg và FFprobe từ https://ffmpeg.org/download.html, thêm vào PATH. Mở cửa sổ lệnh mới và kiểm tra `ffmpeg -version`, `ffprobe -version`.
4. Chạy SETUP_WINDOWS.bat; lệnh này tạo môi trường Python riêng, cài thư viện và tạo .env nếu chưa có. Có thể phát sinh tải dữ liệu Internet.
5. Chạy START_SYSTEM.bat. Nếu trình duyệt mở sớm, tải lại trang http://127.0.0.1:8765. Giữ cửa sổ lệnh mở. Ctrl+C để dừng server; hoặc dùng nút Dừng khẩn cấp để tắt sản xuất và gửi mới (không hủy yêu cầu đã gửi đi).

## 2. Thử trước khi cấp quyền
Vào Xưởng nội dung -> Bài DEMO (không đăng). Bản DEMO không thể được duyệt hoặc xuất bản. Vào 5 kênh Facebook -> Cấu hình để sửa tên, chủ đề, giờ, bật kênh. Mặc định mọi kênh và tính năng đăng thật đều tắt. Chọn Tạo kho 7 ngày hoặc Lịch hôm nay, rồi Khởi động. Đơn hàng quá giờ không được tự động tạo; lịch cũ đã tạo giữ nguyên dù đổi cấu hình.

## 3. Gemini
Tạo API key tại https://aistudio.google.com/apikey. Mở .env bằng Notepad, nhập GEMINI_API_KEY. GEMINI_MODEL có thể chỉnh theo model được cấp quyền (mặc định gemini-2.5-flash, phải kiểm tra tính khả dụng). Ứng dụng gửi chủ đề, hồ sơ kênh, tiêu đề gần đây và số liệu tương tác đã lấy về Google; không nhập dữ liệu mật hoặc dữ liệu cá nhân không được phép. Giới hạn số lượt gọi hằng ngày ở Cấu hình; đây không phải ngân sách tiền. Cài giới hạn chi phí tại nhà cung cấp.

## 4. Video Reels
Cấu hình Google Cloud Text-to-Speech, bật API và billing nếu cần, cấp ADC cho tài khoản bằng Google Cloud CLI (`gcloud auth application-default login`) hoặc đặt biến môi trường GOOGLE_APPLICATION_CREDENTIALS đến tệp thông tin xác thực thích hợp. Không gửi tệp này cho người khác. TTS_MODE=google, giọng mặc định vi-VN-Standard-A; thay đổi nếu tài khoản không hỗ trợ. Tùy chọn thử nghiệm TTS_MODE=espeak (cần espeak trong PATH; chất lượng giọng máy, không phải giọng tự nhiên). Font mặc định Arial trên Windows; nếu thiếu, cấu hình FONT_PATH. Reels dùng thẻ chữ gốc, màu riêng theo kênh, giọng đọc từng cảnh và chữ đồng bộ theo cảnh; chưa có AI tạo ảnh, video nền, nhạc hoặc logo tùy chỉnh. Định dạng MP4 H.264 1080×1920, 30fps, AAC; kiểm tra nội bộ 3–90 giây. Kiểm tra yêu cầu Meta hiện hành trước khi xuất bản. Có thể tải video đã dựng từ Xưởng nội dung để đăng thủ công.

## 5. Meta Graph API (chỉ dành cho Facebook Page đủ điều kiện)
- Tạo ứng dụng ở https://developers.facebook.com/ và xác minh điều kiện sử dụng, quyền, chế độ app và App Review/Business Verification nếu cần.
- Dùng cơ chế Facebook chính thức hoặc Graph API Explorer để lấy Page Access Token đúng Page, có quyền pages_manage_posts, pages_read_engagement; pages_show_list dùng để lấy danh sách Page từ tài khoản cấp quyền. Quyền thực tế còn tùy app và tài khoản. Không dùng mật khẩu Facebook, cookie hay tự động vượt xác thực.
- Xác định Page ID thật, không mặc định các ID trong URL là Page ID. Nhập Page ID vào giao diện kênh; nhập Page Access Token tương ứng META_PAGE_1_TOKEN ... META_PAGE_5_TOKEN vào .env. Bấm Xác minh. Chức năng này đối chiếu ID và thuộc tính Page, không đảm bảo đã có đủ quyền đăng. Chưa có OAuth tự động trong MVP; .env chứa secret dạng văn bản cục bộ, bảo vệ bằng quyền truy cập Windows và không chia sẻ/commit.
- META_VERSION mặc định v27.0 theo tài liệu tra cứu; chỉnh phiên bản nếu app của bạn yêu cầu. Tham khảo https://developers.facebook.com/docs/graph-api/reference/page/feed/ và https://developers.facebook.com/docs/graph-api/reference/page/video_reels/.
- Thử trên một Page được phép, một nội dung đã duyệt, lịch tương lai. Chọn Bật đăng thật, nhập BAT DANG THAT; bật xưởng và kênh. Chỉ bật tự duyệt nếu đã kiểm tra chất lượng. Không có chức năng xuất bản trực tiếp lên profile cá nhân; nếu là profile, tải nội dung để đăng tay.
- Bài text được gửi /feed; Reel khởi tạo, upload, finish rồi đối chiếu trạng thái. Timeout/lỗi sau khi bắt đầu gửi chuyển Cần đối chiếu, không tự retry để tránh đăng trùng. Kiểm tra Meta Business Suite; chưa có màn hình xác nhận thủ công trạng thái bài text không rõ kết quả. Bản MVP chưa thử với Meta thật. Reels khai báo AI theo trường API tài liệu hiện có.

## 6. Vận hành và kiểm soát
Mỗi kênh có 3 lịch cố định: bài sáng / Reels trưa / bài tối. Kế hoạch 7 ngày chỉ tạo đơn hàng, không tạo ngay tất cả file; worker xử lý tuần tự theo lịch và hạn mức AI. Tác vụ đã duyệt quá 30 phút không được tự đăng bù. Khi chạy trên máy cá nhân, mất điện/tắt máy/ngủ sẽ ngừng lập lịch. Không dùng hai tiến trình server trên cùng dữ liệu; giới hạn một người dùng trên loopback. Nút dừng không thể thu hồi yêu cầu đang xử lý ở dịch vụ bên ngoài.

Chế độ bán tự động mặc định: kiểm tra và lưu nội dung trước khi bấm Duyệt. Chế độ auto là opt-in từng kênh, dựa trên phân loại AI, kiểm tra từ khóa nhạy cảm và trùng lặp (heuristic, có thể sai). Không tự duyệt thông tin pháp luật, sức khỏe, BHXH, tài chính; cần kiểm chứng nguồn bằng con người. Không có hệ thống truy xuất nguồn tin trực tiếp trong MVP. Để kiểm tra chất lượng nên dùng chế độ duyệt tay. Tác vụ mới có thể được tạo thủ công: nhập thời gian, chủ đề; nhập nội dung tay nếu chưa có Gemini.

Chỉ số: lấy cảm xúc/bình luận/chia sẻ bằng nút Lấy số liệu trên bài đã đăng. Số liệu thiếu được lưu null. Chưa có đồng bộ Insights tự động, reach/watch time, tự động nghiên cứu xu hướng, chatbot bình luận, Telegram, mô hình dự phòng, OAuth, Cloud hoặc tự cập nhật mã nguồn.

Sao lưu: nút Sao lưu database tạo bản sao SQLite trong data/backups; media và .env cần sao lưu riêng, mã hóa và giữ bí mật. Muốn phục hồi, dừng chương trình, sao lưu thư mục data hiện tại, thay studio.sqlite3 bằng bản backup và xử lý tệp WAL/SHM cũ sau khi đã đóng mọi tiến trình. Không xóa media đang được tham chiếu. Database và video nằm trong data/.

## 7. Kiểm thử và nâng cấp Antigravity
Chạy `python test_studio.py` trong môi trường đã cài thư viện. Kiểm thử video cần FFmpeg, FFprobe và espeak; có thể skip khi thiếu. TEST_RESULTS.txt ghi kết quả trên môi trường Linux (chưa kiểm thử Windows thực tế). Kiểm thử Meta bài text là mock, không phát sinh đăng thật; chưa có kiểm thử đầu-cuối Gemini, Google TTS, Meta Reels với quyền thật. Đọc NEXT_STEPS_ANTIGRAVITY.md để nâng cấp từ mã nguồn, không xây lại từ đầu.