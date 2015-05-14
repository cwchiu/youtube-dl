# 目錄結構
```
/bin            ; 可執行檔
/devscripts     ; 開發用腳本
/docs           ; 文件產生器
/test           ; 單元測試
/youtube_dl     ; 主程式
    /extractor  ; 擴充下載
        - __init__.py  ;
        - common.py    ; 共用套件, class InfoExtractor, SearchInfoExtractor
    - __init__.py   ; 程式進入點, 解析命令列參數並傳給 YoutubeDL.py
    - options.py ; 解析命令列參數, 使用 optparse
    - YoutubeDL.py ; 專案核心
```

# 擴充套件下載方式
## /youtube_dl/extractor/__init__.py
* 採用名稱規範，類別名稱結尾必須是 IE
* 預先 import 實做類別, 免除動態 import
* 掃描支援下載器的程式碼
```
_ALL_CLASSES = [
    klass
    for name, klass in globals().items()
    if name.endswith('IE') and name != 'GenericIE'
]
_ALL_CLASSES.append(GenericIE)
```
* get_info_extractor(ie_name) - 取得正確 class
* gen_extractors() - 產生所有下載的類別實體
* extract(self, url) - 呼叫真正下載器的 _real_extract(self, url)

# class InfoExtractor
## suitable(cls, url)
比對類別定義的 _VALID_URL 是否與 url 吻合

## working(cls)
檢查目前下載器是否可用

# class YoutubeDL

## download()
- extract_info() => 迭代 IE 的 suitable() 找尋合適的 IE => 執行 IE.extract() => process_ie_result()
- 
## extract_info()

## process_video_result()
