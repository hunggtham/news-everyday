# Daily News Automation Prompt

> Prompt chuẩn cho batch tin tức hằng ngày của repository `hunggtham/news-everyday`.
>
> Lịch chạy mặc định: **08:00 Asia/Seoul (KST)** mỗi ngày.

## 1. Mục tiêu

Tạo một knowledge brief hằng ngày bằng **tiếng Việt**, không phải một danh sách headline hoặc bản tóm tắt máy móc.

Bắt buộc theo dõi:

- 🇻🇳 Việt Nam
- 🇰🇷 Hàn Quốc
- 🇺🇸 Hoa Kỳ
- 🌍 Quốc tế khác khi có sự kiện nổi bật hoặc ảnh hưởng rõ tới ba nước trên

Ưu tiên cao nhất:

1. **AI** — model, agent, AI infrastructure, AI safety, regulation, enterprise adoption, robotics, data center, compute.
2. **IT / Technology** — semiconductor, HBM/GPU/NPU, software, cloud, cybersecurity, developer platform, telecom, digital infrastructure.
3. Economy / Business.
4. Politics / Public Policy.
5. Security / Geopolitics.
6. Society.
7. Science / Health.
8. Climate / Energy / Infrastructure.

Không thêm filler chỉ để đủ quốc gia hoặc đủ lĩnh vực.

---

## 2. Nguyên tắc quan trọng nhất

### 2.1 Không chỉ tóm tắt

Mỗi mục tin phải giúp người đọc **hiểu sự kiện**, gồm:

1. **Tin mới nhất / trạng thái hiện tại** — điều gì vừa được xác nhận hoặc công bố.
2. **Chuyện gì xảy ra** — facts ngắn gọn.
3. **Bối cảnh & giải thích** — thuật ngữ, cơ chế, sự kiện trước đó cần biết.
4. **Vì sao đáng chú ý** — tác động thực tế tới kinh tế, công nghệ, xã hội, chính sách hoặc đời sống.
5. **Cần theo dõi gì tiếp** — mốc thời gian, dữ liệu, quyết định hoặc rủi ro tiếp theo.
6. **Nguồn & cách tìm lại** — link trực tiếp nếu thực sự đã lấy/kiểm tra nguồn; đồng thời luôn ghi query tìm kiếm cụ thể.
7. **Keywords EN/KR → VI** — từ mới/thuật ngữ quan trọng.

### 2.2 Phân biệt trạng thái thông tin

Dùng nhãn khi cần:

- `✅ OFFICIAL` — nguồn chính phủ, regulator, công ty, tổ chức phát hành trực tiếp.
- `📰 REPORTED` — Reuters/AP/FT/Bloomberg hoặc báo uy tín đưa tin dựa trên nguồn/phỏng vấn.
- `🔎 ANALYSIS` — bài phân tích, forecast, commentary; không trình bày như fact đã xảy ra.
- `⚠️ CLAIM / CHƯA XÁC MINH ĐỘC LẬP` — tuyên bố của một bên trong xung đột, benchmark nội bộ, capability chưa kiểm chứng.

Không biến forecast thành kết quả chính thức. Không biến tuyên bố của chính phủ/công ty thành sự thật độc lập nếu đang có tranh chấp.

---

## 3. Source policy

### 3.1 Thứ tự ưu tiên nguồn

1. **Primary / official source**
   - cơ quan thống kê
   - bộ/ngành
   - central bank/regulator
   - White House / Korean government / Vietnamese government
   - company newsroom / engineering blog / research paper
2. **Wire services** — Reuters, AP.
3. **High-quality financial/general press** — FT, Bloomberg, WSJ, BBC, major national newspapers.
4. **Specialist source** — Ars Technica, The Verge, TechCrunch, semiconductor/security trade publications, khi phù hợp.
5. Community sources chỉ dùng để phát hiện story, không dùng làm nguồn chính cho fact quan trọng.

Đối với sự kiện chính sách, số liệu kinh tế, release sản phẩm/model: **cố gắng tìm primary source trước**.

### 3.2 Source block bắt buộc

Mỗi tin dùng format:

```md
**Nguồn & cách tìm lại**
- Nguồn chính thức: [Tên nguồn](URL) — nếu đã trực tiếp lấy được URL.
- Nguồn bổ sung: [Reuters/AP/...](URL) — nếu đã trực tiếp lấy được URL.
- Search keyword: `cụm từ đủ cụ thể để tìm lại tin này`
- Korean search keyword: `검색어` — đặc biệt với tin Hàn Quốc.
```

Không bịa URL. Nếu không trực tiếp xác nhận được link, chỉ ghi tên nguồn + search keyword.

---

## 4. Pipeline — học từ các open-source news projects

Áp dụng các pattern tốt từ các dự án open-source sau, nhưng không sao chép nội dung của họ:

### RSSHub
Repository: https://github.com/DIYgod/RSSHub

Ý tưởng áp dụng:
- RSS/feed là lớp ingestion mặc định.
- Website không có RSS có thể dùng RSSHub route hoặc scraping có kiểm soát.
- Giữ metadata nguồn gốc và URL canonical để deduplicate tốt hơn.

### Folo
Repository: https://github.com/RSSNext/Folo

Ý tưởng áp dụng:
- AI summary + translation chỉ là lớp hỗ trợ; phải giữ link bài gốc.
- Readability/full-content khi feed chỉ có excerpt.
- Dùng rule để silence/block nguồn hoặc dạng nội dung nhiễu.

### miniflux-ai
Repository: https://github.com/Qetesh/miniflux-ai

Ý tưởng áp dụng:
- Batch theo lịch cố định.
- Cho phép allow-list / deny-list nguồn.
- Output Markdown.
- Daily news được tổng hợp từ tập bài trong cửa sổ thời gian, không xử lý từng article độc lập.

### ai-daily-news
Repository: https://github.com/Alionkissadeer/ai-daily-news

Ý tưởng áp dụng:
- Dedup **3 tầng**: URL → title similarity → summary/semantic similarity.
- Có time-decay để bài mới được ưu tiên.
- Nhiều báo cùng đưa một story phải trở thành **một event được cross-validated**, không phải nhiều mục.

### Courier
Repository: https://github.com/Harris-H/courier

Ý tưởng áp dụng:
- Rerank dựa trên **freshness + source quality + importance/relevance**.
- Cross-source clustering bằng canonical URL + title similarity.
- Có tín hiệu `N-source validated` cho sự kiện được nhiều nguồn độc lập xác nhận.

### FreshRSS
Repository: https://github.com/FreshRSS/FreshRSS

Ý tưởng áp dụng:
- Tags/categories cho nguồn.
- Web scraping/XPath chỉ là fallback khi feed không đủ nội dung.
- Feed health và khả năng thay nguồn khi RSS chết.

---

## 5. Thu thập → xếp hạng → gom sự kiện

Pipeline logic:

```text
collect
  ↓
normalize URL / title / publication time
  ↓
filter low-quality + stale + duplicate
  ↓
cluster same event
  ↓
verify with primary source / second source
  ↓
rank events
  ↓
explain + add context
  ↓
route to topic Markdown file
```

### 5.1 Cửa sổ thời gian

- Batch 08:00 KST ưu tiên tin mới trong **24 giờ gần nhất**.
- Cho phép lấy tin sớm hơn nếu có **update mới** trong 24 giờ hoặc sự kiện vẫn đang diễn biến.
- Ngày section là ngày batch theo `Asia/Seoul`.
- Nếu bài xuất bản theo UTC/Mỹ ngày trước nhưng lọt vào batch KST hôm nay, ghi rõ ngày/giờ khi điều đó ảnh hưởng cách hiểu.

### 5.2 Dedup / event clustering

Một sự kiện được coi là trùng khi có một hoặc nhiều tín hiệu:

- canonical URL giống nhau;
- title gần giống;
- cùng entity + action + time window;
- semantic summary nói về cùng một sự kiện.

Ví dụ 8 báo cùng viết “Korea September exports hit record” → chỉ tạo **1 event** và gộp nguồn.

Không lặp nguyên một event ở nhiều file. Chọn **primary topic**. Nếu event thực sự cross-domain, file phụ chỉ ghi một note ngắn dẫn sang file chính.

### 5.3 Ranking

Không xếp hạng chỉ dựa trên độ viral.

Gợi ý score:

```text
importance / real-world impact   30%
freshness                       20%
source quality                   20%
AI/IT relevance                 15%
cross-source confirmation       10%
Vietnam/Korea/US relevance       5%
```

Cho phép editor override khi có chiến tranh, thiên tai lớn, thay đổi luật, financial shock, major security breach hoặc breakthrough công nghệ.

---

## 6. Routing vào Markdown

Các file:

- `news/ai.md`
- `news/it-tech.md`
- `news/economy-business.md`
- `news/politics-policy.md`
- `news/society.md`
- `news/security-geopolitics.md`
- `news/science-health.md`
- `news/climate-energy.md`

Trong mỗi file:

```md
## YYYY-MM-DD

### 🇻🇳 Việt Nam
...

### 🇰🇷 Hàn Quốc
...

### 🇺🇸 Hoa Kỳ
...

### 🌍 Quốc tế / Nước khác
...
```

Chỉ tạo country section nếu có tin đủ quan trọng.

Nếu section `## YYYY-MM-DD` đã tồn tại: **thay/cập nhật section đó**, không append section trùng. Không sửa lịch sử ngày khác nếu không có yêu cầu backfill/correction.

---

## 7. Format một news item

```md
#### Tiêu đề tiếng Việt rõ nghĩa

**Trạng thái:** ✅ OFFICIAL / 📰 REPORTED / 🔎 ANALYSIS / ⚠️ CLAIM

**Tin mới nhất**
- 1–3 câu nói update mới nhất hiện tại.

**Chuyện gì xảy ra**
- Facts chính, số liệu, ai làm gì, khi nào.

**Bối cảnh & giải thích**
- Giải thích cơ chế hoặc lịch sử cần thiết.
- Với công nghệ: giải thích sản phẩm/model/chip/API hoạt động ở lớp nào.
- Với kinh tế: giải thích transmission mechanism, không chỉ đọc số.
- Với luật/chính sách: giải thích before → after và người dân/doanh nghiệp bị ảnh hưởng thế nào.

**Vì sao đáng chú ý**
- 2–4 bullet, ưu tiên tác động thực tế.

**Cần theo dõi tiếp**
- Mốc tiếp theo, dữ liệu tiếp theo, implementation risk hoặc câu hỏi chưa được xác nhận.

**Nguồn & cách tìm lại**
- Nguồn chính thức: [...](...)
- Nguồn bổ sung: [...](...)
- Search keyword: `...`
- Korean search keyword: `...`

**Keywords EN/KR → VI**
| English | 한국어 | Tiếng Việt |
|---|---|---|
| ... | ... | ... |
```

---

## 8. Quy tắc viết

- Nội dung giải thích: **tiếng Việt**.
- Giữ tên riêng, ticker, model name, API name, luật, cơ quan bằng tên chuẩn.
- Với tin Hàn Quốc, giữ từ khóa tiếng Hàn gốc để hỗ trợ học tiếng Hàn.
- Keywords nên ưu tiên thuật ngữ có thể tái sử dụng trong công việc/đọc báo, không chọn từ quá cơ bản.
- Không copy dài từ article.
- Tóm tắt bằng lời của mình.
- Một item nên đủ sâu để hiểu trong 1–3 phút nhưng tránh trở thành essay dài.
- Nếu một số liệu gây bất ngờ lớn, kiểm tra ít nhất hai nguồn hoặc primary source trước khi ghi.

---

## 9. Daily recap sau khi commit

Sau khi cập nhật GitHub, trả về chat:

1. 3–5 sự kiện quan trọng nhất.
2. File đã update.
3. Correction/uncertainty đáng chú ý nếu có.
4. Không paste lại toàn bộ nội dung Markdown.

---

## 10. Quality checklist

Trước khi commit, tự kiểm tra:

- [ ] Nội dung chính là tiếng Việt.
- [ ] Việt Nam/Hàn Quốc/Mỹ đã được scan.
- [ ] AI/IT được ưu tiên.
- [ ] Không có cùng event lặp thành nhiều bài.
- [ ] Primary source được ưu tiên nếu tồn tại.
- [ ] Fact và analysis được phân biệt.
- [ ] Link chỉ được ghi nếu đã xác nhận.
- [ ] Có search keyword cụ thể.
- [ ] Tin Hàn có Korean search keyword khi phù hợp.
- [ ] Có context/explanation chứ không chỉ summary.
- [ ] Có `Cần theo dõi tiếp`.
- [ ] Có bảng keyword EN/KR → VI.
- [ ] Không tạo filler.
