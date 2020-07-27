# Training
2020/7/27
[Docker overview](https://docs.docker.com/get-started/overview/)

# Docker概述
Docker是一個用於開發，交付和運行應用程序的開放平台。Docker使您能夠將應用程序與基礎架構分開，從而可以快速交付軟件。借助Docker，您可以以與管理應用程序相同的方式來管理基礎架構。通過利用Docker的快速交付，測試和部署代碼的方法，您可以大大減少編寫代碼和在生產環境中運行代碼之間的延遲。

## Docker平台
Docker提供了在鬆散隔離的環境（稱為container）中打包和運行應用程序的功能。隔離和安全性使您可以在給定主機上同時運行多個container。container是輕量級的，因為它們不需要管理程序的額外負擔，而是直接在主機的內核中運行。這意味著與使用虛擬機相比，可以在給定的硬件組合上運行更多的container。您甚至可以在實際上是虛擬機的主機中運行Docker container！

Docker提供了工具和平台來管理container的生命週期：
  * 使用container開發應用程序及其支持組件。
  * container成為分發和測試您的應用程序的單元。
  * 準備就緒後，可以將應用程序作為container或協調服務部署到生產環境中。無論您的生產環境是本地數據中心，雲提供商還是二者的混合體，其工作原理都相同。

## Docker引擎
Docker Engine是具有以下主要組件的client-server應用程序：
* 服務器是一種長期運行的程序，稱為daemon（ dockerd命令）。
* REST API，它指定程序可以用來與daemon進行通信並指示其操作的接口。
* 命令行界面（CLI）client（docker命令）。

![Docker引擎組件流程](https://docs.docker.com/engine/images/engine-components-flow.png)

CLI使用Docker REST API通過腳本或直接CLI命令來控制Docker daemon或與Docker daemon進行交互。許多其他Docker應用程序都使用基礎API和CLI。

Daemon創建和管理Docker object，例如images，containers，networks和volumes。

## 我可以將Docker用於什麼?

### 快速，一致地交付您的應用程序
Docker通過允許開發人員使用提供您的應用程序和服務的本地container在標準化環境中工作，從而簡化了開發生命週期。container非常適合持續集成和持續交付（CI / CD）工作流程。
請考慮以下示例方案：
* 您的開發人員在本地編寫代碼，並使用Docker container與同事共享他們的工作。
* 他們使用Docker將其應用程序推送到測試環境中，並執行自動和手動測試。
* 當開發人員發現錯誤時，他們可以在開發環境中對其進行修復，然後將其重新部署到測試環境中以進行測試和驗證。
* 測試完成後，將修復程序推送給生產環境就像將更新的映像推送到生產環境一樣簡單。

### 響應式部署和擴展

Docker基於container的平台允許高度可移植的工作負載。Docker container可以在開發人員的本地筆記本電腦上，數據中心內的物理或虛擬機上，雲提供商上或混合環境中運行。

Docker的可移植性和輕量級的特性還使您可以輕鬆地動態管理工作負載，並根據業務需求指示實時擴展或拆除應用程序和服務。

### 在同一硬件上運行更多工作負載

Docker輕巧快速。它為基於虛擬機管理程序的虛擬機提供了可行且經濟高效的替代方案，因此您可以利用更多的計算能力來實現業務目標。Docker非常適合高密度環境以及中小型部署，而您需要用更少的資源做更多的事情。

## Docker架構

Docker使用client-server架構。Docker client與Docker daemon進行對話，該守護進程完成了構建，運行和分發Docker container的繁重工作。Docker client和daemon可以在同一系統上運行，也可以將Docker client連接到遠程Docker daemon。Docker client和daemon在UNIX套接字或網絡接口上使用REST API進行通信。

![Docker架構圖](https://docs.docker.com/engine/images/architecture.svg)

## Docker daemon
Docker daemon（```dockerd```）偵聽Docker API請求並管理Docker對象，例如images，containers，networks和volumes。daemon還可以與其他daemon通信以管理Docker服務。

## Docker client
Docker client（```docker```）是許多Docker用戶與Docker交互的主要方式。當您使用諸如之類的命令時```docker run```，client會將這些命令發送到```dockerd```，以執行這些命令。該```docker```命令使用Docker API。Docker client可以與多個daemon通信。

## Docker registries
Docker 註冊表存儲Docker映像。Docker Hub是任何人都可以使用的公共註冊表，並且Docker配置為默認在Docker Hub上查找映像。您甚至可以運行自己的私人註冊表。

使用```docker pull```或```docker run```命令時，所需的images將從配置的註冊表中提取。使用該```docker push```命令時，會將映像推送到配置的註冊表。

## Docker objects
使用Docker時，您正在創建和使用images，containers，network，volumes，插件和其他對象。本節是其中一些對象的簡要概述。

### IMAGES
一個image是用於創建一個Docker container指令的只讀模板。通常，一個映像基於另一個映像，並進行一些其他自定義。例如，您可以構建基於該ubuntu 映像的映像，但安裝Apache Web服務器和您的應用程序，以及運行該應用程序所需的配置詳細信息。

您可以創建自己的images，也可以僅使用其他人創建並在註冊表中發布的image。要構建自己的映像，您可以使用簡單的語法創建一個Dockerfile，以定義創建映像並運行它所需的步驟。Dockerfile中的每條指令都會在映像中創建一個層。當您更改Dockerfile並重建映像時，僅重建那些已更改的層。與其他虛擬化技術相比，這是使映像如此輕巧，小型和快速的部分原因。

### CONTAINERS
Container是image的可運行實例。您可以使用Docker API或CLI創建，啟動，停止，移動或刪除container。您可以將container連接到一個或多個網絡，將存儲連接到它，甚至根據其當前狀態創建一個新映像。

默認情況下，container與其他container及其主機之間的隔離度相對較高。您可以控制container的網絡，存儲或其他基礎子系統與其他container或與主機的隔離程度。

container由其映像以及在創建或啟動時為其提供的任何配置選項定義。刪除container後，未存儲在永久性存儲中的狀態更改將消失。

示例```docker ru```命令
以下命令運行一個ubuntu container，以交互方式附加到本地命令行會話，然後運行```/bin/bash```。

```$ docker run -i -t ubuntu /bin/bash```
當您運行此命令時，會發生以下情況（假設您使用的是默認註冊表配置）：

1. 如果您在```ubuntu```本地沒有該映像，則Docker會將其從已配置的註冊表中拉出，就像您已```docker pull ubuntu```手動運行一樣。

2. Docker會創建一個新的container，就像您已```docker container create```手動運行命令一樣。

3. Docker將一個讀寫文件系統分配給container，作為其最後一層。這允許運行中的container在其本地文件系統中創建或修改文件和目錄。

4. Docker創建了一個網絡接口，將container連接到默認網絡，因為您沒有指定任何網絡選項。這包括為container分配IP地址。默認情況下，container可以使用主機的網絡連接連接到外部網絡。

5. Docker啟動container並執行```/bin/bash```。由於container是交互式運行的，並且已附加到您的終端（由於```-i```和```-t``` 標誌），因此您可以在輸出記錄到終端時使用鍵盤提供輸入。

6. 當您鍵入```exit```以終止```/bin/bash```命令時，container將停止但不會被刪除。您可以重新啟動或刪除它。

### SERVICES
服務允許你擴展在多個Docker daemons的containers，這是所有工作一起作為一個群有多個*mangers*和*workers*。群集的每個成員都是Docker daemon，所有daemon都使用Docker API進行通信。服務允許您定義所需的狀態，例如在任何給定時間必須可用的服務副本的數量。默認情況下，該服務在所有工作節點之間是負載平衡的。對於消費者而言，Docker服務似乎是一個單獨的應用程序。Docker Engine在Docker 1.12及更高版本中支持集群模式。

## 底層技術
Docker用**Go**編寫，並利用Linux內核的多個功能來交付其功能。

## Namespace
Docker使用一種稱為```namespaces```提供container的隔離工作區的技術。運行container時，Docker會為該container創建一組名稱空間。
這些名稱空間提供了一層隔離。container的每個方面都在單獨的名稱空間中運行，並且其訪問僅限於該名稱空間。

Docker Engine在Linux上使用以下名稱空間：

* **The```pid```namespace**：進程隔離（PID：進程ID）。
* **The```net```namespace**：管理網絡接口（NET：網絡）。
* **The```ipc```namespace**：管理訪問IPC資源（IPC：進程間通信）。
* **The```mnt```namespace**：管理文件系統掛載點（MNT：摩）。
* **The```uts```namespace**：隔離內核和版本標識符。（UTS：Unix時間共享系統）。

## 對照組
Linux上的Docker Engine還依賴於另一種稱為控制組 （```cgroups```）的技術。cgroup將應用程序限制為一組特定的資源。控制組允許Docker Engine將可用的硬件資源共享給container，並有選擇地實施限制和約束。例如，您可以限制特定container可用的內存。

## 聯合文件系統
聯合文件系統或UnionFS是通過創建圖層進行操作的文件系統，使其非常輕便且快速。Docker Engine使用UnionFS為container提供構建模塊。Docker Engine可以使用多個UnionFS變體，包括AUFS，btrfs，vfs和DeviceMapper。

## Container 格式
Docker Engine將名稱空間，控制組和UnionFS組合到一個稱為container格式的包裝器中。默認container格式為```libcontainer```。將來，Docker可能會通過與BSD Jails或Solaris Zones等技術集成來支持其他container格式。
