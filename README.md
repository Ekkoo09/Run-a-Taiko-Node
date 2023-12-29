# Run-a-Taiko-Node

You can establish your node by either configuring it on a personal computer or by utilizing a Virtual Private Server (VPS), which is well-suited for managing websites, applications, or other internet-based services, including nodes.

For my setup, I selected [Contabo](https://contabo.com/en/), a well-regarded provider of VPS rentals. Opting for a plan with at least 100 GB of storage is advisable to ensure sufficient space for long-term node operation.

My suggestion would be to go for the Cloud VPS M option. [https://contabo.com/en/](https://contabo.com/en/)

![Contabo VPS Options](https://github.com/Ekkoo09/Taiko-Node/blob/main/images/Contabo1.png?raw=true)

1. Select Payment Term 
2. Region - Germany
3. Storage Type -400 GB SSD
4. Image - Go to Apps & Panels > Docker

![Contabo Image](https://github.com/Ekkoo09/Taiko-Node/blob/main/images/Contabo2.png?raw=true)

5. Log in & Password for your Server

![Contabo login](https://github.com/Ekkoo09/Taiko-Node/blob/main/images/Contabo3.png?raw=true)

6. Object Storage - Default
7. Networking - Default
8. Add-Ons - Default

After finalizing your payment, you'll get an email verifying your purchase. Hold tight for a follow-up email, which will provide details about your VPS, encompassing your access credentials.

You'll need to download and use Putty, a tool that enables you to securely connect to your VPS and utilize its functionalities.
Download it here [https://www.putty.org/](https://www.putty.org/)

![Putty login](https://github.com/Ekkoo09/Taiko-Node/blob/main/images/Putty1.png?raw=true)

Enter the VPS IP Address then Click OPEN

Login : root

Password: (is the one you set earlier on Contabo VPS) 

![Root Password](https://github.com/Ekkoo09/Taiko-Node/blob/main/images/Putty2.png?raw=true)

1. Install essential components 
    
    Before setting up the node it is important to update your VPS
    
    ```bash
    sudo apt-get update && sudo apt-get upgrade -y
    ```
    
    Install Essential Codes
    
    ```bash
    sudo apt install pkg-config curl git-all build-essential libssl-dev libclang-dev ufw
    ```
    
    Reply Y and proceed
    
2. Check if Docker is up-to-date
    
    ```bash
    docker version
    ```
    
    If docker is not installed, run the following command:
    
    ```bash
    sudo apt-get install ca-certificates curl gnupg lsb-release
    ```
    
    Now add Docker’s official GPG key:
    
    ```bash
    sudo mkdir -p /etc/apt/keyrings
    curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
    ```
    
    proceed. . . ..
    We Are to set up Repository: (just copy and paste)
    
    ```bash
    echo \
      "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
      $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
    ```
    
    Let’s grant Docker file permission just in case, before updating the package index
    
    ```bash
    sudo chmod a+r /etc/apt/keyrings/docker.gpg
    sudo apt-get update
    ```
    
    after granting, updated index, Install latest version of docker
    
    ```bash
    sudo apt-get install docker-ce docker-ce-cli containerd.io docker-compose-plugin
    ```
    
    Reply with Y and press ENTER
    Now install Docker compose :
    
    ```bash
    sudo apt install docker-compose
    ```
    
    reply with Y
    
    Lets verify the **ENGINE INSTALLATION** is successful by runinng the `hello-world`
    
    ```bash
    sudo docker run hello-world
    ```
    
    You should get this.
    
    ![Hello World](https://github.com/Ekkoo09/Taiko-Node/blob/main/images/Putty3.png?raw=true)
    
    Now check Docker compose version :
    
    ```bash
    docker-compose -v
    ```
    
    Lets Install SCREEN
    
    ```bash
    sudo apt install screen
    ```
    
    Create new screen session
    
    ```bash
    sudo screen -S taiko
    ```
    
    **********************************NODE INSTALLATION**********************************
    
    1. Download Node
        
        ```bash
        git clone https://github.com/taikoxyz/simple-taiko-node.git
        cd simple-taiko-node
        ```
        
    2. Node Configuration
        
        ```bash
        cp .env.sample .env
        ```
        
    3. Edit Configuration
        
        ```bash
        nano .env
        ```
        
        ```bash
        Sample:
        < L1_ENDPOINT_HTTP=https://rpc.sepolia.org/
        < L1_ENDPOINT_WS=wss://ethereum-sepolia.blockpi.network/v1/ws/56f9eaZZZZZZZZZZZZZZZZZZZZZZZZZZ
        < ENABLE_PROVER=false
        < L1_PROVER_PRIVATE_KEY=f3982a81dc1372d731b6YYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYY
        < PROVE_BLOCK_TX_GAS_LIMIT=2500000
        < MIN_ACCEPTABLE_PROOF_FEE=10
        < ENABLE_PROPOSER=true
        < L1_PROPOSER_PRIVATE_KEY=9125dbab06f560546688b7a5f1551cdd36bbXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
        < L2_SUGGESTED_FEE_RECIPIENT=0xE2fa3e074b9A804cb082DE23dMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMM
        < PROPOSE_BLOCK_TX_GAS_TIP_CAP=20000000000
        < BLOCK_PROPOSAL_FEE=10
        < PROVER_ENDPOINTS=
        ```
        
    4. Start the Node
        
        ```bash
        docker compose up
        ```
        
    5. Check Logs
        
        ```bash
        docker compose logs -f
        ```
        
    6. Wait at least 12hours to sync with the current block
        
        View Grafana Dashboard to see status
        
        ```bash
        open http://localhost:3001/d/L2ExecutionEngine/l2-execution-engine-overview
        ```
   You can verify that your node is syncing by checking that the chain head on the dashboard (see below) is increasing. Once the chain head matches what's on the block explorer, you are fully synced.
   ![Grafana](https://raw.githubusercontent.com/Ekkoo09/Taiko-Node/main/images/node-dashboard.webp?token=GHSAT0AAAAAACLYD3XQBY5SUNSTDUPDEQLSZMOT5KA)
