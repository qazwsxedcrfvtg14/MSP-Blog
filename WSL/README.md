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
    * 開啟 WSL 有兩種方法，一個是用圖形介面，一個是下指令，可以自由選擇。
    * 方法一
        * 請打開Windows【設定】(快捷鍵 Win+I )  
        ![img](1.PNG)  
        * 打開【更新與安全性】，進去【開發人員專用】分頁，勾選【開發人員模式】  
        ![img](2.PNG)  
        * 這時系統可能會叫你重開機，如果有的話，請重開  
        * 進到【設定】裡的【APP】，然後點選畫面右方(有時會在畫面最下方)的【程式與功能】  
        ![img](3.PNG)  
        * 然後點選【開啟或關閉 Windows 功能】，在裡面找到【適用於 Linux 的 Windows 子系統(搶鮮版 (Beta))】勾選後按【確定】  
        ![img](4.PNG)  
    * 方法二
        * 打開管理員權限的 Powershell，輸入以下指令
        ```
        Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Windows-Subsystem-Linux
        ```
    * 跑了一陣子之後 Windows 會叫你重新開機，然後請重新開機。  
    * 重開機完後 WSL 已經安裝完成。
    * 接下來要安裝 Ubuntu 的映像
    * 叫出【執行】(快捷鍵 Win+R )，輸入【bash】後按確定  
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
    * 首先先參考 15063 的 WSL 安裝方式
    * 然後到 Windows Store 上下載喜歡的 Linux 映像
    * Ubuntu https://www.microsoft.com/en-us/store/p/ubuntu/9nblggh4msv6
    * openSUSE Leap 42 https://www.microsoft.com/en-us/store/p/opensuse-leap-42/9njvjts82tjx
    * SUSE Linux Enterprise Server 12 https://www.microsoft.com/en-us/store/p/suse-linux-enterprise-server-12/9p32mwbh6cns
* 官方說明文件
    * https://msdn.microsoft.com/zh-tw/commandline/wsl/install_guide
## 注意事項
* 當最後一個 bash.exe 結束的時候，就會把所有WSL的程序結束(包含背景執行的)，因此當有背景執行的 WSL 程式時，記得不要把所有的 bash.exe 都關掉。
* 因為 WSL 沒有 systemd，因此裝在系統上的服務不會自己開起來，要手動的打開
```
sudo service XXX-service start
```
* 使用 gdb 的時候，有時候某些 libary 載入到記憶體上的位置會和正常的Linux不一樣，因此打 CTF 的 Pwn 時，在 WSL 上的結果很有可能不符合預期。
* 無法在上面使用虛擬化技術，因此在上面無法使用 virtual box 或是 kvm (qemu) 之類的東西，不過 qemu 的管理器可以使用，但只能用來管理其他電腦上的 qemu。
* 當你在上面開啟 port 的時候，要注意不會跟其他 Windows 程式衝到。並且當你想要讓其他人能連到的話，需要去 Windows 的防火牆中設定，讓某些 port 能夠通行。
* 可以在 WSL 中執行 Windows 的 exe 程式，不過檔案需要放在 Windows 的目錄中，而不是 WSL 的 Linux 目錄上。
## 額外的東西
* ssh server
    * 跟Windows 10內建的 ssh server 衝突，所以記得要改Port或是關 Windows10 的 ssh 功能
    * 安裝 ssh-server
    ```
    sudo apt install openssh-server
    ```
    * 修改【/etc/ssh/sshd_config】內的這些東西
    ```
    AllowUsers <yourusername>
    PasswordAuthentication yes
    UsePrivilegeSeparation no
    ```
    * 然後要手動啟動 ssh-server 他
    ```
    sudo service ssh --full-restart
    ```
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
    * 裝上這個，你就有 firefox、gnome-terminal、gedit、Libre Office 等等東西可以用了
    ```
    sudo apt install ubuntu-desktop
    ```
* Hime
    * 參考這個
        * http://goodjack.blogspot.tw/2013/08/linux-phonetic-setting.html
    * 不過有幾點要注意，第一個是要開啟【支援遠端用戶端程式】
    * 然後在 VcXsrv 中 Ctrl-Space這個組合鍵無法使用，因此需替換成 Shift-Space
    * 最後記得加上以下的環境變數
    ```
    export GTK_IM_MODULE=hime
    export QT_IM_MODULE=hime
    ```
* 聲音
    * 我個人的做法是安裝pulse audio，然後這裡有別人編好的版本
        * https://github.com/kitor/wsl/raw/master/pulse6.zip
    * 然後解壓縮後執行裡面的```bin/pulseaudio.exe```
    * 接下來回到 bash 裡面，執行
    ```
    sudo add-apt-repository ppa:therealkenc/wsl-pulseaudio
    sudo apt install --no-install-recommends libpulse0 pavucontrol

    ```
    * 然後可能要寫個 script 讓他自動重啟，因為他好像一段時間沒有播放聲音就會自己關掉。
    * 如果還有問題請參考這裡 https://github.com/Microsoft/BashOnWindows/issues/486
* VLC
    * 這個純粹就只是為了好玩用的，不一定要裝 :P
    * 記得確定 pulse audio 有打開喔。
    ```
    sudo apt install vlc
    ```
    * bash 中輸入 vlc 打開之後
    * 【Tools】->【Video】->【Output】->【Color ASCII art video output】->【OK】
    * 把 vlc 關掉
    * 把環境變數的 ```DISPLAY``` 刪掉(或是改成空字串)
    * 執行
    ``` 
    cvlc <影片位置> 
    ```
    * 欣賞漂亮的 ASCII Art 吧  
    ![img](7.PNG)  
* Visual Studio Code
    * 在 Windows 上的 VS Code 中的設定中加入以下內容，即可將 WSL 的 bash 變成預設的 shell。
    ```
    "terminal.integrated.shell.windows": "C:\\WINDOWS\\Sysnative\\bash.exe",
    "terminal.external.windowsExec": "C:\\WINDOWS\\Sysnative\\bash.exe",
    ```
    * 但是目前沒辦法把 VS Code 裝在 WSL
        * https://github.com/Microsoft/BashOnWindows/issues/2293
* PTT
    * 很多人都不知道其實 PTT 可以用 ssh 的方式上去。
    ```
    ssh bbsu@ptt.cc
    ```
* 安裝 WSL 在其他磁碟
    * 此做法我沒有親自確認，不確定有效
    * 先用Windows的磁碟管理建立一個虛擬的磁碟(vhdx)，把其放在任意位置
    * 把它掛載在【使用者目錄/AppData/Local/lxss】上
    * 接著再安裝 Ubuntu 的映像
* 改裝成 Arch 
    * 先下載 Arch 的映像
        * https://www.archlinux.org/download/
    * 解開後 chroot 進去 arch 的 Live CD 根目錄上
    * 照著上面安裝 https://wiki.archlinux.org/index.php/installation_guide
    * 裝完之後，把所有的bash.exe都關掉
    * 用檔案總管進到【使用者目錄/AppData/Local/lxss】裡，找到Arch的根目錄，把它【移動】到最外面
    * 把Arch的根目錄重新命名成 rootfs
    * 打開 bash 測試是否成功
## WSL技術原理
* https://blogs.msdn.microsoft.com/wsl/