# Mongo DB 與 傳統資料庫 架構的對應 #

|                              | Mongo DB                                              | Relation DB                                        |
|------------------------------|-------------------------------------------------------|----------------------------------------------------|
| 文件導向與傳統資料庫差異     | one JSON                                              | multi table join                                   |
| 模式自由與傳統資料庫差異     | Schema-free                                           | 資料庫的資料表增加欄位都會造成麻煩                 |
| 分散式儲存與傳統資料庫的差異 | 橫向擴展(Scale out)，增加新node會自動分片(auto-shard) | 縱向擴展Scale up，將原RDB資料dumo出來放到更大的RDB |
| 元件對應                     | Database                                              | Database                                           |
| 元件對應                     | Collection                                            | Table                                              |
| 元件對應                     | Document                                              | Row                                                |
| 元件對應                     | Field                                                 | Column                                             |
| 語法                         | AdHoc(insert自動產生)                                 | SQL(欄位需要事先定義好)                            |

P.S. Mongo DB會自動產生 `_id` 欄位，作為**primary key**