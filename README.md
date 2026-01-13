# Reeduc

## Sobre o projeto

**Reeduc** é uma plataforma educacional para pesssoas restritas de liberdade construída com **Django**. O projeto tem como objetivo centralizar a gestão de estudantes, cursos e publicações educativas, oferecendo suporte ao ensino de diferentes áreas do conhecimento, como matemática, língua portuguesa e outras disciplinas, por meio da organização e disponibilização de conteúdos educacionais em um ambiente digital.
---

## Stack

- **Python:** 3.11+ (compatível com Django 5.2.7)
- **Django:** 5.2.7
- **Banco de dados:** PostgreSQL (imagem do Docker: postgres:16)
- **Servidor WSGI (produção):** gunicorn
- **Middleware de arquivos estáticos:** whitenoise
- **Gerenciamento de roles:** django-role-permissions
- **Outras libs:** python-decouple, psycopg2-binary

---

## Controle de Acesso (Níveis de Perfil)

O projeto implementa controle de acesso baseado em papéis (RBAC). Os papéis e permissões são definidos em `core/roles.py` e o módulo de roles está configurado em `reeduc/settings.py` via `ROLEPERMISSIONS_MODULE = 'core.roles'`.

Papéis incluídos atualmente (exemplos): **admin**, **professor**, **aluno**. Cada papel possui permissões específicas (criar/editar/excluir cursos e publicações, visualizar relatórios, etc.) que governam o acesso às funcionalidades da aplicação.

Você pode atribuir papéis a usuários usando a API do pacote `django-role-permissions`, por exemplo:

```python
from rolepermissions.roles import assign_role
assign_role(user, 'professor')
```

E verificar permissões em views/templates com utilitários como `has_permission`, `has_role` ou através de decorators providos pelo pacote.

---

## Rodando o projeto localmente

Siga estes passos mínimos para executar o projeto em sua máquina:

1. Clone o repositório

   ```bash
   git clone <repo-url>
   cd Reeduc
   ```

2. Crie e ative um ambiente virtual (venv)

   ```bash
   python -m venv .venv
   source .venv/Scripts/activate  # Windows: .venv\\Scripts\\activate
   ```

3. Instale dependências

   ```bash
   pip install -r requirements.txt
   ```

4. Configure variáveis de ambiente (As variáveis estão de maneira hardcoded em settings.py, dessa forma não é preciso criar o arquivo ".env")

   - Crie um arquivo `.env` na raiz com as variáveis (exemplo mínimo):

     ```env
     SECRET_KEY=troque_esta_chave
     DATABASE_NAME=reeduc_db
     DATABASE_USER=reeduc_user
     DATABASE_PASSWORD=123456789
     DATABASE_HOST=localhost
     DATABASE_PORT=5432
     DEBUG=True
     ```

   - O projeto usa `python-decouple` e lê variáveis via `os.getenv`.

5. Suba o banco de dados com Docker (recomendado para desenvolvimento)

   ```bash
   docker compose up -d postgres pgadmin
   ```

   - O `docker-compose.yml` já define um serviço `postgres` e `pgadmin`.

6. Rode migrações e crie um superuser

   ```bash
   python manage.py migrate
   python manage.py createsuperuser
   ```

7. Inicie o servidor de desenvolvimento

   ```bash
   python manage.py runserver
   ```

8. Acesse

   - Site: http://127.0.0.1:8000/
   - Admin: http://127.0.0.1:8000/admin/
   - PgAdmin (se usado via Docker): http://localhost:5050/

---

## Diagrama ERD (Modelo de Dados)

-  O diagrama ERD está localizado em `docs/erd.png`.

---

## Capturas de tela do sistema

<img width="1912" height="914" alt="reeduc - area administrativa" src="https://github.com/user-attachments/assets/58e7e270-24ab-43a4-992f-687a046129ff" />
<img width="1912" height="914" alt="reeduc - login" src="https://github.com/user-attachments/assets/252d2bef-7501-4478-8fc5-6121ecf90c11" />
<img width="1912" height="914" alt="reeduc - home" src="https://github.com/user-attachments/assets/eb0c5d69-d3e9-48c1-8969-e93a8dc913b5" />
<img width="1912" height="914" alt="reeduc - dashboard" src="https://github.com/user-attachments/assets/ba2039ac-cc21-4922-84c6-7611a0cd27d5" />
<img width="1912" height="914" alt="reeduc - tela de listagem de publicacao educacional" src="https://github.com/user-attachments/assets/2847be9a-f315-4ff2-aae1-66487af75ce7" />
<img width="1912" height="914" alt="reeduc - tela de listagem de curso" src="https://github.com/user-attachments/assets/8be48dce-ad53-4938-8e70-e7d25375b654" />
<img width="1912" height="914" alt="reeduc - tela de criacao de publicacao educacional" src="https://github.com/user-attachments/assets/648acec0-e727-41f0-b7a6-752a8de124b6" />
<img width="1912" height="914" alt="reeduc - tela de criacao de curso" src="https://github.com/user-attachments/assets/19fb5ae1-5900-4bb5-ad99-ab0b5456eeec" />
<img width="1912" height="914" alt="reeduc - sign up" src="https://github.com/user-attachments/assets/25855b1c-1891-4212-9a23-211574ad24a5" />
<img width="1912" height="914" alt="reeduc - modal de exclusao de publicacao educacional" src="https://github.com/user-attachments/assets/7debcc20-b584-4ddf-a322-57692931d043" />
<img width="1912" height="914" alt="reeduc - modal de exclusao de curso" src="https://github.com/user-attachments/assets/99ecb006-d7ab-4b5e-a2ad-523cf6508a6f" />

---

## Execução com Docker (opcional)

- O `docker-compose.yml` já inclui um serviço `postgres` e `pgadmin`.
- Para rodar toda a stack via Docker, adicione um serviço `web` (subir a imagem do Django) ou descomente/ajuste o bloco `web` já presente no `docker-compose.yml` e execute:

  ```bash
  docker compose up --build
  ```

---

## 🤝 Contribuição

- Fique à vontade para abrir issues e pull requests.
- Siga as boas práticas: branches por feature, testes, descrições claras em PRs.

---



