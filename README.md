# インターネットTV

## テーブル設計
**チャンネル**  
**channel テーブル**
|カラム名|データ型|NULL|キー|初期値|AUTO INCREMENT|例|
| ---------- | ---------- | ---------- | ---------- | ---------- | ---------- | ---------- |
|channel_id  |BIGINT      |NO          |PRIMARY     |            |YES         |000000000000|
|channel_name|VARCHAR(100)|NO          |UNIQUE      |            |            |ニュース1    |
- ユニークキー制約：channel_name  
同じチャンネル名があることで重複データが存在することになる為

---

**時間帯**  
**time_slot テーブル**
|カラム名|データ型|NULL|キー|初期値|AUTO INCREMENT|例|
| ------------- | ---------- | ---------- | ---------- | ---------- | ---------- | ----------------- |
|time_slot_id   |BIGINT      |NO          |PRIMARY     |            |YES         |000000000000       |
|start_time     |DATETIME    |NO          |            |            |            |2024-01-01 00:00:00|
|end_time       |DATETIME    |NO          |            |            |            |2024-01-01 00:30:00|
|program_slot_id|BIGINT      |NO          |FOREIGN     |            |            |000000000000       |
- 外部キー制約：program_slot_id

---

**番組枠**  
**program_slot テーブル**
|カラム名|データ型|NULL|キー|初期値|AUTO INCREMENT|例|
| ------------- | ---------- | ---------- | ---------- | ---------- | ---------- | ----------------- |
|program_slot_id|BIGINT      |NO          |PRIMARY     |            |YES         |000000000000       |
|channel_id     |BIGINT      |NO          |FOREIGN     |            |            |000000000000       |
|program_id     |BIGINT      |NO          |FOREIGN     |            |            |000000000000       |
- 外部キー制約：channel_id
- 外部キー制約：program_id

---

**番組**  
**program テーブル**
|カラム名|データ型|NULL|キー|初期値|AUTO INCREMENT|例|
| ---------- | ---------- | ---------- | ---------- | ---------- | ---------- | ---------- |
|program_id  |BIGINT      |NO          |PRIMARY     |            |YES         |000000000000|
|title       |VARCHAR(100)|NO          |            |            |            |鬼滅の刃     |
|content     |TEXT        |NO          |            |            |            |「週刊少年ジ..|

---

**ジャンル**  
**genre テーブル**
|カラム名|データ型|NULL|キー|初期値|AUTO INCREMENT|例|
| ---------- | ---------- | ---------- | ---------- | ---------- | ---------- | ---------- |
|genre_id    |BIGINT      |NO          |PRIMARY     |            |YES         |000000000000|
|genre_name  |VARCHAR(100)|NO          |UNIQUE      |            |            |ニュース／報道|
- ユニークキー制約：genre_name  
同じジャンル名があることで重複データが存在することになる為

---

**番組 ジャンル**  
**programs_genres テーブル**
|カラム名|データ型|NULL|キー|初期値|AUTO INCREMENT|例|
| ---------------- | ---------- | ---------- | ---------- | ---------- | ---------- | ---------- |
|programs_genres_id|BIGINT      |NO          |PRIMARY     |            |YES         |000000000000|
|program_id        |BIGINT      |NO          |FOREIGN     |            |            |000000000000|
|genre_id          |BIGINT      |NO          |FOREIGN     |            |            |000000000000|
- 外部キー制約：program_id
- 外部キー制約：genre_id

---

**シーズン**  
**season テーブル**
|カラム名|データ型|NULL|キー|初期値|AUTO INCREMENT|例|
| ----------- | ---------- | ---------- | ---------- | ---------- | ---------- | ---------- |
|season_id    |BIGINT      |NO          |PRIMARY     |            |YES         |000000000000|
|season_number|INT         |NO          |            |            |            |1           |
|program_id   |BIGINT      |NO          |FOREIGN     |            |            |000000000000|
- 外部キー制約：program_id

---

**エピソード**  
**episode テーブル**
|カラム名|データ型|NULL|キー|初期値|AUTO INCREMENT|例|
| ------------- | ---------- | ---------- | ---------- | ---------- | ---------- | ---------- |
|episode_id     |BIGINT      |NO          |PRIMARY     |            |YES         |000000000000|
|episode_number |INT         |NO          |            |            |            |1           |
|episode_title  |VARCHAR(100)|NO          |            |            |            |残酷        |
|epicode_content|TEXT        |NO          |            |            |            |家族と共に山..|
|play_time      |TIME        |NO          |            |            |            |01:00:00    |
|release_date   |DATE        |NO          |            |            |            |2024-01-01  |
|views          |BIGINT      |NO          |            |0           |            |0           |
|season_id      |BIGINT      |YES         |FOREIGN     |            |            |000000000000|
- 外部キー制約：season_id

---

## テーブル構築手順
1. **データベース作成**
  ```sql
  CREATE DATABASE internet_tv;
  USE internet_tv;
  ```

---

2. **テーブル作成**  
    1. **channelテーブル作成**
      ```sql
      CREATE TABLE channel
      (channel_id BIGINT NOT NULL AUTO_INCREMENT,
      channel_name VARCHAR(100) NOT NULL,
      PRIMARY KEY (channel_id),
      UNIQUE KEY (channel_name));
      ```
    
    2. **programテーブル作成**
      ```sql
      CREATE TABLE program
      (program_id BIGINT NOT NULL AUTO_INCREMENT,
      program_title VARCHAR(100) NOT NULL,
      program_content TEXT NOT NULL,
      PRIMARY KEY (program_id));
      ```

    3. **program_slotテーブル作成**
      ```sql
      CREATE TABLE program_slot
      (program_slot_id BIGINT NOT NULL AUTO_INCREMENT,
      channel_id BIGINT NOT NULL,
      program_id BIGINT NOT NULL,
      PRIMARY KEY (program_slot_id),
      FOREIGN KEY (channel_id) REFERENCES channel (channel_id),
      FOREIGN KEY (program_id) REFERENCES program (program_id));
      ```

    4. **time_slotテーブル作成**
      ```sql
      CREATE TABLE time_slot
      (time_slot_id BIGINT NOT NULL AUTO_INCREMENT,
      start_time DATETIME NOT NULL,
      end_time DATETIME NOT NULL,
      program_slot_id BIGINT NOT NULL,
      PRIMARY KEY (time_slot_id),
      FOREIGN KEY (program_slot_id) REFERENCES program_slot (program_slot_id));
      ```

    5. **genreテーブル作成**
      ```sql
      CREATE TABLE genre
      (genre_id BIGINT NOT NULL AUTO_INCREMENT,
      genre_name VARCHAR(100) NOT NULL,
      PRIMARY KEY (genre_id),
      UNIQUE KEY (genre_name));
      ```

    6. **programs_genresテーブル作成**
      ```sql
      CREATE TABLE programs_genres
      (programs_genres_id BIGINT NOT NULL AUTO_INCREMENT,
      program_id BIGINT NOT NULL,
      genre_id BIGINT NOT NULL,
      PRIMARY KEY (programs_genres_id),
      FOREIGN KEY (program_id) REFERENCES program (program_id),
      FOREIGN KEY (genre_id) REFERENCES genre (genre_id));
      ```

    7. **seasonテーブル作成**
      ```sql
      CREATE TABLE season
      (season_id BIGINT NOT NULL AUTO_INCREMENT,
      season_number INT NOT NULL,
      program_id BIGINT NOT NULL,
      PRIMARY KEY (season_id),
      FOREIGN KEY (program_id) REFERENCES program (program_id));
      ```

    8. **episodeテーブル作成**
      ```sql
      CREATE TABLE episode
      (episode_id BIGINT NOT NULL AUTO_INCREMENT,
      episode_number INT NOT NULL,
      episode_title VARCHAR(100) NOT NULL,
      episode_content TEXT NOT NULL,
      play_time TIME NOT NULL,
      release_date DATE NOT NULL,
      views BIGINT NOT NULL DEFAULT 0,
      season_id BIGINT,
      PRIMARY KEY (episode_id),
      FOREIGN KEY (season_id) REFERENCES season (season_id));
      ```

---

  3. **サンプルデータの格納**  
    1. **channelテーブル データ格納**
      ```sql
      INSERT INTO channel (channel_name)
      VALUES ('news1'),
             ('entertainment'),
             ('comedy'),
             ('anime');
      ```

    2. **programテーブル データ格納**
      ```sql
      INSERT INTO program (program_title, program_content)
      VALUES ('kimetunoyaiba', 'syukansyounennjumpnoninnkimannga'),
             ('doramaA', 'doramaAnogaiyouyaarasuji'),
             ('testanime', 'kokokarahajimaru');
      ```

    3. **genreテーブル データ格納**
      ```sql
      INSERT INTO genre (genre_name)
      VALUES ('action'),
             ('dorama'),
             ('anime');
      ```

    4. **programs_genresテーブル データ格納**
      ```sql
      INSERT INTO programs_genres (program_id, genre_id)
      VALUES (1, 3),
             (2, 2),
             (3, 3);
      ```

    5. **seasonテーブル データ格納**
      ```sql
      INSERT INTO season (season_number, program_id)
      VALUES (1, 1),
             (2, 1),
             (3, 1);
      ```

    6. **episodeテーブル データ格納**
      ```sql
      INSERT INTO episode (episode_number, episode_title, episode_content, play_time, release_date, season_id)
      VALUES (1, 'zannkoku', 'kamadotannjirohaonitaijiniiku', '00:30:00', '2024-01-15', 1),
             (2, 'urokodakisakonnji', 'tannjirouhatomiokagiyuuno', '00:30:00', '2024-02-01', 1);
      ```
    
    7. **program_slotテーブル データ格納**
      ```sql
      INSERT INTO program_slot (channel_id, program_id)
      VALUES (17, 1),
             (17, 3);
      ```

    8. **time_slotテーブル データ格納**
      ```sql
      INSERT INTO time_slot (start_time, end_time, program_slot_id)
      VALUES ('2024-01-01 00:00:00', '2024-01-01 00:30:00', 1),
             ('2024-01-01 00:30:00', '2024-01-01 01:00:00', 2);
      ```
      