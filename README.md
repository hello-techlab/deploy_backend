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
O projeto foi planejado mirando uma arquitetura divida em microsserviços, para que cada funcionalidade fosse o mais desacoplada quanto possível das outras. Portanto, para iniciar o sistema é necessário clonar todos os repositórios da aplicação, e inserir alguns arquivos de configuração a mais.

O aplicativo exibido foi deployado numa instância de máquina virtual do Google Cloud Platform. Este procedimento de deploy supõe que se tem acesso à uma instância com IP público, e que o nome de domínio http://techlab-oauth.mooo.com/ é redirecionado para este IP público.

## 2.1 - Clonando os repositórios

Todos os repositórios devem ser clonados no mesmo nível desde repositório deploy_backend.

```
cd /local/do/deploy
git clone https://github.com/techlabxplus/deploy_backend
git clone https://github.com/techlabxplus/techlab_app
git clone https://github.com/techlabxplus/nginx
git clone https://github.com/techlabxplus/bd
git clone https://github.com/techlabxplus/node-authentication-gateway
git clone https://github.com/techlabxplus/api-emails
git clone https://github.com/techlabxplus/servico_agendamento
git clone https://github.com/techlabxplus/servico_questionarios
```

## 2.2 - Inserindo arquivos sensíveis

Será preciso inserir manualmente alguns arquivos de configuração, que não foram incluídos nos repositórios por conterem dados sensíveis. 

- node-authentication-gateway: arquivos ".env" e ".env.example", configurando as credenciais de autenticação para o Google.

- api-emails: arquivos ".env" e ".env.example", configurando as credenciais do email utilizado para envio das mensagens executado pelo aplicativo.

- servico_agendamento: arquivo "credentials.json", configurando as credenciais que o aplicativo precisa para acessar a agenda Google dos atendentes. 

Mais informações são fornecidas no README de cada um destes repositórios. Caso precise de mais informações, contate a equipe de desenvolvimento através do email leonardo.prati@usp.br


## 2.3 - Primeira execução 

#### 1. Para executar os containers da aplicação: 

```
cd /local/do/deploy/deploy_backend
sudo docker-compose up --build -d
```

#### 2. Na primeira inicialização, o banco de dados estará vazio. É necessário carregar o script de inicialização do banco.

#### 3. Acesse http://techlab-oauth.mooo.com/database_adminer.

#### 4. Preencha os campos da seguinte maneira:

```
System: PostgreSQL
Server: database
Username: postgres
Password: Consulte a equipe de desenvolvimento
Database: Deixe este campo em branco
```

#### 5. Na tela de administração, selecione "Create database".

#### 6. Na tela "Create database", nomeie o novo banco como "smBD" e clique em "save".

#### 7. Na tela de administração, clique no link nome "smBD".

#### 8. Na tela de schema, selecione a opção "import". Faça upload do arquivo "script.sql", contido no repositório "bd", e execute.

#### 9. Uma mensagem informando que as queries foram executadas com sucesso será exibida.

#### 10. Agora é preciso reiniciar o aplicativo:

```
cd /local/do/deploy/deploy_backend
sudo docker-compose down
sudo docker-compose up -d
```

Pronto! O sistema está pronto para utilização!
