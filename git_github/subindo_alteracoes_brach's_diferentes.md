# Git e GitHub
**Como trazer as alterações da branch `develop` para outra branch**
***

## 🔄 Passo a Passo para mesclar a branch `develop` com a sua branch de trabalho

### 1️⃣ Verificar em qual branch você está

Antes de iniciar o processo, confirme em qual branch você está:
> git branch

Se não estiver na branch desejada, mude para ela com:
> git checkout nome_da_branch
>
🔹 Exemplo:
> git checkout feature/login

***

### 2️⃣ Atualizar a branch `develop`

Antes de mesclar, é importante garantir que a branch `develop` esteja atualizada com as últimas alterações do repositório remoto:
> git checkout develop  
> git pull origin develop

***

### 3️⃣ Voltar para a sua branch

Após atualizar a `develop`, volte para a sua branch de trabalho:
> git checkout nome_da_branch

***

### 4️⃣ Mesclar a `develop` com a sua branch

Agora, traga as alterações da branch `develop` para a sua branch:
> git merge develop

📌 Se houver conflitos, o Git vai te avisar e será necessário resolvê-los manualmente antes de continuar.

***

## 📘 Resumo dos Comandos

| Comando                          | Descrição                                                     |
|----------------------------------|---------------------------------------------------------------|
| `git branch`                     | Verifica a branch atual                                       |
| `git checkout nome_da_branch`    | Muda para a branch desejada                                   |
| `git checkout develop`           | Vai para a branch `develop`                                   |
| `git pull origin develop`        | Atualiza a `develop` com a versão do repositório remoto       |
| `git checkout nome_da_branch`    | Volta para sua branch                                         |
| `git merge develop`              | Mescla a `develop` na sua branch atual                        |

***

Se preferir uma linha do tempo mais linear (histórico mais limpo), você também pode usar:
> git rebase develop  
🟠 Atenção: `rebase` deve ser usado com cuidado, principalmente se estiver trabalhando em equipe.
