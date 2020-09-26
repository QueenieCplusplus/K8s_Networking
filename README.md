# K8s_Networking
Network Resources for K8s

# Pod 豌豆莢子與同儕和對容器而言

均擁有一獨立的 IP 位址，Pod 之間無關乎是否運行在同一個 Node (虛擬主機上)，均要求透過 Pods IP 與對方 IP 進行存取。
此優點在於使用者無需考慮建立 Pod 之間的連線，或是 containers port 到 Nodes port 的連接上。

此過程為 



                                               IP-per-Pod
           container port in PodB --- PodB IP ----------- PodA IP ---- container port in PodA
                                                   |
                                                   |
                                                   
                                                 Node Port
                                                 
                                                   |
                                                   |
                                                  
                                               Cluster IP
                                                   
                                                   
* IP per Pod 

 此網路連結的空間是平行的，對於叢集內的所有 Pod 來說，也沒有 NAT 的轉換。
 
     # ip address show
 
* container to Pod

  同一 Pod 內的容器都是用同一 IP 位址，所以僅需要將 IP 指定為 localhost 和連接對方的連接阜 port 即可。
  因為同一豌豆莢子內的容器都是共用一個網路資源，彼此本身也是耦合關係，所以用 bind() 互相繫結彼此。
  
  缺點：因為共用資源，所以缺乏隔離性。
  
 
 # K8s 的網路實現
 
 使用微服務概念。
 
 (1) 容器與容器是共用網路資源的耦合關係。
 
 (2) Pod 與 Pod 可直接利用 IP 進行 Socket 溝通。
 
 (3) Pod 與 Service of Node 可藉由 ______  進行通訊。
 
     * 同一 Node 的 Pod 之間通訊
     
     * 不同 Node 的 Pod 之間通訊
 
 (4) Cluster 有叢集自身的 IP 並且結合 Node 的 Port 產生對外的服務端點。
 
 # K8s 網路架構圖
 
 

                             外部存取

                                ｜
                                ｜
                                ｜

                              LB   ----R/W status of service ----   Service Agent

                         /       |                                      \
                        /        |                                       \
                       /         |                                        \
                  --- Slave--  --Slave---                                       
                  | Pod Pod | | Pod Pod |                             K8s Master （API Server, etcd）
                  ｜   sw   |  |   sw   |
                  ---Router--  --Router-- 


  * LB：效果同 GCE。
  
  * Service Agent: 感測服務的變化，進而透過 API 監控服務。

