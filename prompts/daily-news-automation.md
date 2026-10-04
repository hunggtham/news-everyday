# Daily News Automation Prompt

> Source-of-truth cho automation của `hunggtham/news-everyday`.
>
> Mục tiêu: tạo **knowledge brief bằng tiếng Việt**, cập nhật nhiều lần trong ngày, đủ rộng để mỗi topic có nhiều sự kiện khác nhau nhưng vẫn deduplicate những bài viết cùng một event.

## 1. Phạm vi

Bắt buộc scan 🇻🇳 Việt Nam, 🇰🇷 Hàn Quốc, 🇺🇸 Hoa Kỳ; thêm 🌍 quốc tế khi có sự kiện nổi bật.

Topic/file:

1. `news/ai.md`
2. `news/it-tech.md`
3. `news/economy-business.md`
4. `news/politics-policy.md`
5. `news/security-geopolitics.md`
6. `news/society.md`
7. `news/science-health.md`
8. `news/climate-energy.md`

AI và IT có ưu tiên cao nhất.

---

## 2. Coverage target: 3–5 event khác nhau/topic/ngày

Deduplication **không có nghĩa mỗi topic chỉ có một tin**.

Mỗi `## YYYY-MM-DD` trong một topic nên hướng tới **3–5 sự kiện khác nhau** nếu có đủ tin chất lượng. Có thể ít hơn nếu ngày đó ít tin; có thể nhiều hơn khi có breaking news lớn.

### Nhiều báo cùng một sự kiện

Ví dụ 5 báo cùng viết về một model mới:

```text
Reuters ─┐
AP      ─┤
FT      ─┼─> 1 EVENT: model mới
WSJ     ─┤
TechCrunch┘
```

Đây là **1 event có nhiều source**, không phải 5 event.

Việc được nhiều nguồn độc lập đưa tin là tín hiệu:

- sự kiện quan trọng hơn;
- độ xác nhận cao hơn;
- tăng `cross-source confirmation` score;
- nhưng event đó vẫn chỉ chiếm **1 slot** trong 3–5 event/topic.

Sau khi merge, tiếp tục tìm **các câu chuyện khác cùng topic**.

### Novelty rule

Candidate mới phải được so với các event đã có trong section hôm nay:

- cùng event + có update thực chất → update event cũ;
- cùng event + chỉ có thêm báo viết lại → tối đa bổ sung source, không tạo event mới;
- câu chuyện khác → có thể thêm thành event mới.

---

## 3. Một news item phải giải thích, không chỉ tóm tắt

Mỗi event gồm:

1. **Tin mới nhất** — trạng thái mới nhất đã xác nhận.
2. **Chuyện gì xảy ra** — facts/số liệu/ai làm gì/khi nào.
3. **Bối cảnh & giải thích** — cơ chế, thuật ngữ, sự kiện trước đó.
4. **Vì sao đáng chú ý** — tác động thực tế.
5. **Cần theo dõi tiếp** — mốc, dữ liệu, implementation risk hoặc câu hỏi chưa xác nhận.
6. **Nguồn & cách tìm lại** — link đã kiểm tra + search keyword cụ thể.
7. **Keywords EN/KR → VI**.

---

## 4. Trạng thái thông tin

- `✅ OFFICIAL` — primary/official source.
- `📰 REPORTED` — Reuters/AP/FT/Bloomberg hoặc báo uy tín.
- `🔎 ANALYSIS` — forecast/commentary/phân tích.
- `⚠️ CLAIM / CHƯA XÁC MINH ĐỘC LẬP` — tuyên bố một phía/capability chưa kiểm chứng.

Không biến forecast thành kết quả chính thức. Không biến government/company claim thành fact độc lập nếu còn tranh chấp.

---

## 5. Source policy

Ưu tiên:

1. Primary/official: chính phủ, regulator, statistics office, central bank, company newsroom/engineering blog, research paper.
2. Reuters / AP.
3. FT / Bloomberg / WSJ / BBC / major national press.
4. Specialist press: Ars Technica, The Verge, TechCrunch, semiconductor/security publications.
5. Community chỉ để phát hiện story.

Mỗi event:

```md
**Nguồn & cách tìm lại**
- Nguồn chính thức: [Tên](URL) — nếu đã xác nhận URL.
- Nguồn bổ sung: [Reuters/AP/...](URL) — nếu đã xác nhận URL.
- Search keyword: `query cụ thể`
- Korean search keyword: `검색어`
```

Không bịa URL.

---

## 6. Open-source patterns áp dụng

Tham khảo pattern từ:

- RSSHub — feed ingestion, canonical URL.
- Folo — giữ bài gốc; AI summary/translation chỉ là lớp hỗ trợ; filter noise.
- miniflux-ai — scheduled batch, allow/deny list, Markdown output.
- ai-daily-news — URL → title → semantic dedup; time decay; multi-source validation.
- Courier — freshness + source quality + importance reranking; cross-source clustering.
- FreshRSS — category/tag, feed health, scraping fallback.

Pipeline:

```text
collect
→ normalize URL/title/time/entity
→ filter low-quality/stale/noise
→ cluster same event
→ merge sources
→ verify primary + secondary source
→ rank
→ compare novelty with today's saved events
→ select/update 3–5 distinct events/topic/day
→ explain + context + next watch
→ write Markdown
```

Gợi ý ranking:

```text
importance / real-world impact   30%
freshness                       20%
source quality                   15%
cross-source confirmation       10%
novelty vs saved events         10%
AI/IT relevance                 10%
Vietnam/Korea/US relevance       5%
```

---

## 7. Multi-run schedule trong ngày — Asia/Seoul

Mỗi automation đọc prompt này, sau đó chỉ sửa file thuộc scope của nó.

### A. AI & IT — 08:00 / 14:00 / 20:00 KST

Scope:

- `news/ai.md`
- `news/it-tech.md`

Mục tiêu:

- 08:00: dựng baseline từ tin Mỹ đêm trước + châu Á sáng sớm;
- 14:00: bổ sung release/deal/security/news mới trong giờ châu Á;
- 20:00: bổ sung tin chiều tối Hàn/Việt và đầu phiên Mỹ nếu đã xuất hiện;
- cuối ngày mỗi file active hướng tới 3–5 event khác nhau.

### B. Economy & Business — 09:00 / 15:00 / 21:00 KST

Scope:

- `news/economy-business.md`

Ưu tiên official macro data, trade, markets, corporate results/deals, FDI, employment, consumer trends. Mỗi lần chạy phải đọc section hiện tại để bổ sung event mới thay vì ghi đè mù quáng.

### C. Politics & Security — 10:00 / 16:00 / 22:00 KST

Scope:

- `news/politics-policy.md`
- `news/security-geopolitics.md`

Luôn phân biệt official statement, confirmed fact, reported information và disputed claim. Với breaking geopolitical event, update cùng event thay vì tạo nhiều item gần giống nhau.

### D. Society / Science / Climate — 11:00 / 17:00 / 23:00 KST

Scope:

- `news/society.md`
- `news/science-health.md`
- `news/climate-energy.md`

Science/health phải phân biệt rõ:

- preprint;
- peer-reviewed paper;
- observational study;
- clinical trial;
- regulatory approval;
- guideline/standard of care.

Không suy rộng một nghiên cứu nhỏ thành kết luận y khoa chung.

---

## 8. Merge/update khi chạy nhiều lần

Trước khi ghi:

1. đọc `## YYYY-MM-DD` hiện tại;
2. đếm số event hiện có trong từng file;
3. tìm candidate mới trong cửa sổ kể từ lần scan gần nhất, đồng thời nhìn lại tối đa 24 giờ để không bỏ sót;
4. cluster candidate với event hiện có;
5. update event cũ nếu có diễn biến mới;
6. thêm event mới nếu có novelty và đủ quan trọng;
7. hướng tới 3–5 event/topic/ngày nhưng **không thêm filler**;
8. không sửa lịch sử ngày khác trừ correction/backfill được yêu cầu.

Không tạo `## YYYY-MM-DD` trùng.

Không lặp nguyên một event ở nhiều file. Chọn primary topic; file phụ chỉ note ngắn nếu cross-domain thực sự cần thiết.

---

## 9. Format Markdown

```md
## YYYY-MM-DD

### 🇻🇳 Việt Nam
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

Country section chỉ tạo khi có tin chất lượng; không cần đủ mọi nước trong từng topic.

---

## 10. Quy tắc viết

- Nội dung chính bằng **tiếng Việt**.
- Giữ tên riêng, ticker, model/API/product/law/agency chuẩn.
- Tin Hàn Quốc giữ keyword tiếng Hàn gốc.
- Keyword chọn thuật ngữ tái sử dụng được.
- Không copy dài article; paraphrase.
- Số liệu bất thường: ưu tiên primary source hoặc cross-check ít nhất hai nguồn.
- Policy/law: giải thích before → after + đối tượng chịu ảnh hưởng.
- Economy: giải thích transmission mechanism khi hữu ích.
- Tech: giải thích model/chip/API ở lớp nào trong stack.

---

## 11. Recap sau mỗi run

Sau commit:

1. chỉ nêu event **mới hoặc có update thực chất**;
2. file đã thay đổi;
3. số event hiện có trong ngày cho từng file scope;
4. correction/uncertainty nếu có;
5. nếu không có thay đổi đủ quan trọng, không tạo commit chỉ để “đã chạy”.

---

## 12. Quality checklist

- [ ] Nội dung chính là tiếng Việt.
- [ ] Scan đúng scope và VN/KR/US.
- [ ] Mỗi topic active hướng tới 3–5 event khác nhau/ngày.
- [ ] Nhiều bài cùng event đã merge.
- [ ] Cross-source count chỉ tăng importance, không tăng số event.
- [ ] Candidate mới có novelty so với event đã lưu.
- [ ] Primary source được ưu tiên.
- [ ] Fact / analysis / claim được phân biệt.
- [ ] URL chỉ ghi khi xác nhận.
- [ ] Có search keyword cụ thể.
- [ ] Có Korean search keyword khi hữu ích.
- [ ] Có Keywords EN/KR → VI.
- [ ] Không filler để ép đủ quota.
