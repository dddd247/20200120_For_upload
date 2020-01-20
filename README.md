# README

## hackmd 連結
https://hackmd.io/@CvRTKeXtQHWYQPEw15gcZA/S1nIVDU9r

## 環境說明
Operating System: Win10
使用到 Visual Studio 2017 建置專案
python版本 : python 3.7.4

## 檔案說明
- DOWNLOAD_FILE.py
> 由於原本檔案太大，所以將部分檔案上傳至雲端空間，需利用DOWNLOAD_FILE.py下載檔案並解壓縮到所需位置。
>> python DOWNLOAD_FILE.py : 下載專題所需檔案(.zip)
>> python DOWNLOAD_FILE.py --mode opencv : 另外提供為我們所使用的opencv版本(3.1.0)。

- main.py
> 主程式，輸入(--input-left)img_L、(--input-right)img_R與(--output)output_pfm檔案位置，透過左右圖計算出disparity map後輸出到(--output)output_pfm所指定位置。
>> 使用方法：
>> ex. 
>> python main.py --input-left ./data/Real/TL0.bmp --input-right ./data/Real/TR0.bmp --output ./Results/Real/TLD0.pfm

- demo_real.bat
> .bat檔案，方便快速執行python指令，來產生real data所對應的disparity
>> 目的：
>> python main.py --input-left ./data/Real/TL0~9.bmp --input-right ./data/Real/TR0~9.bmp --output ./Results/Real/TLD0~9.pfm

- demo_synthetic.bat
> .bat檔案，方便快速執行python指令，來產生synthetic data所對應的disparity
>> 目的:
>> python main.py --input-left ./data/Synthetic/TL0~9.png --input-right ./data/Synthetic/TR0~9.png --output ./Results/Synthetic/TLD0~9.pfm

- evaluate.py
> 用來比較Synthetic我們做出來的disparity與Ground Truth的error_rate
> 使用方式 "python evaluate.py *我們做的.pfm檔案位置*   *對應GT.pfm檔案位置* "

- visualize.py
> 用來觀察結果(.pfm檔)，使用方式 ex. "python visualize.py ./TLD0.pfm"

- others ...
> 一些function用來幫助其他程式運作。
> 

## 使用方式
1. Download專題所需檔案
先執行DOWNLOAD_FILE.py，會下載data.zip、mccnn.zip、SDR.zip至DOWNLOAD資料夾中，解壓縮這些.zip檔案(解壓縮至此)，移動(data、mccnn、SDR)資料夾到與DOWNLOAD_FILE.py相同的目錄下。
> 文件說明：
> data為Final Project所提供的Synthetic、Real data
> mccnn為mccnn的pre-trained model
> SDR為用來進行disparity refinement的程式
> 

2. Visual studio 2017 建置專案
於SDR資料夾內做：
```
mkdir build
cd build
cmake .. -G "Visual Studio 15 2017 Win64" -T host=x64
# cmake 選用Visual Studio更高版本也沒問題 (ex. Visual Studio 16 2019)，而較低版本因沒有測試過所以不能保證能否正常運作。
# 若無error發生，下一步使用Visual Studio 2017打開並編譯(Release mode)fdr.sln，建置成功則可進行步驟5。
```

3. 解決步驟2所可能遇到的問題
* 在SDR中的fdr資料夾內的CMakeLists.txt裡`set(OpenCV_DIR "C:/opencv/build")`，可能因為路徑問題導致cmake過程有error，更改opencv所存放路徑即可
* 在opencv路徑正確下也可能發生下圖問題：
  ![](https://i.imgur.com/D4jowPf.png)
  這裡提供一個解決方案，首先將fdr資料夾內的CMakeLists.txt中有關opencv的部分刪除
  (如下圖所示，刪除藍色部分後存檔)
  ![](https://i.imgur.com/3nZVgxO.png)
  完成後重作步驟2，此時opencv會是未找到的狀況(如下圖所示)：
  ![](https://i.imgur.com/Jz3WnBY.png)
  依照此網址步驟有很大機率解決問題[http://bc-yang.blogspot.com/2017/08/opencv-c.html](http://bc-yang.blogspot.com/2017/08/opencv-c.html)


4. 步驟三網址所描述的詳細過程
切換Debug mode到Release mode：
![](https://i.imgur.com/jCsGihr.png)
(1) 專案 -> 屬性
![](https://i.imgur.com/JhWIna5.png)
(2) 點開VC++目錄，
![](https://i.imgur.com/5lwso5c.png)
include目錄新增：
C:\opencv\build\include
C:\opencv\build\include\opencv
C:\opencv\build\include\opencv2
![](https://i.imgur.com/ZOjAq2X.png)
程式庫目錄新增 :
C:\opencv\build\x64\vc14\lib
![](https://i.imgur.com/Tov2mty.png)
點開連結器，
點開輸入：
![](https://i.imgur.com/50nU8ls.png)
其他相依性新增：
C:\opencv\build\x64\vc14\lib\opencv_world310.lib
C:\opencv\build\x64\vc14\lib\opencv_world310d.lib
(依照所下載的opencv版本更改檔名)
![](https://i.imgur.com/RnGosn1.png)
build專案，建置成功後即可關閉visual studio：
![](https://i.imgur.com/Qx7LkF1.png)

5. 使用main.py計算disparity
```
python main.py --input-left ... --input-right ... --output ...
```
如圖所示：
![](https://i.imgur.com/xx249Z0.png)


