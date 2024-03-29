# ffmpeg
---
+ ## 介紹
    全名是Fast Forward MPEG(Moving Picture Experts Group)，為開源的影音多媒體處理框架，可以進行影音的解碼、編碼、編碼轉換、混合、抽取、串流和濾鏡，無論影音格式是從哪個地方出來的，從過去到現在的影音格式它幾乎都能夠支援，也可以拿來下載m3u8的影片

+ ## 語法
  ```
  ffmpeg [global_options] {[input_file_options] -i input_file} ... {[output_file_options] output_file} ...
  ffmepg [全域參數的設定] {[輸入文件參數設定] -i 輸入的文件/輸入的網址} {[輸出文件參數設定] 輸出的文件/輸出的網址}
  ```

+ ## 參數
  + ### 全域參數
    | 參數指令                    | 參數功能                   |
    | :-------------------------- | :------------------------- |
    | -loglevel                   | 設定日誌紀錄級別           |
    | -v                          | 設定日誌紀錄級別           |
    | -report                     | 生成報告                   |
    | -max_alloc                  | 設定一個已分配區塊的最大值 |
    | -y                          | 覆蓋輸出文件               |
    | -n                          | 不覆蓋輸出文件             |
    | -ignore_unknown             | 忽略未知的stream types     |
    | -filter_threads             | 非複雜濾波器線程的數量     |
    | -filter_complex_threads     | -filter_complex的線程數量  |
    | -stats                      | 在編碼期間內print進度報告  |
    | -max_error_rate             | 設定錯誤率多少以上報錯     |
    | -bits_per_raw_sample number | 設定每個原始樣本的位數     |
    | -vol volume                 | 改變音量                   |

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

  + ### 字幕參數
    | 參數指令          | 參數功能                         |
    | :---------------- | :------------------------------- |
    | -s size           | 設定Frame的大小(WxH)             |
    | -sn               | 禁止字幕                         |
    | -scodec codec     | 設定字幕的編解碼器               |
    | -stag fourcc/tag  | 設定字幕tag                      |
    | -fix_sub_duration | fix subtitles duration           |
    | -canvas_size size | 設定Canvas的大小(WxH)            |
    | -spre preset      | 設定字幕選項的預設值(預設處理集) |

+ ## 常用功能
  + ### 影片轉檔
    用`-i`放入欲轉檔的檔案，後面接著放欲輸出的檔案
    輸入與輸出檔案最好都用`"`給包住
    ```
    ffmpeg -i "input_video" "output_video"
    ```
    PS:
    輸入與輸出文件最好都已`"`包著
  
  + ### 查看影片資訊
    ```
    ffmpeg -i "input_video"
    ```
    輸出資訊結果如下圖
    ![](Image/影片資訊查看.png)

  + ### 處理過程隱藏
    由於下載m3u8的時候過程內容太過繁雜，因此，想將過程隱藏起來，就要設定日誌級別，可以設定error才顯示或者warning就顯示之類的
    ```
    ffmpeg -loglevel loglevel -i "input_video" "output_video"
    ```
    PS:
    `-loglevel` 設定日誌紀錄級別
    **loglevel**
    `error` 過程有error才顯示
    `quiet` 過程都不顯示

  + ### 改變影片大小
    ```
    ffmpeg -i "input_video" -s "WxH" "output_video"
    ```
    PS:
    `-s` 調整影片的寬高參數
    `W` 影片的寬
    `H` 影片的高

  + ### 影片快速合併
    方法1.
    ```
    ffmpeg -i "input_video1" -i "input_video2" -i "input_video3" -filter_complex "[0:0][0:1][1:0][1:1][2:0][2:1]concat=n=video_num:v=1:a=1[out]" -map "[out]" "output_video"    
    ```
    PS:
    `-filter_complex`用來連接影片的，後面[video_num:0][video_num:1]，看有幾個影片串幾個[video_num:0]跟[video_num:1]，前面的數字由0開始串到video_num-1的數量，至於：號面的0跟1是指video跟audio的
    `concat`的參數是指定幾部影片合併，3部就填3，v=1:a=1是固定的，因為最終是合成為1部
    `-map` 參數指定輸出格式為合併後的影片
    所以，就是先把要輸入的影片一次輸入近來，再來就是設定幾個[:0][:1]，然後檔案印射出一個

    方法2.
    用`-f concat -safe 0 -i`放入統整欲合併檔名的`txt`檔`-c copy`再放入欲輸出的檔案
    ```
    ffmpeg -f concat -safe 0 -i "file_list.txt" -c copy "output_video"
    ```
    PS:
    `-f concat`設定輸出格式為合併
    `－safe 0`若輸入路徑是相對路徑則不需要
    `-c copy`為不重新編碼
    `txt裡面格式為file 'file_name'`
    注意:若txt所在資料夾與cmd執行的當前資料夾並非一致的時候，那txt裡面的file_name就要包含完整的路徑

  + ### 影片剪輯
    ```
    ffmpeg -i "input_video" -ss "start_time" -t "time_length" -c copy "output_video"
    ```
    PS:
    `-ss` 設定剪輯影片的起始時間，若為一開始則不用設定
    `start_time` 輸入影片幾秒開始剪
    `-t`  設定剪輯影片的時間長度
    `time_length` 影片的時間長度
    `-c copy` 免重新編碼
  
  + ### 擷取影片
    ```
    ffmpeg -i "input_video" -an -c copy "output_video"
    ```
    PS:
    `-an` 取消音訊輸出

  + ### 擷取音訊
    ```
    ffmpeg -i "input_video" -vn -c copy "output_audio"
    ```
    PS:
    `-vn` 取消影片輸出

  + ### 上字幕
    ```
    ffmpeg -i "input_video" -vf "subtitiles=file_subtitle" "output_video"
    ```
    PS:
    `-vf` 設定影片過濾器
    `subtitles` 屬於固定字樣的參數

  + ### 字幕檔轉換
    ```
    ffmpeg -i "file_subtitle" "output_subtitle"
    ```

  + ### 設定影片編碼
    ```
    ffmpeg -i "input_video" -c:v h264 -c:a aac -strict -2 "output_video"
    ```
    PS:
    `-c:v h264`：指定影片編碼格式為h264。
    `-c:a aac`：指定音訊編碼格式為aac。
    `-strict -2`：使用嚴格的編碼選項，以避免轉碼過程中的錯誤。
    強烈建議改編碼要設定bitrate，不然會不一樣喔
  
  + ### 設定影片編碼指定影片編碼規格
    ```
    ffmpeg -i "input_video" -c:v h264 -profile:v high "output_video"
    ```
    PS:
    `-c:v h264`:指定影片編碼格式為h264。
    `-profile:v high`:參數指定使用 H.264 High profile

  + ### 設定bitrate
    ```
    ffmpeg -i "input_video" -b:v bitrate_num "output_video"
    ```
    PS:
    `-b:v` 設定影片bitrate
    `bitrate_num` 設定bitrate的值，若要5000K就打5000K

  + ### 查詢bitrate
    ```
    ffprobe -loglevel quiet -i "input_video" -show_entries stream=bit_rate
    ```
    PS:
    `-show_entries stream=bit_rate` 顯示bitrate
    `-loglevel quiet` 不顯示過程
    最後會出現兩個bit_rate，上面的為video的，下面的為audio的，單位皆為bps

  + ### 設定fps
    ```
    ffmpeg -i "input_video" -r fps_num "output_video"
    ```
    PS:
    `-r` 設定影片的fps
    `fps_num` 設定fps的值，若要30就打30

  + ### 查詢fps
    ```
    ffprobe -loglevel quiet -i "input_video" -show_entries stream=r_frame_rate -select_streams v -of default=noprint_wrappers=1:nokey=1
    ```
    PS:
    `-show_entries stream=r_frame_rate` 顯示fps
    最後會出現兩個r_frame_rate，上面的為video的，下面的為audio的
    `select_streams v` 選擇顯示video的資料，若v改成a就是顯示audio的資料
    `-of default=noprint_wrappers=1:nokey=1` 只顯示數值

  + ### 畫面翻轉/旋轉
    + #### 水平翻轉
      ```
      ffmpeg -i "input_video" -vf "hflip" "output_video"
      ```
      PS:
      `hflip` 為video filter的一個參數

    + #### 垂直翻轉
      ```
      ffmpeg -i "input_video" -vf "vflip" "output_video"
      ```
      PS:
      `vflip` 為video filter的一個參數
    
    + #### 旋轉(90度的倍數)
      ```
      ffmpeg -i "input_video" -vf "transpose=number, transpose=number, ..." "output_video"
      ```
      PS:
      `transpose` 為video filter的一個參數
      `number`的對照如下
      `0` = 逆時針旋轉90度並垂直翻轉
      `1` = 順時針旋轉90度
      `2` = 逆時針旋轉90度
      `3` = 順時針旋轉90度並垂直翻轉
      如要旋轉90的倍數即如上面範例下達多次參數指令並用`逗號`隔開

+ ## 進階知識
  + ### 下載m3u8網址
    + #### 一般使用
      ```
      ffmpeg -i "m3u8_url" -c copy "output_video"
      ```

    + #### 增加headers
      ```
      ffmpeg heades -i "m3u8_url" -c copy "output_video"
      ```
      PS:
      headers是指欲加入的標頭檔，常見的加入如下
      1. -referer "referer_url"
      2. -user_agent "user_agent_url"
      注意:若是其餘的headers，請用-headers來添加，若是多項的話，用$'\r\n'來串接
      """
      ffmpeg -user_agent "value" -referer "value" -headers "origin: value" -i "m3u8檔" -c copy output.mp4
      """


  + ### 字幕設定
    + #### mkv檔加上字幕可以不用過濾器也不用重新編碼
      ```
      ffmpeg -i "input_video" -i "file_subtitle" -c copy "output_video"
      ```
      PS:
      輸入與輸出的檔案一定要是MKV才可以不用影片過濾器及重新編碼
  
  + ### 旋轉(指定角度)
    + #### 一般使用
      ```
      ffmpeg -i "input_video" -vf "rotate=PI/number" "output_video"
      ```
      PS:
      `rotate` 為video filter的一個參數，且為順時針旋轉
      `PI` 是固定的，任意的角度都是用PI的比例來表示
      `number`自己輸入的數字，即為與PI的比例
      
      **注意:直接用此方法旋轉，長寬是會跟原來的影片一樣的，同樣是旋轉90度，如原影片不是正方形就會有部分影像被裁切掉，不夠的部分也會有黑邊，用transpose旋轉就不會有**

    + #### 設定寬高
      ```
      ffmpeg -i "input_video" -vf "rotate=PI/number:ow=number1:oh=number2" "output_video"
      ```
      PS:
      `ow` 為輸出影像的寬，可輸入數字來設定，導入要用`:`
      `oh` 為輸出影像的高，可輸入數字來設定，導入要用`:`
      `number1、number2` 為自行設定的數字
      `iw` 為輸入影像的寬，一般是拿來做為輸出影像比例之用
      `ih` 為輸入影像的高，一般是拿來做為輸出影像比例之用


  + ### 播放速度調整
    + #### 音訊速度調整
      ```
      ffmpeg -i "input_video" -af "atempo=number" "output_video"
      ```
      PS:
      `af` 為audio filter，音訊過濾器
      `atempo` 為audio filter的一個參數，主要影響音訊的播放速度的
      `number` 為原播放速度的幾倍速播放

      **注意:**
      **1. number必須在[0.5, 100.0]範圍內**
      **2. 若number大於2，取樣的時候會跳過一些samples，因此，number大於2又想穩定品質就只好利用"daisy-chain"**
      **3. daisy-chain範例**
        ```
        atempo=sqrt(3),atempo=sqrt(3)
        ```
    + #### 影片速度調整
      ```
      ffmpeg -i "input_video" -vf "setpts=number*PTS" "output_video"
      ``` 
      PS:
      `setpts` 為video filter的一個參數，主要是設定pts的
      `number` 為調整pts的數字
      `PTS` 為時間戳(presentation timestamp)
      例如:原本於時間第1秒的影像，想要它出現在時間第0.5秒的話，那就等同時間戳*0.5，也就是說整體會變成影片2倍速播放的效果
      
      **注意:**
      **影片播放太快的話一樣會有掉影格(frame)的問題，因此，可利用-r參數來設定輸出的fps**

  + ### 指定位置放圖(浮水印)
    + #### 圖放左上角
    ```
    ffmpeg -i "input_video" -i "input_image" -filter_complex "overlay=X:Y" "output_video"
    ```
    PS:
    `-filter_complex` 為混合用的過濾器，通常是用來混合兩種不同的影片/音訊
    `overlay` 為filter的一個參數，主要是設定混合的位置
    `X` 為混合的位置之X座標值
    `Y` 為混合的位置之Y座標值
    **概念上是把輸入的影片跟輸入的圖片混合在一起**
    
    + #### 圖放正中央
      ```
      ffmpeg -i "input_video" -i "input_image" -filter_complex "overlay=(W-w)/2:(H-w)/2" "output_video"
      ```
      PS:
      `W` 為第一個輸入源的寬，亦即影片的寬
      `H` 為第一個輸入源的高，亦即影片的高
      `w` 為第二個輸入源的寬，亦即圖片的寬
      `h` 為第二個輸入源的高，亦即圖片的高
      **概念上是利用算是算出影片的中心點減去圖片的中心點，然後，把位置移到該點去**
      **注意:寬、高的大小寫有差異，不可任意變換**

    + #### 圖放右下角
      ```
      ffmpeg -i "input_video" -i "input_image" -filter_complex "overlay=W-w:H-h" "output.video"
      ```
  
  + ### 去除浮水印
    ```
    ffmpeg -i "input_video" -vf "delogo=x=number_x:y=number_y:w=number_w:h=number_h:show=1" "output_video"
    ```
    PS:
    `delogo` 為video filter中的一個參數，主要用來去除浮水印的
    `number_x` 為設定要去除的x座標
    `number_y` 為設定要去除的y座標
    `number_w` 為設定要去除的範圍寬度
    `number_h` 為設定要去除的範圍高度
    `show` 為設定是否顯示去除範圍的邊界，若為1則會顯示綠框邊界，可視化去除的範圍，預設為0

    **注意:number_x與number_y不要為0，不然會很容易發生錯誤=>Logo area is outside of the frame的情況**
    **若要去掉多個浮水印，要用逗號來分隔，如同多重旋轉一樣**
    **因為通常這只是處理影像不對聲音做處理，所以中間可以插個不對音訊做處理的指令，`-c:a copy`**

  + ### 用python使用ffmpeg
    ```python
    import subprocess

    cmd = f'ffmpeg -i input_video'
    result = subprocess.run(cmd, stdout=subprocess.PIPE, stderr=subprocess.PIPE)
    output = result.stderr.decode('utf-8')
    print(output)

    """
    利用subprocess來執行
    cmd: 要輸入的命令
    stdout: 標準輸出
    stderr: 標準錯誤輸出
    當兩個通信管道都設定為subprocess.PIPE時，則意味著 Python 程式碼可以從子處理程序的標準輸出和標準錯誤輸出中讀取資料
    這樣，在執行 subprocess.run 時，輸出資訊就可以通過 stdout 和 stderr 屬性讀取
    在使用ffmpeg例子中，結果一定要使用stderr，才會看到跟cmd輸出結果一樣的，用stdout，正常情況都是不會看到訊息的
    注意: python中路徑最好用/來弄
    """    
    ```
  
  + ### 使用gpu加速
    + #### 目前僅介紹Nvidia顯卡的部份，需要下載３個東西，顯卡驅動、CUDA工具包、支援CUDA的ffmpeg
    + #### Step1. 下載顯卡驅動並安裝
      [這裡下載](https://www.nvidia.com/zh-tw/geforce/drivers/)
      ![](Image/使用gpu加速_1.png)
    + #### Step2. 下載CUDA工具包並安裝
      [這裡下載](https://developer.nvidia.com/cuda-downloads)
      ![](Image/使用gpu加速_2.png)
    + #### Step3. 下載支援CUDA的ffmpeg並安裝，且環境路徑要指定
      [這裡下載](https://www.gyan.dev/ffmpeg/builds/)
      **下載Full的版本**
      ![](Image/使用gpu加速_3.png)
    + #### Step4. 編碼要指定有支援CUDA的編碼器
      **執行以下指令即可查看有支援CUDA的編碼器資訊**
      ```
      ffmpeg -codecs | findstr cuvid
      ```
      **範例如下圖**
      ![](Image/使用gpu加速_4.png)
      **就看encoders的部份**
    + #### Step5. 輸入ffmpeg指令使用gpu加速嚕
      ```
      ffmpeg -i "input_video" -c:v h264_nvenc "output_video"
      ```
    + #### Step6. 把GPU操到100%就是爽
      ![](Image/使用gpu加速_5.png)




+ ## 參考資料
  ```
  [ffmpeg](https://ffmpeg.org/)

  [wiki](https://zh.wikipedia.org/wiki/FFmpeg)
  ```
+ ## 補充資料
  ```
  [FFmpeg操作參數](https://zhuanlan.zhihu.com/p/145312133)
  ```
