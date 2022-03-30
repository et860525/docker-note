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

## 指令

### 映像檔

#### 取得映像檔

從 Docker Hub倉庫下載 MySQL 8.0資料庫

```bash
sudo docker pull mysql:8.0
```

#### 列出

顯示本機已有的映像檔

```bash
sudo docker images

REPOSITORY   TAG       IMAGE ID       CREATED        SIZE
mysql        8.0    3218b38490ce   3 months ago   516MB
```

#### 建立

我們可以使用 `docker build`來建立一個新的映像檔。

先新建一個目錄和一個 Dockerfile

```bash
mkdir django
cd docker_test
touch Dockerfile 
```

```dockerfile
# This is a comment
FROM ubuntu:18.04
RUN apt update
RUN apt install python3
```

Dockerfile基本語法：

* `#` 是註解
* `FROM` 是告訴 Docker使用哪個映像檔當基底
* `RUN` 這些指令會在建立時執行，例如上面的更新與安裝 Python3

#### 刪除

`rmi`是刪除映像檔， `rm`則是刪除容器。

```bash
sudo docker rmi mysql
# or
sudo docker rmi 3218b38490ce
```

### 容器

#### 新建並啟動

建立一個在背景運作的 MySQL資料庫

```bash
docker run --name sql -p 3306:3306 -e MYSQL_ROOT_PASSWORD=123456 -d mysql:8
```

* `--name` 指定容器的名稱，如果不指定系統將會隨機分配一個名字
* `-p` 將容器的3306端口映射到主機的3306端口
* `-e` 環境參數設定
* `-d` 背景執行
* `mysql:8` 該容器的映像檔，如本機映像檔沒有，則自動從網路抓取

啟動一個 bash 終端，允許使用者進行互動，當使用者登出該終端，則關閉容器。

```bash
sudo docker run -it ubuntu:18.04 /bin/bash
```

* `-t` 選項能讓 Docker分配一個終端並綁到容器的標準輸入上
* `-i` 能讓容器的標準輸入保持打開

#### 啟動已終止容器

```bash
sudo docker start (container_id or container_name)
```

#### 列出容器

* 列出正在運行的容器

  ```bash
  sudo docker ps
  ```

* 列出所有的容器

  ```bash
  sudo docker ps -a
  ```

#### 停止

```bash
sudo docker stop (container_id or container_name)
```

#### 重啟

```bash
sudo docker restart (container_id or container_name)
```

#### 進入容器

* **exec 命令**

  ```bash
  sudo docker ps
  CONTAINER ID   IMAGE          COMMAND   CREATED              STATUS          PORTS     NAMES
  66db6b30cbed   ubuntu/py:v2   "bash"    About a minute ago   Up 52 seconds             testub
  sudo docker exec -it testub bash
  ```

* **attach 命令**

  ```bash
  sudo docker ps
  CONTAINER ID   IMAGE          COMMAND   CREATED              STATUS          PORTS     NAMES
  66db6b30cbed   ubuntu/py:v2   "bash"    About a minute ago   Up 52 seconds             testub
  sudo docker attach testub
  ```

#### 刪除

```bash
sudo docker rm (container_id or container_name)
```
