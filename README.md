# Heartbeat Recognition with MFCC and LSTM

Created: November 4, 2023 1:00 AM
Last Edited Time: November 4, 2023 1:03 AM

# 第一章、緒 論

## 第一節 研究動機

> 由於組員家人患有心臟相關慢性疾病，因而對於心臟問題有一定程度的關心以及防患意識，在尋找期末專題過程中恰好發現本專題所使用之心音資料集，從而開始研究能否開發藉由心音辨識是否患有心臟疾病之人工智慧模型之可行性。
> 

# 第二章、研究內容與方法

## 第一節 資料前處理

> 本節將探討資料前處理步驟，並將本節分為兩大部分：壹 讀取資料、貳 MFCC特徵提取及三 資料類別加權。
> 

### 壹、讀取資料

> 我們使用panda函式庫讀取資料集中已預先分類之訓練(training)以及驗證(validation)資料夾，藉由讀取標記檔案(reference.csv)獲取個別資料之相關資訊，並將儲存之資料已正常(normal)與不正常(abnormal)標記於資料結構中。
> 
> 
> ### 貳、MFCC特徵提取
> 
> 我們將音訊統一率切割為時長5秒的檔案以標準化樣本，並使用梅爾頻率倒譜係數(Mel-Frequency Cepstral
> 
> Coefficient，MFCC)提取音訊特徵。
> 
> 梅爾頻率倒譜係數能夠考慮到人耳對於不同頻率會有不同敏感度的特性，因而常用於語音辨識。語音訊號經過預強調 (Pre-emphasis) 來突顯高頻的部份，之後將訊號轉變為音框(Frame)，並對每一個音框乘上一漢明窗(Hamming Window) 來增加音框的連續性，接下來經由快速傅立葉轉換(Fast Fourier Transform,FFT) 將訊號從時域轉換到頻域上，再將得到的能量頻譜乘上M個三角帶通濾波器(Triangular Bandpass Filters)，獲得每一個濾波器輸出的對數能量(Log energy)，將上述的M個對數能量透過離散餘弦轉換(Discrete Cosine Transform, DCT) 後，即可求得梅爾頻率倒譜係數。
> 
> ### 參、資料類別加權
> 
> 由於資料集中正常(normal)心音以及不正常(abnormal)心音比例相當不均(接近8:2)，我們在訓練模型前將不正常心音的權重增加{Normal: 1.8433734939759037, Abnormal: 8.052631578947368}以平衡資料量不均在訓練過程中可能造成的差異。
> 
> ![images/image2.png](Heartbeat%20Recognition%20with%20MFCC%20and%20LSTM%202c3d880546434ef7aede060973aa7a7d/image2.png)
> 

Figure 1: 正常心音以及不正常心音之比例差距

## 第二節 訓練模型

### 壹、LSTM

> 由於本專題是處理音訊資料，該資料特性為會依時間序列變化的資料類型，我們決定以LSTM訓練資料。
> 
> 
> 長短期記憶（Long Short-Term Memory，LSTM）是一種時間循環神經網路（RNN），能夠解決長序列訓練過程中的梯度消失和梯度爆炸問題，適合於處理和預測時間序列中間隔和延遲非常長的資料。
> 
> ![images/image3.png](Heartbeat%20Recognition%20with%20MFCC%20and%20LSTM%202c3d880546434ef7aede060973aa7a7d/image3.png)
> 

Figure 2: LSTM架構

# 第三章、研究結果

## 第一節 模型訓練結果

> 模型訓練結果以準確度、混淆矩陣及各種模型指標呈現。
> 

![images/image4.png](Heartbeat%20Recognition%20with%20MFCC%20and%20LSTM%202c3d880546434ef7aede060973aa7a7d/image4.png)

![images/image5.png](Heartbeat%20Recognition%20with%20MFCC%20and%20LSTM%202c3d880546434ef7aede060973aa7a7d/image5.png)

Figure 3: 模型準確度

Figure 4: 混淆矩陣

> 各種模型指標：
> 

|  | Precision | Recall | F1-score | Support |
| --- | --- | --- | --- | --- |
| Normal | 1.00 | 0.99 | 0.99 | 488 |
| Abnormal | 0.95 | 0.98 | 0.97 | 124 |
| Accuracy |  |  | 0.99 | 612 |
| Macro avg. | 0.97 | 0.99 | 0.98 | 612 |
| Weighted avg. | 0.99 | 0.99 | 0.99 | 612 |

## 第二節 模型預測應用結果

> 模型預測應用結果以準確度、混淆矩陣及各種模型指標呈現。
> 
1. Figure 5: 模型準確度
    
    ![images/image5.png](Heartbeat%20Recognition%20with%20MFCC%20and%20LSTM%202c3d880546434ef7aede060973aa7a7d/image5.png)
    

![images/image4.png](Heartbeat%20Recognition%20with%20MFCC%20and%20LSTM%202c3d880546434ef7aede060973aa7a7d/image4.png)

Figure 6: 混淆矩陣

> 各種模型指標：
> 

|  | Precision | Recall | F1-score | Support |
| --- | --- | --- | --- | --- |
| Normal | 0.71 | 0.69 | 0.70 | 150 |
| Abnormal | 0.70 | 0.72 | 0.71 | 151 |
| Accuracy |  |  | 0.70 | 301 |
| Macro avg. | 0.70 | 0.70 | 0.70 | 301 |
| Weighted avg. | 0.70 | 0.70 | 0.70 | 301 |

# 第四章、結 論

經過我們的討論及推測，雖然在使用原本資料集內的資料來測試模型時能達到99%的準確度，但在預測時模型卻只能達成70%的準確度。我們認為可能是因為訓練次數不夠或資料數量差距太大導致預測模型準確度太低的結果。雖然經過我們嘗試增加權重進行訓練以後結果依舊無法進步，但或許在未來我們能夠以目前所知以外的技巧來達成更好的結果。