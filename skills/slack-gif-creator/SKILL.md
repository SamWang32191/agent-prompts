---
name: slack-gif-creator
description: 用於建立針對 Slack 最佳化的動態 GIF 的知識與工具程式。提供限制規範、驗證工具與動畫概念。適用於當使用者要求製作 Slack 用動態 GIF 時，例如「幫我做一個 X 正在做 Y 的 GIF 給 Slack 用」。
license: 完整條款請見 LICENSE.txt
---

# Slack GIF 製作工具 (Slack GIF Creator)

這是一套提供用於建立針對 Slack 最佳化動態 GIF 的工具與知識庫。

## Slack 規格要求

**尺寸 (Dimensions)：**
- 表情符號 (Emoji) GIF：128x128（建議）
- 訊息 (Message) GIF：480x480

**參數 (Parameters)：**
- FPS (影格率)：10-30（數值越低檔案越小）
- 顏色數 (Colors)：48-128（顏色越少檔案越小）
- 持續時間 (Duration)：表情符號 GIF 請保持在 3 秒以內

## 核心工作流程

```python
from core.gif_builder import GIFBuilder
from PIL import Image, ImageDraw

# 1. 建立建構器 (Builder)
builder = GIFBuilder(width=128, height=128, fps=10)

# 2. 產生影格 (Frames)
for i in range(12):
    frame = Image.new('RGB', (128, 128), (240, 248, 255))
    draw = ImageDraw.Draw(frame)

    # 使用 PIL 基本圖形繪製你的動畫
    # (圓形、多邊形、線條等)

    builder.add_frame(frame)

# 3. 儲存並進行最佳化
builder.save('output.gif', num_colors=48, optimize_for_emoji=True)

```

## 繪製圖形

### 處理使用者上傳的圖片

如果使用者上傳了一張圖片，請考慮他們是想要：

* **直接使用它**（例如：「讓這張圖動起來」、「把這張圖拆解成影格」）
* **將其作為靈感參考**（例如：「做一個像這樣的東西」）

使用 PIL 載入並處理圖片：

```python
from PIL import Image

uploaded = Image.open('file.png')
# 直接使用，或僅作為顏色/風格的參考

```

### 從頭開始繪製

當從零開始繪製圖形時，請使用 PIL ImageDraw 的基本圖形功能：

```python
from PIL import ImageDraw

draw = ImageDraw.Draw(frame)

# 圓形/橢圓 (Circles/ovals)
draw.ellipse([x1, y1, x2, y2], fill=(r, g, b), outline=(r, g, b), width=3)

# 星形、三角形、任何多邊形 (Stars, triangles, any polygon)
points = [(x1, y1), (x2, y2), (x3, y3), ...]
draw.polygon(points, fill=(r, g, b), outline=(r, g, b), width=3)

# 線條 (Lines)
draw.line([(x1, y1), (x2, y2)], fill=(r, g, b), width=5)

# 矩形 (Rectangles)
draw.rectangle([x1, y1, x2, y2], fill=(r, g, b), outline=(r, g, b), width=3)

```

**請勿使用：** Emoji 字型（跨平台顯示不可靠）或假設此技能中存在預先打包好的圖形庫。

### 讓圖形更好看

圖形看起來應該要精緻且具創意，不要太過陽春。以下是一些做法：

**使用較粗的線條** - 輪廓線和線條的 `width` 始終設為 2 或更高。細線（width=1）看起來會有鋸齒且不專業。

**增加視覺深度**：

* 使用漸層作為背景 (`create_gradient_background`)
* 透過多層形狀增加複雜度（例如：星星裡面還有一個較小的星星）

**讓形狀更有趣**：

* 不要只畫一個普通的圓——加上高光、圓環或圖案
* 星星可以有光暈（在後方畫一個較大、半透明的版本）
* 結合多種形狀（星星 + 閃光，圓形 + 圓環）

**注意配色**：

* 使用鮮豔、互補的顏色
* 增加對比（淺色形狀配深色輪廓，深色形狀配淺色輪廓）
* 考慮整體的構圖

**針對複雜形狀**（愛心、雪花等）：

* 結合多邊形和橢圓來繪製
* 仔細計算對稱點
* 增加細節（愛心可以有高光曲線，雪花有複雜的分支）

發揮創意並注重細節！一個好的 Slack GIF 看起來應該是經過打磨的，而不是像佔位用的草圖。

## 可用的工具程式 (Utilities)

### GIF 建構器 (`core.gif_builder`)

組裝影格並針對 Slack 進行最佳化：

```python
builder = GIFBuilder(width=128, height=128, fps=10)
builder.add_frame(frame)  # 加入 PIL Image 物件
builder.add_frames(frames)  # 加入影格列表
builder.save('out.gif', num_colors=48, optimize_for_emoji=True, remove_duplicates=True)

```

### 驗證器 (`core.validators`)

檢查 GIF 是否符合 Slack 的要求：

```python
from core.validators import validate_gif, is_slack_ready

# 詳細驗證
passes, info = validate_gif('my.gif', is_emoji=True, verbose=True)

# 快速檢查
if is_slack_ready('my.gif'):
    print("Ready!")

```

### 緩動函數 (`core.easing`)

讓動作更平滑，而非只有線性運動：

```python
from core.easing import interpolate

# 進度從 0.0 到 1.0
t = i / (num_frames - 1)

# 套用緩動 (Easing)
y = interpolate(start=0, end=400, t=t, easing='ease_out')

# 可用選項: linear, ease_in, ease_out, ease_in_out,
#           bounce_out, elastic_out, back_out

```

### 影格輔助工具 (`core.frame_composer`)

針對常見需求的便利功能：

```python
from core.frame_composer import (
    create_blank_frame,         # 純色背景
    create_gradient_background,  # 垂直漸層
    draw_circle,                # 畫圓輔助
    draw_text,                  # 簡易文字渲染
    draw_star                   # 五角星
)

```

## 動畫概念

### 搖晃/震動 (Shake/Vibrate)

透過振盪偏移物件的位置：

* 使用 `math.sin()` 或 `math.cos()` 搭配影格索引
* 加入微小的隨機變化以增加自然感
* 應用於 x 和/或 y 座標

### 脈衝/心跳 (Pulse/Heartbeat)

有節奏地縮放物件大小：

* 使用 `math.sin(t * frequency * 2 * math.pi)` 實現平滑脈衝
* 心跳效果：兩次快速脈衝後暫停（調整正弦波）
* 在基礎大小的 0.8 到 1.2 倍之間縮放

### 彈跳 (Bounce)

物件落下並彈起：

* 著地時使用 `interpolate()` 搭配 `easing='bounce_out'`
* 落下時使用 `easing='ease_in'`（加速）
* 透過每影格增加 y 軸速度來模擬重力

### 自轉/旋轉 (Spin/Rotate)

物件繞中心旋轉：

* PIL: `image.rotate(angle, resample=Image.BICUBIC)`
* 搖擺效果：角度使用正弦波而非線性增加

### 淡入/淡出 (Fade In/Out)

逐漸出現或消失：

* 建立 RGBA 圖片，調整 Alpha 通道（透明度）
* 或使用 `Image.blend(image1, image2, alpha)`
* 淡入：alpha 從 0 到 1
* 淡出：alpha 從 1 到 0

### 滑動 (Slide)

物件從螢幕外移動到指定位置：

* 起始位置：影格邊界外
* 結束位置：目標位置
* 使用 `interpolate()` 搭配 `easing='ease_out'` 實現平滑停止
* 過衝效果 (Overshoot)：使用 `easing='back_out'`

### 縮放 (Zoom)

縮放與定位以達到變焦效果：

* 放大 (Zoom in)：比例從 0.1 到 2.0，裁切中心
* 縮小 (Zoom out)：比例從 2.0 到 1.0
* 可以加入動態模糊 (Motion blur) 增加戲劇張力 (使用 PIL filter)

### 爆炸/粒子爆發 (Explode/Particle Burst)

產生向外輻射的粒子：

* 產生具有隨機角度和速度的粒子
* 更新每個粒子：`x += vx`, `y += vy`
* 加入重力：`vy += gravity_constant`
* 隨著時間淡出粒子（降低 alpha 值）

## 最佳化策略

僅在被要求縮小檔案大小時，才實作以下部分方法：

1. **較少的影格** - 降低 FPS（例如用 10 取代 20）或縮短持續時間
2. **較少的顏色** - 設定 `num_colors=48` 而非 128
3. **較小的尺寸** - 設定 128x128 而非 480x480
4. **移除重複內容** - 在 save() 中設定 `remove_duplicates=True`
5. **Emoji 模式** - `optimize_for_emoji=True` 會自動進行最佳化

```python
# 針對 emoji 的最大程度最佳化
builder.save(
    'emoji.gif',
    num_colors=48,
    optimize_for_emoji=True,
    remove_duplicates=True
)

```

## 設計哲學

此技能提供：

* **知識**：Slack 的規格要求與動畫概念
* **工具程式**：GIF 建構器、驗證器、緩動函數
* **靈活性**：使用 PIL 基本功能來建立動畫邏輯

它**不**提供：

* 僵化的動畫模板或預製函式
* Emoji 字型渲染（跨平台顯示不可靠）
* 內建於此技能中的預先打包圖形庫

**關於使用者上傳的說明**：此技能不包含預建圖形，但若使用者上傳圖片，請使用 PIL 載入並處理——根據他們的要求判斷是要直接使用，還是僅作為靈感參考。

發揮創意！結合多種概念（彈跳 + 旋轉、脈衝 + 滑動等）並善用 PIL 的完整功能。

## 依賴套件

為避免污染系統環境，建議使用虛擬環境進行安裝：

```bash
# 1. 建立虛擬環境
python3 -m venv venv

# 2. 啟用虛擬環境 (Mac/Linux)
source venv/bin/activate
# Windows: .\venv\Scripts\activate

# 3. 安裝依賴套件
pip install -r requirements.txt
```

若無 `requirements.txt`，可手動安裝核心套件：

```bash
pip install pillow imageio imageio-ffmpeg numpy
```