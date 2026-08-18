# Guia de Git e GitHub

## 1. O que é Git?

Git é um sistema de controle de versão distribuído, que permite:

* Registrar alterações no código;
* Voltar para versões anteriores;
* Trabalhar com diferentes versões do projeto através de branches;
* Trabalhar em equipe;
* Sincronizar alterações entre repositórios locais e remotos.

### 1.1. Distribuído x Centralizado

Os sistemas distribuídos são caracterizados por cada desenvolvedor possuir uma cópia completa do repositório, sendo possível trabalhar offline. Cada cópia possui o histórico do projeto, assim, não depende constantemente de um servidor central.

Já os sistemas centralizados possuem um servidor central com o repositório principal, onde os desenvolvedores dependem mais desse servidor. Em geral, há uma única fonte central de histórico.

Dessa forma, o Git é um sistema de controle de versão **distribuído**.

### 1.2. Composição de um projeto Git

Um projeto Git normalmente é composto por um repositório remoto e por um ou mais repositórios locais. O repositório remoto é armazenado em um servidor, como o GitHub, e funciona como um ponto de compartilhamento do projeto entre os desenvolvedores. Já o repositório local é a cópia do projeto mantida no computador de cada desenvolvedor, contendo os arquivos e todo o histórico de commits.

Como o Git é um sistema de controle de versão distribuído, cada desenvolvedor possui uma cópia completa do repositório local e pode trabalhar mesmo sem conexão com o servidor remoto.


### 1.3. Estados dos arquivos

Durante o processo de versionamento, os arquivos podem passar por diferentes estados dentro do Git. Esses estados representam as etapas pelas quais uma alteração passa até ser registrada no histórico do projeto.

* Working Directory: É o diretório de trabalho, ou seja, o local onde os arquivos do projeto estão armazenados e onde são feitas as alterações. Nesse estado, as modificações ainda não foram preparadas para serem registradas em um commit;
* Staging Area: É uma área intermediária utilizada para selecionar quais alterações farão parte do próximo commit;
* Repository: É a área onde ficam armazenados os commits e o histórico de versões do projeto. Quando o comando git commit é executado, as alterações que estão na Staging Area são registradas como um novo commit no repositório local.

---

## 2. Comandos principais

### 2.1. ```git add```

Adiciona alterações à Staging Area.

```git add arquivo.txt```

Adicionar todos os arquivos:

* ```git add .```
* ```git add --all```
* ```git add -a (abreviação da opção a cima)```

> git add . adiciona alterações da árvore a partir do diretório atual. git add --all é mais abrangente e considera alterações em todo o repositório.

### 2.2. ```git commit```

Cria um novo ponto no histórico do projeto.

```git commit -m "Adiciona sistema de login"```

Um commit possui, entre outras informações:

* Identificador (hash);
* Autor;
* Data;
* Mensagem;
* Alterações registradas.

Alterar a mensagem do commit: ```git commit --amend -m "Nova mensagem"```

Adicionar um arquivo que foi esquecido no último commit:

```
git add arquivo.txt
git commit --amend --no-edit
```

O --no-edit mantém a mensagem anterior.

Sem --no-edit, o Git abrirá o editor configurado para permitir alterar a mensagem.

> Para usar VS Code como editor padrão:
> ```git config --global core.editor "code --wait"```

Navegar entre commits: ```git checkout hash```

---

### 2.3. Configuração do usuário

> É interessante que seja o mesmo nome e e-mail do GitHub

Configurar nome: ```git config --global user.name "Seu Nome"```

Configurar e-mail: ```git config --global user.email "seu@email.com"```

Visualizar as configurações: ```git config --global --list```

### 2.4. Histórico de commits

Mostra o histórico: ```git log```

Normalmente apresenta:

* Hash do commit;
* Autor;
* Data;
* Mensagem;
* `HEAD`, indicando a posição atual.

O histórico aparece do commit mais recente para o mais antigo. A forma resumido é: ```git log --oneline```

Além disso, para mostrar os últimos N commits: ```git log -n 3```

> Também pode combinar: 
> ```git log -n 3 --oneline```

Para mostrar alterações de cada commit: ```git log --patch``` ou ```git log -p```

Já para mostrar quais arquivos foram alterados e estatísticas de adições e remoções: ```git log --stat```

### 2.5. ```git diff```

Compara o estado atual dos arquivos com a última versão registrada no Git.

```git diff```

Caso o arquivo já esteja na Stagind Area: ```git diff --staged``` ou ```git diff --cached```

É possível comparar commits também, a partir do código hash: ```git diff HASH1 HASH2```

### 2.6. Reverter alterações

Restaurar um arquivo para o último commit: ```git restore arquivo.txt```

Para restaurar todos: ```git restore .```

> Isso descarta alterações não commitadas nesses arquivos.

### 2.7. Retirar um arquivo do `git add`

Se quer retirar o arquivo da Staging Area: ```git restore --staged arquivo.txt```

Também é possível usar: ```git rm --cached arquivo.txt```

Porém, `git rm --cached` tem outro uso importante: ele remove o arquivo do rastreamento do Git, mantendo o arquivo fisicamente no computador.

Para remover todos recursivamente do rastreamento: ```git rm --cached -r .```

### 2.8. ```git reset```

O `reset` move o `HEAD` para outro ponto do histórico.

#### 2.8.1. Soft

```git reset --soft HEAD~1```

Volta um commit, mas mantém as alterações na **Staging Area**.

#### 2.8.2. Mixed

```git reset --mixed HEAD~1```

Volta um commit e mantém as alterações no Working Directory, mas tira da Staging Area.

> `HEAD~1` significa um commit antes do atual.

#### 2.8.3. Hard

```git reset --hard HEAD~1```

Volta um commit e descarta as alterações daquele commit no Working Directory e na Staging Area.

---

### 2.9. ```git revert```

Diferente do `reset`, o `revert` **não apaga o commit anterior do histórico**.

```git revert HEAD```

Ele cria um **novo commit** que desfaz as alterações de um commit anterior. Para aceitar automaticamente a mensagem padrão: ```git revert HEAD --no-edit```

---

## 3. Branches

Uma **branch** é uma ramificação do histórico do projeto. Cada novo commit é criado na branch atual.

Listar branches: ```git branch``` ou ```git branch --list```. A branch atual aparece com `*`.

Criar e trocar de branch: Criar: ```git branch nome-da-branch```

Trocar: ```git checkout nome-da-branch```

Forma moderna: ```git switch nome-da-branch```

> Voltar para a branch anterior: ```git switch -```

Criar e trocar ao mesmo tempo: ```git checkout -b nome-da-branch``` ou ```git switch -c nome-da-branch```

> `git switch` foi criado especificamente para trabalhar com branches pois `git checkout` possui funções adicionais, como restaurar arquivos e acessar commits.

Trocar de branch descartando alterações: ```git checkout -f nome-da-branch```. Ele força a troca de branch e descarta alterações locais que impediriam a troca.

Renomear uma branch: ```git branch -m nome-antigo nome-novo```

Renomear a branch atual: ```git branch -m novo-nome```

Consultar commits de uma branch: ```git log nome-da-branch```. Também é possível trocar para a branch e executar:
```
git switch nome-da-branch
git log
```

> O nome da branch remota não é alterado com um comando único. Normalmente, renomeia localmente, remove a antiga no remoto e envia a nova.

### 3.1. Deletar branches

Branch local: ```git branch -d nome```

Forçar: ```git branch -D nome```

Branch remota: ```git push --delete origin nome```. Isso remove a referência da branch no repositório remoto.

### 3.2. Merge

O `merge` incorpora as alterações de uma branch na branch atual. Se estiver na `main`:

```
git switch main
git merge feature
```

A ideia é trazer as alterações de `feature` para `main`.

**Importante:** o `merge` sempre traz as alterações da branch indicada **para a branch em que está**.

### 3.3. Verificar branches mergeadas

Branches que ainda não foram mergeadas na branch atual: ```git branch --no-merged```

Branches que já foram mergeadas: ```git branch --merged```

### 3.4. Conflitos de merge

Um conflito acontece quando o Git não consegue decidir automaticamente como combinar alterações. Depois de resolver manualmente os arquivos:

```
git add .
git commit
```

Cancelar o merge: ```git merge --abort```

---

## 4. Repositórios remotos

`origin` é o nome padrão dado ao repositório remoto quando você faz um clone. É possível que um repositório local aponte para vários remotos.

Ver repositórios remotos: ```git remote -v```

Adicionar repositório remoto a um local: ```git remote add origin URL```

Alterar URL: ```git remote set-url origin URL```

### 4.1. ```git clone```

Para copiar um repositório remoto: ```git clone URL```

Para escolher o nome da pasta a ser criada: ```git clone URL nome-da-pasta```

O clone traz o repositório e seu histórico para o computador.

### 4.2. ```git push```

Envia commits locais para um repositório remoto: ```git push```

Na primeira vez que enviar uma branch: ```git push --set-upstream origin nome-da-branch``` ou ```git push -u origin nome-da-branch```

### 4.3. ```git pull```

Busca alterações do repositório remoto e tenta integrá-las à branch atual: ```git pull```

> Conceitualmente: ```git pull ≈ git fetch + git merge```

Pode haver conflitos durante a integração.

### 4.4. ```git fetch```

O ```git fetch``` busca informações e commits do repositório remoto, mas **não altera diretamente sua branch atual**. 

Por exemplo: ```git fetch origin```. Depois você pode analisar as alterações e decidir se quer fazer merge ou rebase.

Buscar uma branch específica: ```git fetch origin nome-da-branch```

---

## 5. Rebase

```git rebase main```

Estando em uma branch, esse comando pega os commits dela e os reaplica sobre a versão mais recente da `main`. Pode haver conflitos e, após resolver eles:

```
git add .
git rebase --continue
```

Para cancelar: ```git rebase --abort```

### 5.1. Pull com rebase

```git pull --rebase```

Atualiza as referências do remoto e reaplica seus commits locais sobre a versão mais recente da branch remota. Isso evita criar um commit de merge desnecessário em muitos fluxos de trabalho.

### 5.2. Rebase interativo

```git rebase -i HEAD~N```

Permite editar os últimos N commits. É possível:

* Reordenar commits.
* Alterar mensagens.
* Juntar commits.
* Remover commits.

Algumas opções:

* pick: mantém o commit;
* reword: altera a mensagem;
* edit: pausa para editar;
* squash: junta com o commit anterior;
* fixup: junta sem manter a mensagem;
* drop: remove o commit.

---

## 6. Cherry-pick

Permite pegar um commit específico de outra branch e aplicá-lo na branch atual: ```git cherry-pick HASH```

---

## 7. ```git stash```

O `stash` guarda temporariamente alterações não commitadas. É útil quando está trabalhando em algo, mas precisa trocar de branch sem querer fazer um commit ainda. As alterações são guardadas e o Working Directory volta para um estado limpo.

Ver stashes: ```git stash list```

Exemplo, onde id = 0 é o mais recente:

```
stash@{0}
stash@{1}
```

Aplicar o stash mais recente: ```git stash apply```

Aplicar um stash específico: ```git stash apply stash@{1}```

Aplicar e remover da lista: ```git stash pop```

Remover o stash mais recente: ```git stash drop```

Criar uma branch a partir de um stash: ```git stash branch nome-da-branch```

Criar a partir de um stash específico: ```git stash branch nome-da-branch stash@{1}```

---

## 8. Tags

Uma **tag** é uma referência para um commit específico. Diferentemente de uma branch, uma tag normalmente representa um ponto fixo do histórico.

É possível ter **mais de uma tag apontando para o mesmo commit**.

Listar tags: ```git tag``` ou ```git tag -l```

Mostrar descrição: ```git tag -n```

Mostrar detalhes de uma tag: ```git show v1.0```

Comparar tags: ```git diff v1.0 v2.0```

### 8.1. Lightweight Tag

Cria uma tag simples: ```git tag v1.0```

Ela aponta para o commit atual.

### 8.2. Annotated Tag

Possui informações adicionais, como autor, data e mensagem.

```git tag -a v1.0 -m "Primeira versão"```

### 8.3. Tags em commits específicos

Tag simples: 
```git tag v1.0 HASH```

Tag anotada: ```git tag -a v1.0 -m "Primeira versão" HASH```

### 8.4. Enviar tags para o remoto

Uma tag específica: ```git push origin v1.0```

Todas as tags: ```git push --tags```

### 8.5. Deletar tags

Localmente: ```git tag -d v1.0```

Remotamente: ```git push --delete origin v1.0```

### 8.5. Acessar uma versão por tag

```git checkout v1.0```

Isso coloca o repositório naquele ponto do histórico. Nesse caso, você pode ficar em um estado chamado **detached HEAD**, pois o `HEAD` não está apontando para uma branch.

---

## 9. SSH

SSH permite autenticar o computador em serviços como o GitHub sem precisar fornecer senha a cada operação.

Uma chave SSH normalmente possui chave privada e chave pública, onde a chave pública normalmente termina em ```
.pub```. É possível ter várias chaves SSH.

Para criar uma chave: ```ssh-keygen```

Ela normalmente será armazenada em: ```~/.ssh/```

### 9.1. ssh-agent

O `ssh-agent` pode armazenar chaves carregadas para que não seja necessário fornecê-las novamente durante a sessão.

Adicionar uma chave: ```ssh-add ~/.ssh/nome-da-chave```

A chave **pública** deve ser cadastrada no serviço, como o GitHub.

---

## 10. GitHub

### 10.1. Pull Request

Um **Pull Request (PR)** é uma solicitação para que alterações de uma branch sejam revisadas e incorporadas a outra branch. O PR permite:

* Revisar código.
* Fazer comentários.
* Discutir alterações.
* Executar testes/CI.
* Aprovar ou solicitar mudanças.
* Fazer merge.

### 10.2. Issues

**Issues** são utilizadas para registrar tarefas, bugs, melhorias e outras demandas. Uma issue pode receber:

* Labels.
* Assignees.
* Comentários.
* Milestones.

### 10.3. Milestones

**Milestones** são marcos utilizados para agrupar tarefas relacionadas a um objetivo. Podem ser usadas para organizar metas e versões do projeto, além de linkar com Issues

### 10.4. Force Push

O comando ```git push --force``` força o remoto a aceitar o histórico local. Isso pode sobrescrever alterações que estão no remoto.

Uma opção mais segura é: ```git push --force-with-lease```

O `--force-with-lease` verifica se o remoto ainda está no estado esperado antes de sobrescrever.

## 11. ```git bisect```

É uma ferramenta para encontrar qual commit introduziu um problema. Ela utiliza uma **busca binária** entre um commit que funciona e outro que não funciona. O Git testa commits intermediários para encontrar onde o problema apareceu.

Iniciar: ```git bisect start```

Marcar um commit como ruim: ```git bisect bad H```

Marcar um commit como bom: ```git bisect good A```

O Git selecionará um commit intermediário, até identificar o problema. Depois de encontrar o commit: ```git bisect reset```

## 12. Ignorar alterações de um arquivo localmente

```git update-index --skip-worktree arquivo```

Isso informa ao Git para evitar considerar alterações locais nesse arquivo em determinadas operações.

Para voltar ao comportamento normal: ```git update-index --no-skip-worktree arquivo```

> Isso **não é a mesma coisa que `.gitignore`**. O arquivo continua sendo rastreado pelo Git.

## 13. Arquivos não rastreados

Para remover arquivos não rastreados: ```git clean -f```

Para visualizar primeiro o que seria removido: ```git clean -n```

## 14. Aliases

Aliases permitem criar abreviações para comandos: ```git config --global alias.abv cmd```, onde "abv" é a abreviação desejada para o comando "cmd".

Para remover alias: ```git config --global --unset alias.abv```

---

## 15. Procurar branches

Para procurar branches cujo nome contenha determinado texto: ```git branch | grep algo```

---

## 16. Tabela rápida de comandos

| Comando           | Função                                       |
| ----------------- | -------------------------------------------- |
| `git init`        | Inicializa um repositório                    |
| `git clone`       | Clona um repositório                         |
| `git status`      | Mostra o estado atual                        |
| `git add`         | Adiciona alterações à Staging Area           |
| `git commit`      | Cria um commit                               |
| `git push`        | Envia commits para o remoto                  |
| `git pull`        | Busca e integra alterações do remoto         |
| `git fetch`       | Busca informações do remoto sem integrar     |
| `git log`         | Mostra o histórico                           |
| `git diff`        | Mostra diferenças                            |
| `git branch`      | Gerencia/lista branches                      |
| `git switch`      | Troca de branch                              |
| `git merge`       | Junta branches                               |
| `git rebase`      | Reaplica commits sobre outra base            |
| `git cherry-pick` | Traz um commit específico                    |
| `git stash`       | Guarda alterações temporariamente            |
| `git tag`         | Cria referências para versões                |
| `git revert`      | Desfaz alterações criando novo commit        |
| `git reset`       | Move o histórico/HEAD                        |
| `git clean`       | Remove arquivos não rastreados               |
| `git bisect`      | Encontra o commit que introduziu um problema |
| `git remote`      | Gerencia repositórios remotos                |
| `git config`      | Configura o Git                              |

---