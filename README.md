# インターネットTV

## テーブル設計
**channel テーブル**
|カラム名|データ型|NULL|キー|初期値|AUTO INCREMENT|
| ---------- | ---------- | ---------- | ---------- | ---------- | ---------- |
|id          |BIGINT(20)  |NO          |PRIMARY     |            |YES         |
|channel_name|VARCHAR(100)|NO          |UNIQUE      |            |            |
- ユニークキー制約：channel_name

**genre テーブル**
|カラム名|データ型|NULL|キー|初期値|AUTO INCREMENT|
| ---------- | ---------- | ---------- | ---------- | ---------- | ---------- |
|id          |BIGINT(20)  |NO          |PRIMARY     |            |YES         |
|genre_name  |VARCHAR(100)|NO          |UNIQUE      |            |            |
- ユニークキー制約：genre_name

**program**
|カラム名|データ型|NULL|キー|初期値|AUTO INCREMENT|
| ---------- | ---------- | ---------- | ---------- | ---------- | ---------- |
|id          |BIGINT(20)  |NO          |PRIMARY     |            |YES         |
|title       |VARCHAR(100)|NO          |            |            |            |
|content     |TEXT        |NO          |            |            |            |
|oneoff_id   |BIGINT(20)  |YES         |FOREIGN     |            |            |
|series_id   |BIGINT(20)  |YES         |FOREIGN     |            |            |
- 外部キー制約：oneoff_id
- 外部キー制約：series_id

**program_frame**
|カラム名|データ型|NULL|キー|初期値|AUTO INCREMENT|
| ---------- | ---------- | ---------- | ---------- | ---------- | ---------- |
|id          |BIGINT(20)  |NO          |PRIMARY     |            |YES         |
|start_time  |TIME        |NO          |            |            |            |
|end_time    |TIME        |NO          |            |            |            |
|frame_id    |BIGINT(20)  |NO          |FOREIGN     |            |            |
|program_id  |BIGINT(20)  |NO          |FOREIGN     |            |            |
- 外部キー制約：frame_id
- 外部キー制約：program_id

**programs_genres**
|カラム名|データ型|NULL|キー|初期値|AUTO INCREMENT|
| ---------- | ---------- | ---------- | ---------- | ---------- | ---------- |
|id          |BIGINT(20)  |NO          |PRIMARY     |            |YES         |
|program_id  |BIGINT(20)  |NO          |FOREIGN     |            |            |
|genre_id    |BIGINT(20)  |NO          |FOREIGN     |            |            |
- 外部キー制約：program_id
- 外部キー制約：genre_id

**oneoff**
|カラム名|データ型|NULL|キー|初期値|AUTO INCREMENT|
| ---------- | ---------- | ---------- | ---------- | ---------- | ---------- |
|id          |BIGINT(20)  |NO          |PRIMARY     |            |YES         |
|title       |VARCHAR(100)|NO          |            |            |            |
|content     |TEXT        |NO          |            |            |            |
|play_time   |TIME        |NO          |            |            |            |
|release_date|DATE        |NO          |            |            |            |
|views       |BIGINT      |NO          |            |0           |            |

**series**
|カラム名|データ型|NULL|キー|初期値|AUTO INCREMENT|
| ---------- | ---------- | ---------- | ---------- | ---------- | ---------- |
|id          |BIGINT(20)  |NO          |PRIMARY     |            |YES         |
|season_id   |BIGINT(20)  |NO          |            |            |            |

**season**
|カラム名|データ型|NULL|キー|初期値|AUTO INCREMENT|
| ---------- | ---------- | ---------- | ---------- | ---------- | ---------- |
|id          |BIGINT(20)  |NO          |PRIMARY     |            |YES         |
|series_id   |BIGINT(20)  |NO          |            |            |            |
|episode_id  |BIGINT(20)  |NO          |            |            |            |
- 外部キー制約：series_id
- 外部キー制約：episode_id 

**episode**
|カラム名|データ型|NULL|キー|初期値|AUTO INCREMENT|
| ---------- | ---------- | ---------- | ---------- | ---------- | ---------- |
|id          |BIGINT(20)  |NO          |PRIMARY     |            |YES         |
|title       |VARCHAR(100)|NO          |            |            |            |
|content     |TEXT        |NO          |            |            |            |
|play_time   |TIME        |NO          |            |            |            |
|release_date|DATE        |NO          |            |            |            |
|views       |BIGINT      |NO          |            |0           |            |
