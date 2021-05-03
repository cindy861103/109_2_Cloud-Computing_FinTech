<!--20210503-->
# :memo: Database Services, RDS, DynamoDB  
###### tags: `memo`

## AWS RDS雲端關聯式資料庫
Amazon Relational Database Service(Amazon RDS)是一種Web服務，可讓使用者能夠在雲端中輕鬆設定、操作和擴展關聯式資料庫，其提供經濟實惠、具成本效益、可調整大小的容量，且可自動處理硬體佈建、資料庫設定、修補程式和備份等耗時的管理任務。此服務主要希望能讓使用者能有更多時間專注在應用程式，以提供其所需的快速效能、高可用性、安全性和相容性。主要提供以下六種資料庫引擎：  
* Amazon Aurora(AWS自建的資料庫服務，設定較為複雜)
* PostgreSQL
* MySQL
* MariaDB
* Oracle Database
* SQL Server  
![AWS資料庫引擎](https://i.imgur.com/tUB7q7E.png)

(備註：可以使用 AWS Database Migration Service 將現有的資料庫遷移或複寫到 Amazon RDS上。)

> * AWS RDS 與 DynamoDB 皆屬於「全託管服務」，意旨當資料庫突然故障抑或是一時的流量暴增導致超過初始設定的容量時，其會自動幫使用者擴展，故不需要擔心資料庫超量。  
> * AWS RDS 具有「單點服務」的功能。亦即倘若初始設定為最小計費額度的使用量，但使用期間發現量不足需要擴展，可以一鍵設定到所需要的使用額度【Scale-up】。  

#### 備註：資料庫Replication(雲端-地端資料複寫)機制
Replication泛指資料複製模式，其會把資料完整的同步複製到另一個資料庫的節點上，可以實現運算的擴充與高可用架構，此方式並非分散儲存的概念。
### AWS RDS 實作
1. create database
    * Multi-AZ deployment：
        * 可用於跨國業務只讀的功能
        * 若是雲端的DB一秒讀取和寫入的速度過快，造成server負荷過量而當機，進而無法再從此DB讀取資料，有一種解決方法就是開兩臺DB，A僅設定寫入(沒有開放讀取)，B偵測若是A寫入資料後會利用AWS的電纜自動將A內的資料快速備份到B上，而B僅設定讀取(沒有開放寫入)。
    * Virtual private cloud (VPC)：雲端上的私有網路
        * 資料庫視覺化【方法1】(即時)：<br>若是於RDS上興建一個MySQL的服務，他不會有一個介面像phpMyAdmin一樣可以視覺化資料庫，他只會有一個endpoint，故可以藉由MySQL Workbench或phpMyAdmin等在EC2上連接前述的endpoint。由於EC2和DB都是建立在同一個VPC下，所以可以直接連接溝通，就不用還要copy到外網再連接，可以提升效率。
        *  資料庫視覺化【方法2】：<br>於本機端安裝MySQL Workbench或phpMyAdmin等，個別調整DB的endpoint參數，進而取得AWS RDS上的資料。
    * EndPoint端點：類似一串IP地址，並利用此串地址與DB進行溝通。
3. me
## AWS DynamoDB雲端非關聯式(NoSQL)資料庫
> **NoSQL**之特性：
> * 可以自定義column的名稱，不像SQL是已經既定的。
> * key-value儲存，類似dictionary。
> * 查詢較快速，因為SQL在查詢的時候還需要先關聯才能使用，故許多有資料存取急速需求的公司大多都是使用NoSQL，例如：遊戲軟體公司。

Amazon DynamoDB 是一種鍵值和文件資料庫，可在任何規模下達到小於10毫秒的效能。如同前述所言，其亦為全託管服務、而他具有多區域、多主動式耐用的資料庫特性，加上內建安全性、備份和還原以及記憶體內快取，以供網際網路規模的應用程式使用。依官方文件所言，其每天可以處理超過10兆個請求，而且每秒最多可支援超過2,000萬個請求。
* 全託管的的NoSQL資料庫服務：可提供快速且可預期的效能與無縫的可擴展性，以減少操作與擴展分散式資料庫的管理負擔，不再需要煩惱硬體佈建、設定與組態、複寫、軟體修補或叢集擴展。亦提供靜態加密，解決了保護敏感資料所涉及的操作負擔和複雜性。
* 高度可用性與耐用性：自動將資料表的資料與傳輸流分散到足夠數量的伺服器上，以因應使用者的傳輸量與儲存需求，並同時保持快速、一致的效能。
* 可隨需備份，亦可啟用時間點復原。
* 可自動刪除Table中的過期項目，以協助降低儲存體的使用量及過期資料的儲存成本。
* 案例公司：大多為成長十分快速的公司，例如：Lyft、Airbnb、Redfin、Samsung、Toyota和Capital One等。

## 補充：關聯式 V.S. 非關聯式 之index使用差異
* 關聯式資料庫中的index是可以加快搜尋的功能。
* 非關聯式資料庫(例如：DynamoDB)則屬於key-value類型，index則是唯一可以搜尋到資料的方式，否則就是用Scan進行搜尋，此舉會牽涉到AWS的計費。
(補充：[Day14-概觀(五)DynamoDB基本](https://ithelp.ithome.com.tw/articles/10220577))

## 參考資料
* [Amazon Relational Database Service (RDS)](https://aws.amazon.com/tw/rds/)
* [什麼是 Amazon Relational Database Service (Amazon RDS)？](https://docs.aws.amazon.com/zh_tw/AmazonRDS/latest/UserGuide/Welcome.html)
* [MySQL Replication 主從式架構設定教學](https://reurl.cc/o9GkAD)
* [Amazon DynamoDB](https://aws.amazon.com/tw/dynamodb/)
* [什麼是 Amazon DynamoDB？](https://reurl.cc/0DNANY)

## 實作 - Juypter Notebook
[20210503_AWS_HW5_Cloud_Computing_Database_Services_Uoload](https://reurl.cc/Q7Q0L0)

## 參考資料
* [Boto3 documentation](https://reurl.cc/1gRde8)
* [Boto3 - S3](https://reurl.cc/bzpLE3)
* [DynamoDB documentation](https://reurl.cc/o9GWQv)