# 23:17 — Đường dây cuối

Game bí ẩn tương tác bằng chữ trên web. Người chơi vào vai Minh, đang xem lại hồ sơ tai nạn xe 17 của 15 năm trước. Một đường liên lạc bất ngờ nối Minh với cha mình, người đang lái chính chuyến xe ấy trong đêm xảy ra tai nạn. Minh phải đối chiếu hồ sơ, nói chuyện với người ở đầu dây và đưa ra quyết định trước 23:17.

**Trạng thái:** dự án đang ở giai đoạn thiết kế và chuẩn bị bản thử bằng chữ. Chưa có bản demo hoàn chỉnh hoặc hướng dẫn cài đặt ổn định. Các con số về thời gian và cân bằng là giả định cần kiểm chứng khi chơi thử.

## Trải nghiệm dự kiến

Một vòng chơi kéo dài từ 22:47 đến 23:17 theo **thời gian trong truyện**. Đồng hồ chỉ tiến khi phân tích hồ sơ hoặc yêu cầu người trên xe làm việc tại hiện trường. Đọc lại, suy luận và trò chuyện không trừ phút. Nếu vòng chơi thất bại, Minh giữ kiến thức đã khám phá, còn tình trạng trên xe và lòng tin của cha được đặt lại.

Bản demo tập trung vào bốn việc:

1. Đối chiếu sáu hồ sơ D01–D06 và kiểm tra ba giả thuyết về đèn trước cầu, lần dừng ở trạm 4 và lối lên chòi.
2. Dùng bằng chứng để ông Hải tin yêu cầu dừng xe. Ông ban đầu tưởng Minh là người trực ca bến.
3. Tìm, sơ cứu người gác cầu, kiểm tra lối thoát và đưa cả nhóm lên chòi trước khi lũ quét qua bãi trạm.
4. Nhắc chi tiết riêng trong D06 để cha nhận ra mối liên hệ với Minh và chịu rời vùng nguy hiểm cùng mọi người.

Người chơi có thể thắng ngay vòng đầu; không có manh mối bắt buộc nào chỉ xuất hiện sau thất bại. Trong ba phút còn lại của đường chơi mẫu, người chơi có thể ưu tiên **cảnh báo bến** để chặn xe sau hoặc **ghi lời khai của Lan** để bổ sung chứng cứ về ông Hải. Đây là các nhánh tùy chọn; thời gian mẫu 27 phút chưa được script cân bằng xác nhận.

## Vai trò của AI

Luật game quyết định điều kiện, chi phí, lòng tin và kết quả. Sau khi bản chơi bằng lựa chọn viết sẵn hoạt động, AI sẽ giúp hiểu câu người chơi tự gõ: ý định, bằng chứng được viện dẫn và lập luận. Người chơi xem lại diễn giải trước khi xác nhận. Lời đáp và manh mối quan trọng dùng nội dung đã duyệt; khi AI lỗi, các lựa chọn viết sẵn vẫn dẫn đến cùng đường giải.

AI là giai đoạn sau của demo. Bản thử đầu tiên chạy bằng Python trên terminal và không cần khóa API.

## Cấu trúc repo dự kiến

```text
.
├── README.md
├── .gitignore
├── pyproject.toml
├── content/
│   ├── documents.json        # Nội dung, nguồn và manh mối D01–D06
│   ├── facts.json            # Sự thật và quyền tiết lộ
│   ├── actions.json          # Điều kiện, phí phút và hiệu ứng
│   ├── events.json           # Tin theo giờ, gồm cảnh báo nước dâng
│   └── dialogue.vi.json      # Lời thoại và biến thể đã duyệt
├── src/duong_day_cuoi/
│   ├── models.py             # Trạng thái vòng chơi và kiến thức
│   ├── content_loader.py     # Nạp, kiểm tra JSON
│   ├── rules.py              # Hành động, lòng tin, điều kiện thắng
│   ├── timeline.py           # Đồng hồ và sự kiện cố định
│   └── cli.py                # Bản chơi trên terminal
├── tools/balance.py          # Duyệt trạng thái và đường thắng
├── tests/                   # Kiểm tra luật, ranh giới 23:17 và reset
├── docs/
│   ├── Bao_cao_Duong_day_cuoi_2317.docx
│   ├── story-decisions.md    # Canon và hai dòng thời gian
│   ├── playtest-notes.md     # Quan sát người chơi thử
│   └── ai-policy.md          # Bổ sung khi tích hợp AI
└── frontend/                # React, bổ sung sau bản thử terminal
```

Cấu trúc trên là **đích triển khai**, không phải tuyên bố rằng mọi file đã tồn tại. Khi thêm FastAPI, lớp API gọi cùng lõi luật với CLI và script cân bằng. Không viết riêng điều kiện thắng ở frontend.

## Thứ tự bắt đầu

- **Chuẩn bị 3–5 ngày:** chuyển dòng thời gian và các quyết định đã chốt từ báo cáo vào `docs/story-decisions.md`; viết dữ liệu hồ sơ, hành động và sự kiện; dựng bản chơi chữ ngắn.
- **Lõi Python:** thực hiện đồng hồ, ba loại thao tác, lòng tin, reset và hai kết quả. Viết script tìm đường thắng ngắn nhất, kiểm tra các cổng bắt buộc và nhánh tùy chọn.
- **Chơi thử sớm:** đưa terminal cho 1–2 người chơi mà không giải thích sẵn đường thắng. Ghi chỗ họ tự suy luận, chỗ họ chỉ làm theo danh sách và lý do họ chọn một nhánh tùy chọn.
- **Sau khi vòng chơi ổn:** làm giao diện React và FastAPI, rồi tích hợp AI và trang đánh giá.

Ưu tiên viết lời thoại ngắn có ID và vài biến thể đã duyệt theo trạng thái. Báo cáo dự toán khoảng 61 câu ngắn cùng sáu hồ sơ cho bản đầu; số thực tế sẽ được đếm lại khi viết nội dung.

## Tài liệu và nguyên tắc đóng góp

Báo cáo thiết kế phiên bản 2.2 là tài liệu tham chiếu hiện tại. Khi đổi luật, sửa dữ liệu và `story-decisions.md` cùng lúc, rồi chạy lại kiểm tra cân bằng. Không đưa khóa API, save cá nhân, hội thoại thô hoặc thư mục build vào Git.

Dự án cá nhân của Lê Tuấn Hiệp.
