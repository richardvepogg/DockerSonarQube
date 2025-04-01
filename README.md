# SonarQube com SQL Server via Docker Compose

Este repositório contém um arquivo `docker-compose.yml` para iniciar o SonarQube com um banco de dados SQL Server, facilitando a análise de qualidade de código em um ambiente isolado.

## Pré-requisitos

* Docker e Docker Compose instalados.

## Como usar

1.  Clone este repositório:

    ```bash
    git clone [https://github.com/richardvepogg/DockerSonarQube]
    cd [NOME_DO_DIRETÓRIO_DO_REPOSITÓRIO]
    ```

2.  Execute o Docker Compose:

    ```bash
    docker-compose up -d
    ```

    Este comando irá baixar as imagens necessárias e iniciar os contêineres do SonarQube e do SQL Server.

3.  Acesse o SonarQube no navegador:

    * Abra `http://localhost:9000` no seu navegador.
    * Faça login com as credenciais padrão: `admin` / `admin`. (Recomenda-se alterar a senha após o primeiro login).

4.  Acesse o SQL Server no navegador ou via ferramentas de banco de dados.
    * A porta 1433 está mapeada para localhost, mas o acesso direto pelo navegador não é feito.
    * Use ferramentas de banco de dados como Azure Data Studio, SQL Server Management Studio, etc.
    * Conecte-se a `localhost,1433` com o usuário `sa` e a senha `senha123`.

## Configuração

O arquivo `docker-compose.yml` define os seguintes serviços:

* **sonarqube:**
    * Utiliza a imagem `sonarqube:lts-community`.
    * Depende do serviço `db` (SQL Server).
    * Configura a conexão com o banco de dados SQL Server usando as variáveis de ambiente `SONAR_JDBC_URL`, `SONAR_JDBC_USERNAME` e `SONAR_JDBC_PASSWORD`.
    * Expõe a porta `9000` para acesso ao SonarQube.
    * Utiliza volumes persistentes (`sonar_data` e `sonar_logs`) para armazenar dados e logs.
* **db:**
    * Utiliza a imagem `mcr.microsoft.com/mssql/server:2019-latest`.
    * Configura o SQL Server com a aceitação do EULA (`ACCEPT_EULA`) e a senha do usuário `sa` (`SA_PASSWORD`).
    * Expõe a porta 1433 para conexão ao banco de dados.
    * Utiliza volume persistente (`sonar_db`) para armazenar os dados do banco.
* **networks:**
    * Cria uma rede bridge chamada `sonar_net` para comunicação entre os contêineres.
* **volumes:**
    * Define volumes persistentes para os dados do SonarQube (`sonar_data` e `sonar_logs`) e do SQL Server (`sonar_db`).

## Observações

* **Segurança:** A senha `senha123` é usada para fins de demonstração. Em um ambiente de produção, altere a senha para um valor seguro.
* **Volumes:** Os volumes persistentes garantem que os dados não sejam perdidos ao reiniciar os contêineres.
* **Rede:** A rede `sonar_net` permite que os contêineres se comuniquem entre si.
