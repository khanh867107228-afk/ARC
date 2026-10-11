# Bug Bounty Scout

Agent theo dõi cơ hội bug bounty bằng ChatGPT Automations; GitHub lưu quy trình, trạng thái và nhật ký. Đây là scouting thụ động, chưa phải agent kiểm thử hoặc khai thác tự động.

## Quy trình
1. Đọc trạng thái và báo cáo gần nhất.
2. Tìm chương trình mới, thay đổi phạm vi, khuyến mãi tiền thưởng và sản phẩm mới từ nguồn chính thức.
3. Xác minh điều khoản, ngày ra mắt, trạng thái, tiền thưởng, phạm vi và chính sách tự động hóa. Nếu không đọc được nội dung, đánh dấu chưa xác minh.
4. So sánh URL chuẩn hóa và các trường đã quan sát; first_seen không phải ngày ra mắt.
5. Chấm điểm có bằng chứng, lưu báo cáo và heartbeat. Chỉ thông báo thay đổi đáng kể hoặc lỗi cản trở.

## Điểm ưu tiên
Mỗi tiêu chí 0..5: freshness (25%), scope_clarity (25%), reward_evidence (20%), automation_policy (15%), research_access (15%). Tổng = sum(weight * criterion / 5), trên 100.
Thông tin chưa biết = 0; không cho điểm dựa vào suy đoán. Freshness cần ngày công bố có nguồn hoặc thay đổi đã quan sát so với snapshot trước. Không đủ dữ liệu = needs_verification, không vào danh sách ưu tiên đã xác minh.
Điểm là mức ưu tiên nghiên cứu, không phải xác suất tìm được bug hay thu nhập.

## Giới hạn vận hành
- Tối đa 10 ứng viên, 15 trang nguồn mỗi lượt.
- Chỉ tìm và đọc nguồn công khai; không quét, gửi payload, khai thác hoặc truy cập dữ liệu người khác.
- Không tự nộp report hoặc liên hệ người khác.
- Repo này public: chỉ lưu cơ hội và chính sách công khai; không lưu khóa, token, dữ liệu riêng, PoC hay lỗ hổng chưa công bố.
- Chỉ ghi trong bug-bounty-scout/. Đọc SHA hiện tại trước mỗi update và hợp nhất khi xung đột.
- Nếu thiếu quyền/nguồn, ghi lỗi; không tự suy diễn thành thành công.
- Tác vụ dự kiến mỗi giờ; thời điểm thực thi có thể trễ. Không có daemon hoặc GitHub Actions được triển khai ở đây.

## Dữ liệu
- state.json: trạng thái, ứng viên và heartbeat.
- reports/YYYY-MM-DD.md: báo cáo theo ngày Asia/Ho_Chi_Minh.
- AUTOMATION_PROMPT.md: hướng dẫn thực thi.
