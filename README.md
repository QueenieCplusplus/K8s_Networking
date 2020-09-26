# K8s_Networking
Network Resources for K8s

# Pod 豌豆莢子

均擁有一獨立的 IP 位址，Pod 之間無關乎是否運行在同一個 Node (虛擬主機上)，均要求透過 Pods IP 與對方 IP 進行存取。
此優點在於使用者無需考慮建立 Pod 之間的連線，或是 containers port 到 Nodes port 的連接上。

此過程為 



           container port in PodB --- PodB IP ----------- PodA IP ---- container port in PodA
                                                   |
                                                   |
                                                   
                                                 Node Port
                                                 
                                                   |
                                                   |
                                                  
                                               Cluster IP
                                                   
                                                   

                           
