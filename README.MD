# Connect your bot to Microsoft Teams

- 目前Azure Bot Service已經可以很輕鬆的建置一個Bot，但是要把建置好的Bot串連到Teams平台需要比較多程序，本文將會示範使用App Studio建置一個Teams的App，並且串聯到您已經在Bot Service建立好的對話機器人，最後使用匯出的方式將這個Teams的App以壓縮檔的形式保存，提供開發人員或是企業內部小規模的測試，其他夥伴只要將壓縮檔上傳到Teams即可開始與Bot溝通。
- 本文將會著重在Teams的串接過程，並不會詳細介紹Azure Bot Service的建置流程。

## 事前準備：
1. 一個有效的Microsoft Azure訂閱帳戶
2. 一個已經部屬到Microsoft Bot Service上的對話機器人
3. Microsoft Teams（本文以桌面應用程式版作為示範）
 
## 操作流程
1. 開啟Azure Bot Service中的Teams通道
2. 使用Teams中的App Studio工具建構Teams App
3. 匯出成壓縮檔並且在Teams中上傳測試

### 開啟Azure Bot Service中的Teams通道

1. 首先，進入Azure Portal開啟你已經建立的Bot Service資源頁面。點選左邊選單列中的「Channels」，可以查看目前這個Bot連結到那些通道(通訊軟體)，以及這些通道的運作狀況。

2. 請點選下方【Add a featured channel】中的Teams圖示
![](https://i.imgur.com/YeYznik.png)

3. 點選【Publish】，此處列出2種與Teams串接的方式，本文將會以【Test your bot in Teams】方式為例。這邊無須設定，直接點選下方的【Save】按鈕即可開啟Bot與Teams平台的通道權限。接下來需要到Teams平台中設定Bot的基本資料。
 
 ![](https://i.imgur.com/bFadJcR.png)

>【Test your bot in Teams】是將bot打包成一個套件包，之後就可以直接匯入Microsoft Teams做測試(有點像Chrome一樣可以封裝擴充功能)，不需要繁複的上架申請流程，因此可以做為開發人員的測試用。

### 使用Teams中的App Studio工具建構Teams App
 
1. 首先，開啟Microsoft Teams，點選左下角的【Apps】按鈕
 ![](https://i.imgur.com/FNq3RHu.png)



2. 在左側搜尋列裡面輸入「App Studio」，選擇右邊出現的App Studio應用程式。
 ![](https://i.imgur.com/fWtCP0u.png)


 
3. 接著會彈跳出這個應用程式的相關介紹，第一次需要點選【Add】將這個工具加入你的清單中。（若已經加入過才會顯示【Open】）
 ![](https://i.imgur.com/ZyCn6ru.png)



4. 點選【Add】之後會看到一般的Teams對話頁面，但是我們沒有要跟他對話，而是點選上面的第2個Tab【Manifest editor】進入專案的編輯工具，再點選【Create a new app】開啟新的App專案。
 ![](https://i.imgur.com/TIrlKzw.jpg)
 
5. 開啟專案之後就會進入有關於這個App的所有設定頁面，只要設定完這些項目，就可以完成Teams App的封裝程序囉！
![](https://i.imgur.com/rCE8Z5U.png)



#### 接下來的整個過程其實就是依照App Studio的介面指示來填寫你要建立Teams App的所有資訊。填寫完成之後，這個應用程式就會幫你輸出成特定格式的JSON檔案名叫manifest.json，裡面定義了所有App所需要的資料以及連結到Bot的設定，由於這個JSON檔案格式較為複雜且嚴格，因此Microsoft Teams推出這個App Studio工具來輔助使用者可以更直覺的方式設定，本文將會以App Studio設定頁面為例。若要手動撰寫manifast.json檔案請參照官網的說明文件[【Reference: Manifest schema for Microsoft Teams】](https://docs.microsoft.com/en-us/microsoftteams/platform/resources/schema/manifest-schema)。







6. 以下為App Studio建立一個Teams App的必填欄位：
- App names：
    - 為你的Bot在Teams中命名。
- App ID：
    - 如果你已經在Azure中註冊Bot，請到Azure Bot Service設定頁面中複製【Microsoft App ID】(如下圖)，並貼上到此處。
 ![](https://i.imgur.com/meCDWDo.png)

- Package Name：
    - 這個Bot的套件名稱
- Version：	
    - 應用程式的版本號，如果程式有更新，版本號也要隨之遞增。未來正式上架到Teams App Store之後，透過版本號的改變，用戶端會主動更新App。
- Descriptions：
    - 這個Bot的敘述，有分成【Short description】（80字內）以及【Full description】（4000字內），Teams會依照使用者介面所能顯示的尺寸自動選擇。
- Developer information：
    - 開發者的名稱，以及開發者的網站連結
- App URLs：
    - 這個Bot的說明網站連結，分別需要輸入【Privacy statement】以及【Terms of use】
- Branding：
    - 上傳Bot的icon，分成大圖(192x192)以及縮圖(32x32)，圖片尺寸需要完全符合條件，Teams會在不同介面呈現不同icon，說明請參閱[【Create an app package for your Microsoft Teams app -- Icon】](https://docs.microsoft.com/en-us/microsoftteams/platform/concepts/build-and-test/apps-package#icons)

7. 輸入完【App details】之後，點選左邊選單的【Bots】>【Set up】>【Existing bot】

8. 點選【Connect to a different bot id】，輸入您在Azure Bot Service上建立Bot的"Microsoft App ID"且需要與上一個步驟輸入的Microsoft App ID相同（如果目前使用的Teams帳號與建立Azure Bot Service帳號相同，也可以選擇【Select one of my existing bots】，下方會直接列出目前已經建置好的Bot Service供您選擇）
![](https://i.imgur.com/F37j0eI.png)
9. 【Scope】請依據您的Bot使用目的選擇允許出現的範圍：
    - Personal：一對一聊天室
    - Teams：團隊聊天室中
    - Group Chat：您與他人建立的聊天室
10. 點選【Create bot】之後即可與您的Azure Bot Service連結
11. 其餘的App設定可以依據專案的需求選擇，詳細請參閱[【Reference: Manifest schema for Microsoft Teams】](https://docs.microsoft.com/en-us/microsoftteams/platform/resources/schema/manifest-schema)，本文僅示範最基本的部屬範例
12. 輸入完成之後點選左邊選單的【Test and distribute】>【Download】，App Studio就會依照剛剛GUI介面設定的資訊產生一份manifast.json檔案，並且打包成壓縮檔供您下載。這份檔案就是將Azure Bot Service與Teams串接的關鍵檔案。

#### 到目前為止，已經完成了Bot與Teams連結的流程，接下來只要將下載的zip壓縮檔傳送到Teams中即可開始與Bot溝通

1. 點選Teams App左側選單中的【…】(More Added Apps)功能，再點選【More apps >】按鈕
![](https://i.imgur.com/JwvjKoG.png)

2. 點選【Uploaded a custom app】即可將剛剛製作完成的Bot 壓縮檔匯入，匯入後再點選【Add】即可開始與Bot溝通，您亦可將這個zip壓縮檔傳送給您的夥伴，對方只要依照相同的方式將zip檔案上傳也可與您的Bot開始對話。
![](https://i.imgur.com/dso6HgU.png)
![](https://i.imgur.com/es2spjF.png)
## 結語:
- 本篇文章示範如何將已經在Bot Service建置好的對話機器人串聯至Teams平台，適合企業負責對話機器人的開發人員，完成本文的測試之後，可以點選【Test and distribute】中的【Submit】，並且開始平台上架的流程。