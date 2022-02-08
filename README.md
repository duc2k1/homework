# homework

## Bài tập - Buổi 3 - Mongodb - 2022-02-02

### 1: use <name_database>

### 2: insertOne(data)

### 3:

Hãy khởi tạo một mẫu dữ liệu json từ 100 - 200 dòng dữ liệu với mô tả
sau:
Đây là dữ liệu lưu trữ thông tin phim
Một bộ phim bao gồm các thông tin như: tên phim, tác giả, thời
lượng (phút), đánh giá (1 - 5 sao), danh sách tên diễn viên, ngày
công chiếu, quốc gia:
Su dung json generator để tạo dữ liệu:

```json
[
    '{{repeat(100, 200)}}',
    {
    name: "{{lorem(1, "words")}}",
    author: "{{lorem(1, "words")}}",
    duration: "{{integer(60, 180)}}",
    rating: "{{integer(1, 5)}}",
    cast: "{{lorem(1, "words")}}",
    releaseDate: "{{date(new Date(2015, 0, 1), new Date(), "YYYY-MM-dd")}}",
    country: "{{lorem(1, "words")}}"
    }
]
```

### 4:

- tìm kiếm bộ phim với tên “Titanic”: find({name: "Titanic"})
- tìm kiếm bộ phim có chứa chữ “titanic”: find({name: /titanic/i})
- tìm kiếm bộ phim bắt đầu bằng chũ “T” không phân biết kỹ tự in hoa thường: find({name: /^T/i})
- tìm kiếm bộ phim thuộc quốc gia Việt Nam và Hoa kỳ: find({country: /(Viet Nam|Hoa kỳ)/i})
- tìm kiếm bộ phim công chiếu trong tháng 1: find({release_date: /^\d{4}-01-\d{2}/})
- tìm kiếm bộ phim có điểm đánh giá hơn 4: find({rating: {$gt: 4}})
- tìm kiếm bộ phim với điều kiện (Công chiếu ở Mỹ và điểm đánh giá trên 3) hoặc (công chiếu ở Việt Nam và điểm đánh giá trên 4): find({$or: [{country: /(Viet Nam|Mỹ)/i, rating: {$gt: 3}}, {country: /(Hoa kỳ|Việt Nam)/i, rating: {$gt: 4}}]})
- tìm kiếm bộ phim của diễn viên A hoặc B: find({$or: [{actors: "A"}, {actors: "B"}]})
- tìm kiếm bộ phim của cả diễn viên A và B: find({$and: [{actors: "A"}, {actors: "B"}]})
- tìm kiếm bộ phim của không có diễn viên A và B: find({$and: [{actors: {$not: "A"}}, {actors: {$not: "B"}}]})

### 5:

- cập nhật một bộ phim có tên là “Titatic” thành “Titatic 2”: updateOne({name: "Titanic"}, {$set: {name: "Titanic 2"}})
- thêm field “plot” cho bộ phim “Titatic” với nội dung 1 đoạn văn tuỳ ý: updateOne({name: "Titanic"}, {$set: {plot: "test test"}})
- thêm field “recommended” với giá trị là “true” cho tất cả phim có điểm đánh giá hơn 4: updateMany({rating: {$gt: 4}}, {$set: {recommended: "true"}})
- cập nhật hoặc thêm mới bộ phim nếu chưa có với tên là “Game of Throne” và trả về kết quả mới nhất: updateOne({name: "Game of Throne"}, {$set: {name: "Game of Throne"}}, {upsert: true})

### 6:

- sử dụng $match, $group và $count để đếm từng mức điểm đánh giá có bao nhiêu phim: aggregate([{$match: {rating: {$gt: 0}}}, {$group: {\_id: "$rating", count: {$sum: 1}}}, {$sort: {\_id: 1}}])
- thêm dữ liệu “year” cho tất cả bộ phim cho kết quả trả về. Gợi ý: Hãy sử dụng $dateToString và $addField : aggregate([{$match: {release_date: {$exists: true}}}, {$addFields: {year: {$dateToString: {format: "%Y", date: "$release_date"}}}}, {$sort: {year: 1}}])
- sắp xếp phim theo điểm đánh giá từ cao tới thấp, bỏ qua 2 bộ phim đầu tiên và láy ra 5 bộ phim tiếp theo. Gợi ý: Sử dụng $sort, $skip và $limit: aggregate([{$match: {rating: {$gt: 0}}}, {$sort: {rating: -1}}, {$skip: 2}, {$limit: 5}])
- xuất ra danh sách tất cả diễn viên hiên có trong database. Gợi ý: Sử dụng $unwind, $group và $count: aggregate([{$unwind: "$actors"}, {$group: {\_id: "$actors", count: {$sum: 1}}}, {$sort: {count: -1}}])
- xuất ra danh sách phim có số lượng diễn viên nhiều hơn 2. Gợi ý: Sử dụng $size, $addField và $match: aggregate([{$match: {actors: {$size: {$gt: 2}}}}])
