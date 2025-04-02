# SonarQube com PostgreSQL via Docker Compose

Este repositório contém um arquivo `docker-compose.yml` para configurar um ambiente SonarQube com um banco de dados PostgreSQL, ambos em contêineres Docker.

## Pré-requisitos

* Docker instalado
* Docker Compose instalado

## Como usar

1.  **Clone este repositório:**

    ```bash
    git clone <URL_DO_SEU_REPOSITÓRIO>
    cd <NOME_DO_REPOSITÓRIO>
    ```

2.  **Inicie os contêineres Docker:**

    ```bash
    docker-compose up -d
    ```

    Este comando irá baixar as imagens necessárias (SonarQube e PostgreSQL) e iniciar os contêineres em modo detached (em segundo plano).

3.  **Acesse o SonarQube:**

    Após a inicialização dos contêineres, você pode acessar o SonarQube em seu navegador web através do seguinte endereço:

    ```
    http://localhost:9000
    ```

    O login padrão é:

    * Usuário: `admin`
    * Senha: `admin`

    **Importante:** Altere a senha padrão após o primeiro login por motivos de segurança.

## Estrutura do `docker-compose.yml`

O arquivo `docker-compose.yml` define dois serviços:

* **`sonarqube`:**
    * Utiliza a imagem `sonarqube:lts-community`.
    * Depende do serviço `db` (PostgreSQL) para iniciar.
    * Configura a conexão JDBC para o PostgreSQL usando variáveis de ambiente:
        * `SONAR_JDBC_URL`: `jdbc:postgresql://db:5432/sonar`
        * `SONAR_JDBC_USERNAME`: `sonar`
        * `SONAR_JDBC_PASSWORD`: `sonar`
    * Mapeia a porta 9000 do contêiner para a porta 9000 do host.
    * Monta volumes para dados e logs do SonarQube.
* **`db`:**
    * Utiliza a imagem `postgres:13`.
    * Configura o PostgreSQL com variáveis de ambiente:
        * `POSTGRES_USER`: `sonar`
        * `POSTGRES_PASSWORD`: `sonar`
        * `POSTGRES_DB`: `sonar`
    * Mapeia um volume para os dados do banco de dados.
* **`sonar_net`:**
    * Cria uma rede bridge para comunicação entre os contêineres.
* **Volumes:**
    * Define volumes nomeados para persistência de dados.

## Limpeza

Para parar e remover os contêineres e volumes:

```bash
docker-compose down -v
