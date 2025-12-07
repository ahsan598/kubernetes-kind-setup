# 📘 Multipass – Essential Commands Cheat Sheet

### 📦 Install Multipass
```sh
# Windows (winget)
winget find multipass
winget install canonical.multipass

# Windows (chocolatey)
choco install multipass -y

# verify multipass version
multipass version
```

### Register the VirtualBox backend
```sh
multipass set local.driver=virtualbox
multipass set local.virtualbox.use_gui=true

# verify backend enabled
multipass get local.driver
```


---

### ▶️ Launch an Instance
```sh
multipass launch                        # default Ubuntu VM
multipass launch --name vm1             # launch with name
multipass launch 22.04 --name test      # specific Ubuntu version

multipass launch --cpus 2 --mem 4G --disk 20G --name dev
```

### 📋 List Instances
```sh
multipass list
multipass shell vm1                     # get shell access
multipass info vm1
```

### 🔄 Instance Lifecycle
```sh
multipass start vm1
multipass stop vm1
multipass restart vm1

multipass suspend vm1
multipass resume vm1

multipass delete vm1                # mark as deleted
multipass purge                     # remove deleted instances completely
```


### 🔧 Execute Commands in VM
```sh
multipass exec vm1 -- ls -l /
multipass exec vm1 -- sudo apt update
```


### 📂 Mount a Local Directory Inside VM
```sh
multipass mount C:\work vm1:/home/ubuntu/work
multipass mount /home/user/app vm1:/app

multipass unmount vm1:/app
```


### 🌐 Networking Commands
```sh
multipass info vm1
multipass info vm1 | grep IPv4              # get IP address

multipass info --all
```


### 📦 Image Management
```sh
multipass find

multipass launch docker --name docker-vm            # Launch using found image
```


### File Transfers
```sh
multipass transfer vm1:/home/ubuntu/output.txt .            # Host → VM

multipass transfer vm1:/home/ubuntu/output.txt .            # VM → Host
```


### 🛠️ Configuration & Debug
```sh
multipass logs vm1
multipass version

# Factory Reset
multipass set local.driver=hyperv               # example: change driver
multipass stop --all
multipass delete --all
multipass purge
```


### 📑 Useful Examples
```sh
multipass launch --name dev --cpus 4 --mem 8G --disk 40G 22.04


multipass launch docker --name docker-host
multipass shell docker-host
```