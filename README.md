# News Everyday

Knowledge brief tin tức hằng ngày cho **Việt Nam 🇻🇳, Hàn Quốc 🇰🇷, Hoa Kỳ 🇺🇸** và các sự kiện quốc tế nổi bật.

Mục tiêu của repository này **không phải lưu một đống headline hoặc summary ngắn**. Mỗi tin quan trọng phải giúp người đọc hiểu:

1. Tin mới nhất hiện tại là gì?
2. Chuyện gì thực sự đã xảy ra?
3. Bối cảnh/cơ chế phía sau là gì?
4. Vì sao nó quan trọng?
5. Tiếp theo cần theo dõi điều gì?
6. Tìm lại nguồn bằng link hoặc keyword nào?
7. Những thuật ngữ English / 한국어 nào đáng học?

## ⏰ Lịch cập nhật

- Batch tự động: **08:00 Asia/Seoul (KST) mỗi ngày**.
- Cửa sổ ưu tiên: khoảng **24 giờ gần nhất**.
- Nếu một sự kiện cũ có update mới, update mới vẫn được đưa vào batch.
- Ngày section dùng ngày theo **KST**; khi timezone dễ gây hiểu nhầm, nội dung sẽ ghi rõ thời điểm công bố thực tế.

## ⭐ Thứ tự ưu tiên

1. **AI**
2. **IT / Technology**
3. Economy / Business
4. Politics / Public Policy
5. Security / Geopolitics
6. Society
7. Science / Health
8. Climate / Energy / Infrastructure

Không thêm filler chỉ để đủ quốc gia hoặc đủ lĩnh vực.

## 📂 Cấu trúc

- [`news/ai.md`](news/ai.md) — AI models, agents, AI infrastructure, safety, regulation, adoption
- [`news/it-tech.md`](news/it-tech.md) — software, cloud, cybersecurity, semiconductor, telecom, developer platforms
- [`news/economy-business.md`](news/economy-business.md) — GDP, inflation, trade, finance, markets, investment, companies
- [`news/politics-policy.md`](news/politics-policy.md) — government, law, courts, regulation, administrative reform
- [`news/society.md`](news/society.md) — education, labor, demographics, housing, culture, public services
- [`news/security-geopolitics.md`](news/security-geopolitics.md) — diplomacy, defense, North Korea, war, sanctions, alliances
- [`news/science-health.md`](news/science-health.md) — science, medicine, biotech, public health, research
- [`news/climate-energy.md`](news/climate-energy.md) — climate, weather, oil/gas, electricity, infrastructure, resilience

Automation prompt chuẩn được lưu tại:

- [`prompts/daily-news-automation.md`](prompts/daily-news-automation.md)

File prompt này là **source-of-truth** cho cách thu thập, ranking, dedup, clustering, verification và viết nội dung hằng ngày.

## 🧠 Format của một news item

```md
## YYYY-MM-DD

### 🇰🇷 Hàn Quốc

#### Tiêu đề tiếng Việt rõ nghĩa

**Trạng thái:** ✅ OFFICIAL / 📰 REPORTED / 🔎 ANALYSIS / ⚠️ CLAIM

**Tin mới nhất**
- Update mới nhất.

**Chuyện gì xảy ra**
- Facts chính.

**Bối cảnh & giải thích**
- Giải thích thuật ngữ, cơ chế, trước → sau hoặc nguyên nhân → hệ quả.

**Vì sao đáng chú ý**
- Tác động thực tế.

**Cần theo dõi tiếp**
- Mốc tiếp theo / dữ liệu tiếp theo / điểm chưa chắc chắn.

**Nguồn & cách tìm lại**
- Nguồn chính thức: [Tên nguồn](URL)
- Nguồn bổ sung: [Reuters/AP/...](URL)
- Search keyword: `exact search query`
- Korean search keyword: `정확한 검색어`

**Keywords EN/KR → VI**
| English | 한국어 | Tiếng Việt |
|---|---|---|
| ... | ... | ... |
```

## 🧾 Trạng thái nguồn

- `✅ OFFICIAL` — cơ quan chính phủ/regulator/công ty/paper công bố trực tiếp.
- `📰 REPORTED` — Reuters, AP hoặc nguồn báo chí uy tín đưa tin.
- `🔎 ANALYSIS` — phân tích/forecast, không trình bày như fact đã xảy ra.
- `⚠️ CLAIM` — tuyên bố của một bên chưa được xác minh độc lập.

Đối với số liệu kinh tế, thay đổi luật, model/product release và chính sách lớn, ưu tiên **primary source trước** rồi dùng báo chí để bổ sung bối cảnh.

## 🔁 Dedup & event clustering

Không làm kiểu:

```text
Reuters: Korea exports hit record
Yonhap: Korea exports hit record
Korea Times: Korea exports hit record
Bloomberg: Korea exports hit record
        ↓
4 news items ❌
```

Mà gom thành:

```text
1 EVENT
├─ official data
├─ Reuters
├─ local source
└─ specialist analysis
        ↓
1 explained, cross-validated news item ✅
```

Một event chỉ nên có **một topic chính** để tránh lặp giữa nhiều file.

## 🛠️ Open-source design references

Workflow tham khảo các pattern tốt từ:

- [RSSHub](https://github.com/DIYgod/RSSHub) — tạo/chuẩn hóa RSS cho nhiều nguồn.
- [Folo](https://github.com/RSSNext/Folo) — reader, AI summary/translation, readability và rules.
- [Miniflux-AI](https://github.com/Qetesh/miniflux-ai) — scheduled AI news, filters, Markdown output.
- [AI Daily News](https://github.com/Alionkissadeer/ai-daily-news) — multi-stage dedup + recency scoring + Markdown publishing.
- [Courier](https://github.com/Harris-H/courier) — reranking + cross-source clustering.
- [FreshRSS](https://github.com/FreshRSS/FreshRSS) — feed organization, tagging, scraping fallback.

Các project này chỉ được dùng để tham khảo **kiến trúc/pipeline**, không sao chép nội dung báo.

## ✅ Quality rules

- Nội dung chính bằng tiếng Việt.
- Việt Nam / Hàn Quốc / Hoa Kỳ luôn được scan; quốc tế thêm khi đáng kể.
- AI và IT được ưu tiên cao nhất.
- Không lặp cùng một event thành nhiều headline.
- Không biến forecast thành official result.
- Không biến claim trong xung đột thành fact độc lập.
- Không bịa URL; nếu chưa xác minh link thì chỉ ghi nguồn + search keyword.
- Luôn thêm keyword English/Korean khi hữu ích.
- Không có tin đáng kể thì **để trống**, không thêm filler.
