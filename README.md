# jornada-de-dados-23-24

# Documentação do projeto

https://dcgallo.github.io/jornada-de-dados-23-24/


# Como este repositório foi criado?

Seguem abaixo os passos que segui para criar um repositório local sincronizado com o Github DO ZERO, desde a instalação do WSL2 no Windows até a sincronização com o Github.


# WSL2

Executar o Power Shell no Windows
Executar o comando "wsl --install"
Reboot
Executar novamente o Power Shell no Windows
Executar "WSL"
Definir usuário e senha

# Git e Github - versionamento

Pré-requisito:
WSL (SO Linux) ou Power Shell (PS)
Git instalado:

Instalação do Git:
No prompt executar:
- sudo apt-get update (atualiza a versão do apt-get)
- sudo apt-get install git (instala o git)

Todos os comandos abaixo são executados em prompt (WSL ou PS)

Se não tem ainda uma chave de ssh criada no Git:
- Rodar comando: git config --global user.name "<seu nome>"
- Rodar comando: git config --global user.email <seu e-mail no github>
- Configurar branch principal do git local como "main" - rode o comando: "git config --global init.defaultBranch main"
- Ir no diretório .ssh do usuário:/home/<usuario>/.ssh - se não tiver o .ssh criar -> mkdir .ssh
- Rodar comando: ssh-keygen -t ed25519 -C "<seu e-mail do github>"
- Copiar resultado do comando acima ou o conteúdo do arquivo /home/<usuario>/.ssh/id_ed25519.pub: 
  Exemplo do conteúdo do arquivo id_ed25519.pub: 
    ssh-ed25519 KLJLijsdlkska983274kldsfhjsosaklvnvoisof9u3//ds887sfhhl\klkowiea\8q3 <seu e-mail no github>
- No Github ir no seu perfil e em Settings
- Clicar na opção SSH and GPG Keys 
- Criar uma nova chave em "New SSH Keys" inserindo o nome e conteúdo do arquivo id_ed25519.pub

Criar um repositório no Github:
- No seu perfil do Github (github.com/<seu usuario> vá em "Repositories"
- New
- Informe o nome do repositório <repositorio-do-projeto> (sem espaços em branco)
- Selecione a opção (Public ou Private)

Criar repositorio Git local:
- Vá ao diretório onde quer criar o seu projeto
- criar pasta <local do projeto> -> mkdir <projeto>
- git init
- git remote add origin git@github.com:git-bpo/<repositorio-do-projeto>.git
- git branch -M main (para garantir que a branch do Github seja a main e não a master)

Adicionando novos arquivos no diretório ao repositório:
- git add .
- git commit -a -m "mensagem"
- git push origin <branch>

Atualizando diretório local com alterações no repositório:
- git pull origin <branch>
  
Deletando arquivo local e no repositório:
- git rm -r "arquivo" (-r usado se tiver subdiretório)
- git commit -m "Mensagem opcional"
- git push -u origin <branch>

Criando uma nova branch:
- git checkout -b <new-branch>
- git add . (para atualizar (deletar, alterar ou criar) os arquivos do diretório local na nova branch)
- git commit -m "Mensagem opcional" (para atualizar o git que tem novos arquivos na nova branch)
- git push -u origin <new-branch> (para atualizar o Github com a nova branch)
  
Atualizando diretorio local em relação ao repositório remoto:
- git pull --set-upstream origin <branch>

Alternando entre branches:
- git switch <branch>

Voltando versão do repositório:
- git log -> mostra as versões do repositório

  Exemplo:
  $ git log
  commit c4331ca5f8fec909b18da51db4ad222be889b11b (HEAD -> main, origin/main)
  Author: Danilo Gallo <dcgallo@gmail.com>
  Date:   Tue Nov 28 14:53:12 2023 -0300

      atualizando repositório

  commit 6d2033b186d3a39a808d052c60fc085bb2eb5717
  Author: Danilo Gallo <dcgallo@gmail.com>
  Date:   Fri Nov 24 15:14:52 2023 -0300

      first commit
  gallo@DESKTOP-J6K7ASH:~/workshop-projeto-dados$ 

Observação: antes de voltar uma versão pode criar uma nova branch para preservar a atual:
- git switch -c <nova-branch> -> cria uma nova branch preservando os commit já efetuados

Volta a versão do checkout
- git checout "<id do commit>" -> exemplo acima do 1º commit "6d2033b186d3a39a808d052c60fc085bb2eb5717"


# Configuração do ambiente WSL para projetos

1-Criar diretório para o projeto
mkdir <projeto>

2-Criar diretório .pyenv no home
cd ~/<projeto>
mkdir .pyenv
cd .pyenv

3- Instalar do Pyenv (este passo é para ser feito apenas na 1ª vez em um mesmo ambiente, não é necessário fazer para cada projeto na mesma máquina)
Fazer download do Pyenv
git clone https://github.com/pyenv/pyenv.git ~/.pyenv

4-Configurar o ambiente para uso do Pyenv
echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.profile
echo 'command -v pyenv >/dev/null || export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.profile
echo 'eval "$(pyenv init -)"' >> ~/.profile
echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.bash_profile
echo '[[ -d $PYENV_ROOT/bin ]] && export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.bash_profile
echo 'eval "$(pyenv init -)"' >> ~/.bash_profile

5-Instalar pre-requisitos para fazer o build do python
sudo apt update
sudo apt install build-essential libssl-dev zlib1g-dev \
libbz2-dev libreadline-dev libsqlite3-dev curl \
libncursesw5-dev xz-utils tk-dev libxml2-dev libxmlsec1-dev libffi-dev liblzma-dev

reiniciar o wsl
sudo shutdown -h 0

6-Instalar python
pyenv install 3.11.3
--> se der um erro BUILD FAILED executar comando "sudo apt-get install make", pois o make não é default no Ubuntu

para usar o comando 'python' em vez de 'python3' onde 3 é a versão principal use:
echo 'alias python="python3"' >> ~/.bashrc
source ~/.bashrc

7-definir a versão do python a utilizar no projeto específico:

Vá ao diretório do projeto:
cd ~
cd <diretório do projeto>

Execute a instalação do python na versão desejada:
pyenv local 3.11.3
--> será criado um arquivo .python-version que contém a versão local do python desse diretório
--> se sair do diretório a versão do python será a versão global

8-Instalar o Poetry para instalação e gerenciamento de dependencias de versão
pip install poetry
poetry config virtualenvs.in-project true
poetry init -> irá solicitar as informações abaixo
----
This command will guide you through creating your pyproject.toml config.

Package name [workshop-projeto-dados]:  
Version [0.1.0]:  
Description []:  Projeto feito no workshop do Luciano Galvão
Author [Danilo Gallo <dcgallo@gmail.com>, n to skip]:  
License []:  
Compatible Python versions [^3.11]:  

Would you like to define your main dependencies interactively? (yes/no) [yes] 
You can specify a package in the following forms:
  - A single name (requests): this will search for matches on PyPI
  - A name and a constraint (requests@^2.23.0)
  - A git url (git+https://github.com/python-poetry/poetry.git)
  - A git url with a revision (git+https://github.com/python-poetry/poetry.git#develop)
  - A file path (../my-package/my-package.whl)
  - A directory (../my-package/)
  - A url (https://example.com/packages/my-package-0.1.0.tar.gz)

Package to add or search for (leave blank to skip): 

Would you like to define your development dependencies interactively? (yes/no) [yes] 
Package to add or search for (leave blank to skip): 

Generated file

[tool.poetry]
name = "workshop-projeto-dados"
version = "0.1.0"
description = "Projeto feito no workshop do Luciano Galvão"
authors = ["Danilo Gallo <dcgallo@gmail.com>"]
readme = "README.md"

[tool.poetry.dependencies]
python = "^3.11"


[build-system]
requires = ["poetry-core"]
build-backend = "poetry.core.masonry.api"


Do you confirm generation? (yes/no) [yes]

^^^^^^
Será criado um arquivo pyproject.toml com as informações acima
OBSERVAÇÃO: Para gerenciar instalações necessário utilizar o "poetry add" ou "poetry remove"
----
poetry env use 3.11.3
poetry shell (ativa o anviente virtual)

9-Se não for utilizar o Poetry deve criar um ambiente virtual no diretório do projeto manualmente
python -m venv .venv
. ./.venv/bin/activate -> ativa o ambiente virtual


10-Configurar o diretório com o Github - consultar texto 'configuracao github.txt'
*** ainda não atualizar ***

11-Configurar vscode para apresentar a pasta .git (pode usar para outras pastas e arquivos)
- criar pasta .vscode
- ir para o diretório .vscode
- criar um arquivo settings.json com o conteúdo:
{
    "files.exclude": {
        "**/.git": false
    }
}

12-Configurar o .gitignore (para que arquivos e pastas não serem atualizadas no git)
- criar arquivo .gitignore na pasta raiz
- incluir os nomes de arquivos e pastas que vc quer que sejam ignorados na atualização do git
Dica: baixar um arquivo pronto com o conteúdo para o .gitignore em https://www.toptal.com/developers/gitignore informar "python" e clicar em Criar e copiar o conteúdo gerado para dentro do seu arquivo .gitignore

13-incronizar seu repositório local com o Github
Ir ao diretório raíz do projeto
git add .
git commit -m "Primeira sincronização do repositório local com o Github"
git push








