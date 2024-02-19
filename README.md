# Install-Union-node
Install Union Node on Ubuntu 22.04

** 1.Install Docker
**
```
sudo apt update 
sudo apt install apt-transport-https ca-certificates curl software-properties-common
```
```
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
```
```
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```
```
sudo apt update
sudo apt-cache policy docker-ce
sudo apt install docker-ce
sudo systemctl status docker
```
![1docer](https://github.com/DinhCongTac221/Install-Union-node/assets/27664184/4677ffc9-59f8-40ca-a1b9-f4c3d513d8a5)

Ctrl+C to exit the screen

** 2.Install Docker Compose on Ubuntu 22.04**

```
sudo mkdir -p ~/.docker/cli-plugins/
curl -SL https://github.com/docker/compose/releases/download/v2.3.3/docker-compose-linux-x86_64 -o ~/.docker/cli-plugins/docker-compose
sudo chmod +x ~/.docker/cli-plugins/docker-compose
sudo docker compose version
```

** 3.Intall Unionvisor
**
```
sudo docker pull ghcr.io/unionlabs/bundle-testnet-6:v0.19.0
```
```
sudo mkdir ~/.unionvisor
```
Change DinhCongtact221 to your node name
```
sudo docker run \
  --volume ~/.unionvisor:/.unionvisor \
  --volume /tmp:/tmp \
  -it ghcr.io/unionlabs/bundle-testnet-6:v0.19.0 \
  init --moniker DinhCongtact221 \
  --network union-testnet-6 \
  --seeds "b37de4c50e26f7cde4c7b6ce06046a6693ffef2c@union.testnet.4.seed.poisonphang.com:26656"
  ```
  
  Create and open compose.yaml file
  ```
sudo touch compose.yaml
sudo  nano compose.yaml
```
Copy and paste into it
```
services:
  node:
    image: ghcr.io/unionlabs/bundle-testnet-6:v0.19.0
    volumes:
      - ~/.unionvisor:/.unionvisor
      - /tmp:/tmp
    ports:
      - "26657:26657"
      - "26656:26656"
      - "1317:1317"
      - "9093:9093"
    logging:
      driver: "json-file"
      options:
        max-size: "20m"
    restart: unless-stopped
    command: run --poll-interval 1000

```
Ctrl +x, Y, enter to save it
![3compose](https://github.com/DinhCongTac221/Install-Union-node/assets/27664184/63de2ff8-76e9-4d36-a54e-f97ed7dfd9af)



** 4.open port
**
```
sudo ufw enable
sudo ufw allow 26657
sudo ufw allow 26656
sudo ufw allow 1317
sudo ufw allow 9093
sudo ufw status
```

![4 port](https://github.com/DinhCongTac221/Install-Union-node/assets/27664184/d2efd1f5-cd84-45ff-86ba-b9e204ee2a52)

** 5.Run Your Union Node
**

```
sudo COMPOSE_FILE=/root/compose.yaml docker compose up -d
```
![5run](https://github.com/DinhCongTac221/Install-Union-node/assets/27664184/1da0ba62-8581-45ab-a821-103b0482f54f)


To check logs of your node:
```
sudo docker ps -a
sudo docker logs -f containerID
```

![6 dockerlogc](https://github.com/DinhCongTac221/Install-Union-node/assets/27664184/59b9544a-25d4-4b49-b790-7874ba36fdbd)
![7 running](https://github.com/DinhCongTac221/Install-Union-node/assets/27664184/cc62a995-042b-4f90-9c3d-d23d776e0669)


Ctrl +C to exit running screen

** 6.Install Uniond
**
```
sudo alias uniond='docker run -v ~/.unionvisor:/.unionvisor -v /tmp:/tmp --network host -it ghcr.io/unionlabs/uniond-release:v0.19.0 --home /.unionvisor'
```
** 7.Create wallet: 
**
```
sudo uniond keys add nameofyourwallet
```
typer password of your wallet
![8password](https://github.com/DinhCongTac221/Install-Union-node/assets/27664184/a9e1bd06-4d4a-4ba7-b802-31a66aae4043)

Copy address and save Mnemonic
![xong](https://github.com/DinhCongTac221/Install-Union-node/assets/27664184/0525a7c7-0133-40aa-8471-95f719db313d)

if the guide help you, please give me a follow: https://twitter.com/DinhcongTac221
Thanks
