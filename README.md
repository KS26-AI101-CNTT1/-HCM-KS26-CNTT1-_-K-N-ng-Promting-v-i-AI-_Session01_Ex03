Đề bài / Yêu cầu

1. Mục tiêu

- Phân tích cơ chế tách từ (Tokenization) dựa trên thuật toán Byte-Pair Encoding (BPE).

- Giải thích được nguyên nhân chênh lệch chi phí và tiêu tốn token giữa văn bản Tiếng Việt và Tiếng Anh.

2. Bối cảnh & Bảng đo lường Token

Hoàng - chuyên viên Marketing MartX - chạy thử nghiệm 2 đoạn văn bản cùng dung lượng ý tưởng (100 từ) qua công cụ Tokenizer của OpenAI và thu được bảng kết quả sau:

Văn bản thử nghiệm	Ngôn ngữ	Số từ (Words)	Số Token thực tế (Tokens)	Tỷ lệ Chênh lệch (Token/Word)

Mẫu A: "MartX Supermarket offers the best fresh organic vegetables..."	Tiếng Anh	100 từ	118 Tokens	1.18 Tokens/từ

Mẫu B: "Siêu thị MartX cung cấp các loại rau củ hữu cơ tươi ngon nhất..."	Tiếng Việt	100 từ	285 Tokens	2.85 Tokens/từ

Thắc mắc của Hoàng: "Có phải hệ thống đếm Token của AI bị lỗi đếm trùng khi gặp tiếng Việt không?"

3. Ràng buộc Kỹ thuật

Bẫy: Nhầm tưởng 1 từ = 1 token. Thực tế LLM đếm theo sub-word units. Tiếng Việt có dấu thanh và ít xuất hiện trong tập dữ liệu Pre-training nên bị thuật toán BPE cắt thành nhiều mảnh byte nhỏ.

4. Yêu cầu đầu ra

- Phần 1 (Phân tích BPE): Dựa vào Mục 2.1 & 2.2 Lesson 03, giải thích cơ chế cắt từ của BPE đối với ký tự tiếng Việt có dấu. Bóc tách lý do khiến Mẫu B tốn gấp 2.4 lần Token của Mẫu A.

Hệ thống không bị lỗi đếm trùng, mà thuật toán Byte-Pair Encoding (BPE) buộc phải chia nhỏ từ tiếng Việt có dấu thành nhiều mảnh byte do cấu trúc mã hóa và dữ liệu huấn luyện. Điều này khiến Mẫu B (285 Tokens) tốn lượng token gấp 2,4 lần so với Mẫu A (118 Tokens).

1. Cơ chế cắt từ của BPE với tiếng Việt có dấu

Nguyên lý chia nhỏ (Sub-word): LLM không đếm theo từ hoàn chỉnh mà băm văn bản thành các đơn vị dưới từ hoặc mảnh byte. BPE chỉ gộp các ký tự thành 1 token nếu chúng xuất hiện đủ nhiều trong dữ liệu gốc.

Phân mảnh ký tự có dấu (UTF-8): Chữ tiếng Việt chứa dấu thanh và dấu mũ tốn nhiều byte hơn chữ tiếng Anh chuẩn ASCII. BPE thường cắt rời chữ cái gốc và dấu thanh thành nhiều mảnh byte vụn (ví dụ 1 từ tiếng Việt bị tách làm 2–3 tokens).

2. Lý do Mẫu B tốn gấp 2,4 lần Token của Mẫu A

Mất cân bằng dữ liệu Pre-training: Tiếng Anh chiếm tỷ trọng áp đảo nên từ điển BPE lưu sẵn từ nguyên vẹn, đạt mức nén 1,18 Tokens/từ (118 Tokens).

Tần suất xuất hiện thấp: Tiếng Việt chiếm tỷ lệ nhỏ nên không có sẵn từ hoàn chỉnh trong từ điển, buộc hệ thống chia nhỏ từng từ và đẩy tỷ lệ lên 2,85 Tokens/từ (285 Tokens, gấp 2,41 lần Mẫu A).

- Phần 2 (Tối ưu Prompt): Đề xuất 2 kỹ thuật viết Prompt Tiếng Việt giúp Hoàng tiết kiệm từ 30-40% số lượng Token tiêu thụ mà không làm biến đổi nội dung.

Để giảm từ 30% đến 40% lượng Token tiêu thụ mà vẫn giữ nguyên vẹn ý nghĩa đầu vào, Hoàng nên áp dụng kỹ thuật viết theo từ khóa cô đọng (Telegraphic Style) kết hợp với cấu trúc khung lệnh tiếng Anh. Hai phương pháp này giúp loại bỏ hoàn toàn các mảnh byte thừa thãi từ hư từ và dấu thanh tiếng Việt.

Phần 2: Hai kỹ thuật viết Prompt tối ưu 30–40% Token

Kỹ thuật 1: Viết theo phong cách từ khóa cô đọng (Telegraphic Style)

Hoàng cần lược bỏ toàn bộ từ đệm giao tiếp, đại từ xưng hô lịch sự và các hư từ không mang thông tin chính (như "hãy giúp tôi", "làm ơn", "của", "những", "các", "một cách"). Thay vào đó, hãy dùng dấu gạch đầu dòng đi kèm các cụm danh từ và động từ cốt lõi để cắt giảm trực tiếp 30%–40% từ thừa mà AI vẫn hiểu chính xác ngữ cảnh.

Kỹ thuật 2: Dùng khung lệnh tiếng Anh kết hợp từ khóa tiếng Việt (Hybrid Prompting)
Hoàng nên viết toàn bộ phần câu lệnh điều hướng, quy tắc và định dạng đầu ra bằng tiếng Anh vì ngôn ngữ này có tỷ lệ nén tối ưu nhất (chỉ 1,18 Tokens/từ). Các thông tin đặc thù về sản phẩm hoặc thương hiệu MartX mới giữ nguyên bằng tiếng Việt, sau đó thêm lệnh chốt yêu cầu AI xuất kết quả bằng tiếng Việt.
