# Guia de Git e GitHub

## 1. O que é Git?

Git é um sistema de controle de versão distribuído, que permite:

* Registrar alterações no código;
* Voltar para versões anteriores;
* Trabalhar com diferentes versões do projeto através de branches;
* Trabalhar em equipe;
* Sincronizar alterações entre repositórios locais e remotos.

### 1.1 Distribuído x Centralizado

Os sistemas distribuídos são caracterizados por cada desenvolvedor possuir uma cópia completa do repositório, sendo possível trabalhar offline. Cada cópia possui o histórico do projeto, assim, 
não depende constantemente de um servidor central.

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

## 2. Comandos

### 2.1 ```git add```

Adiciona alterações à Staging Area.

```git add arquivo.txt```

Adicionar todos os arquivos:

* git add .
* git add --all
* git add -a (abreviação da opção a cima)

> git add . adiciona alterações da árvore a partir do diretório atual. git add --all é mais abrangente e considera alterações em todo o repositório.

---

### 2.2 ```git commit```

Cria um novo ponto no histórico do projeto.

```git commit -m "Adiciona sistema de login"```

Um commit possui, entre outras informações:

* Identificador (hash);
* Autor;
* Data;
* Mensagem;
* Alterações registradas.

#### 2.2.1 Alterar a mensagem do commit:

git commit --amend -m "Nova mensagem"

#### 2.2.2. Adicionar um arquivo que foi esquecido no último commit:

```
git add arquivo.txt
git commit --amend --no-edit
```

O --no-edit mantém a mensagem anterior.

#### 2.2.3. Sem --no-edit:

```git commit --amend```

O Git abrirá o editor configurado para permitir alterar a mensagem.

> Para usar VS Code como editor padrão:
> ```git config --global core.editor "code --wait"```

---

### 2.3. Configuração do usuário

> É interessante que seja o mesmo nome e e-mail do GitHub

Configurar nome:

```git config --global user.name "Seu Nome"```

Configurar e-mail:

```git config --global user.email "seu@email.com"```

Visualizar as configurações:

```git config --global --list```

### 2.4. Histórico de commits

Mostra o histórico:

```git log```

Normalmente apresenta:

* Hash do commit;
* Autor;
* Data;
* Mensagem;
* `HEAD`, indicando a posição atual.

O histórico aparece do commit mais recente para o mais antigo. A forma resumido é:

```git log --oneline```

Além disso, para mostrar os últimos N commits

```git log -n 3```

> Também pode combinar: 
> ```git log -n 3 --oneline```

Para mostrar alterações de cada commit

```git log --patch``` ou ```git log -p```

Já para mostrar quais arquivos foram alterados e estatísticas de adições e remoções: 

```git log --stat```

---




### 2.5. ```git diff```

Compara o estado atual dos arquivos com a última versão registrada no Git.

```git diff```

Caso o arquivo já esteja na Stagind Area:

```git diff --staged``` ou ```git diff --cached```

---

### 2.6. Reverter alterações

Restaurar um arquivo para o último commit:

```git restore arquivo.txt```

Para restaurar todos:

```git restore .```

> Isso descarta alterações não commitadas nesses arquivos.

### 2.7. Retirar um arquivo do `git add`

Se quer retirar o arquivo da Staging Area:

```git restore --staged arquivo.txt```

Também é possível usar:

```git rm --cached arquivo.txt```

Porém, `git rm --cached` tem outro uso importante: ele remove o arquivo do rastreamento do Git, mantendo o arquivo fisicamente no computador.

Para remover todos recursivamente do rastreamento:

```git rm --cached -r .```

### 2.8. ```git reset```

O `reset` move o `HEAD` para outro ponto do histórico.

#### Soft

```git reset --soft HEAD~1```

Volta um commit, mas mantém as alterações na **Staging Area**.

#### Mixed

```git reset --mixed HEAD~1```

Volta um commit e mantém as alterações no Working Directory, mas tira da Staging Area.

> `HEAD~1` significa um commit antes do atual.

#### Hard

```git reset --hard HEAD~1```

Volta um commit e descarta as alterações daquele commit no Working Directory e na Staging Area.

---

### 2.9. ```git revert```

Diferente do `reset`, o `revert` **não apaga o commit anterior do histórico**.

```git revert HEAD```

Ele cria um **novo commit** que desfaz as alterações de um commit anterior. Para aceitar automaticamente a mensagem padrão:

```git revert HEAD --no-edit```

---

## 3. Branches

Uma **branch** é uma ramificação do histórico do projeto. Cada novo commit é criado na branch atual.

### 3.1. Listar branches

```git branch``` ou ```git branch --list```

A branch atual aparece com `*`.

### 3.2. Criar e trocar de branch

Criar: ```git branch nome-da-branch```

Trocar: ```git checkout nome-da-branch```

Forma moderna: ```git switch nome-da-branch```

> Voltar para a branch anterior: ```git switch -```

Criar e trocar ao mesmo tempo: ```git checkout -b nome-da-branch``` ou ```git switch -c nome-da-branch```

> `git switch` foi criado especificamente para trabalhar com branches pois `git checkout` possui funções adicionais, como restaurar arquivos e acessar commits.

### 3.3. Trocar de branch descartando alterações

```git checkout -f nome-da-branch```

Força a troca de branch e descarta alterações locais que impediriam a troca.

### 3.4. Renomear branches

Renomear uma branch: ```git branch -m nome-antigo nome-novo```

Para renomear a branch atual:

```git branch -m novo-nome```

> O nome da branch remota não é alterado com um comando único. Normalmente, renomeia localmente, remove a antiga no remoto e envia a nova.

### 3.5. Deletar branches

Branch local: ```git branch -d nome```

Forçar: ```git branch -D nome```

Branch remota: ```git push --delete origin nome```

Isso remove a referência da branch no repositório remoto.

### 3.6. Consultar commits de uma branch

```git log nome-da-branch```

Também é possível trocar para a branch e executar:

```
git switch nome-da-branch
git log
```

### 3.6. Merge

O `merge` incorpora as alterações de uma branch na branch atual. Se estiver na `main`:

```
git switch main
git merge feature
```

A ideia é trazer as alterações de `feature` para `main`.

**Importante:** o `merge` sempre traz as alterações da branch indicada **para a branch em que está**.

### 3.7. Verificar branches mergeadas

Branches que ainda não foram mergeadas na branch atual: ```git branch --no-merged```

Branches que já foram mergeadas: ```git branch --merged```

### 3.8. Conflitos de merge

Um conflito acontece quando o Git não consegue decidir automaticamente como combinar alterações. 

Depois de resolver manualmente os arquivos:

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
