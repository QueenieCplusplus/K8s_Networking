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
  
  
  

  
