# 🗺️ Roadmap de Resolução: Erros de Git, Remotos e Ramificações Divergentes

Este mapa descreve a jornada do código local até o GitHub, detalhando cada erro encontrado, a sua causa raiz e a solução aplicada durante a configuração do repositório **Pedido**.

---

## 🛑 Etapa 1: Vinculação do Repositório Remoto

### ❌ O Erro
```bash
error: No such remote 'origin'
```
* **Causa Raiz:** Tentou-se usar o comando `git remote set-url`, que serve apenas para **alterar** uma URL que já existe. Como o repositório local era totalmente novo, o identificador `origin` ainda não existia nas configurações do Git.
* **A Solução:** Criar o remoto do zero utilizando o comando de adição:
  ```bash
  git remote add origin git@github.com:jomaaph/pedido.git
  ```

---

## 🛑 Etapa 2: Autenticação e Existência do Repositório

### ❌ O Erro
```bash
ERROR: Repository not found.
fatal: Could not read from remote repository.
Please make sure you have the correct access rights and the repository exists.
```
* **Causa Raiz:** Erro de digitação (typo) na URL do Git. O comando inicial apontava para o utilizador `jomaph` (com um "a"), mas o nome correto no GitHub era `jomaaph` (com dois "a"). O GitHub retorna "não encontrado" para caminhos incorretos por questões de privacidade e segurança de repositórios privados.
* **A Solução:** Corrigir a URL do repositório remoto para o endereço exato com o comando de alteração:
  ```bash
  git remote set-url origin git@github.com:jomaaph/pedido.git
  ```

---

## 🛑 Etapa 3: Ramificações Divergentes (Divergent Branches)

### ❌ O Erro
```bash
hint: You have divergent branches and need to specify how to reconcile them.
fatal: Need to specify how to reconcile divergent branches.
```
* **Causa Raiz:** O Git local e o GitHub tinham históricos que começaram de formas diferentes e independentes. O GitHub já continha um commit inicial (o arquivo `README.md` criado automaticamente pela interface web), enquanto o ambiente local tinha os arquivos de código Java criados na máquina. O Git bloqueia a ação até que se decida como fundir os dois lados.
* **A Solução:** Definir a estratégia padrão do Git para resolução de puxadas (`pull`) como mesclagem tradicional (`merge`):
  ```bash
  git config pull.rebase false
  ```

---

## 🛑 Etapa 4: Rejeição de Envio Não Linear (Non-Fast-Forward)

### ❌ O Erro
```bash
 ! [rejected]        main -> main (non-fast-forward)
error: failed to push some refs to 'github.com:jomaaph/pedido.git'
hint: Updates were rejected because the tip of your current branch is behind its remote counterpart.
```
* **Causa Raiz:** O histórico local continuava desalinhado e considerado "atrás" do servidor remoto. Como o repositório remoto continha alterações que não existiam localmente, o Git impediu o envio padrão para evitar a perda acidental do arquivo `README.md` remoto.
* **A Solução Aplicada:** Forçar o envio local para se sobrepor ao servidor remoto. Como o projeto estava no início e o histórico local continha o código correto de desenvolvimento, o comando `--force` resolveu o bloqueio:
  ```bash
  git push -u origin main --force
  ```
* **Estratégia Alternativa Sem Force (Boa Prática):** Integrar o histórico remoto de forma explícita antes de fazer o envio definitivo:
  ```bash
  git pull origin main --allow-unrelated-histories
  git push -u origin main
  ```

---

## 🏁 Estado Final Alcançado
* **Local:** Projeto Java estruturado na branch principal `main`.
* **Remoto:** URL devidamente apontada para o perfil correto: `git@github.com:jomaaph/pedido.git`.
* **Sincronização:** Histórico local enviado com sucesso e visível no painel web do GitHub.
