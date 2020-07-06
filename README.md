# Instalação Docker e deploy
O seguinte repositório contém os scripts para instalação do docker e docker-compose em uma máquina Ubuntu. Adicionalmente é explicada a maneira de fazer o deploy para execução dos containers da aplicação.

# 1 - Instalação

## docker
Para instalar o docker:
```
sudo chmod +x install_docker_ce.sh
sudo ./install_docker_ce.sh
```

## docker-compose
Para instalar o docker-compose:
```
sudo chmod +x install_docker_compose.sh
sudo ./install_docker_compose.sh
```

# 2 - Deploy
Para executar os containers da aplicação

```
docker-compose up --build -d
```

## Referência
O procedimento de instalação utilizado foi aquele descrito [aqui](https://docs.docker.com/install/linux/docker-ce/ubuntu/) (docker-ce) e [aqui](https://docs.docker.com/compose/install/) (docker-compose).root@deploy:/deploy/deploy_backend# 

