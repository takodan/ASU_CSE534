# Unit1
## MTD動態目標防禦概念
1. 目標：
    1. 透過使系統動態化，主動改變網路狀態，使攻擊者更難進行探索和預測
    2. 不斷調整"Attack Surface攻擊面(攻擊者已掌握的可攻擊節點信息)"，增加不確定性
    3. 最終目的是增加攻擊者工作負荷，
    4. 防護方式：預防，緩解，欺騙，恢復
2. 步驟
    1. 制定戰略
        1. 態勢感知：了解目前系統，攻擊圖
        2. 探索更改方式
            1. 增加多樣性可以增加恢復性
            2. 增加複雜性可以縮小攻擊面
    2. 創建機制
        1. 創建
        2. 分析
        3. 評估
        4. 部署
3. MTD類別
    1. 基於網路
        1. 地址隨機化：MAC，IP，端口，ID
        2. 網路重構
        3. 網路會話
        4. 欺騙
4. 系統化方法
    1. 漏洞和攻擊場景分析
        1. 掃描服務部署
        2. 主動和被動狀態監控
        3. 生成攻擊圖
    2. 檢測和預測
        1. 基於主機的非侵入式檢測
        2. 集中式和分散式
        3. 攻擊場景的相關性
    3. 對策選擇和策略檢查
        1. 風險評估
        2. 策略管理
    4. 部署和攻擊場景更新
        1. 攻擊場景更新
        2. 微服務，服務細分
5. 重點
    1. 態勢感知
        1. 監控
        2. 檢測
        3. 有狀態跟蹤
        4. 反饋和調整
    2. 可用性
        1. 對系統傷害
        2. 服務鏈完整
        3. 資源
        4. 成本
    3. 風險管理
        1. AG建模
        2. 度量評估
        3. 策略政策
        4. 決策制定
## 虛擬化安全基礎設施
1. "APT高級持續攻擊"模型
    1. IaaS資源是共享的
    2. 攻擊者可能駐留內部
    3. 攻擊步驟
        1. 偵查
        2. 建立立足點
        3. 橫向移動
        4. 攻擊，數據滲漏
        5. 掩蓋，清除痕跡
    4. 邊界保護
        1. 傳統安全性：使用物理設備建立網路邊界
        2. 雲：虛擬化保護設備，使用部分VM作為保護
