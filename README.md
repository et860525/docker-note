# Docker

## 虛擬機器 vs 容器

### 虛擬機器(Virtual Machine)

**目標**：將一個應用程式所需要執行的環境打包，並建立一個獨立的環境。

透過 [Hypervisor](https://zh.wikipedia.org/wiki/Hypervisor)在虛擬機器和底層的實體硬體之間進行協調，Hypervisor負責將實體運算資源(如處利器、記憶體和儲存設備等等)配置給每個虛擬機器。它會讓虛擬機器之間分開，不會互相干擾彼此。簡單來說，Hypervisor可以讓你電腦上的作業系統(Host OS)上再裝一個作業系統(Guest OS)，而他們不會相互干擾。

### 容器(Container)

**目標**：改善虛擬機要安裝Guest OS所導致啟動慢、佔太多記憶體的問題。

Container將應用程式與所需的環境(程式碼、函式庫...)進行打包的動作，在 Container中不需要安裝作業系統就能執行應用程式。Container運行的地方就是 `Host OS`的核心系統層。

所有 Container共用 Host OS，並建立資源控管機制來分配 Host OS的資源，省去建立與執行 Guest OS的時間，也能做到各個 Container互相獨立。

Container的優點為：

* 不需要安裝OS
* 資源更有效運用
* 執行速度較快
* 儲存空間較小
* 轉移性高
* 具有隔離性

## Docker 三個基本元素

Docker最重要的三個元素為：

* 映像檔(Image)
* 容器(Container)
* 倉庫(Repository)

### 映像檔 (Image)

**映像檔**就是一個唯獨的模板，用來產生容器的實體。例如：一個 MYSQL服務、一個 Python編譯環境、一個 Ubuntu作業系統。

### 容器 (Container)

**容器**是用映像檔所建立出來的實例。可以被啟動、開始、停止、刪除。每個容器都是相互隔離。

可以把容器看做是一個簡易版的 Linux 環境（包括root使用者權限、程式空間、使用者空間和網路空間等）和在其中執行的應用程式。

容器在啟動時會建立一層可以被修改的可寫層，讓容器的功能可以再擴充。

### 倉庫 (Repository)

**倉庫**是集中放映像檔的地方，倉庫註冊伺服器（Registry）上則存放著多個倉庫。

最大的公開倉庫註冊伺服器是[Docker Hub](https://hub.docker.com/)，存放了數量龐大的映像檔供使用者下載，可以輕鬆在上面找到各式各樣現成實用的映像檔。
