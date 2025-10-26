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

**O que este comando faz:**

- Cria um arquivo de configuração SSH
- Define qual chave usar para o GitHub
- Automatiza a autenticação

```shell script
vim ~/.ssh/config
echo "Host github.com
  HostName github.com
  User git
  IdentityFile ~/.ssh/id_ed25519
  IdentitiesOnly yes
  AddKeysToAgent yes" > ~/.ssh/config
chmod 600 ~/.ssh/config
```

**Explicação linha por linha:**

- `Host github.com` - Define configuração para o GitHub
- `IdentityFile ~/.ssh/id_ed25519` - Aponta para sua chave privada
- `chmod 600` - Define permissões de segurança (somente você pode ler/escrever)

#### 2. **Configurar Git Mergetool:**

**O que é mergetool?**  
É uma ferramenta visual que ajuda a resolver conflitos quando duas pessoas editam o mesmo arquivo. Ao invés de editar manualmente, você vê as diferenças lado a lado.

```shell script
git config --global merge.tool code
git config --global mergetool.code.cmd 'code --wait --merge "$REMOTE" "$LOCAL" "$BASE" "$MERGED"'
git config --global mergetool.keepBackup false
git config --global diff.tool vscode
git config --global difftool.vscode.cmd 'code --wait --diff "$LOCAL" "$REMOTE"'
```

#### 3. **Configurar Git localmente:**

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

```shell script
git clone git@github.com:rafael-negrao/receitas-equipe-X.git
cd receitas-equipe-X
```

3. **Criar estrutura inicial:**

```shell script
mkdir -p receitas/massas receitas/sobremesas receitas/salgados
echo  > autores.md
```

4. **Criar arquivo .gitignore:**

**Boas práticas:**
- Sempre inclua `.gitignore` no projeto
- Não versione senhas ou tokens
- Ignore dependências que podem ser baixadas (ex: `node_modules/`)

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

5. **Fazer commit inicial:**

**O que é um commit?**  
É um "snapshot" (foto) do projeto em determinado momento. Cada commit tem:
- ID único (hash SHA)
- Mensagem descritiva
- Autor e data
- Mudanças realizadas

```shell script
git add .
git commit -m "feat: cria estrutura inicial do projeto de receitas"
git push origin main
```

5. **Adicionar colaboradores:**

> Executar este passo caso  o item 1 foi executado

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

```shell script
git clone git@github.com:rafael-negrao/receitas-equipe-X.git
cd receitas-equipe-X
```

2. **Criar e mudar para nova branch:**

```shell script
git checkout -b feature/receita-carbonara
```
**Decompondo o comando:**
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

- Executar o comando abaixo
```shell script
mkdir -p receitas/massas
```

**Decompondo o comando:**
- `mkdir` = "make directory" (criar diretório)
- `-p` = parâmetro que significa "parents" (pais/ancestrais)
- `receitas/massas` = caminho das pastas a serem criadas

O que o -p faz:
Cria todas as pastas necessárias no caminho, mesmo que não existam.

- Executar o comando abaixo
```shell
echo > receitas/massas/carbonara.md
```
**Decompondo o comando:**
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

```shell script
git status
git add receitas/massas/carbonara.md
```

6. **Fazer commit:**

```shell script
git commit -m "feat: adiciona receita de carbonara tradicional"
```

7. **Enviar para o repositório remoto:**

```shell script
git push origin feature/receita-carbonara
```

### Instruções para Desenvolvedor 2

**Missão:** Adicionar receita de sobremesa (trabalhar em paralelo)

1. **Clonar repositório e criar branch:**

```shell script
git clone https://github.com/LIDER/receitas-equipe-X.git
cd receitas-equipe-X
git checkout -b feature/receita-brownie
```

2. **Criar arquivo de receita:**

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

```shell script
git add receitas/sobremesas/brownie.md
git commit -m "feat: adiciona receita de brownie de chocolate"
git push origin feature/receita-brownie
```

### Instruções para o Revisor

**Missão:** Validar as mudanças e fazer merge

1. **Verificar branches remotas:**

```shell script
git fetch --all
git branch -a
```

2. **Para cada branch de feature, revisar:**

```shell script
git checkout feature/receita-carbonara
git log --oneline
git diff main..feature/receita-carbonara
```

3. **Validar mensagens de commit:**
    - Seguem padrão Conventional Commits?
    - São claras e descritivas?
    - Estão no imperativo?

4. **Fazer merge na main:**

```shell script
git checkout main
git merge feature/receita-carbonara
git push origin main
```

5. **Repetir para a segunda feature**

### Instruções para o Documentador

**Missão:** Manter documentação atualizada

1. **Atualizar README.md:**

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

2. **Atualizar autores.md:**

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

## 📝 Parte 3: Simulando Conflitos (30 minutos)

### Cenário de Conflito

Dois desenvolvedores editarão o mesmo arquivo simultaneamente.

### Desenvolvedor 1

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

```shell script
git checkout main
git merge feature/atualiza-readme
git push origin main
```

2. **Tentar merge da segunda branch:**

```shell script
git merge feature/adiciona-dicas
# CONFLITO!

git mergetool
```

3. **Resolver conflito:**

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

## 📝 Parte 4: Explorando o Histórico (20 minutos)

### Todos os Membros

**Exercício conjunto:** Explorar o histórico do projeto

1. **Ver histórico completo:**

```shell script
git log --oneline --graph --all
```

2. **Ver diferenças entre commits:**

```shell script
git log -p -2
```

3. **Ver quem modificou cada linha:**

```shell script
git blame README.md
```

4. **Ver mudanças de um arquivo específico:**

```shell script
git log --follow receitas/massas/carbonara.md
```

5. **Buscar commits por autor:**

```shell script
git log --author="Nome"
```

6. **Ver estado do repositório:**

```shell script
git status
git remote -v
git branch -a
```

### ✅ Checkpoint 4

- [ ] Todos conseguem visualizar o histórico
- [ ] Entendem como navegar pelos commits
- [ ] Identificam autores de cada mudança

---

## 🎯 Desafio Final (10 minutos)

### Cada Equipe Deve

1. **Criar uma tag para versão 1.0:**

```shell script
git tag -a v1.0 -m "Versão 1.0: Catálogo inicial com 2 receitas"
git push origin v1.0
```

2. **Adicionar .gitattributes:**

```textmate
*.md text eol=lf
*.jpg binary
*.png binary
```

3. **Fazer commit final:**

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

