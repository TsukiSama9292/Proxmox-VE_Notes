# 擴展空間

### 檢查
確認了 `nvme0n1` 存在，但裡面有 `nvme0n1p1` 沒有去分配
```bash
root@pve:~# lsblk -f
NAME               FSTYPE      FSVER    LABEL UUID                                   FSAVAIL FSUSE% MOUNTPOINTS
nvme0n1                                                                                             
└─nvme0n1p1  
```

### 進入 gdisk 編輯
```bash
gdisk /dev/nvme0n1
```

### 移除分割區
輸入`d`
理論上只有一個分割區會自動移除
不用輸入編號

### 建立分割區
輸入`n`
直接 Enter 或手動輸入`1`，建立1號分割區

### 配置空間
First sector (磁區開頭)，直接 Enter
Last sector (磁區結尾)，直接 Enter 就是全滿

### 選擇 GUID
輸入`8e00`

### 跳出確認操作與否
輸入 `Y`

### 重啟電腦
```bash
reboot
```

### 初始化並加入 LVM 群組
```bash
pvcreate /dev/nvme1n1p4
vgextend pve /dev/nvme1n1p4
```

### **注意**，留點空間給 `metadata`
它是用於紀錄寫入那些操作的重要磁區
只要東西夠多，這個總有爆滿的時候
所以說多配置給它一點
```bash
lvextend --poolmetadatasize +4G /dev/pve/data
```
### 配置空間， `root` 與 `data(thin-pool)`
```bash
# lvextend -l +20%FREE /dev/pve/root # (可選)，這個是用來備份、保存ISO、保存模板的空間，這裡可以用 +100G 之類的形式
lvextend -l +100%FREE /dev/pve/data
```

### **操作失誤**，全部給了 `thin-pool`
在沒有任何 VMs 的時候可以移除掉 `thin-pool`
```bash
lvremove pve/data
```
然後配置比預設(1G)更大的`poolmetadatasize`，剩餘才給 `thin-pool` 和 `root`
```bash
# 可以不是 90 %，而是 100 % 給 thin-pool
lvcreate --type thin-pool --name data \ 
  --poolmetadatasize 4G -l +90%FREE pve
# lvextend -l +100%FREE /dev/pve/root # (可選)
```