# Setup Guide

# Prerequisites

- Docker desktop https://www.docker.com/products/docker-desktop/

# Exporters implementation (On Edge)

- Nvidia Gpu Exporter
    - run the `nvidia_gpu_exporter.exe`
    - Default port: 9835
- ping_exporter
    
    [prometheus的网络ping监控exporter_姚__的博客-CSDN博客](https://blog.csdn.net/qq_32969313/article/details/124878153)
    
    - run command: `main.exe -port 8888 -pingaddr yourIP -count 4`
        - fill the target IP address in.
    - Default port: 8888
- windows_exporter
    - run the`windows_exporter.exe`
    - Default port: 9182

# Prometheus(On linux)

- In the central server, run:`docker run -p 9090:9090 prom/prometheus`
    
    ### Modify the config & the target & getting IP
    
    - Open prometheus.yml in docker Files/etc
    - Replace it by prometheus.yml prepared in the repository
    - Replace the IP & port number with according exporters
    
    ### EX:
    
    ```yaml
      - job_name: "prometheus"
        static_configs:
          - targets: ["localhost:9090"]
          
      - job_name: NVIDIA
        static_configs:
        
          - targets: ['192.168.4.167:9835'] #we need the ip address of the edge and the port number to have the exporter.
    ```
    
- back to the [localhost:9090](http://localhost:9090) we should be able to see the exporter is up
    
    ![Screenshot 2023-07-19 at 1.11.54 PM.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/845865e2-1621-40d9-9c10-ec56b24ce9ff/Screenshot_2023-07-19_at_1.11.54_PM.png)
    

---

# Grafana Setup

- Open Docker Desktop
- Run command: `docker run -d --name=grafana -p 3000:3000 grafana/grafana`
    - A container and image will appear in docker.
- visit `“localhost:3000”`
    - type `admin`  for account and password.
    
    ### Add data source
    
    1. click the three stripe button → Connections →  Add new connection → Prometheus → create a prometheus data source
    2. prometheus server url: [`http://host.docker.internal:9090/`](http://host.docker.internal:9090/)  
    3. click `save & test`
    
    ### Import the JSON file
    
    - Click the + on top of the page → import a dashboard
    - Upload the XRCUBE.json file.
