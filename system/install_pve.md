# 安裝 PVE 系統

## 前置任務

- 一個空的 **隨身碟** 或是 **外接SSD**
- 快速格式化需要灌系統的電腦硬碟(可用 Live Ubuntu)
- 下載 **Proxmox VE 鏡像** 和 **燒錄程式**
- 將 Proxmox VE 鏡像燒入至 **隨身碟** 或是 **外接SSD**
- 鍵盤x1  + 滑鼠x1 + 螢幕x1

## 執行步驟

1. 將燒錄好的 Proxmox VE 隨身碟插入未開機的電腦後板
2. 按下開機，點鍵盤進入 BIOS 的鍵(可能是: Del/F12/F10/F2...)
3. 進入 BIOS
    - 關閉 `快速啟用`
    - 調整開機磁碟順序，Proxmox VE 隨身碟調成第一順序
    - 保存並且重啟
4. 開機後等待數秒進入 Proxmox VE 安裝介面
    - 選擇第一個，有畫面，就像是在 Windows 安裝軟體一樣簡單
    - 使用條款，直接同意
    - 地區選 Asia/Taipei，城市選 Taiwan(都行)
    - 域名隨便打，除非要做集群
    - 信箱隨便打，無所謂
    - 注意 **磁碟** 分配，建議在此處先處理好。
        - 磁碟直接佔全滿
        - root 保存 `Backup`, `ISO image`, `Container template`
        - swap 寫原始記憶體 1/4 差不多就行
    - 網路配置，單節點的 PVE 推薦不要直接拿公網 IP，用路由下的 NAT 網路
    - 瑣碎雜事...最後確認配置
    - 安裝安裝...等到最後要進行重新啟動
5. 系統會進入藍色底的 grub，等待十秒或直接 Enter 進入系統
6. 在可拜訪主機的網路下，連線 `https://主機IP:8006` 進入 Proxmox VE WebUI
7. 若是在實驗室，我推薦先安裝 [Tailscale](https://tailscale.com/) 作為自己電腦和 PVE 的 VPN
    - 打開官網免費註冊
    - [不同系統下載方式不同](https://tailscale.com/download/linux)
    - Proxmox VE 和自己電腦都安裝好，都執行`tailscale up`並且登入
    - 可以到 [管理介面](https://login.tailscale.com/admin/machines) 點擊不同主機的 `...` 點擊 `Disable key expiry`
    - 然後就可以嘗試看看連線是否正常，因為已經關閉 key 過期，所以之後都能連線