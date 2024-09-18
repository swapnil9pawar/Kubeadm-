RUN THIS COMMAND IN MASTER NODE & wORKER NODE
      sudo swapoff -a
      # sysctl params required by setup, params persist across reboots
      cat <<EOF | sudo tee /etc/sysctl.d/k8s.conf
      net.ipv4.ip_forward = 1
      EOF

      # Apply sysctl params without reboot
      sudo sysctl --system
      sysctl net.ipv4.ip_forward
      sudo apt-get update
      sudo apt-get install ca-certificates curl
      sudo install -m 0755 -d /etc/apt/keyrings
     sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
     sudo chmod a+r /etc/apt/keyrings/docker.asc
     echo   "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
     $(. /etc/os-release && echo "$VERSION_CODENAME") stable" |   sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
     sudo apt-get update
     sudo apt-get install containerd.io
     containerd config default > /etc/containerd/config.toml  AFTER EDIT THE config.toml AND SEARCH LINE --> SystemdCgroup = False  insted of False change it nto true
    vi /etc/containerd/config.toml
     sudo systemctl restart containerd
     sudo apt-get update
     sudo apt-get install -y apt-transport-https ca-certificates curl gpg
     curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.31/deb/Release.key | sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg
     echo 'deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.31/deb/ /' | sudo tee /etc/apt/sources.list.d/kubernetes.list
     sudo apt-get update
     sudo apt-get install -y kubelet kubeadm kubectl
     sudo apt-mark hold kubelet kubeadm kubectl
     kubeadm init --apiserver-advertise-address 10.139.168.11 --pod-network-cidr 10.244.0.0/16 --cri-socket unix:///var/run/containerd/containerd.sock [USE THIS COMMAND ON ONLY MASTER NODE. INSTED OF 10.139.168.11 THIS IP YOU SHOULD USE YOUR SERVER MASTER NODE PRIVATE IP]
     AFTER HITING 30 NUMBER COMMAND YOU WILL APREAR SOME COMMAND AND TOKEN (mkdir -p $HOME/.kube && sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config &&sudo chown $(id -u):$(id -g) $HOME/.kube/config AND TOKEN LIKE kubeadm init --apiserver-advertise-address 10.139.168.11 --pod-network-cidr 10.244.0.0/16 --cri-socket unix:///var/run/containerd/containerd.sock ONLY TOKEN PASTE ON WORKER NODE OTHER COMMAND WILL PASTE ONLY ON MASTER NODE 
     kubectl apply -f https://reweave.azurewebsites.net/k8s/v1.30/net.yaml
     kubectl get node

