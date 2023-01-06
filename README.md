## Mig-Parted Installation
#### First clone the repo
`git clone https://github.com/NVIDIA/mig-parted.git`
#### Install mig-parted lib
`wget https://github.com/NVIDIA/mig-parted/releases/download/v0.5.0/nvidia-mig-manager_0.5.0-1_amd64.deb`<br>
`sudo dpkg -i nvidia-mig-manager_0.5.0-1_amd64.deb`

#### Change config file in the mig-parted repo`
`nano mig-parted/examples/config.yaml`<br>
In the config.yaml file, change the custom-config to following lines.
```
custom-config:
    - devices: [0,1,2,3] # Mig-parted will not be applied on these gpus
      mig-enabled: false
    - devices: [4,5,6,7] # Mig-parted will be applied on these gpus
      mig-enabled: true
      mig-devices:
        "3g.20gb": 2 
```

#### Reset all the gpus. Make sure all the processes on gpus are closed
`sudo nvidia-smi --gpu-reset`
#### Apply the mig config
`sudo nvidia-mig-parted apply -f examples/config.yaml -c custom-config`
#### This command will print the applied config
`sudo nvidia-mig-parted export`
#### Assert the config
`sudo nvidia-mig-parted assert -f examples/config.yaml -c custom-config`
#### List all the gpus to see mig gpus id
`sudo nvidia-smi –L`
#### If reseting gpus give an error then you can list the process used by nvidia and kill them. This part is required for some A100 machine but not others.
`sudo fuser -v /dev/nvidia*`
`sudo kill -1 1222 (dont kill fabric manager kill rest, 1222 is PID number)`



