# overview
1. Understand the principle of network firewall
2. Use Iptables firewall
3. Understand the principle of IDS
4. Use Snort IDS


# Unit1
## 防火牆
1. 可以是軟件或是硬件
2. 除保護免受外部威脅, 也可以過濾內部
3. 篩選授權流量, 區隔授信任網路和不受信任網路
4. 傳統防火牆基於鏈路層, 網路層, 傳輸層, 檢查此些報頭
5. 現今防火牆也用來區隔虛擬網路, 管理一台主機內部的網路
6. 防火牆要求
    1. 準確性: 正確判斷網路違規行為
    2. 靈活性: 可以更新策略或處理衝突
    3. 效率
7. 防火牆功能
    1. 定義一個阻塞點檢查傳入傳出流量
        1. ACL權限控制表: 限制可以訪問的客戶端, 也稱為firewall rule
        2. 也可以提供NAT功能
    2. 防火牆日誌
8. 防火牆限制
    1. 防火牆主應基於權限控制, 無法阻止偽裝
    2. 主要檢查L2到L4報頭, 比較難檢測應用程序層
    3. 無法阻擋不經過阻塞點的威脅
    4. 無法阻擋授權用戶進行非授權活動
    5. 基於人為設置, 防火牆無法識別配置不當的策略
    6. 傳統防火牆是無狀態的, 無法識別是傳入或傳出
9. 常見防火牆架構
    1. 互聯網
    2. 外部路由防火牆: 讓互聯網可以訪問DMZ 內主機
    3. DMZ非軍事化區: 內部可以互相訪問
        1. web服務器
        2. 應用服務器
        3. 代理服務器
    4. 內部路由器: 讓互聯網不能訪問重要數據
    5. 數據服務器
10. 常見防火牆分類
    1. single box單盒防火牆
        1. 屏蔽路由器
            1. 主要作用是阻止互聯網訪問內部網路
            2. 缺點
                1. 通常較為簡單, 功能較為陽春
                2. 日誌功能缺少或較差
                3. ACL 功能簡單較難管理
                4. 只考慮L3, L4
        2. dual-host/dual-homed雙宿主主機
            1. 由主機連接互聯網和路由器或交換機
            2. 通常會禁止路由器功能, 而是作為代理服務器讓內部使用, 也使得此主機可以訪問應用程序層
            3. 缺點
                1. 許要管理許多用戶帳戶
                2. 代理某些服務可能會需要特殊軟件
                3. 單一故障點, 主機較為複雜故障率也可能較高
        3. 多功能盒
            1. 以主機取代路由器, 也稱為軟路由
            2. 共享以上優缺點
    2. 屏蔽主機架構
        1. 同樣使用屏蔽路由器, 但能互聯網只能訪問堡壘主機
        2. 其他主機只能藉由堡壘主機訪問網路, 功能類似雙宿主主機
        3. 相比雙宿主主機由於主機在內部網路, 堡壘主機可以由其他備用主機替代
    3. 屏蔽子網架構
        1. 互聯網主要可以訪問堡壘主機, 但是堡壘主機和其他主機之間還有一個防火牆作區隔
        2. 外部路由器只做簡單的過濾(黑名單), 保護周邊網路/DMZ上的主機
        3. 內部路由器只會讓特定的主機訪問互聯網(白名單)
11. 其他防火牆網路部署
    1. 多堡壘主機: 不同主機負責不同服務, 外部路由器設定更多規則
    2. 合併路由器: 一個路由器負責外部和內部路由
    3. 外部路由器合併堡壘主機: 多功能盒加上內部路由器
    4. 內部路由器合併堡壘主機: 因為堡壘主機較可能被入侵, 不推薦
    5. 多部路由合併使用: 一個網路有多個路由, 可能效率較低維護上十分困難, 可能形成環, 不推薦
    6. 有不同網路供應商: 由不同的內部路由器連接到單一內部路網路
12. 分布式防火牆
    1. 內部切分子網以防火牆隔離
    2. 防火牆統一管理維護上較簡單, 但也有可能有安全風險
13. 微分防火牆
    1. 在數據中心或雲環境建立小區域, 運坐在單獨主機內

## NAT網路地址轉換
1. NAT 作用在L3網路層和L4傳輸層, IP轉換和端口轉換
2. 大多數主機是客戶端在私域底下, 互聯網通信需要找到公有IP再轉換為私有IP
3. 過去switch 就主要是進行NAT, 現在多合併進路由器
4. 以Iptables軟件防火牆為例
    1. 端口資訊傳入, 進行prerouting ,其中會查看是否需要進行NAT
    2. 如果需要進行NAT, 會改寫IP後 進入forward ,再直接連接到postrouting 到端口發送
    3. 如果是內到外, 通常會改寫源IP, 外到內會改寫目標IP
    4. 如果不需NAT會進入input 後傳送給本地
5. Iptables軟件防火牆的NAT操作
    1. DNAT(destination): 重寫目標IP
    2. SNAT(source): 重寫源IP
    3. MASQ: 只要是經過此防火牆到目標出口端口, 源IP 都改成運行主機的IP
6. 基於NAT 的端口映射
    1. 也稱為NAPT 或PAT
    2. 傳統NAT只做IP對映, 現今對映可以包含端口
    3. 例如公有IP的80端口對映A主機的8080端口, 公有IP的81可以對映B主機的8080端口


# Unit2
## 入侵和检测
1. 網路安全機制
    1. 預防: 例如訪問控制(防火牆)
    2. 檢測: 審計流量和入侵檢測(IDS(intrusion detection system))
    3. 容忍: 被入侵能否承受攻擊(ITS(intrusion tolerance system), 備份)
2. 安全目標CIA
    1. C: 完整性
    2. I: 保密性
    3. A: 可用性
3. 入侵檢測
    1. sniffing嗅探
    2. analysis分析
    3. identify識別
4. 具體事例
    1. 遠程root
    2. web服務器破壞
    3. 猜測/ 破解密碼
    4. 複製/ 查看敏感數據
    5. 數據包嗅探(UDP就較難有效加密)
    6. 盜用身份(網路釣魚)
    7. 盜用已登入的電腦
5. IDS入侵檢測系統
    1. 組成
        1. 傳感器: 收集數據
        2. 分析器: 處理傳感器得到的數據
        3. 用戶介面
    2. 流程
        1. 數據預處理: 將訊息轉換成能觀察的數據
        2. 檢測引擎: 利用檢測模型分辨入侵活動
        3. 決策引擎: 根據決策表進行反應
    3. 檢測模型:
        1. 濫用檢測: 基於簽名
            1. 拆包, 根據過去制定規則檢測
            2. 無法檢測到新攻擊
        2. 異常檢測: 基於統計
            1. 訂定正常基準, 如果偏離可能是攻擊
            2. 網路流量, 系統負載都可以是檢測對象
            3. 只知道可能有異常, 但不能確定是哪個部分遭受攻擊
6. 主機的IDS
    1. 使用OS審計和監控
    2. 缺點
        1. 依賴人力進行安裝更新到每一台主機
        2. 如果被root, IDS或日誌可能會被篡改
        3. 如果被攻擊只能得到局部視圖
7. 網路的IDS
    1. 數據包捕獲(例如使用libcap)
    2. 協議, 內容分析
    3. 簽名匹配, 數據解析
6. IPS入侵防禦系統
    1. 在線運行過濾, 即時檢測並攔截, 數據包必須先進行檢測再向下發送, 類似於防火牆
    2. 相較於網路IDS 是並行檢測, 數據會複製到IDS 檢測, 並不會在一開始阻擋有問題的傳輸
    3. IDS 可能會因為一些攻擊手段沒有接受到數據包
    4. IPS缺點是處理速度會影響網路運行速度
7. 防火牆, IDS, IPS差異
    1. 防火牆: 側重檢查包報頭, 拒絕不匹配的數據包, 例如iptables
    2. IDS: 側重整個數據包, 檢測到事件並紀錄, 例如snort
    3. IPS: 側重整個數據包, 檢測到已知事件可以拒絕, 可以使用防火牆加IDS 來達成
8. 檢測關鍵指標
    1. IPS: 要更注意false positive, 因為被拒絕的包就不會進入主機
    2. IDS: 要更注意false negative, 因為要盡可能的偵測到攻擊以進行分析
9. 常見工具
    1. 企業工具: Cisco safe, Symantec
    2. Honeyd: 蜜罐創造一個虛擬系統讓人攻擊, 以便取得攻擊手段, 追蹤攻擊者等等
    3. Snort: 開源的網路IDS

## Snort
1. 流程
    1. 網路數據包
    2. libpcap->過濾的事件包
    3. 事件引擎->事件流
    4. 策略腳本解釋器->通知
2. 架構
    1. 數據包解碼器: 分析L2(MAC), L3(IP), L4(port)
    2. 預處理器: 處理收到的數據包
        1. 可以根據差件進行不同處理
        2. 可以重新彙編(整合)TCP流
    3. 檢測引擎: 和規則對比
        1. 匹配認定為攻擊會進入日誌和警報系統
        2. 非攻擊會直接丟棄
        3. 也可以改進行轉發
    4. 日誌紀錄和警報系統
5. Snort規則
    1. 字定義規則寫在"/ect/snort/rule/local.rule"
    2. 激活: 激活動態規則, 並決定是否要發送警報
    3. 動態: 被激活後才會進行對比
    4. 警報: 發送警報並紀錄, 默認存在"/var/log/snort"
    5. 忽略: 忽略特定規則的包
    6. 紀錄: 只記錄簿發送
6. 規則事例
    1. 規則頭 
    - "Alert tcp 1.1.1.1 any -> 2.2.2.2 any
    - 警報 tcp報頭 1.1.1.1 任何端口 發送給 2.2.2.2 任何端口
    2. 規則選項, 實際會和規則頭寫在同一行
    - "(flag:SF; msg:"SYN-FIN Scan";
    - sid:103); rev:4;"
    - 如果TCP flag是SF 發送警報並紀錄訊息
    - 規則id:103, 規則版本rev:4
    - 更改規則必須更改規則版本以確保Snort更新規則
### 更多規則規範參考21:51
### 實際規則事例參考27:16
7. 輸出模塊
    1. 可以輸出到文件或以XML輸出
    2. 發送SNMP信息
    3. 紀錄到syslog
    4. 紀錄到數據褲
    5. 修改路由器或防火牆配置
    6. 可以由差件增加功能
