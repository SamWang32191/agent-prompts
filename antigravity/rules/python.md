---
trigger: always_on
---

# 當你在本機環境開發 Python 時，**原則上**不要直接安裝全域依賴（Docker 等隔離環境除外）。請參考以下標準流程：

## 1. 建立虛擬環境
```
# (Windows 若無 python3 指令，請嘗試使用 python 或 py)
python3 -m venv venv
```

## 2. 啟用虛擬環境

```
# Mac/Linux:
source venv/bin/activate
# Windows:
# .\venv\Scripts\activate
```

## 3. 安裝依賴套件 (確保 requirements.txt 存在)
```
if [ -f requirements.txt ]; 
    then pip3 install -r requirements.txt;
fi
```