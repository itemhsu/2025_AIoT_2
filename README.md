# 2025_AIoT_2
2025 AIoT lesson 2 : Factory Demo
# ESP32-P4-EYE 迷你智慧相機應用示例說明

本示例係基於 **ESP32-P4-EYE 開發板** 所打造，完整展示一套功能齊全的 **迷你智慧相機應用系統**。該應用支援多種基礎與進階攝影功能，包括 **即時拍照、定時拍照、影片錄製、相簿瀏覽預覽**，並可透過 **USB 掛載方式將 SD 卡作為隨身碟存取**，方便影像資料的讀取與管理。

## 影像功能與參數調校

在影像調校方面，系統提供彈性化的影像參數設定介面，使用者可依需求調整：

- 影像解析度  
- 飽和度（Saturation）  
- 對比度（Contrast）  
- 亮度（Brightness）  
- 色度（Hue / Chroma）  

以因應不同場景與光線條件，確保最佳成像品質。

## 智慧視覺與 AI 辨識能力

在基礎影像功能之上，本應用進一步整合多項 **智慧視覺辨識功能**，包含：

- **人臉偵測（Face Detection）**
- **行人偵測（Person Detection）**
- **基於 YOLOv11 Nano 模型的即時物件偵測**

上述功能可在邊緣端即時執行，大幅提升裝置在 **智慧監控、人機互動、AIoT 應用** 等情境中的視覺分析與智慧判斷能力。

## 硬體資源與周邊整合

整體專案充分發揮 **ESP32-P4-EYE 開發板** 的硬體整合優勢，綜合運用多種關鍵周邊資源，包括：

- **MIPI-CSI 高速攝影機介面**  
  - 支援高品質影像擷取與即時串流  

- **SPI 介面 LCD 顯示模組**  
  - 即時顯示拍攝畫面、操作選單與系統狀態  

- **USB High-Speed（HS）介面**  
  - 實現高速資料傳輸與 USB Mass Storage（SD 卡掛載）功能  

- **實體按鍵與旋轉編碼器**  
  - 提供直覺且高可用性的人機操作體驗  

- **SD 卡儲存系統**  
  - 用於照片與影片資料的長期保存與管理  


### Source
change working directory
```
cd ~/SageMaker
cd esp
```
clone example source
```
git clone --recursive https://github.com/espressif/esp-dev-kits.git
```

### 應用補丁說明

當 **像素時鐘（Pixel Clock）設定為 80 MHz** 時，**SPI 介面的預設時鐘來源** 在部分情況下可能暫時無法滿足系統的時序需求，進而導致顯示或資料傳輸不穩定。為確保系統運作的可靠性，需手動套用修正補丁 **`0004-fix-spi-default-clock-source.patch`**。

請依照以下步驟完成補丁套用前的準備作業。

### 應用補丁說明

當 **像素時鐘（Pixel Clock）設定為 80 MHz** 時，**SPI 介面的預設時鐘來源** 在部分情況下可能無法滿足系統時序需求，進而導致顯示或資料傳輸不穩定。為提升系統整體穩定性，請依照以下步驟套用修正補丁 **`0004-fix-spi-default-clock-source.patch`**。

---

#### 1) 切換至指定的 ESP-IDF 版本

本補丁係針對 **ESP-IDF `release/v5.5` 分支**中的特定提交版本  
**`98cd765953dfe0e7bb1c5df8367e1b54bd966cce`** 所設計。  
為確保補丁可正確套用並生效，請先切換至對應版本：

```bash
cd ~/SageMaker/esp/esp-idf
git checkout release/v5.5
git checkout 98cd765953dfe0e7bb1c5df8367e1b54bd966cce
```

---

#### 2) 將補丁檔複製至 ESP-IDF 根目錄

請將補丁檔 **`0004-fix-spi-default-clock-source.patch`** 複製到 ESP-IDF 的根目錄，以便後續套用補丁作業，例如：

```bash
cp 0004-fix-spi-default-clock-source.patch ~/SageMaker/esp/esp-idf/
```

---

#### 3) 套用補丁

完成上述準備後，請在 ESP-IDF 根目錄下執行以下指令以套用補丁：

```bash
git apply 0004-fix-spi-default-clock-source.patch
```

若套用成功，系統將修正 SPI 預設時鐘來源在高像素時鐘條件下可能引發的時序問題，提升整體系統穩定性。
