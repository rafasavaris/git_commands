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