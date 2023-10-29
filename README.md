# sprint2.docker
## Desafio 1
### Restaurar e instalar o docker na VM01

Para restaura a sua VM, você pode fazer de duas maneiras:<br>
1- Abra o seu Virtual Box
Selecione a sua VM com o botão direito, selecione exibir no explorer (H), você vai estar dentro da pasta da sua Vm
Suba um nível e saia da pasta, selecione a sua Vm novamente com o botão direito,  copie e cole ela no mesmo local
Assim vc terá uma copia<br>
2- faça um clone da sua VM

Instalando o Docker em sua Vm da Oracle Linux

https://docs.docker.com/engine/install/centos/

Caso você esteja fazendo isso em uma vm que acabou de criar, não será preciso remover versões anteriores.
Em caso de dúvida digite o comando abaixo:<br>

```bash
sudo yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-engine
```

Para instalar o Docker em sua máquina virtual, antes você precisa configurar o repositório Docker com o

```bash
sudo yum install -y yum-utils
```

assim conseguimos instalar o pacote

```bash
yum-config-manager
```

para instalar o repósitorio do Docker

```bash
sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
```


Querendo, pode habilitar os repositórios de teste com o comando abaixo

```bash
sudo yum-config-manager --enable docker-ce-nightly
```

Instale os serviços do Docker

```bash
sudo yum install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

Dê um start no serviço

```bash
sudo systemctl start docker
```
reset a máquina

```bash
sudo systemctl enable Docker
```
  
## Desafio 2
### Passos para Instalar o PostgreSQL no Oracle Linux

Faça o download da imagem

```bash
docker pull postgres
```
Verifique a versão

```bash
psql --version
```

1. **Persistência de Dados:** 

Para rodar a imagem do PostgreSQL e persistir os dados do banco em um volume Docker, você pode seguir os seguintes passos:

1. **Criar um Volume Docker:**

Primeiro, crie um volume Docker onde os dados do banco de dados serão armazenados. Execute o seguinte comando:

```bash
docker volume create nome_volume
```
  
Substitua `nome_do_volume` pelo nome que você deseja para o volume.

2. **Rodar a Imagem do PostgreSQL com o Volume:**

Em seguida, você pode rodar a imagem do PostgreSQL e configurá-la para usar o volume Docker que você acabou de criar:

  ```bash
  docker run --name meu-postgres -e POSTGRES_PASSWORD=sua_senha -v nome_do_volume:/var/lib/postgresql/data -d postgres
  ```
Certifique-se de substituir `sua_senha` pela senha que você deseja para o superusuário do PostgreSQL e `nome_do_volume` pelo nome do volume que você criou.
Agora, os dados do seu banco de dados PostgreSQL serão armazenados no volume Docker.<br> Esse método garante que os dados persistam mesmo que o contêiner PostgreSQL seja removido ou atualizado.

## Desafio 3
### Instalando o docker no linux, criando uma imagem do wordpress com banco de dados e persistindo os dados usando docker-compose

Crie um diretório

```bash
mkdir diretório
```

entre no diretório 

```bash
cd diretório
```

Nesse diretório vamos criar o arquivo docker-compose.yml

```bash
touch docker-compose.yml
```

agora vamos entrar no arquivo com o nano

```bash
nano docker-compose.yml
```

E adicionar as configurações:

```bash
version: '3'
services:
  db:
    image: mysql:latest
    environment:
      MYSQL_ROOT_PASSWORD: example_password
      MYSQL_DATABASE: wordpress
      MYSQL_USER: wordpress
      MYSQL_PASSWORD: wordpress_password
    volumes:
      - db_data:/var/lib/mysql

  wordpress:
    image: wordpress:latest
    ports:
      - "80:80"
    environment:
      WORDPRESS_DB_HOST: db
      WORDPRESS_DB_USER: wordpress
      WORDPRESS_DB_PASSWORD: wordpress_password
      WORDPRESS_DB_NAME: wordpress
    depends_on:
      - db

volumes:
  db_data:
```

Inserido as configurações, salve e saia do arquivo<br>

Ctrl+o para salvar <br>

Crtl+x para sair <br>

Agora vamos iniciar o docker-compose.yml

```bash
docker-compose up -d
```

Para verificar a sua instalação, acesse o localhost com o seu ip e a porta que foi disponibilizada<br>

Ex.: http://seu-ip:80



