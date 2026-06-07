<div align="center">

<img src="https://img.shields.io/badge/Java-17-ED8B00?style=for-the-badge&logo=openjdk&logoColor=white"/>
<img src="https://img.shields.io/badge/Spring_Boot-4.0.6-6DB33F?style=for-the-badge&logo=spring-boot&logoColor=white"/>
<img src="https://img.shields.io/badge/MySQL-8.0-4479A1?style=for-the-badge&logo=mysql&logoColor=white"/>
<img src="https://img.shields.io/badge/Thymeleaf-3.x-005F0F?style=for-the-badge&logo=thymeleaf&logoColor=white"/>
<img src="https://img.shields.io/badge/Docker-ready-2496ED?style=for-the-badge&logo=docker&logoColor=white"/>

<br/><br/>

<h1>🛡️ Sentinela 0800</h1>
<p><strong>Plataforma de denúncias e consulta de números de telefone suspeitos</strong></p>
<p>Projeto A3 — Sistemas Distribuídos e Mobile · USJT 2026.1 · Parceria Bradesco</p>

</div>

---

## 📌 Sobre o Projeto

O **Sentinela 0800** é uma aplicação web desenvolvida como resposta ao crescimento dos golpes de falsas centrais de atendimento bancário — um dos tipos de fraude mais comuns no Brasil.

A plataforma permite que qualquer cidadão **denuncie um número de telefone suspeito** e **consulte se um número já foi reportado**, calculando automaticamente o nível de risco com base no histórico de denúncias registradas.

> Quanto mais denúncias um número acumula, maior o seu nível de risco — protegendo outras pessoas de cair no mesmo golpe.

---

## ✨ Funcionalidades

- 📋 **Registrar denúncia** — informe o número suspeito, tipo do golpe, estado e uma descrição do ocorrido
- 🔍 **Consultar número** — verifique se um número já foi denunciado e veja seu nível de risco
- 📊 **Dashboard de estatísticas** — total de denúncias, números bloqueados e percentual por tipo de golpe
- 🚦 **Classificação de risco automática** — Alto, Médio ou Sem denúncias, calculada em tempo real
- 💡 **Dicas de segurança** — orientações para identificar e evitar golpes de falsa central

---

## 🏗️ Arquitetura

```
┌─────────────────────────────────────────────────┐
│                   Navegador                     │
│                    (Thymeleaf)                  │
└───────────────────┬─────────────────────────────┘
                    │ HTTP
┌───────────────────▼─────────────────────────────┐
│              Spring Boot (MVC)                  │
│   Controllers → Services → Repositories (JPA)  │
└───────────────────┬─────────────────────────────┘
                    │ JPA / Hibernate
┌───────────────────▼─────────────────────────────┐
│                 MySQL 8.0                       │
│  usuario │ denuncia │ telefone_suspeito │ ...   │
└─────────────────────────────────────────────────┘
```

---

## 🛠️ Tecnologias Utilizadas

| Camada | Tecnologia | Descrição |
|---|---|---|
| Back-end | Java 17 + Spring Boot 4 | API e lógica de negócio |
| Front-end | Thymeleaf 3 + HTML/CSS | Templates server-side |
| Persistência | Spring Data JPA + Hibernate | ORM e acesso ao banco |
| Banco de Dados | MySQL 8.0 | Armazenamento relacional |
| Validação | Spring Validation | Validação de formulários |
| Utilitários | Lombok | Redução de boilerplate Java |
| Containerização | Docker + Docker Compose | Ambiente isolado e portável |
| Versionamento | Git + GitHub | Controle de versão |

---

## 🗄️ Modelo de Dados

```
usuario
├── id (PK)
├── nome
├── email (UNIQUE)
├── estado
└── criado_em

telefone_suspeito
├── id (PK)
├── numero (UNIQUE)
├── total_denuncias
└── bloqueado

denuncia
├── id (PK)
├── usuario_id (FK → usuario)
├── telefone_id (FK → telefone_suspeito)
├── tipo_golpe
├── descricao
├── prejuizo
├── status (NOVO | EM_ANALISE | CONFIRMADO | RESOLVIDO)
└── criado_em

estatistica
├── id (PK)
├── mes
├── ano
├── tipo_golpe
└── total
```

---

## 🚀 Como Executar

### Pré-requisitos

- [JDK 17+](https://adoptium.net/)
- [MySQL 8.0+](https://dev.mysql.com/downloads/)
- [Maven](https://maven.apache.org/) ou use o `./mvnw` incluso no projeto
- [Docker](https://www.docker.com/) *(opcional, para rodar em container)*

### 1. Clonar o repositório

```bash
git clone https://github.com/HenryGava/Bradesco-A3-USJT-2026.1.git
cd Bradesco-A3-USJT-2026.1
```

### 2. Criar o banco de dados

Execute o script SQL no seu MySQL:

```bash
mysql -u root -p < "Banco de Dados.sql"
```

Ou abra o arquivo `Banco de Dados.sql` no MySQL Workbench / Database Client (VS Code) e execute.

### 3. Configurar a conexão

Edite o arquivo `src/main/resources/application.properties`:

```properties
spring.datasource.url=jdbc:mysql://127.0.0.1:3306/Sentinela0800
spring.datasource.username=root
spring.datasource.password=SUA_SENHA_AQUI

spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=true
spring.thymeleaf.cache=false
```

### 4. Rodar a aplicação

```bash
./mvnw spring-boot:run
```

Acesse: **http://localhost:8080**

---

### 🐳 Executar com Docker *(diferencial)*

```bash
docker-compose up --build
```

A aplicação e o banco sobem automaticamente. Acesse: **http://localhost:8080**

---

## 📁 Estrutura do Projeto

```
src/
└── main/
    ├── java/com/sentinela/
    │   ├── controller/       # Controllers Spring MVC
    │   ├── model/            # Entidades JPA
    │   ├── repository/       # Interfaces Spring Data
    │   └── service/          # Regras de negócio
    └── resources/
        ├── templates/        # Páginas HTML com Thymeleaf
        │   ├── layout.html
        │   ├── index.html
        │   ├── denunciar.html
        │   └── consultar.html
        ├── static/css/       # Estilos
        └── application.properties
```

---

## 🚦 Lógica de Classificação de Risco

| Denúncias registradas | Nível de risco |
|---|---|
| 0 | 🟢 Sem denúncias |
| 1 a 4 | 🟡 Médio risco |
| 5 ou mais | 🔴 Alto risco |

---

## 👥 Equipe

| Nome | GitHub |
|---|---|
| Henry Gava | [@HenryGava](https://github.com/HenryGava) | RA : 825122158
| Arthur Frederico Piasse Pereira | [@ArthurPiasse](https://github.com/ArthurPiasse) | RA : 824219186
| Guilherme Nolasco Tucunduva | [@...](cole o link aq) | RA : 82426695
| Jhonattan da Costa Almeida | [@Jhon030308](https://github.com/Jhon030308) | RA : 82421914
| Pedro Henrique Carvalho Vieira | [@phcarvalho19](https://github.com/phcarvalho19) | RA : 824211592
| Sandy Andrade Pinho | [@SandyAndrade...](https://github.com/Sandy-andrade) | RA : 8261105964
| Gustavo Resende Fernandes | [@...](cole o link aq) | RA : 82414925
| Gustavo Almeida dos Santos Cunha  | [@...](cole o link aq) | RA : 824233519

---

## 📄 Licença

Este projeto foi desenvolvido para fins acadêmicos como parte da disciplina de **Sistemas Distribuídos e Mobile** da USJT, em parceria com o **Bradesco**, no semestre 2026.1.

---

<div align="center">
  <sub>Feito com ☕ Java e muito cuidado · USJT 2026.1</sub>
</div>
