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

# Inicializador

1. Criar o arquivo de serviço
Abra o terminal e crie um novo arquivo de configuração para o systemd:
```
sudo vim /etc/systemd/system/valheim.service

```

2. Configurar o conteúdo do serviço
Cole o conteúdo abaixo, ajustando os caminhos conforme a sua instalação:

```
[Unit]
Description=Valheim Dedicated Server
After=network.target

[Service]
Type=simple
# Substitua 'ubuntu' pelo seu usuário do sistema
User=ubuntu
# Coloque o caminho completo para a pasta onde está o script
WorkingDirectory=/home/ubuntu/Steam
# Coloque o caminho completo para o arquivo .sh
ExecStart=/bin/bash /home/ubuntu/Steam/my_server.sh
Restart=on-failure
RestartSec=10
KillSignal=SIGINT

[Install]
WantedBy=multi-user.target
```

Dica Importante: Certifique-se de que o seu arquivo my_server.sh tenha permissão de execução. Se não tiver, rode: chmod +x /home/ubuntu/valheim_server/my_server.sh.

3. Ativar e Iniciar o Serviço
Agora, você precisa avisar o sistema que um novo serviço foi criado e habilitá-lo para o boot:

Recarregar o systemd:
```
sudo systemctl daemon-reload
```

Habilitar a inicialização automática:
```
sudo systemctl enable valheim.service
```

Iniciar o servidor agora:
```
sudo systemctl start valheim.service
```

4. Comandos Úteis de Gerenciamento

Agora que o Valheim é um serviço do sistema, você pode controlá-lo facilmente:

Ver status (se está online ou se deu erro):
```
sudo systemctl status valheim.service
```

Parar o servidor: 
```
sudo systemctl stop valheim.service
```

Reiniciar o servidor: 
```
sudo systemctl restart valheim.service
```

Ver os logs em tempo real: 
```
journalctl -u valheim.service -f
```
1. Criar o arquivo de serviço
Abra o terminal e crie um novo arquivo de configuração para o systemd:

Bash: sudo nano /etc/systemd/system/valheim.service

2. Configurar o conteúdo do serviço
Cole o conteúdo abaixo, ajustando os caminhos conforme a sua instalação:

```
[Unit]
Description=Valheim Dedicated Server
After=network.target

[Service]
Type=simple
# Substitua 'ubuntu' pelo seu usuário do sistema
User=ubuntu
# Coloque o caminho completo para a pasta onde está o script
WorkingDirectory=/home/ubuntu/Steam
# Coloque o caminho completo para o arquivo .sh
ExecStart=/bin/bash /home/ubuntu/Steam/my_server.sh
Restart=on-failure
RestartSec=10
KillSignal=SIGINT

[Install]
WantedBy=multi-user.target
```

Dica Importante: Certifique-se de que o seu arquivo my_server.sh tenha permissão de execução. Se não tiver, rode: chmod +x /home/ubuntu/valheim_server/my_server.sh.

3. Ativar e Iniciar o Serviço
Agora, você precisa avisar o sistema que um novo serviço foi criado e habilitá-lo para o boot:

Recarregar o systemd:
```
sudo systemctl daemon-reload
```

Habilitar a inicialização automática:
```
sudo systemctl enable valheim.service
```

Iniciar o servidor agora:
```
sudo systemctl start valheim.service
```

4. Comandos Úteis de Gerenciamento

Agora que o Valheim é um serviço do sistema, você pode controlá-lo facilmente:

Ver status (se está online ou se deu erro):
```
sudo systemctl status valheim.service
```

Parar o servidor: 
```
sudo systemctl stop valheim.service
```

Reiniciar o servidor: 
```
sudo systemctl restart valheim.service
```

Ver os logs em tempo real: 
```
journalctl -u valheim.service -f
```




