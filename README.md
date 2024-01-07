# インターネットTV

## テーブル設計
**チャンネル**  
**channel テーブル**
|カラム名|データ型|NULL|キー|初期値|AUTO INCREMENT|例|
| ---------- | ---------- | ---------- | ---------- | ---------- | ---------- | ---------- |
|channel_id  |BIGINT(20)  |NO          |PRIMARY     |            |YES         |000000000000|
|channel_name|VARCHAR(100)|NO          |UNIQUE      |            |            |ニュース1    |
- ユニークキー制約：channel_name  
同じチャンネル名があることで重複データが存在することになる為

**時間帯**  
**time_slot テーブル**
|カラム名|データ型|NULL|キー|初期値|AUTO INCREMENT|例|
| -------------- | ---------- | ---------- | ---------- | ---------- | ---------- | ----------------- |
|time_slot_id    |BIGINT(20)  |NO          |PRIMARY     |            |YES         |000000000000       |
|start_time      |DATETIME    |NO          |            |            |            |2024-01-01 00:00:00|
|end_time        |DATETIME    |NO          |            |            |            |2024-01-01 00:30:00|
|program_frame_id|BIGINT(20)  |NO          |FOREIGN     |            |            |000000000000       |
- 外部キー制約：program_frame_id

**番組枠**  
**program_slot テーブル**
|カラム名|データ型|NULL|キー|初期値|AUTO INCREMENT|例|
| ----------     | ---------- | ---------- | ---------- | ---------- | ---------- | ----------------- |
|program_frame_id|BIGINT(20)  |NO          |PRIMARY     |            |YES         |000000000000       |
|channel_id      |BIGINT(20)  |NO          |FOREIGN     |            |            |000000000000       |
|program_id      |BIGINT(20)  |NO          |FOREIGN     |            |            |000000000000       |
- 外部キー制約：channel_id
- 外部キー制約：program_id

**番組**  
**program テーブル**
|カラム名|データ型|NULL|キー|初期値|AUTO INCREMENT|例|
| ---------- | ---------- | ---------- | ---------- | ---------- | ---------- | ---------- |
|program_id  |BIGINT(20)  |NO          |PRIMARY     |            |YES         |000000000000|
|title       |VARCHAR(100)|NO          |            |            |            |鬼滅の刃     |
|content     |TEXT        |NO          |            |            |            |「週刊少年ジ..|

**ジャンル**  
**genre テーブル**
|カラム名|データ型|NULL|キー|初期値|AUTO INCREMENT|例|
| ---------- | ---------- | ---------- | ---------- | ---------- | ---------- | ---------- |
|genre_id    |BIGINT(20)  |NO          |PRIMARY     |            |YES         |000000000000|
|genre_name  |VARCHAR(100)|NO          |UNIQUE      |            |            |ニュース／報道|
- ユニークキー制約：genre_name  
同じジャンル名があることで重複データが存在することになる為

**番組 ジャンル**  
**programs_genres テーブル**
|カラム名|データ型|NULL|キー|初期値|AUTO INCREMENT|例|
| ---------------- | ---------- | ---------- | ---------- | ---------- | ---------- | ---------- |
|programs_genres_id|BIGINT(20)  |NO          |PRIMARY     |            |YES         |000000000000|
|program_id        |BIGINT(20)  |NO          |FOREIGN     |            |            |000000000000|
|genre_id          |BIGINT(20)  |NO          |FOREIGN     |            |            |000000000000|
- 外部キー制約：program_id
- 外部キー制約：genre_id

**シーズン**  
**season テーブル**
|カラム名|データ型|NULL|キー|初期値|AUTO INCREMENT|例|
| ----------- | ---------- | ---------- | ---------- | ---------- | ---------- | ---------- |
|season_id    |BIGINT(20)  |NO          |PRIMARY     |            |YES         |000000000000|
|season_number|BIGINT(10)  |NO          |            |            |            |1           |
|program_id   |BIGINT(20)  |NO          |FOREIGN     |            |            |000000000000|
- 外部キー制約：program_id

**エピソード**  
**episode テーブル**
|カラム名|データ型|NULL|キー|初期値|AUTO INCREMENT|例|
| ------------ | ---------- | ---------- | ---------- | ---------- | ---------- | ---------- |
|episode_id    |BIGINT(20)  |NO          |PRIMARY     |            |YES         |000000000000|
|episode_number|BIGINT(20)  |NO          |            |            |            |1           |
|title         |VARCHAR(100)|NO          |            |            |            |残酷        |
|content       |TEXT        |NO          |            |            |            |家族と共に山..|
|play_time     |TIME        |NO          |            |            |            |01:00:00    |
|release_date  |DATE        |NO          |            |            |            |2024-01-01  |
|views         |BIGINT      |NO          |            |0           |            |0           |
|season_id     |BIGINT(20)  |YES         |FOREIGN     |            |            |000000000000|
- 外部キー制約：season_id
