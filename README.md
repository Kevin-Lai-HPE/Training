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

## IMAGES
一個image是用於創建一個Docker container指令的只讀模板。通常，一個映像基於另一個映像，並進行一些其他自定義。例如，您可以構建基於該ubuntu 映像的映像，但安裝Apache Web服務器和您的應用程序，以及運行該應用程序所需的配置詳細信息。

您可以創建自己的images，也可以僅使用其他人創建並在註冊表中發布的image。要構建自己的映像，您可以使用簡單的語法創建一個Dockerfile，以定義創建映像並運行它所需的步驟。Dockerfile中的每條指令都會在映像中創建一個層。當您更改Dockerfile並重建映像時，僅重建那些已更改的層。與其他虛擬化技術相比，這是使映像如此輕巧，小型和快速的部分原因。

## CONTAINERS
Container是image的可運行實例。您可以使用Docker API或CLI創建，啟動，停止，移動或刪除container。您可以將container連接到一個或多個網絡，將存儲連接到它，甚至根據其當前狀態創建一個新映像。

默認情況下，container與其他container及其主機之間的隔離度相對較高。您可以控制container的網絡，存儲或其他基礎子系統與其他container或與主機的隔離程度。

container由其映像以及在創建或啟動時為其提供的任何配置選項定義。刪除container後，未存儲在永久性存儲中的狀態更改將消失。
