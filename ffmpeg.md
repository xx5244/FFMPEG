# ffmpeg
---
+ ## 介紹
    全名是Fast Forward MPEG(Moving Picture Experts Group)，為開源的影音多媒體處理框架，可以進行影音的解碼、編碼、編碼轉換、混合、抽取、串流和濾鏡，無論影音格式是從哪個地方出來的，從過去到現在的影音格式它幾乎都能夠支援，也可以拿來下載m3u8的影片

+ ## 參數
  + ### 主要參數
    | 參數指令   | 參數功能                           |
    | :--------- | :--------------------------------- |
    | -h         | 參數明細                           |
    | -i         | 設定輸入檔案                       |
    | -f         | 設定輸出格式                       |
    | -y         | 若輸出檔案存在則覆蓋               |
    | -fs        | 超過指定檔案大小則結束轉換         |
    | -t         | 指定檔案輸出的時間長度，秒為單位   |
    | -ss        | 指定檔案從幾秒開始做轉換，秒為單位 |
    | -title     | 設定標題                           |
    | -timestamp | 設定時間戳                         |
    | -vsync     | 增減Frame使影音同步                |
    | -c         | 指定輸出檔案的編碼                 |
    | -metadata  | 更改輸出檔案的元資料               |
  + ### 影像參數
    | 參數指令        | 參數功能                                                     |
    | :-------------- | :----------------------------------------------------------- |
    | -b:v            | 設定影像流量，預設為200Kbit/秒                               |
    | -r              | 設定影格率值，預設為25                                       |
    | -s              | 設定畫面的寬與高                                             |
    | -aspect         | 設定畫面的比例                                               |
    | -vn             | 不處理影像，於僅針對聲音做處理時使用                         |
    | -vcodec( -c:v ) | 設定影像影像編解碼器，未設定時則使用與輸入檔案相同之編解碼器 |
  + ### 聲音參數
    | 參數指令         | 參數功能                                                           |
    | :--------------- | :----------------------------------------------------------------- |
    | -b:a             | 設定每Channel（最近的SVN版為所有Channel的總合）的流量              |
    | -ar              | 設定採樣率                                                         |
    | -ac              | 設定聲音的Channel數                                                |
    | -acodec ( -c:a ) | 設定聲音編解碼器，未設定時與影像相同，使用與輸入檔案相同之編解碼器 |
    | -an              | 不處理聲音，於僅針對影像做處理時使用                               |
    | -vol             | 設定音量大小，256為標準音量                                        |

+ ## 常用功能
  + ### 影片轉檔
    用`-i`放入欲轉檔的檔案，後面接著放欲輸出的檔案
    輸入與輸出檔案最好都用`"`給包住
    ```
    ffmpeg -i "input_video" "output_video"
    ```
    PS:
    輸入與輸出文件最好都已`"`包著

  + ### 影片快速合併
    用`-f concat -safe 0 -i`放入統整欲合併檔名的`txt`檔`-c copy`再放入欲輸出的檔案
    ```
    ffmpeg -f concat -safe 0 -i "file_list.txt" -c copy "output_video"
    ```
    PS:
    `-f concat`設定輸出格式為合併
    `－safe 0`若輸入路徑是相對路徑則不需要
    `-c copy`為不重新編碼
    `txt裡面格式為file 'file_name'`

  + ### 影片剪輯
    ```
    ffmpeg -i "file_name" -ss start_time -t time_length -c copy "output_video"
    ```
    PS:
    `-ss` 設定剪輯影片的起始時間，若為一開始則不用設定
    `-t`  設定剪輯影片的時間長度
    `-c copy` 免重新編碼
  
  + ### 擷取視訊
    ```
    ffmpeg -i "file_name" -an -c copy "output_video"
    ```
    PS:
    `-an` 取消音訊輸出

  + ### 擷取音訊
    ```
    ffmpeg -i "file_name" -vn -c copy "output_audio"
    ```
    PS:
    `-vn` 取消視訊輸出

  + ### 上字幕
    ```
    ffmpeg -i "file_name" -vf "subtitiles=file_subtitle" "output_video"
    ```
    PS:
    `-vf` 設定視訊過濾器
    `subtitles` 屬於固定字樣的參數

  + ### 字幕檔轉換
    ```
    ffmpeg -i "file_subtitle" "output_subtitle"
    ```
  + ### 畫面旋轉
  
  

+ ## 進階功能
  + ### 字幕設定
    + #### mkv檔加上字幕可以不用過濾器也不用重新編碼
      ```
      ffmpeg -i "file_name" -i "file_subtitle" -c copy "output_video"
      ```
      PS:
      輸入與輸出的檔案一定要是MKV才可以不用視訊過濾器及重新編碼
      
  + ### 播放速度調整 
  + ### 指定位置放圖
+ ## 參考資料
  ```
  [ffmpeg](https://ffmpeg.org/)

  [wiki](https://zh.wikipedia.org/wiki/FFmpeg)
  ```
+ ## 補充資料
  ```
  [FFmpeg操作參數](https://zhuanlan.zhihu.com/p/145312133)
  ```