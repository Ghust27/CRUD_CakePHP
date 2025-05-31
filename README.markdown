# CakePHP CMS – Gestão de Categorias e Produtos

Um sistema de gerenciamento de **Categorias** e **Produtos** desenvolvido em CakePHP. Este projeto demonstra operações básicas de CRUD (Create, Read, Update, Delete) para duas entidades principais: **Categories** e **Products**.

---

## 📋 Sumário

- [Tecnologias Utilizadas](#tecnologias-utilizadas)  
- [Pré-requisitos](#pré-requisitos)  
- [Instalação](#instalação)  
- [Configuração do Banco de Dados](#configuração-do-banco-de-dados)  
- [Estrutura das Tabelas (SQL)](#estrutura-das-tabelas-sql)  
- [Executando o Servidor de Desenvolvimento](#executando-o-servidor-de-desenvolvimento)  
- [Utilização (CRUD)](#utilização-crud)  


---

## 🛠 Tecnologias Utilizadas

- **PHP** (>= 7.2)  
- **CakePHP** v4.x  
- **Composer** (para gerenciar dependências)  
- **MySQL** como banco de dados  
- **CakePHP ORM** para mapeamento objeto-relacional  
- **Bootstrap** (usado nos templates gerados pelo bake)  

---

## ⚙️ Pré-requisitos

Antes de rodar o projeto localmente, certifique-se de ter instalado:

1. **PHP 7.2+**, com extensões:
   - `pdo_mysql`
   - `mbstring`
   - `intl` (recomendado)
   - `openssl`, `ctype`, `json` (geralmente já disponíveis)

2. **Composer** (https://getcomposer.org)  
3. **MySQL** rodando localmente ou em servidor acessível  
4. (Opcional) Um cliente SQL (MySQL Workbench, phpMyAdmin, DBeaver etc.) para executar scripts SQL  

---

## 📥 Instalação

1. **Clone** este repositório (ou baixe o ZIP/RAR e extraia para uma pasta local):  
   ```bash
   git clone https://github.com/Ghust27/CRUD_CakePHP.git
   cd cms
   ```

2. Instale as dependências via Composer:
   ```bash
   composer install
   ```

3. Copie o arquivo de configuração de ambiente e ajuste as credenciais:
   ```bash
   cp config/app_local.example.php config/app_local.php
   ```

4. Abra `config/app_local.php` e configure a conexão com o banco de dados:
   ```php
   'Datasources' => [
       'default' => [
           'host'      => 'localhost',
           'username'  => 'seu_usuario_mysql',
           'password'  => 'sua_senha_mysql',
           'database'  => 'nome_do_banco',
           'driver'    => 'Cake\Database\Driver\Mysql',
           'encoding'  => 'utf8mb4',
           'timezone'  => 'UTC',
           'cacheMetadata' => true,
           // ... demais configurações padrão
       ],
   ],
   ```
---

## 💾 Configuração do Banco de Dados

Crie um banco de dados vazio (por exemplo, `cake_cms_db`) e execute os seguintes comandos SQL para criar as tabelas necessárias:

```sql
-- Tabela de Categorias
CREATE TABLE categories (
  id INT AUTO_INCREMENT PRIMARY KEY,
  name VARCHAR(255) NOT NULL,
  description TEXT NOT NULL,
  created DATETIME,
  modified DATETIME
);

-- Tabela de Produtos
CREATE TABLE products (
  id INT AUTO_INCREMENT PRIMARY KEY,
  name VARCHAR(255) NOT NULL,
  description TEXT NOT NULL,
  price DECIMAL(10,2) NOT NULL,
  category_id INT NOT NULL,
  created DATETIME,
  modified DATETIME,
  FOREIGN KEY (category_id) REFERENCES categories(id)
);
```



---


## ▶️ Executando o Servidor de Desenvolvimento

Dentro da pasta raiz do projeto (onde está o `bin/cake`), rode:

```bash
bin/cake server
```

Por padrão, o CakePHP inicia um servidor embutido em `http://localhost:8765`.  
Abra o navegador e acesse:

- Página inicial do CakePHP:
  ```
  http://localhost:8765/
  ```

Se tudo estiver corretamente configurado, você verá a tela padrão de boas-vindas do CakePHP.

---

## 🧑‍💻 Utilização (CRUD)

O sistema possui duas entidades principais: **Categories** (Categorias) e **Products** (Produtos). Abaixo estão as principais rotas de cada uma:

### 1. Categories (Categorias)

- **Listar categorias (Index)**  
  URL: `http://localhost:8765/categories`  
  Mostra todas as categorias cadastradas.

- **Visualizar detalhes de uma categoria (View)**  
  URL: `http://localhost:8765/categories/view/{id}`  
  Replace `{id}` pelo ID da categoria.

- **Adicionar nova categoria (Add)**  
  URL: `http://localhost:8765/categories/add`  
  Campos obrigatórios:
  - Name (nome da categoria)
  - Description (descrição da categoria)

- **Editar uma categoria existente (Edit)**  
  URL: `http://localhost:8765/categories/edit/{id}`  
  Altere `Name` e/ou `Description`.

- **Excluir uma categoria (Delete)**  
  URL (via link/ação): `http://localhost:8765/categories/delete/{id}`  
  **Importante**: Antes de excluir uma categoria, não deve haver produtos vinculados a ela (caso contrário, acontece erro de “Integrity constraint violation”). Apague todos os produtos cujo `category_id` aponte para esta categoria.

### 2. Products (Produtos)

- **Listar produtos (Index)**  
  URL: `http://localhost:8765/products`  
  Exibe todas as linhas da tabela de produtos, com campo “Category” preenchido pelo nome da categoria via associação (`contain`).

- **Visualizar detalhes de um produto (View)**  
  URL: `http://localhost:8765/products/view/{id}`  
  Exibe todos os campos do produto, incluindo a Categoria à qual pertence.

- **Adicionar novo produto (Add)**  
  URL: `http://localhost:8765/products/add`  
  Campos obrigatórios:
  - Name (nome do produto)
  - Description (descrição do produto)
  - Price (preço, ex.: 100.00)
  - Category (selecionar a categoria a partir de um dropdown populado por Categories)

- **Editar um produto existente (Edit)**  
  URL: `http://localhost:8765/products/edit/{id}`  
  Podemos alterar qualquer campo, inclusive atribuir a outra Categoria.

- **Excluir um produto (Delete)**  
  URL (via link/ação): `http://localhost:8765/products/delete/{id}`  
  Ao excluir, a listagem é atualizada sem aquele registro e um feedback (Flash) indica sucesso ou falha.

---
