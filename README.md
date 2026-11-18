## Evolução do projeto

O PetCuida está em desenvolvimento.
Esta versão contempla o cadastro de animais
e a geração de manuais personalizados em PDF.
# PetCuida — MVP

Aplicação web para voluntários registrarem informações de cães acolhidos e gerarem um manual personalizado em PDF para apoiar a adoção responsável.

## Tecnologias
- Python 3.12
- Django 5.2
- SQLite 3
- Bootstrap 5.3.3
- ReportLab 5.1.0

## Funcionalidades
- Login e logout de voluntários (autenticação nativa do Django).
- Cadastro e listagem de animais.
- Consulta dos dados cadastrados.
- Geração de manual PDF e histórico de gerações com status `gerado` ou `erro`.
- Rotas protegidas por autenticação, formulários com CSRF e validação no servidor.

## Executar localmente

> Recomendado: Python 3.12 instalado.

### Windows (PowerShell)
```powershell
py -3.12 -m venv .venv
.venv\Scripts\Activate.ps1
python -m pip install --upgrade pip
pip install -r requirements.txt
python manage.py migrate
python manage.py createsuperuser
python manage.py runserver
```

### Linux / macOS
```bash
python3.12 -m venv .venv
source .venv/bin/activate
python -m pip install --upgrade pip
pip install -r requirements.txt
python manage.py migrate
python manage.py createsuperuser
python manage.py runserver
```

Acesse http://127.0.0.1:8000/ e entre com o usuário criado em `createsuperuser`. O painel administrativo fica em `/admin/`.

## Testes automatizados
```bash
python manage.py test
```

Os testes cobrem proteção da página inicial, acesso autenticado à listagem e geração do PDF.

## Acesso por celular na mesma rede
Execute `python manage.py runserver 0.0.0.0:8000`, descubra o IP local do computador e acesse `http://IP-DO-COMPUTADOR:8000/` no celular conectado à mesma rede. Para esse teste local, inclua o IP em `ALLOWED_HOSTS` via variável `DJANGO_ALLOWED_HOSTS`. Não exponha o servidor de desenvolvimento à internet.

## Modelo de dados
- `Animal`: dados básicos, comportamento, alimentação, rotina e cuidados.
- `Manual`: FK para `Animal`, data de geração, arquivo PDF e status.
- Relacionamento: um Animal pode possuir vários Manuais (1:N).

## Versionamento com Git e GitHub
Crie um repositório vazio no GitHub e, na pasta do projeto, execute:

```bash
git init
git add .
git commit -m "chore: estrutura inicial do projeto Django"
git branch -M main
git remote add origin https://github.com/SEU-USUARIO/petcuida-mvp.git
git push -u origin main
```

### Estratégia de branches e commits
Trabalhe em branches curtas por funcionalidade e abra Pull Requests para `main`:

```bash
git checkout -b feat/autenticacao
# implemente e teste
 git add .
git commit -m "feat: adiciona autenticação de voluntários"
git push -u origin feat/autenticacao
```

Sugestão de sequência de commits:
1. `chore: estrutura inicial do projeto Django`
2. `feat: implementa autenticação de voluntários`
3. `feat: adiciona cadastro e listagem de animais`
4. `feat: gera manual personalizado em PDF`
5. `test: adiciona testes do fluxo principal`
6. `docs: documenta instalação e validação do MVP`

Use mensagens no padrão Conventional Commits (`feat`, `fix`, `test`, `docs`, `refactor`, `chore`). Não versione `db.sqlite3`, arquivos em `media/`, senhas, chaves secretas ou dados reais de adotantes/voluntários. Faça commits pequenos e execute os testes antes de integrar alterações.

## Limitações desta versão
- Protótipo/MVP local: não inclui hospedagem pública, integração com WhatsApp, QR Code, aplicativo nativo ou acompanhamento pós-adoção.
- O PDF é gerado no servidor local; o compartilhamento com o adotante é realizado pelo voluntário fora do sistema.
- Cadastro de usuários é administrativo: crie contas de voluntários pelo Django Admin ou `createsuperuser`.
- A aplicação não substitui avaliação ou orientação veterinária.

## Antes de produção
Defina `DJANGO_SECRET_KEY` forte via variável de ambiente, `DJANGO_DEBUG=False`, `DJANGO_ALLOWED_HOSTS`, HTTPS e armazenamento/backup apropriados. Configure também proteção contra tentativas repetidas de login (rate limiting), política de retenção dos PDFs e permissões de acesso aos arquivos. O servidor `runserver` é apenas para desenvolvimento.
