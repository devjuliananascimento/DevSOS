# Passo a Passo para Novo Projeto Odoo 17

## 1. Preparar o ambiente
- Instale Python 3.10+ e PostgreSQL.
- Crie e ative um ambiente virtual:
  ```bash
  python3 -m venv venv
  source venv/bin/activate
  ```

## 2. Instalar dependências
- Instale as dependências do Odoo:
  ```bash
  pip install -r requirements.txt
  ```

## 3. Configurar o banco de dados
- Crie um usuário e banco no PostgreSQL:
  ```sql
  sudo -u postgres psql
  CREATE USER seu_usuario WITH PASSWORD 'sua_senha';
  CREATE DATABASE seu_banco OWNER seu_usuario;
  GRANT ALL PRIVILEGES ON DATABASE seu_banco TO seu_usuario;
  \q
  ```

## 4. Configurar o odoo.conf
- Edite ou crie o arquivo `odoo.conf`:
  ```ini
  [options]
  admin_passwd = admin
  db_host = False
  db_port = False
  db_user = seu_usuario
  db_password = sua_senha
  db_name = seu_banco
  addons_path = /caminho/para/odoo/addons
  ```

## 5. Inicializar o banco
- Rode:
  ```bash
  ./odoo-bin -c odoo.conf -d seu_banco -i base
  ```

## 6. Iniciar o servidor
- Para rodar o Odoo:
  ```bash
  ./odoo-bin -c odoo.conf
  ```
- Acesse no navegador:  
  http://localhost:8069

---

Se quiser criar um novo módulo, peça o passo a passo!
