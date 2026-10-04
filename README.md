# News Everyday

Knowledge brief hằng ngày cho **Việt Nam 🇻🇳, Hàn Quốc 🇰🇷, Hoa Kỳ 🇺🇸** và các diễn biến quốc tế nổi bật.

Mục tiêu của repo không phải là lưu càng nhiều headline càng tốt, mà là giữ **nhiều sự kiện đáng chú ý trong mỗi chủ đề**, giải thích đủ bối cảnh để đọc lại vẫn hiểu.

## Nguyên tắc nội dung

- Nội dung chính bằng **tiếng Việt**.
- Giữ keyword quan trọng bằng **English / 한국어 → Tiếng Việt**.
- AI và IT là hai lĩnh vực ưu tiên cao nhất.
- Mỗi topic active hướng tới **3–5 event KHÁC NHAU mỗi ngày** nếu có đủ tin chất lượng.
- Nhiều bài báo viết về cùng một sự kiện được **gom thành một event nhiều nguồn**, không tính thành nhiều news item.
- Một event xuất hiện trên nhiều nguồn độc lập được coi là tín hiệu importance/cross-validation, nhưng vẫn chỉ chiếm một slot.
- Không thêm filler chỉ để đủ quota.
- Ưu tiên primary/official source, sau đó Reuters/AP và nguồn uy tín khác.
- Mỗi news item phải có context, tác động, điều cần theo dõi tiếp, source link/search keyword và vocabulary.

## Topic files

- [`news/ai.md`](news/ai.md) — AI models, agents, AI infrastructure, regulation, robotics, compute
- [`news/it-tech.md`](news/it-tech.md) — semiconductor, HBM/GPU/NPU, software, cloud, cybersecurity, telecom
- [`news/economy-business.md`](news/economy-business.md) — macro, trade, finance, companies, FDI, employment
- [`news/politics-policy.md`](news/politics-policy.md) — government, law, regulation, elections, courts
- [`news/security-geopolitics.md`](news/security-geopolitics.md) — diplomacy, defense, North Korea, conflicts, alliances
- [`news/society.md`](news/society.md) — education, labor, demographics, housing, migration, public services
- [`news/science-health.md`](news/science-health.md) — science, medicine, biotech, public health, space
- [`news/climate-energy.md`](news/climate-energy.md) — climate, extreme weather, energy, transport, infrastructure

## Automation schedule — Asia/Seoul

Thay vì chỉ chạy một batch lúc 08:00, mỗi nhóm topic được scan lại **3 lần/ngày**.

| Automation | Topic | Giờ KST |
|---|---|---|
| AI & IT News | `ai.md`, `it-tech.md` | 08:00 · 14:00 · 20:00 |
| Economy & Business News | `economy-business.md` | 09:00 · 15:00 · 21:00 |
| Politics & Security News | `politics-policy.md`, `security-geopolitics.md` | 10:00 · 16:00 · 22:00 |
| Society Science Climate News | `society.md`, `science-health.md`, `climate-energy.md` | 11:00 · 17:00 · 23:00 |

Mỗi lần chạy:

1. đọc section `## YYYY-MM-DD` hiện tại;
2. tìm tin mới/update kể từ lần trước và nhìn lại tối đa 24 giờ để tránh bỏ sót;
3. cluster các bài cùng event;
4. update event cũ nếu có diễn biến mới thực chất;
5. thêm event mới nếu có novelty;
6. hướng tới 3–5 event khác nhau/topic/ngày;
7. chỉ commit khi có thay đổi đáng kể.

## Event clustering

Ví dụ 5 báo cùng viết về một sự kiện:

```text
Reuters ─┐
AP      ─┤
FT      ─┼─> 1 EVENT
WSJ     ─┤
TechCrunch┘
```

Sau khi merge event đó, pipeline tiếp tục tìm những câu chuyện khác trong cùng topic để đạt độ bao phủ rộng hơn.

## Format một news item

```md
## YYYY-MM-DD

### 🇰🇷 Hàn Quốc

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

## Prompt source-of-truth

Toàn bộ rule chi tiết cho ingestion, ranking, deduplication, event clustering, multi-run merge, source verification và output nằm tại:

[`prompts/daily-news-automation.md`](prompts/daily-news-automation.md)

Automation phải đọc prompt này trước mỗi run.

## Open-source patterns tham khảo

Pipeline tham khảo các pattern hữu ích từ:

- RSSHub
- Folo
- Miniflux / miniflux-ai
- ai-daily-news
- Courier
- FreshRSS

Các ý chính được áp dụng: RSS ingestion, canonical URL, source quality, freshness, URL/title/semantic dedup, event clustering, multi-source validation, Markdown output và scheduled batching.
