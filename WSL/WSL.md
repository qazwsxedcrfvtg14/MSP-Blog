# Windows Subsystem for Linux
## 他是什麼，能吃嗎
* Windows Subsystem for Linux（簡稱WSL）是一個為在Windows 10上能夠原生運行Linux二進制可執行文件（ELF格式）的兼容層。
* 它是由微軟與Canonical公司合作開發，目標是使純正的Ubuntu 16.04 "Xenial Xerus"映像能下載和解壓到用戶的本地計算機，並且映像內的工具和實用工具能在此子系統上原生運行。
* WSL提供了一個微軟開發的Linux兼容內核接口（不包含Linux代碼），使得Linux用戶模式的二進制文件能在其上執行。
* 運行原理很像是Linux上Wine的Windows版。
## 官方Github Reop
* https://github.com/Microsoft/BashOnWindows/
* 如果遇到bug或是想要有新的feature，請上去發Issues。
## Release Note
* https://msdn.microsoft.com/en-us/commandline/wsl/release_notes?f=255&MSPPError=-2147217396
* 可以在這裡看到最新的Windows Inside中，WSL被改進了什麼東西，和出現了什麼bug。
## 安裝方法
* 以下僅考慮正式Release的Windows版本(Insider請去看Release Note)
* 如果你的電腦不是Intel的64位元架構的話(例如x86、arm架構)
    * 請買新電腦
* 如果你的Windows OS組建低於14393
    * 請升級您的Windows
    * https://support.microsoft.com/en-us/help/3159635/windows-10-update-assistant
* 如果你的Windows OS組建是14393
    * 我還是建議升級您的Windows，因為這個版本的功能不是很完整，使用時會遇到很多坑
    * 如果真的要安裝可以，請參考15063的WSL安裝方法
    * https://support.microsoft.com/en-us/help/3159635/windows-10-update-assistant
* 如果你的Windows OS組建是15063
    * 請打開Windows【設定】(快捷鍵 Win+I )  
    ![img](1.PNG)  
    * 打開【更新與安全性】，進去【開發人員專用】分頁，勾選【開發人員模式】  
    ![img](2.PNG)  
    * 這時系統可能會叫你重開機，如果有的話，請重開  
    * 進到【設定】裡的【APP】，然後點選畫面右方(有時會在畫面最下方)的【程式與功能】  
    ![img](3.PNG)  
    * 然後點選【開啟或關閉 Windows 功能】，在裡面找到【適用於 Linux 的 Windows 子系統(搶鮮版 (Beta))】勾選後按【確定】  
    ![img](4.PNG)  
    * 跑了一陣子之後 Windows 會叫你重新開機，然後請重新開機。  
    * 重開機完後，叫出【執行】(快捷鍵 Win+R )，輸入【bash】後按確定  
    ![img](5.PNG)  
    * 接下來他會問一推問題，請依照需求回答，例如使用者帳號密碼之類的。  
    * 以後要打開他可以直接叫出【執行】(快捷鍵 Win+R )，輸入【bash】後按確定後打開。  
        * 其執行檔位置在【C:\Windows\System32\bash.exe】
    * WSL的管理方法
    ```
    lxrun

    在 LX 子系統上執行系統管理作業

    使用方式:
        /install - 安裝子系統
            選擇性引數:
                /y - 不提示使用者接受或建立子系統使用者
        /uninstall - 解除安裝子系統
            選擇性引數:
                /full - 執行完整解除安裝
                /y - 不提示使用者確認
        /setdefaultuser - 設定 bash 將以其身分啟動的子系統使用者。若該使用者不存在，將會建立該使用者。
            選擇性引數:
                username - 提供使用者名稱
                /y - 若提供使用者名稱，不提示建立密碼
        /update - 更新子系統的套件索引
    ```
* 如果你的Windows OS組建是16215以上
    * 可以到Windows Store上下載WSL
    * Ubuntu https://www.microsoft.com/en-us/store/p/ubuntu/9nblggh4msv6
    * openSUSE Leap 42 https://www.microsoft.com/en-us/store/p/opensuse-leap-42/9njvjts82tjx
    * SUSE Linux Enterprise Server 12 https://www.microsoft.com/en-us/store/p/suse-linux-enterprise-server-12/9p32mwbh6cns
## 額外的東西
* ssh server
    * 跟Windows 10內建的 ssh server 衝突
* apache2
* 圖形介面
    * 我個人推薦使用的 X Server 是 VcXsrv Windows X Server
        * https://sourceforge.net/projects/vcxsrv/
    * 安裝完之後請在啟動裡面加入有內容的 .bat 檔，讓 X Server 可以每次開機都直接啟動
        * 開啟啟動資料夾可以直接【執行】【shell:startup】
    ```
    @echo 0
    start "" "C:\Program Files\VcXsrv\vcxsrv.exe" :0 -ac -terminate -lesspointer -multiwindow -clipboard -wgl
    exit
    ```
    * 然後進到 bash 裡面，在使用者的環境變數裡面增加
    ```
    DISPLAY=127.0.0.1:0
    ```
    * 如果使用```DISPLAY=:0```的話，本機上沒有問題，但是```ssh -X```的時候就會出現問題。
    * 加入環境變數的方法有很多種，我個人的做法是在【~/.bashrc】裡面加入。
    ```
    export DISPLAY=127.0.0.1
    ```
* Ubuntu Desktop
* Hime
* 聲音
    * 我個人的做法是安裝pulse audio，然後這裡有別人編好的版本
        * https://github.com/kitor/wsl/raw/master/pulse6.zip
    * 然後解壓縮後執行裡面的```bin/pulseaudio.exe```
    * 接下來回到 bash 裡面，執行
    ```
    sudo add-apt-repository ppa:therealkenc/wsl-pulseaudio
    sudo apt install --no-install-recommends libpulse0 pavucontrol

    ```
    * 如果還有問題請參考這裡 https://github.com/Microsoft/BashOnWindows/issues/486
* VLC