# Laboratório Prático – Módulo 1: Dinâmica em Equipe sobre Git

## 🎯 Objetivo

Colocar em prática os conceitos fundamentais do Git através de uma dinâmica colaborativa, simulando um cenário real de desenvolvimento em equipe.

---

## 👥 Informações da Turma

- **Total de alunos:** 15 (alguns poderão faltar)
- **Duração estimada:** 40 minutos
- **Formato:** Trabalho em equipe com rotação de papéis

---

## 📋 Pré-requisitos

- Git instalado em todas as máquinas
- Conta no GitHub (ou GitLab/Bitbucket)
- Editor de texto ou IDE
- Acesso à internet

---

## 🎭 Estrutura da Dinâmica

### Divisão de Equipes

A turma será dividida em **3 equipes de 5 alunos** (ajustar conforme presença).

Cada equipe terá os seguintes papéis rotativos:

| Papel               | Responsabilidade                          | O que faz                                   |
|---------------------|-------------------------------------------|---------------------------------------------|
| **Líder Técnico**   | Cria o repositório, gerencia conflitos    | Configuração inicial e integração do código |
| **Desenvolvedor 1** | Trabalha na feature A                     | Desenvolve funcionalidade independente      |
| **Desenvolvedor 2** | Trabalha na feature B                     | Desenvolve funcionalidade independente      |
| **Revisor**         | Valida commits, verifica mensagens        | Garante qualidade e padrões do código       |
| **Documentador**    | Atualiza documentação, registra problemas | Mantém documentação sincronizada            |

> 💡 **Importante:** Todos devem passar por pelo menos 2 papéis diferentes durante a dinâmica.

---

## 🏗️ Cenário do Projeto

Cada equipe desenvolverá um **catálogo de receitas culinárias** em formato Markdown.

### Estrutura do Projeto

```
receitas-equipe-X/
├── README.md          # Documentação principal do projeto
├── .gitignore         # Arquivos que o Git deve ignorar
├── receitas/          # Pasta com todas as receitas
│   ├── massas/        # Categoria de receitas
│   ├── sobremesas/    # Categoria de receitas
│   └── salgados/      # Categoria de receitas
└── autores.md         # Créditos dos contribuidores
```

**Por que essa estrutura?**

- Organização por categorias facilita navegação
- `.gitignore` evita commits de arquivos desnecessários
- `README.md` é o "cartão de visitas" do projeto
- `autores.md` registra as contribuições

---

## 📝 Parte 1: Configuração Inicial

### Requisitos, Conceito: Autenticação SSH

**O que é SSH?**  
SSH (Secure Shell) é um protocolo de comunicação segura. Ao invés de digitar usuário e senha toda vez, você usa um par de chaves (pública e privada) para se autenticar.

**Por que usar SSH com Git?**

- Mais seguro que HTTPS
- Não precisa digitar senha a cada push/pull
- Recomendado para uso profissional

### Requisitos

#### 1. **Configurar SSH:**

- *Executar este comando:*

```shell script
echo "Host github.com
  HostName github.com
  User git
  IdentityFile ~/.ssh/id_ed25519
  IdentitiesOnly yes
  AddKeysToAgent yes" > ~/.ssh/config
chmod 600 ~/.ssh/config
```

**O que este comando faz:**

- Cria um arquivo de configuração SSH
- Define qual chave usar para o GitHub
- Automatiza a autenticação

**Explicação linha por linha:**

- `Host github.com` - Define configuração para o GitHub
- `IdentityFile ~/.ssh/id_ed25519` - Aponta para sua chave privada
- `chmod 600` - Define permissões de segurança (somente você pode ler/escrever)

#### 2. **Configurar Git Mergetool:**

**O que é mergetool?**  
É uma ferramenta visual que ajuda a resolver conflitos quando duas pessoas editam o mesmo arquivo. Ao invés de editar manualmente, você vê as diferenças lado a lado.

- *Executar este comando:*

```shell script
git config --global merge.tool code
git config --global mergetool.code.cmd 'code --wait --merge "$REMOTE" "$LOCAL" "$BASE" "$MERGED"'
git config --global mergetool.keepBackup false
git config --global diff.tool vscode
git config --global difftool.vscode.cmd 'code --wait --diff "$LOCAL" "$REMOTE"'
```

#### 3. **Configurar Git localmente:**

- *Executar este comando:*

```shell script
git config --global user.name "Seu Nome"
git config --global user.email "seu@email.com"
git config --list
```

### Instruções para o Líder Técnico

**O que é um repositório?**  
É como uma "pasta inteligente" que guarda:

- Todo o código do projeto
- Histórico completo de mudanças
- Quem mudou o quê e quando
- Diferentes versões (branches)

1. **Criar repositório no GitHub:**

> Usar este passo quando o repositorio não existir

- Nome: `receitas-equipe-X` (substituir X pelo número da equipe)
- Visibilidade: Público (qualquer um pode ver)
- Adicionar README.md (documentação inicial)

**Por que começar com README?**  
É a primeira coisa que as pessoas veem ao acessar o projeto. Explica o que é e como usar.

2. **Clonar localmente:**

- *Executar este comando:*

```shell script
git clone git@github.com:rafael-negrao/receitas-equipe-X.git
cd receitas-equipe-X
```

3. **Criar estrutura inicial:**

- *Executar este comando:*

```shell script
mkdir -p receitas/massas receitas/sobremesas receitas/salgados
echo "# Este arquivo mantém o diretório no Git" > receitas/massas/.gitkeep
echo "# Este arquivo mantém o diretório no Git" > receitas/sobremesas/.gitkeep
echo "# Este arquivo mantém o diretório no Git" > receitas/salgados/.gitkeep
echo > autores.md
```

**Decompondo o comando:**

- `mkdir -p receitas/massas receitas/sobremesas receitas/salgados`
    - `mkdir` = "make directory" (criar diretório)
    - `-p` = parâmetro que significa "parents" (pais/ancestrais)
        - O que o `-p` faz: Cria todas as pastas necessárias no caminho, mesmo que não existam.
    - `receitas/massas` = caminho das pastas a serem criadas
    - `receitas/sobremesas` = caminho das pastas a serem criadas
    - `receitas/salgados` = caminho das pastas a serem criadas
- `echo > receitas/massas/carbonara.md`
    - `echo` = comando que imprime/exibe texto
    - `>` = operador de redirecionamento de saída
    - `autores.md` = caminho e nome do arquivo a ser criado


4. **Criar arquivo .gitignore:**

```.gitignore (gitignore)
# Arquivos temporários
*.tmp
*.bak
*~

# Arquivos do sistema
.DS_Store
Thumbs.db

# Arquivos de IDE
.vscode/
.idea/
```

**Boas práticas:**

- Sempre inclua `.gitignore` no projeto
- Não versione senhas ou tokens
- Ignore dependências que podem ser baixadas (ex: `node_modules/`)

5. **Fazer commit inicial:**

Observar os arquivos que foram criados e estão pendentes de commit.

- *Executar este comando:*

```shell script
git status
```

Fazer o commit inicial.

- *Executar este comando:*

```shell script
git add .
git commit -m "feat: cria estrutura inicial do projeto de receitas"
git push origin main
```

**O que é um commit?**  
É um "snapshot" (foto) do projeto em determinado momento. Cada commit tem:

- ID único (hash SHA)
- Mensagem descritiva
- Autor e data
- Mudanças realizadas

**Decompondo o comando:**

- `git add .`

```
git add .
│   │   └─ Ponto (representa "tudo no diretório atual")
│   └─ Subcomando add (adicionar)
└─ Comando principal git
```

- `git commit -m "feat: cria estrutura inicial do projeto de receitas"`

```
git commit -m "feat: cria estrutura inicial do projeto de receitas"
│   │      │   └─ Mensagem do commit
│   │      └─ Flag -m (message - permite escrever mensagem inline)
│   └─ Subcomando commit (gravar mudanças)
└─ Comando principal git
```

- `git push origin main`

```
git push origin main
│   │    │      └─ Branch de destino (branch principal)
│   │    └─ Nome do repositório remoto (apelido do GitHub)
│   └─ Subcomando push (enviar)
└─ Comando principal git
```

5. **Adicionar colaboradores:**

> Executar este passo caso o item 1 foi executado

- Settings → Collaborators → Add people
- Adicionar todos os membros da equipe

### ✅ Checkpoint 1

- [ ] Repositório criado no GitHub
- [ ] Estrutura de pastas criada
- [ ] .gitignore configurado
- [ ] Todos os membros têm acesso ao repositório

---

## 📝 Parte 2: Desenvolvimento Colaborativo

### Instruções para Desenvolvedor 1

**Missão:** Adicionar receita de massa

1. **Clonar repositório:**

- *Executar este comando:*

```shell script
git clone git@github.com:rafael-negrao/receitas-equipe-X.git
cd receitas-equipe-X
```

2. **Criar e mudar para nova branch:**

Puxar as atualizações

- *Executar este comando:*

```shell script
git pull
```

- *Executar este comando:*

```shell script
git checkout -b feature/receita-carbonara
```

**Decompondo o comando:**

- `git checkout -b feature/receita-carbonara`
    - `git checkout` = mudar de branch
    - `-b` = criar nova branch
    - `feature/receita-carbonara` = nome da branch

**Convenção de nomenclatura:**

- `feature/` = nova funcionalidade
- `fix/` = correção de bug
- `docs/` = documentação
- Nome descritivo do que será feito

**O que acontece internamente?**

- Git cria novo ponteiro a partir da branch atual
- Seu workspace muda para essa nova branch
- Commits serão feitos nessa branch


3. **Criar arquivo de receita:**

- *Executar este comando:*

```shell script
mkdir -p receitas/massas
echo > receitas/massas/carbonara.md
```

**Decompondo o comando:**

- `mkdir -p receitas/massas`
    - `mkdir` = "make directory" (criar diretório)
    - `-p` = parâmetro que significa "parents" (pais/ancestrais)
        - O que o `-p` faz: Cria todas as pastas necessárias no caminho, mesmo que não existam.
    - `receitas/massas` = caminho das pastas a serem criadas
- `echo > receitas/massas/carbonara.md`
    - `echo` = comando que imprime/exibe texto
    - `>` = operador de redirecionamento de saída
    - `receitas/massas/carbonara.md` = caminho e nome do arquivo a ser criado

4. **Editar o arquivo com a receita:**

- Pode usar o Editor Bloco de Notas ou o VS Code

```markdown
# Carbonara Tradicional

## Ingredientes

- 400g de espaguete
- 200g de bacon em cubos
- 4 gemas
- 100g de queijo parmesão ralado
- Pimenta do reino a gosto

## Modo de Preparo

1. Cozinhe o espaguete em água fervente com sal
2. Frite o bacon até ficar crocante
3. Misture as gemas com o queijo
4. Escorra a massa e misture tudo rapidamente
5. Sirva imediatamente com pimenta
```

5. **Adicionar à staging area:**

Observar os arquivos que foram criados e estão pendentes de commit.

- *Executar este comando:*

```shell script
git status
```

Adicionar os arquivos na área de staging.

- *Executar este comando:*

```shell script
git add receitas/massas/carbonara.md
```

**Decomposição:**

- `git status`

```
git status
│   └─ Subcomando status (estado/situação)
└─ Comando principal git
```

**🎯 Por que git status é tão importante?**

É como um **painel de controle** que responde:

| Pergunta                         | Resposta do git status      |
|----------------------------------|-----------------------------|
| Em qual branch estou?            | On branch main              |
| Tenho mudanças não salvas?       | Lista arquivos modificados  |
| O que está pronto para commit?   | Lista arquivos staged       |
| Estou sincronizado com o remoto? | Your branch is ahead/behind |
| Há arquivos novos?               | Lista untracked files       |
| Há conflitos?                    | Mostra arquivos em conflito |

**Regra de ouro:** Execute git status **antes e depois** de qualquer operação Git!

6. **Fazer commit:**

- *Executar este comando:*

```shell script
git commit -m "feat: adiciona receita de carbonara tradicional"
```

7. **Enviar para o repositório remoto:**

- *Executar este comando:*

```shell script
git push origin feature/receita-carbonara
```

### Instruções para Desenvolvedor 2

**Missão:** Adicionar receita de sobremesa (trabalhar em paralelo)

1. **Clonar repositório e criar branch:**

Fazer o clone do projeto.

- *Executar este comando:*

```shell script
git clone git@github.com:rafael-negrao/receitas-equipe-X.git
cd receitas-equipe-X
```

Criar uma nova branch.

- *Executar este comando:*

```shell script
git checkout -b feature/receita-brownie
```

2. **Criar arquivo de receita:**

- *Executar este comando:*

```shell script
mkdir -p receitas/sobremesas
echo > receitas/sobremesas/brownie.md
```

3. **Editar o arquivo:**

- Pode usar o Editor Bloco de Notas ou o VS Code

```markdown
# Brownie de Chocolate

## Ingredientes

- 200g de chocolate meio amargo
- 150g de manteiga
- 3 ovos
- 1 xícara de açúcar
- 1 xícara de farinha de trigo

## Modo de Preparo

1. Derreta o chocolate com a manteiga
2. Bata os ovos com o açúcar
3. Misture tudo e adicione a farinha
4. Asse a 180°C por 25 minutos
```

4. **Fazer commit e push:**

Observar os arquivos que foram criados e estão pendentes de commit.

- *Executar este comando:*

```shell script
git status
```

Fazer o commit e push.

- *Executar este comando:*

```shell script
git add receitas/sobremesas/brownie.md
git commit -m "feat: adiciona receita de brownie de chocolate"
git push origin feature/receita-brownie
```

### Instruções para o Revisor

**Missão:** Validar as mudanças e fazer merge

1. **Clonar repositório:**

Fazer o clone do projeto.

- *Executar este comando:*

```shell script
git clone git@github.com:rafael-negrao/receitas-equipe-X.git
```

2. **Verificar branches remotas:**

- *Executar este comando:*

```shell script
git fetch --all
git branch -a
```

> Usar o `q` para voltar ao terminal

**Decompondo o comando:**

- `git fetch --all`

```
git fetch --all
│   │     └─ Flag: buscar de todos os remotos
│   └─ Subcomando fetch (buscar)
└─ Comando principal git
```

**O que é git fetch?**

- É o comando que sincroniza metadados do repositório remoto com seu repositório local.

O que ele baixa:

- ✅ Novos commits que existem no remoto
- ✅ Novas branches criadas por outros
- ✅ Atualizações em branches existentes
- ✅ Tags e referências
- ❌ NÃO modifica seus arquivos de trabalho
- ❌ NÃO faz merge automático

Analogia:

- É como verificar notificações sem abrir as mensagens
- Você fica sabendo o que há de novo, mas não aplica ainda

Diferença: fetch vs pull

| Comando   | O que faz                               | Seguro?                  | Quando usar                        |
|-----------|-----------------------------------------|--------------------------|------------------------------------|
| git fetch | Baixa info, **não aplica**              | ✅ Sim                    | Revisar antes de integrar          |
| git pull  | Baixa info **e aplica** (fetch + merge) | ⚠️ Pode causar conflitos | Quando quer integrar imediatamente |

Visualização:

```
git fetch
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Remoto (GitHub)          Local
┌─────────────┐         ┌───────────────────────┐
│ main        │         │ origin/main (atualiza)│
│ ├─ commit A │  fetch  │ ├─ commit A           │
│ ├─ commit B │ ──────► │ ├─ commit B           │
│ └─ commit C │         │ └─ commit C (novo!)   │
└─────────────┘         │                       │
                        │ main (não muda)       │
                        │ └─ commit B           │
                        └───────────────────────┘
                             ↑
                        Seus arquivos
                        não mudam!


git pull (fetch + merge)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Remoto (GitHub)          Local
┌─────────────┐         ┌─────────────────────┐
│ main        │         │ origin/main         │
│ ├─ commit A │  pull   │ ├─ commit A         │
│ ├─ commit B │ ──────► │ ├─ commit B         │
│ └─ commit C │         │ └─ commit C         │
└─────────────┘         │                     │
                        │ main (atualiza!)    │
                        │ └─ commit C         │
                        └─────────────────────┘
                             ↑
                        Seus arquivos
                        são atualizados!
```

3. **Para cada branch de feature, revisar:**

Fazer o checkout da branch.

- *Executar este comando:*

```shell script
git checkout feature/receita-carbonara
git log --oneline
```

> Usar o `q` para voltar ao terminal

Fazer a comparação entre as duas branches.

- *Executar este comando:*

```shell script
git diff main..feature/receita-carbonara
```

> Usar o `q` para voltar ao terminal

**Decomposição:**

- `git log --oneline`
    - O que é git log?
        - É o comando para explorar o histórico do projeto. Mostra todos os commits feitos, como um "livro de registro" ou "linha do tempo".

```
git log --oneline
│   │   └─ Flag: formato resumido (uma linha por commit)
│   └─ Subcomando log (histórico/registro)
└─ Comando principal git
```

- `git diff main..feature/receita-carbonara`

```
git diff main..feature/receita-carbonara
│   │    │    └─ Branch de destino (ponto final da comparação)
│   │    └─ Operador de range (intervalo)
│   │    └─ Branch de origem (ponto inicial da comparação)
│   └─ Subcomando diff (diferenças)
└─ Comando principal git
```

**O que é git diff?**

É o comando para comparar versões de arquivos, commits ou branches.

Analogia:

- Como a função "comparar documentos" do Word
- Mostra o que foi **adicionado, removido ou modificado**
- Linha por linha, caractere por caractere

4. **Validar mensagens de commit:**
    - Seguem padrão Conventional Commits?
    - São claras e descritivas?
    - Estão no imperativo?

5. **Fazer merge na main:**

Fazer o checkout da branch main.

- *Executar este comando:*

```shell script
git checkout main
```

Fazer o merge da branch feature/receita-carbonara com a main.

- *Executar este comando:*

```shell script
git merge feature/receita-carbonara
```

> Digitar `:wq` + enter para salvar, fechar o arquivo e voltar ao terminal

Fazer o push do merge para main

- *Executar este comando:*

```shell script
git push origin main
```

6. **Repetir o processo para a segunda feature `feature/receita-brownie`**

Fazer o checkout da branch.

- *Executar este comando:*

```shell script
git checkout feature/receita-brownie
git log --oneline
```

> Usar o `q` para voltar ao terminal

Fazer a comparação entre as duas branches.

- *Executar este comando:*

```shell script
git diff main..feature/receita-brownie
```

> Usar o `q` para voltar ao terminal

Fazer o checkout da branch main.

- *Executar este comando:*

```shell script
git checkout main
```

Fazer o merge da branch feature/receita-brownie com a main.

- *Executar este comando:*

```shell script
git merge feature/receita-brownie
```

> Digitar `:wq` + enter para salvar, fechar o arquivo e voltar ao terminal

Fazer o push do merge para main

- *Executar este comando:*

```shell script
git push origin main
```

### Instruções para o Documentador

**Missão:** Manter documentação atualizada

1. **Atualizar `README.md`:**

Fazer o clone do projeto.

- *Executar este comando:*

```shell script
git clone git@github.com:rafael-negrao/receitas-equipe-X.git
```

- *Executar este comando:*

```shell script
git pull
```

- Ficar atento para as pessoas do projeto, substituir [Nome] pelos nomes dos membros da equipe

```markdown
# Catálogo de Receitas - Equipe X

## 📖 Sobre o Projeto

Este é um catálogo colaborativo de receitas culinárias.

## 👥 Autores

- [Nome] - Líder Técnico
- [Nome] - Desenvolvedor 1
- [Nome] - Desenvolvedor 2
- [Nome] - Revisor
- [Nome] - Documentador

## 📚 Receitas Disponíveis

### Massas

- Carbonara Tradicional

### Sobremesas

- Brownie de Chocolate

## 🚀 Como Contribuir

1. Clone o repositório
2. Crie uma branch para sua receita
3. Adicione a receita na pasta apropriada
4. Faça commit com mensagem clara
5. Envie pull request
```

2. **Atualizar `autores.md`:**

```markdown
# Autores e Contribuições

## Equipe X

| Nome | Papel | Contribuições |
|------|-------|---------------|
| [Nome] | Líder Técnico | Setup inicial, merges |
| [Nome] | Dev 1 | Receita carbonara |
| [Nome] | Dev 2 | Receita brownie |
| [Nome] | Revisor | Code review |
| [Nome] | Documentador | Documentação |
```

3. **Fazer commit das atualizações:**

- *Executar este comando:*

```shell script
git add README.md autores.md
git commit -m "docs: atualiza documentação do projeto"
git push origin main
```

### ✅ Checkpoint 2

- [ ] Duas receitas adicionadas em branches separadas
- [ ] Commits seguem boas práticas
- [ ] Merges realizados sem conflitos
- [ ] Documentação atualizada

---

## 📝 Parte 3: Simulando Conflitos

### Cenário de Conflito

Dois desenvolvedores editarão o mesmo arquivo simultaneamente.

### Desenvolvedor 1

- *Executar este comando:*

```shell script
git checkout main
git pull origin main
git checkout -b feature/atualiza-readme

# Editar README.md adicionando seção de Ingredientes Comuns
echo "## 🥕 Ingredientes Comuns\n- Sal\n- Azeite\n- Alho" >> README.md

git add README.md
git commit -m "docs: adiciona seção de ingredientes comuns"
git push origin feature/atualiza-readme
```

### Desenvolvedor 2 (trabalha ao mesmo tempo)

- *Executar este comando:*

```shell script
git checkout main
git pull origin main
git checkout -b feature/adiciona-dicas

# Editar README.md adicionando seção de Dicas
echo "## 💡 Dicas de Cozinha\n- Use ingredientes frescos\n- Leia a receita completa antes" >> README.md

git add README.md
git commit -m "docs: adiciona seção de dicas de cozinha"
git push origin feature/adiciona-dicas
```

### Líder Técnico (resolve conflito)

1. **Fazer merge da primeira branch:**

Recuperar todas as atualizações do projeto.

- *Executar este comando:*

```shell script
git pull
```

Fazer o checkout da branch `feature/atualiza-readme` para criar ela localmente no repositório.

- *Executar este comando:*

```shell script
git checkout feature/atualiza-readme
```

Fazer o checkout da branch `main`

- *Executar este comando:*

```shell script
git checkout main
```

Fazer o merge da branch `feature/atualiza-readme` com a `main`.

- *Executar este comando:*

```shell
git merge feature/atualiza-readme
```

2. **Tentar merge da segunda branch:**

Fazer o checkout da branch `feature/adiciona-dicas` para criar ela localmente no repositório.

- *Executar este comando:*

```shell script
git checkout feature/adiciona-dicas
```

Fazer o checkout da branch `main`

- *Executar este comando:*

```shell script
git checkout main
```

- *Executar este comando:*

```shell script
git merge feature/adiciona-dicas
# CONFLITO!
```

- *Executar este comando:*

```shell script
git mergetool
```

3. **Resolver conflito:**

- *Executar este comando:*

```shell script
# Abrir README.md e resolver manualmente
# Manter ambas as seções

git add README.md
git commit -m "merge: resolve conflito entre features de documentação"
git push origin main
```

### ✅ Checkpoint 3

- [ ] Conflito criado intencionalmente
- [ ] Conflito resolvido corretamente
- [ ] Histórico mantém ambas as contribuições

---

## 📝 Parte 4: Explorando o Histórico

### Todos os Membros

**Exercício conjunto:** Explorar o histórico do projeto

1. **Ver histórico completo:**

- *Executar este comando:*

```shell script
git log --oneline --graph --all
```

**Decomposição do comando:**

```
git log --oneline --graph --all
│   │   │         │       └─ Flag: mostrar TODAS as branches
│   │   │         └─ Flag: desenhar gráfico ASCII
│   │   └─ Flag: formato resumido (uma linha)
│   └─ Subcomando log (histórico)
└─ Comando principal git
```

2. **Ver diferenças entre commits:**

- *Executar este comando:*

```shell script
git log -p -2
```

**Decomposição do comando:**

```
git log -p -2
│   │   │  └─ Limitador: mostrar apenas 2 commits
│   │   └─ Flag: mostrar patch (diferenças/mudanças)
│   └─ Subcomando log (histórico)
└─ Comando principal git
```

3. **Ver quem modificou cada linha:**

- *Executar este comando:*

```shell script
git blame README.md
```

**Decomposição do comando:**

```
git blame README.md
│   │     └─ Arquivo a ser analisado
│   └─ Subcomando blame (culpar/atribuir autoria)
└─ Comando principal git
```

**O que é git blame?**

Definição:
- Mostra **linha por linha**:
  - Quem escreveu/modificou
  - Quando foi modificado
  - Em qual commit
  - Conteúdo da linha
- **Por que o nome "blame" (culpa)?**
  - Nome histórico para "atribuir responsabilidade"
  - Não é necessariamente negativo
  - Ajuda a entender contexto e histórico

4. **Ver mudanças de um arquivo específico:**

- *Executar este comando:*

```shell script
git log --follow receitas/massas/carbonara.md
```

**Decomposição do comando:**

```
git log --follow receitas/massas/carbonara.md
│   │   │        └─ Caminho do arquivo a ser rastreado
│   │   └─ Flag: seguir renomeações/movimentações
│   └─ Subcomando log (histórico)
└─ Comando principal git
```

**O que é --follow?**

Definição:
- Rastreia o histórico de um arquivo **através de renomeações**
- Continua seguindo mesmo se o arquivo:
  - Foi renomeado
  - Foi movido para outra pasta
  - Teve seu caminho alterado

5. **Buscar commits por autor:**

- *Executar este comando:*

```shell script
git log --author="Nome"
```

**Decomposição do comando:**

```
git log --author="Nome"
│   │   │         └─ Nome do autor a ser filtrado
│   │   └─ Flag: filtrar por autor
│   └─ Subcomando log (histórico)
└─ Comando principal git
```

**O que é `--author`?**

Definição:
- Filtra commits pelo nome ou email do autor
- Usa expressão regular (regex) para buscar
- Case-sensitive por padrão (diferencia maiúsculas/minúsculas)

Funciona com:
- Nome completo: `"João Silva"`
- Nome parcial: `"João"`
- Email: `"joao@email.com"`
- Parte do email: `"@email.com"`


6. **Ver estado do repositório: `git status`**

- *Executar este comando:*

```shell script
git status
```

**Decomposição do Comando:**

```
git status
│   └─ Subcomando status (estado/situação)
└─ Comando principal git
```

**O que é `git status`?**

Definição:
- Mostra o estado atual do repositório
- É como um painel de controle ou dashboard
- Não modifica nada - apenas informa

O que ele mostra:
- ✅ Branch atual
- ✅ Status de sincronização com remoto
- ✅ Arquivos modificados
- ✅ Arquivos na staging area
- ✅ Arquivos não rastreados (untracked)
- ✅ Conflitos de merge

7. **Ver estado do repositório: `git remote -v`**

- *Executar este comando:*

```shell script
git remote -v
```

**Decomposição do Comando**

```
git remote -v
│   │      └─ Flag: verbose (detalhado) - mostra URLs
│   └─ Subcomando remote (repositórios remotos)
└─ Comando principal git
```

**O que é git remote?**

Definição:

- Gerencia **conexões com repositórios remotos** (GitHub, GitLab, Bitbucket, etc.)
- Repositórios remotos são **versões do seu projeto hospedadas na internet ou rede**
- Permite colaboração entre múltiplos desenvolvedores

Analogia:
- Como uma "lista de contatos" para repositórios
- Cada remoto tem um **apelido** (alias) e um **endereço** (URL)

8. **Ver estado do repositório: `git branch -a`**

- *Executar este comando:*

```shell script
git branch -a
```

**Decomposição do Comando**

```
git branch -a
│   │      └─ Flag: all (todas) - locais + remotas
│   └─ Subcomando branch (ramificações)
└─ Comando principal git
```
 
**O que é git branch?**

Definição:
- Gerencia **branches** (ramificações) do projeto
- Branches são **linhas de desenvolvimento paralelas**
- Permite trabalhar em features sem afetar o código principal

Analogia:
- Como "universos paralelos" do seu projeto
- Cada branch é uma versão independente
- Podem ser mescladas (merged) posteriormente

### ✅ Checkpoint 4

- [ ] Todos conseguem visualizar o histórico
- [ ] Entendem como navegar pelos commits
- [ ] Identificam autores de cada mudança

---

## 🎯 Desafio Final

### Cada Equipe Deve

1. **Criar uma tag para versão 1.0:**

- *Executar este comando:*

```shell script
git tag -a v1.0 -m "Versão 1.0: Catálogo inicial com 2 receitas"
git push origin v1.0
```

**Decomposição do Comando**
- `git tag -a v1.0 -m "Versão 1.0: Catálogo inicial com 2 receitas"`
```
git tag -a v1.0 -m "Versão 1.0: Catálogo inicial com 2 receitas"
│   │   │  │    │  └─ [4] Mensagem da tag
│   │   │  │    └─ [3] Flag: message (mensagem)
│   │   │  └─ [2] Nome da tag
│   │   └─ [1] Flag: annotated (anotada)
│   └─ Subcomando tag (etiqueta/marcador)
└─ Comando principal git
```
- `git push origin v1.0`
```
git push origin v1.0
│   │    │      └─ [3] Nome da tag a ser enviada
│   │    └─ [2] Nome do repositório remoto
│   └─ [1] Subcomando push (enviar/empurrar)
└─ Comando principal git
```

**O que é uma Tag no Git?**

Definição:
- Tag = Marcador de um commit específico
- Como um "bookmark" ou "snapshot" nomeado
- Usado principalmente para marcar versões/releases

Analogia:
- Como um selo postal que marca um momento específico
- Como uma foto instantânea de determinado estado do projeto
- Como um marco histórico que não muda

2. **Adicionar `.gitattributes`:**

```text
*.md text eol=lf
*.jpg binary
*.png binary
```

3. **Fazer commit final:**

- *Executar este comando:*

```shell script
git add .gitattributes
git commit -m "chore: adiciona configuração de atributos do git"
git push origin main
```

---

## 🚨 Problemas Comuns e Soluções

### "Não consigo fazer push"

**Causa:** Provavelmente o remoto está à frente  
**Solução:**

```shell script
git pull origin main
git push origin main
```

### "Meu commit apareceu na branch errada"

**Causa:** Estava na branch incorreta  
**Solução:**

```shell script
git log --oneline  # Anotar hash do commit
git checkout branch-correta
git cherry-pick <hash>
```

### "Não sei em qual branch estou"

**Solução:**

```shell script
git branch  # Mostra branch atual com *
git status  # Também mostra a branch
```

---

## 📚 Recursos de Apoio

### Comandos Rápidos

```shell script
# Ver status
git status

# Ver histórico
git log --oneline --graph

# Ver branches
git branch -a

# Ver remotos
git remote -v

# Desfazer último commit (mantém mudanças)
git reset --soft HEAD~1

# Ver diferenças
git diff
```

### Links Úteis

- [Git Cheat Sheet](https://education.github.com/git-cheat-sheet-education.pdf)
- [Visualizing Git](https://git-school.github.io/visualizing-git/)
- [Learn Git Branching](https://learngitbranching.js.org/?locale=pt_BR)

