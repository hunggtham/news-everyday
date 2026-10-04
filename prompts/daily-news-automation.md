# Daily News Automation Prompt

> Source-of-truth cho toàn bộ automation của repository `hunggtham/news-everyday`.
>
> Mục tiêu: tạo **knowledge brief bằng tiếng Việt**, cập nhật nhiều lần trong ngày, đủ rộng để không bỏ sót tin quan trọng nhưng vẫn tránh lặp cùng một sự kiện.

## 1. Phạm vi bắt buộc

Theo dõi:

- 🇻🇳 Việt Nam
- 🇰🇷 Hàn Quốc
- 🇺🇸 Hoa Kỳ
- 🌍 Quốc tế khác khi có sự kiện nổi bật hoặc ảnh hưởng rõ tới ba nước trên

Các topic/file:

1. `news/ai.md` — AI, model, agent, AI infrastructure, safety, regulation, robotics, compute, data center.
2. `news/it-tech.md` — semiconductor, HBM/GPU/NPU, software, cloud, cybersecurity, developer platform, telecom, digital infrastructure.
3. `news/economy-business.md` — macro, trade, finance, markets, companies, FDI, employment, consumer trends.
4. `news/politics-policy.md` — government, law, elections, regulation, courts, administrative reform.
5. `news/security-geopolitics.md` — diplomacy, defense, North Korea, sanctions, conflicts, alliances.
6. `news/society.md` — education, labor, demographics, housing, migration, culture, public services.
7. `news/science-health.md` — research, medicine, biotech, public health, space.
8. `news/climate-energy.md` — climate, weather, electricity, oil/gas, nuclear, renewables, transport, infrastructure.

AI và IT có mức ưu tiên cao nhất và được quét nhiều lần trong ngày.

---

## 2. Quy tắc mới: mỗi topic phải có nhiều sự kiện khác nhau

Không được hiểu deduplication là “mỗi topic chỉ còn một tin”.

Mục tiêu mỗi ngày cho **mỗi topic có đủ news**:

- ưu tiên **3–5 sự kiện khác nhau** trong `## YYYY-MM-DD`;
- có thể ít hơn 3 nếu thật sự không có đủ tin chất lượng;
- có thể vượt 5 khi có breaking news lớn, nhưng tránh biến file thành feed dài;
- mỗi event phải là một câu chuyện riêng, không phải nhiều bài cùng nói lại một việc.

Ví dụ đúng:

```text
AI ngày X
├─ OpenAI ra model mới
├─ Hàn Quốc công bố AI policy mới
├─ Nvidia/Broadcom ký deal hạ tầng
├─ Việt Nam có đầu tư AI/data center mới
└─ EU/Mỹ có regulation đáng chú ý
```

Ví dụ sai:

```text
AI ngày X
├─ Reuters viết về OpenAI model mới
├─ AP viết về OpenAI model mới
├─ Bloomberg viết về OpenAI model mới
├─ FT viết về OpenAI model mới
└─ TechCrunch viết về OpenAI model mới
```

Năm bài trên phải được gom thành **1 event nhiều nguồn**.

### 2.1 Nhiều nguồn là tín hiệu importance, không phải số lượng news

Nếu cùng một sự kiện xuất hiện trên 5+ nguồn độc lập uy tín:

- tăng điểm `cross-source confirmation`;
- đánh dấu đây là event đáng chú ý hơn;
- có thể ghi `N-source validated` trong ghi chú nội bộ;
- nhưng vẫn chỉ chiếm **1 slot** trong target 3–5 event của topic.

Sau đó tiếp tục tìm các event khác cùng topic để đủ chiều rộng.

---

## 3. Không chỉ tóm tắt

Mỗi news item phải giúp người đọc **hiểu sự kiện**, gồm:

1. **Tin mới nhất / trạng thái hiện tại** — update mới nhất đã được xác nhận.
2. **Chuyện gì xảy ra** — facts, số liệu, ai làm gì, khi nào.
3. **Bối cảnh & giải thích** — khái niệm/cơ chế/sự kiện trước đó cần biết.
4. **Vì sao đáng chú ý** — tác động thực tế.
5. **Cần theo dõi gì tiếp** — mốc tiếp theo, dữ liệu tiếp theo, implementation risk, câu hỏi chưa xác nhận.
6. **Nguồn & cách tìm lại** — URL nếu đã trực tiếp kiểm tra + query tìm kiếm cụ thể.
7. **Keywords EN/KR → VI** — từ mới/thuật ngữ quan trọng.

Một item nên đủ sâu để hiểu trong khoảng 1–3 phút, nhưng tránh thành essay dài.

---

## 4. Trạng thái thông tin

Dùng nhãn khi phù hợp:

- `✅ OFFICIAL` — primary source trực tiếp.
- `📰 REPORTED` — Reuters/AP/FT/Bloomberg hoặc báo uy tín đưa tin.
- `🔎 ANALYSIS` — forecast/commentary/phân tích.
- `⚠️ CLAIM / CHƯA XÁC MINH ĐỘC LẬP` — tuyên bố một phía, capability chưa xác minh, số liệu tranh chấp.

Không biến forecast thành kết quả chính thức. Không biến company/government claim thành fact độc lập nếu chưa được xác minh.

---

## 5. Source policy

Ưu tiên:

1. **Primary / official source** — chính phủ, regulator, central bank, statistics office, company newsroom/engineering blog, research paper.
2. Reuters / AP.
3. FT / Bloomberg / WSJ / BBC / major national press.
4. Specialist source — Ars Technica, The Verge, TechCrunch, semiconductor/security trade press khi phù hợp.
5. Community source chỉ dùng để phát hiện story, không làm nguồn chính cho fact quan trọng.

### Source block bắt buộc

```md
**Nguồn & cách tìm lại**
- Nguồn chính thức: [Tên nguồn](URL) — nếu đã trực tiếp xác nhận URL.
- Nguồn bổ sung: [Reuters/AP/...](URL) — nếu đã trực tiếp xác nhận URL.
- Search keyword: `cụm từ đủ cụ thể để tìm lại tin`
- Korean search keyword: `검색어` — đặc biệt cho tin Hàn Quốc.
```

Không bịa URL.

---

## 6. Pattern học từ open-source news projects

Áp dụng các pattern tốt, không sao chép nội dung:

- **RSSHub** — ingestion từ RSS/route, canonical URL, metadata nguồn.
- **Folo** — giữ link gốc; AI summary/translation chỉ là lớp hỗ trợ; silence/filter noise.
- **miniflux-ai** — batch theo lịch; allow-list/deny-list; Markdown output; tổng hợp theo tập bài.
- **ai-daily-news** — dedup URL → title similarity → semantic similarity; time decay; multi-source validation.
- **Courier** — rerank theo freshness + source quality + importance; cross-source clustering.
- **FreshRSS** — tags/categories; feed health; scraping/XPath chỉ là fallback.

### Pipeline

```text
collect
  ↓
normalize URL/title/time/entity
  ↓
filter low-quality/stale/noise
  ↓
cluster same event
  ↓
merge sources for each event
  ↓
verify primary + secondary source
  ↓
rank events inside each topic
  ↓
select 3–5 distinct events/topic/day
  ↓
explain + context + next watch
  ↓
update topic Markdown
```

---

## 7. Ranking sự kiện

Gợi ý score:

```text
importance / real-world impact   30%
freshness                       20%
source quality                   15%
AI/IT relevance                 10%
cross-source confirmation       10%
novelty vs events already saved 10%
Vietnam/Korea/US relevance       5%
```

`novelty` rất quan trọng: event đã có trong file hôm nay chỉ được update khi có diễn biến mới thực chất, không được chiếm thêm một slot.

Editor override được phép cho chiến tranh, thiên tai lớn, major policy/law change, financial shock, major cyber breach hoặc breakthrough công nghệ.

---

## 8. Multi-run automation trong ngày

Các automation khác nhau sẽ cùng đọc prompt này nhưng chỉ sửa file/topic được giao.

### 08:00 KST — Morning AI & IT

- scope: `news/ai.md`, `news/it-tech.md`
- lookback: ưu tiên 12–24 giờ
- tạo nền 3–5 event/topic nếu có đủ tin
- đặc biệt quét Hàn Quốc, Việt Nam, Mỹ và các release diễn ra trong giờ Mỹ đêm trước

### 10:00 KST — Economy & Business

- scope: `news/economy-business.md`
- ưu tiên official macro data, markets, trade, companies, FDI, earnings, policy transmission
- target 3–5 event/ngày

### 12:00 KST — Politics & Security

- scope: `news/politics-policy.md`, `news/security-geopolitics.md`
- target 3–5 event/topic/ngày
- luôn phân biệt official statement / reported fact / disputed claim

### 14:00 KST — AI & IT Midday Refresh

- scope: `news/ai.md`, `news/it-tech.md`
- chỉ thêm event mới hoặc update thực chất từ sáng
- không lặp lại event cũ chỉ vì xuất hiện thêm báo mới
- nếu topic sáng mới có 1–2 event, tiếp tục tìm để đạt khoảng 3–5 event chất lượng

### 16:00 KST — Society / Science / Climate

- scope: `news/society.md`, `news/science-health.md`, `news/climate-energy.md`
- target 3–5 event/topic/ngày nếu có đủ chất lượng
- science/health phải phân biệt preprint, peer-reviewed paper, trial, regulatory approval, guideline

### 20:00 KST — Evening AI/IT + Day Completion

- ưu tiên `news/ai.md`, `news/it-tech.md`
- sau đó scan nhanh tất cả topic để phát hiện major event còn thiếu
- bổ sung các tin Mỹ mới xuất hiện trong ngày KST
- mục tiêu cuối ngày: mỗi topic active có khoảng 3–5 event khác nhau
- không thêm filler để đạt quota nếu chất lượng thấp

Các automation không được tạo `## YYYY-MM-DD` trùng. Chúng phải đọc section ngày hiện tại trước rồi **merge/update** nội dung.

---

## 9. Dedup / event clustering giữa nhiều lần chạy

Một event được coi là trùng khi có một hoặc nhiều tín hiệu:

- canonical URL giống nhau;
- title tương đồng;
- cùng entity + action + time window;
- semantic summary nói về cùng một sự kiện.

Khi automation chạy sau:

1. đọc các event đã lưu trong `## YYYY-MM-DD`;
2. so sánh candidate mới với event hiện có;
3. nếu cùng event nhưng có update mới: cập nhật **Tin mới nhất**, nguồn và `Cần theo dõi tiếp`;
4. nếu chỉ có thêm bài báo viết lại cùng nội dung: chỉ bổ sung source nếu thực sự tăng độ tin cậy, không tạo event mới;
5. nếu là câu chuyện mới cùng topic: thêm event mới cho đến khoảng 3–5 event chất lượng.

Không lặp nguyên một event ở nhiều file. Chọn **primary topic**. Nếu thực sự cross-domain, file phụ chỉ note ngắn dẫn sang file chính.

---

## 10. Format Markdown

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

Không bắt buộc mỗi quốc gia phải có tin ở từng topic; ưu tiên chất lượng.

Mỗi event:

```md
#### Tiêu đề tiếng Việt rõ nghĩa

**Trạng thái:** ✅ OFFICIAL / 📰 REPORTED / 🔎 ANALYSIS / ⚠️ CLAIM

**Tin mới nhất**
- ...

**Chuyện gì xảy ra**
- ...

**Bối cảnh & giải thích**
- ...

**Vì sao đáng chú ý**
- ...

**Cần theo dõi tiếp**
- ...

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

## 11. Quy tắc viết

- Nội dung chính: **tiếng Việt**.
- Giữ tên riêng, ticker, model/API/product/law/agency bằng tên chuẩn.
- Tin Hàn Quốc giữ keyword tiếng Hàn gốc.
- Keywords chọn thuật ngữ tái sử dụng được, không chọn từ quá cơ bản.
- Không copy dài article; paraphrase.
- Số liệu bất thường phải cross-check primary source hoặc ít nhất hai nguồn uy tín.
- Tin science/health không được suy rộng quá mức từ nghiên cứu nhỏ.
- Tin policy/law phải giải thích before → after + đối tượng bị ảnh hưởng.
- Tin economy phải giải thích transmission mechanism khi có ích.
- Tin tech phải giải thích sản phẩm/model/chip/API nằm ở lớp nào trong stack.

---

## 12. Recap sau mỗi automation

Sau commit, chỉ gửi recap ngắn:

1. event mới hoặc update quan trọng vừa thêm;
2. file đã cập nhật;
3. hiện topic đó có bao nhiêu event trong ngày;
4. correction/uncertainty đáng chú ý;
5. không paste lại toàn bộ Markdown.

---

## 13. Quality checklist

Trước khi commit:

- [ ] Nội dung chính là tiếng Việt.
- [ ] Scan đúng scope của automation.
- [ ] Không có cùng event lặp thành nhiều bài.
- [ ] Mỗi topic active hướng tới 3–5 event khác nhau/ngày.
- [ ] Nhiều nguồn của cùng event được merge, không tính thành nhiều event.
- [ ] Event mới có novelty so với nội dung đã lưu.
- [ ] Primary source được ưu tiên nếu tồn tại.
- [ ] Fact / analysis / claim được phân biệt.
- [ ] Link chỉ ghi nếu đã xác nhận.
- [ ] Có search keyword cụ thể.
- [ ] Tin Hàn Quốc có Korean search keyword khi hữu ích.
- [ ] Có Keywords EN/KR → VI.
- [ ] Không tạo filler chỉ để đủ quota.
