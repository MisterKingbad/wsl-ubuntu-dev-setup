# Configuração completa do ambiente WSL

Este tutorial configura um ambiente de desenvolvimento no WSL com:

- Ubuntu
- Dependências do sistema
- Git
- Pyenv
- Python
- Poetry
- NVM
- Node.js
- SSH para GitHub

## 1. Instalação do WSL

Abra o **PowerShell como Administrador**.

Primeiro, veja as distribuições disponíveis:

```powershell
wsl --list --online
```

Instale a versão mais recente do Ubuntu disponível, usando exatamente o nome apresentado pelo comando anterior.

Exemplo para Ubuntu 26.10:

```powershell
wsl --install Ubuntu-26.10
```

Depois da instalação, reinicie o computador caso o Windows solicite.

Abra o Ubuntu e finalize a criação do usuário e senha do Linux.

## 2. Atualização do sistema

Dentro do Ubuntu/WSL:

```bash
sudo apt update && sudo apt upgrade -y
```

## 3. Instalação das dependências

Instale Git, compiladores e as bibliotecas necessárias para compilar versões do Python usando o Pyenv:

```bash
sudo apt install -y \
git \
build-essential \
libssl-dev \
zlib1g-dev \
libbz2-dev \
libreadline-dev \
libsqlite3-dev \
wget \
curl \
llvm \
libncurses-dev \
xz-utils \
tk-dev \
liblzma-dev \
libgdbm-dev \
libnss3-dev \
libffi-dev
```

Atualize novamente:

```bash
sudo apt update && sudo apt upgrade -y
```

Verifique o Git:

```bash
git --version
```

## 4. Instalação do Pyenv

Instale o Pyenv:

```bash
curl -fsSL https://pyenv.run | bash
```

Abra o `.bashrc`:

```bash
sudo nano ~/.bashrc
```

No final do arquivo, adicione:

```bash
export PYENV_ROOT="$HOME/.pyenv"
[[ -d $PYENV_ROOT/bin ]] && export PATH="$PYENV_ROOT/bin:$PATH"
eval "$(pyenv init - bash)"
```

Salve usando:

```text
CTRL + X
YES
ENTER
```

Recarregue o shell:

```bash
. ~/.bashrc
```

Verifique:

```bash
pyenv --version
```

## 5. Instalação do Python

Veja as versões disponíveis:

```bash
pyenv install -l | less
```

Para sair da listagem:

```text
q
```

Instale o Python desejado.

Exemplo:

```bash
pyenv install 3.12.12
```

Defina como versão global:

```bash
pyenv global 3.12.12
```

Recarregue o shell:

```bash
. ~/.bashrc
```

Confira:

```bash
python --version
```

ou:

```bash
python3 --version
```

Também confira:

```bash
pyenv versions
```

## 6. Instalação do Poetry

Instale o Poetry:

```bash
curl -sSL https://install.python-poetry.org | python3 -
```

Após a instalação, será necessário adicionar o Poetry ao `PATH`.

Abra o `.bashrc`:

```bash
sudo nano ~/.bashrc
```

No final do arquivo, adicione:

```bash
export PATH="$HOME/.local/bin:$PATH"
```

Salve usando:

```text
CTRL + X
YES
ENTER
```

Atualize o shell:

```bash
. ~/.bashrc
```

Verifique se o Poetry está disponível:

```bash
poetry --version
```

Caso queira verificar onde ele está instalado:

```bash
which poetry
```

## 7. Instalação do NVM

Instale o NVM:

```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.4/install.sh | bash
```

Abra novamente o `.bashrc`:

```bash
sudo nano ~/.bashrc
```

Confirme se estas linhas foram adicionadas pelo instalador:

```bash
export NVM_DIR="$([ -z "${XDG_CONFIG_HOME-}" ] && printf %s "${HOME}/.nvm" || printf %s "${XDG_CONFIG_HOME}/nvm")"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"
```

Caso não estejam presentes, adicione-as no final do arquivo.

Salve usando:

```text
CTRL + X
YES
ENTER
```

Recarregue:

```bash
. ~/.bashrc
```

Verifique:

```bash
nvm --version
```

## 8. Instalação do Node.js

Liste as versões disponíveis:

```bash
nvm ls-remote
```

Instale a versão desejada.

Exemplo:

```bash
nvm install 22.23.2
```

Defina como padrão:

```bash
nvm alias default 22.23.2
```

Ative:

```bash
nvm use 22.23.2
```

Confira:

```bash
node --version
npm --version
```

## 9. Configuração do Git

Configure seu nome:

```bash
git config --global user.name "Seu Nome"
```

Configure seu e-mail:

```bash
git config --global user.email "seu-email@exemplo.com"
```

Confira:

```bash
git config --global --list
```

## 10. Criar chave SSH para o GitHub

Primeiro, veja se já existem chaves:

```bash
ls -la ~/.ssh
```

Crie uma nova chave SSH:

```bash
ssh-keygen -t ed25519 -C "seu-email@exemplo.com"
```

Quando aparecer:

```text
Enter file in which to save the key
```

Pressione:

```text
ENTER
```

Para utilizar o caminho padrão:

```text
~/.ssh/id_ed25519
```

Depois será solicitada uma senha para proteger a chave.

Você pode definir uma senha ou simplesmente pressionar `ENTER` duas vezes para deixar sem senha.

## 11. Iniciar o SSH Agent

Inicie o `ssh-agent`:

```bash
eval "$(ssh-agent -s)"
```

Adicione sua chave:

```bash
ssh-add ~/.ssh/id_ed25519
```

Confira:

```bash
ssh-add -l
```

## 12. Copiar chave SSH

Exiba a chave pública:

```bash
cat ~/.ssh/id_ed25519.pub
```

A saída será parecida com:

```text
ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAA... seu-email@exemplo.com
```

Copie toda a linha.

Outra opção no WSL é copiar diretamente para a área de transferência do Windows:

```bash
cat ~/.ssh/id_ed25519.pub | clip.exe
```

## 13. Adicionar chave SSH ao GitHub

No GitHub, acesse:

```text
Settings
SSH and GPG keys
New SSH key
```

No campo **Title**, coloque algo que identifique o computador.

Exemplo:

```text
Notebook WSL
```

Em **Key type**, mantenha:

```text
Authentication Key
```

Cole a chave pública no campo **Key**.

Depois clique em:

```text
Add SSH key
```

## 14. Testar conexão SSH com GitHub

Execute:

```bash
ssh -T git@github.com
```

Na primeira conexão poderá aparecer:

```text
Are you sure you want to continue connecting (yes/no/[fingerprint])?
```

Digite:

```text
yes
```

Se tudo estiver correto, será exibida uma mensagem semelhante a:

```text
Hi usuario! You've successfully authenticated, but GitHub does not provide shell access.
```

Isso significa que a autenticação SSH está funcionando.

## 15. Clonar projetos usando SSH

No GitHub, utilize a opção **SSH** ao copiar o endereço do repositório.

O endereço será parecido com:

```text
git@github.com:empresa/projeto.git
```

Clone:

```bash
git clone git@github.com:empresa/projeto.git
```

## 16. Verificação final

### Git

```bash
git --version
```

### SSH

```bash
ssh -T git@github.com
```

### Pyenv

```bash
pyenv --version
```

### Python

```bash
python --version
```

### Poetry

```bash
poetry --version
```

### NVM

```bash
nvm --version
```

### Node

```bash
node --version
```

### NPM

```bash
npm --version
```

O ambiente WSL estará pronto para trabalhar.
