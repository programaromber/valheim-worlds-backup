# valheim-worlds-backup
Como criar um servidor dedicado do valheim na aws

Requisitos:
- Conta AWS
- Máquina virtual linux (Ubuntu, mais barata)


## AWS EC2
Primeiro passo é criar a instancia ec2 na aws seguindo esse tutorial:
- https://www.youtube.com/watch?v=ZkLAz8zrKjM
- Lembrar de  criar a sua credencial putty para acessar o console

## Criar instancia UBUNTU
- t3.medium
- 8GB SSD
- Criar uma Security Group com regras TCP e UDP nas portas `2456-2458` (Portal do server)
- Após fazer a criação e execução da instancia você precisa configurar o servidor no ubunto.

## Configuração da máquina Ubuntu
Comandos:
```
sudo apt update
sudo apt -y dist-upgrade
sudo apt-get -y install lib32gcc-s1
sudo apt-get -y install libsdl2-2.0-0
sudo apt-get install -y ca-certificates libpulse-dev libatomic1
sudo add-apt-repository multiverse
sudo dpkg --add-architecture i386
sudo apt-get update
sudo apt-get install -y steamcmd
cd /home/ubuntu/Steam/
steamcmd +force_install_dir /home/ubuntu/Steam +login anonymous +app_update 896660 validate +exit
- /home/ubuntu/Steam/steamcmd.sh +login anonymous +force_install_dir /home/ubuntu/Steam +app_update 896660 validate +exit
cp start_server.sh my_start_server.sh
screen -S scr
vim my_start_server.sh
- editar o nome do server e senha
./my_start_server.sh
```

## Restaurar Backup

Pasta do mundo no linux:
```
cd /home/ubuntu/.config/unity3d/IronGate/Valheim
git clone https://github.com/programaromber/valheim-worlds-backup.git
cd valheim-worlds-backup
```

Copiar os arquivos do mundo para a pasta destino:
```
cp [nome_arquivo] ../
```
- Repetir para cada arquivo.

# Backup

Comandos:
```
cd /home/ubuntu/.config/unity3d/IronGate/Valheim/worlds_local
git add .
git commit -m "backup"
git push -u origin main
```
login do github
- username
- senha token classico




