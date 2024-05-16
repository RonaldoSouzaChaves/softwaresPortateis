# Softwares portáteis

Guia para a configuração da versão portátil de alguns softwares no *Windows 10*.

- [Git](#git)
  - [Git + Visual Studio Code](#git--visual-studio-code)
  - [Configurando o Git](#configurando-o-git)
- [LaTeX](#latex)
  - [LaTeX + Visual Studio Code](#latex--visual-studio-code)
- [MongoDB](#mongodb)
- [PostgreSQL](#postgresql)
- [Python](#python)
  - [Python + Visual Studio Code](#python--visual-studio-code)
- [R](#r)
  - [R + Visual Studio Code](#r--visual-studio-code)
- [Visual Studio Code](#visual-studio-code)

## Git

O download da versão portátil do *Git* pode ser feito através do site oficial, estando localizado logo abaixo do download da versão instalável. Basta baixar o arquivo ```.zip``` e extraí-lo para a pasta desejada.

### Git + Visual Studio Code

Para usar o *Git* portátil no *VS Code*:

1. Abra as configurações do *VS Code*, abrindo em seguida o arquivo ```settings.json``` (este é o arquivo que armazena as configurações do *VS Code*, podendo ser encontrado na aba de configurações);
2. Dentro do ```settings.json```, insira o código a seguir:
```
"git.ignoreMissingGitWarning": true,
"git.enabled": true,
"git.path": "C:\\caminhoDoGit\\bin\\git.exe",
"terminal.integrated.shell.windows": "C:\\caminhoDoGit\\bin\\bash.exe",
"terminal.integrated.profiles.windows": {
        "Git Bash": {
        "path": "C:\\caminhoDoGit\\Git\\bin\\bash.exe",
        "args": [],
        //"icon": "C:\\caminhoDoGit\\Git\\mingw64\\share\\git\\git-for-windows.ico",
        "cwd": "${workspaceFolder}",
        "confirmation": "shell",
        "name": "Git Bash"
        }
    },
"terminal.integrated.defaultProfile.windows": "Git Bash"
```

### Configurando o Git

- Definindo o nome de usuário:
```
git config --global user.name "seuNome"
```
- Definindo o e-mail do usuário:
```
git config --global user.email seu@email.com
```
- Verificando qual é o atual editor de texto padrão:
```
git config --global core.editor
```
- Removendo o atual editor de texto padrão:
```
git config --global --unset core.editor
```
- Definindo uma versão portátil do *Visual Studio Code* como editor de texto padrão:
```
git config --global core.editor "'C:/caminhoDoVSCode/Code.exe' --wait"
```
- Definindo *main* como nome padrão da branch principal:
```
git config --global init.defaultBranch main
```
- Verificando as configurações atuais:
```
git config --global --list
```

## LaTeX

1. Procure por *TinyTex* no *GitHub* ou no *Google*. Feito o download, execute o arquivo ```install-bin-windows.bat```;
2. Após a execução, a pasta com o conteúdo do *TinyTex* estará localizada em ```C:/Users/<seuUsuário>/AppData/Roaming```. Caso queira mover o *TinyTex* para outro diretório, basta mover a pasta citada anteriormente para onde desejar e, em seguida, adicionar ao *path* do *Windows* o novo caminho para o executável *tlgmr*. Para tal, abra o *CMD* na pasta na qual está localizado o *TinyTex*, e execute o seguinte comando:
```
"C:\caminhoDoTinyTex\TinyTeX\bin\windows\tlmgr" path add
```
3. O passo 2 garante que o caminho para o *tlgmr* seja adicionado ao *path* do usuário atual, e não ao *path* global, o que pode acarretar em problemas. Se caso após a execução do passo 3, o *TinyTex* não estiver funcionando corretamente, adicione o caminho da pasta na qual está localizado o *tlgmr* ao *path* global manualmente. Basta ir nas configurações de variáveis de ambiente do sistema e adicionar o seguinte caminho ao *path* global:
```
C:\caminhoDoTinyTex\TinyTeX\bin\windows
```

### LaTeX + Visual Studio Code

Para usar o *TinyTex* no *VS Code*, basta instalar a extensão *LaTeX Workshop* no *VS Code*.

## MongoDB

1. Faça o download das versões ```.zip``` do *Community Server* e do *Compass* no site oficial do *MongoDB*;
2. Extraia ambos os arquivos para as pastas que desejar. Não use espaços em branco nos nomes das pastas;
3. Dentro da pasta raiz do *Community Server*, crie uma nova pasta chamada ```mongoData```. Esta pasta será usada para armazenar os dados do *MongoDB*;
4. Ainda na pasta raiz do *Community Server*, localize a pasta ```bin``` e, dentro dela, crie um arquivo de texto chamado ```mongo```. Dentro deste arquivo insere-se o caminho da pasta ```mongoData```, para que o *MongoDB* armazene seus dados nela. Para isso, insira o script abaixo no arquivo ```mongo``` e, feito isso, salve-o no formato ```.conf```;
```
storage:
    dbPath: C:\caminhoParaOMongoDB\mongoData
```
5. Crie um arquivo ```.bat``` chamado ```startMongoDBServer``` para automatizar a inicialização do *Community Server*. Para tal, insira no arquivo ```.bat``` criado o script abaixo e, feito isso, salve o arquivo na pasta ```bin``` do *Community Server*;
```
@echo off
cd /d C:/caminhoDoCommunityServer/bin
mongod --config C:/caminhoDoCommunityServer/bin/mongo.conf
```
6. Pronto. Para usar o *MongoDB*, basta abrir o ```startMongoDBServer.bat``` (não execute como administrador, apenas abra o arquivo) e deixar o terminal que é executado pelo ```.bat``` aberto enquanto o *MongoDB* for usado. Após usar o *MongoDB*, encerre o terminal usando o comando ```Ctrl + C```.

## PostgreSQL

1. Vá ao portal de downloads do *PostgreSQL* e faça o download de seus arquivos binários (que são os arquivos ```.zip```);
2. Extraia o conteúdo do ```.zip``` para sua pasta de preferência;
3. Crie um arquivo ```.bat``` chamado ```init``` na pasta raiz do *PostgreSQL*. Nele, insira o seguinte script:
```
@ECHO ON
@SET PATH="%CD%\bin";%PATH%
@SET PGDATA=%CD%\data
@SET PGDATABASE=postgres
@SET PGUSER=postgres
@SET PGPORT=5432
@SET PGLOCALEDIR=%CD%\share\locale
REM descomente a linha abaixo somente na primeira vez que o servidor for iniciado
REM %CD%\bin\initdb -U postgres -A trust -E UTF8
%CD%\bin\pg_ctl -D %CD%/data -l logfile start
ECHO "aperte ENTER para interromper o servidor"
pause
%CD%\bin\pg_ctl -D %CD%/data stop
```
4. Abra o arquivo ```init.bat``` para que a configuração do *PostgreSQL* seja iniciada. Atente-se a simplesmente abrir o arquivo, não executando-o como administrador;
5. Pronto. Para usar o *PostgreSQL*, basta abrir o ```init.bat``` (não execute como administrador, apenas abra o arquivo) e deixar o terminal que é executado pelo ```.bat``` aberto enquanto o *PostgreSQL* for usado. Após usar o *PostgreSQL*, encerre o terminal apertando a tecla ```ENTER```. [Fonte](https://github.com/malexple/postgresql-portable).

## Python

1. Procure por *WinPython* no *GitHub* ou no *Google*;
2. Faça o download do arquivo ```.exe``` encontrado nas releases do repositório *WinPython* no *GitHub*. Este arquivo ```.exe``` é um arquivo de auto-extração do *7-Zip*, ou seja, um arquivo ```.zip```;
3. Extraia o ```.zip``` para a pasta que desejar.

### Python + Visual Studio Code

Para usar o *WinPython* no *VS Code*:

1. Vá nas configurações do *VS Code*, e na barra de busca, procure por ```interpreter``` ou ```interpretador```;
2. Procure um bloco de configuração entitulado ```Python: Default Interpreter Path```. É aqui que se diz ao *VS Code* onde está localizado o *WinPython*;
3. Escreva o caminho para a pasta ```python-3.11.5.amd64``` (ou equivalente) do *WinPython*, seguindo o formato abaixo:
```
C:\caminhoDoWinPython\python-3.11.5.amd64
```

## R

Faça o download [desta](https://github.com/selkamand/r-portable-windows) versão portátil do *R* e extraia o arquivo ```.zip``` para a pasta que desejar.

### R + Visual Studio Code

Para usar o *R* portátil no *VS Code*:

1. Vá nas configurações do *VS Code*, e na barra de busca, procure por ```rpath```;
2. Procure um bloco de configuração chamado ```Rpath: Windows```. É aqui que se diz ao *VS Code* onde está localizado o *R*;
3. Escreva o caminho para o arquivo ```R.exe```, seguindo o formato abaixo:
```
C:\caminhoDoR\bin\R.exe
```
4. Repita os passos 1, 2 e 3, mas dessa vez, altere o ```rterm``` ao invés do ```rpath```. O caminho para o arquivo ```R.exe``` neste caso é diferente:
```
C:\caminhoDoR\bin\x64\R.exe
```
5. Feito isso, instale a extensão *R* no *VS Code*.

## Visual Studio Code

1. Faça o download da versão ```.zip``` do *VS Code* no portal oficial;
2. Extraia o arquivo ```.zip``` para a pasta que desejar;
3. Na pasta raiz do *VS Code*, crie uma nova pasta entitulada ```data```. Esta pasta é que armazenará todas as configurações do *VS Code*. Perder esta pasta significa perder as configurações do *VS Code*.