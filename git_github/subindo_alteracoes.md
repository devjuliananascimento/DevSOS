# Git e Git Hub
**Explicações e instruções de como subir alterações para o Git Hub**
***

## 📌 Passo a Passo para subir alterações para o Git Hub

### 1️⃣ Verificar em qual branch você está

Antes de tudo, verifique se você está na branch correta (geralmente main ou master):
>git branch
> 
Se precisar mudar para a branch correta:
>git checkout main #ou 'master', se for o caso
> 
***
### 2️⃣ Verificar quais arquivos foram modificados

Antes de adicionar arquivos ao commit, veja quais foram alterados:
>git status
> 
Isso mostra os arquivos novos, modificados ou deletados.
***
### 3️⃣ Adicionar os arquivos ao commit

Agora, adicione os arquivos ao controle do Git. Você tem duas opções:

Adicionar arquivos específicos:
>git add nome-do-arquivo.extensao
> 
Adicionar todos os arquivos modificados:
>git add .
> 
O . significa "todos os arquivos que foram modificados".
***
### 4️⃣ Criar um commit com uma mensagem descritiva

Depois de adicionar os arquivos, é hora de salvar essas mudanças localmente no Git com um commit:
> git commit -m "Mensagem descritiva do que foi alterado"
> 
🔹 Exemplo:
>git commit -m "Corrigido bug na tela de login"
> 
***
### 5️⃣ Enviar as alterações para o GitHub

Agora que os arquivos foram commitados localmente, vamos enviá-los para o repositório remoto no GitHub:
> git push origin main  # ou 'master', dependendo da branch principal
> 
***
### 6️⃣ Verificar no GitHub

Agora, vá até o seu repositório no GitHub e veja se as mudanças foram aplicadas corretamente.
***
📌 Resumo dos comandos

| Comando                        | Descrição                                  |
|---------------------------------|--------------------------------------------|
| `git branch`                   | Verifica a branch atual                    |
| `git checkout main`            | Muda para a branch correta (se necessário) |
| `git status`                   | Mostra arquivos modificados               |
| `git add .`                    | Adiciona todos os arquivos para commit    |
| `git commit -m "Descrição"`    | Cria o commit com uma mensagem            |
| `git push origin main`         | Envia para o GitHub                       |

