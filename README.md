# Training
2020/7/27
[Docker overview](https://docs.docker.com/get-started/overview/)

# Docker概述
Docker是一個用於開發和運行應用程式的開放平台。Docker將應用程式與基礎架構分開，以此加速軟體開發。借助Docker，可以以管理應用程式相同的方式來管理基礎架構。通過利用Docker可以減少coding和在生產環境中運行程式之間的延遲。

## Docker平台
Docker提供了打包以及在鬆散的隔離環境中運行應用程式的功能(稱為container)。隔離環境的安全性使可以使的主機上同時運行多個container。
container是輕量級的，因為它們不需要額外負擔管理程式，而是直接在內核中運行。這代表說與使用虛擬機相比，可以在相對應的硬體上運行更多的container。甚至可以在虛擬機的主機中運行Docker container！

Docker提供了工具和平台來管理container的生命週期：
  * 使用container開發應用程式和其支援的組件。
  * container成為發布和測試應用程式的單位。
  * 可以將應用程式作為container或服務部署到生產環境中。無論生產環境是local、cloud或者兩者的混合，都是一樣的。

## Docker Engine
Docker Engine是具有以下主要組件的client-server應用程式：
* 服務器是一種長期運行的程式，稱為daemon（ dockerd 命令）。
* REST API，它指定程式可以用來與daemon交流並指示其如何進行操作。
* CLI client（ docker 命令）。

![Docker引擎組件流程](https://docs.docker.com/engine/images/engine-components-flow.png)

CLI透過 Docker REST API 、script或直接執行commands，以此交流或控制Docker daemon。許多其他Docker應用程式都使用基礎API和CLI。
Daemon創建和管理Docker object，例如images，containers，networks和volumes。

## 可以將Docker用於

### 快速並且一致性的交付應用程式
Docker通過允許開發人員使用local container在標準化的環境中工作，從而簡化了開發生命週期。container非常適合持續集成和持續交付（CI / CD）工作流程。
以下示例：
* 開發人員在本地編寫代碼，並使用Docker container與同事共享工作。
* 他們使用Docker將應用程式push到測試環境中，並執行自動和手動測試。
* 當開發人員發現錯誤時，他們可以在開發環境中對其進行修復，然後將其重新部署到測試環境中以進行測試和驗證。
* 測試完成後，將修復程式push給顧客，就像將更新image 將其push到生產環境一樣簡單。

### 響應式部署和擴展

Docker的container基於平台允許高可移植的工作負載。Docker container可以在開發人員的筆記本上，數據中心內的物理或虛擬機上，雲上或混合環境中運行。

Docker的可移植性和輕量級的特性，使得可以輕鬆地動態管理工作負載，並根據業務需求擴展或減少應用程式和服務。

### 在同一硬件上運行更多工作負載

Docker為基於虛擬機的環境提供了可行且具有成本效益的替代方案，因此可以利用更多的計算能力來實現商業目的。Docker非常適合高密度環境以及中小型部署，並且可以用更少的資源做更多的事情。

## Docker架構

Docker使用client-server架構。Docker daemon完成構建、運行和分發Docker container的工作。Docker client和daemon可以在同一系統上運行，也可以將Docker client遠端連線到Docker daemon。Docker client通過UNIX sockets或網路使用REST API進行和daemon通信。

![Docker架構圖](https://docs.docker.com/engine/images/architecture.svg)

## Docker daemon
Docker daemon（```dockerd```）偵聽Docker API請求並管理Docker對象，例如images，containers，networks和volumes。daemon還可以與其他daemon溝通以管理Docker服務。

## Docker client
Docker client（```docker```）是許多Docker用戶與Docker交互的主要方式。當使用諸如```docker run```之類的命令時，client會將這些命令發送到```dockerd```執行這些命令。該```docker```命令使用Docker API。Docker client可以與多個daemon通信。

## Docker registries
Docker registries儲存Docker images。Docker Hub是任何人都可以使用的public registry，並且Docker的默認配置是在Docker Hub上查找images。可以運行自己的private registry。
使用```docker pull```或```docker run```命令時，所需的images將從配置的registry中提取。使用該```docker push```命令時，會將images push到配置的registry。

## Docker objects
使用Docker時，創建和操作的基本物件:images，containers，network，volumes，插件和其他objects。

### IMAGES
image是一個用於創建一個Docker container指令的read-only模板。通常一個image是基於另一個image進行了一些其他自定義。例如，可以構建基於該ubuntu image的image，但需要安裝Apache Web服務器和應用程式，以及運行該應用程式所需的其他配置資訊。
可以創建自己的images，也可以使用其他人創建並在registry中發布的image。要構建自己的image，可以創建一個Dockerfile，以定義創建image、運行image所需的步驟。Dockerfile中的每條指令都會在映像中創建一個layer。當更改Dockerfile並重建image時，只有更改的那須layer會重建。

### CONTAINERS
Container是image的可運行實例。可以使用Docker API或CLI去創建、啟動、停止、移動或刪除container。可以將container連接到一個或多個網路，將儲存連接給它，甚至創建一個基於當前狀態的新image。
默認情況下，container與其他container及其主機之間的隔離度相對較高。可以控制container的網路，儲存或其他基礎子系統與其他container或與主機的隔離程度。
container由其image以及在創建或啟動時為其提供的配置選項定義。刪除container後，未儲存在永久性儲存中的狀態更改將消失。

示例```docker ru```命令
ubuntu container，以交互方式附加到本地命令行會話，然後運行```/bin/bash```。

```$ docker run -i -t ubuntu /bin/bash```
當運行此命令時，會發生以下情況（假設使用的是默認registry配置）：

1. 如果在```ubuntu```本地沒有這個images，則Docker會將其從已配置的registry中拉出，就像已手動運行```docker pull ubuntu```一樣。

2. Docker會創建一個新的container，運行命令```docker container create```。

3. Docker將一個讀寫文件系統分配給container，作為其最後一層。這將允許運行中的container在其loacl文件系統中創建或修改文件及目錄。

4. 沒有指定任何網路選項，Docker將創建了一個網路介面以此將container連接到默認的網路連線。這包括給予container一個IP地址。在默認情況下，container可以使用主機的網路連縣到外部網路。

5. Docker啟動container並執行```/bin/bash```。由於container是交互式運行的，並且已連接到終端（由於```-i```和```-t``` 標誌），因此可以在輸出記錄到終端時使用鍵盤提供輸入。

6. 當輸入```exit```以終止```/bin/bash```命令時，container將停止但不會被刪除。可以重新啟動或刪除它。

### SERVICES
服務允許在多個Docker daemons的擴展containers，這些daemons可以與多個*mangers*和*workers*一起工作。群集的每個成員都是Docker daemon，所有daemon都使用Docker API進行通信。服務允許定義所需的狀態，例如在任何給定時間必須可用的服務副本量。默認情況下，該服務在所有工作節點之間是負載平衡的。對於消費者而言，Docker服務似乎是一個單獨的應用程式。Docker Engine在Docker 1.12及更高版本中支持集群模式。

## 底層技術
Docker用**Go**編寫，並利用Linux內核的多個功能來交付其功能。

## Namespace
Docker使用一種稱為```namespaces```提供container隔離工作區的技術。運行container時，Docker會為該container創建一組namespaces。
container的每個方面都在單獨的namespaces中運行，並且其權限僅限於該namespaces。

Docker Engine在Linux上使用以下namespaces：

* **The```pid```namespace**：行程隔離（PID：ProcessID）。
* **The```net```namespace**：管理網路介面（NET：網路）。
* **The```ipc```namespace**：管理訪問IPC的資源（IPC：行程間通信）。
* **The```mnt```namespace**：管理文件系統掛載點（MNT：Mount）。
* **The```uts```namespace**：隔離內核和版本標識符。（UTS：Unix時間共享系統）。

## Control groups
Linux上的Docker Engine還依賴於另一種稱為*control groups* （```cgroups```）的技術。cgroup將應用程式限制為一組特定的資源。控制組允許Docker Engine將可用的硬體資源共享給container，並可以有選擇的實施限制和約束。例如，可以限制特定container可使用的記憶體。

## 聯合文件系統(Union file systems)
聯合文件系統或UnionFS是通過創建layers進行操作的文件系統，使其非常輕便且快速。Docker Engine使用UnionFS為container提供構建模組。Docker Engine可以使用多個UnionFS變體，包括AUFS，btrfs，vfs和DeviceMapper。

## Container format
Docker Engine將namespaces、Control groups和UnionFS組合到一個稱為container format的包裝中。默認container格式為```libcontainer```。將來，Docker可能會通過與BSD Jails或Solaris Zones等技術集成來支持其他container格式。
