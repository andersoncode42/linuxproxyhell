# Linux Proxy Hell
Um passo a passo pra sair desse inferno

## Scripts de Ambiente

### MeuProxy.sh

**Fonte:** https://wiki.archlinux.org/index.php/proxy_settings

**Descrição:** O objetivo desse script é reaproveitar o código chamando o script em outros arquivos

**Passo 1** - Crie o arquivo `~/MeuProxy.sh`

Conteúdo do arquivo
```bash
#!/bin/bash

# Sete as variaveis abaixo com os dados do proxy
usu="ProxyUsuario"
pass="ProxySenha"
ip="172.17.1.20"
porta="3128"
proxy="http://$usu:$pass@$ip:$porta"
proxy_excecoes="localhost,127.0.0.1,10.0.0.0/8,.dominio.com.br,172.0.0.0/8,192.0.0.0/8"

# Exportando para as variaveis de ambiente
export http_proxy="$proxy"
export https_proxy="$proxy"
export ftp_proxy="$proxy"
export rsync_proxy="$proxy"
export all_proxy="$proxy"
export no_proxy="$proxy_excecoes"

export HTTP_PROXY="$proxy"
export HTTPS_PROXY="$proxy"
export FTP_PROXY="$proxy"
export RSYNC_PROXY="$proxy"
export ALL_PROXY="$proxy"
export NO_PROXY="$proxy_excecoes"
```

**Passo 2** - Sete o arquivo como executavel
`chmod +x ~/MeuProxy.sh`

### bashrc

**Fonte:** https://help.ubuntu.com/community/EnvironmentVariables

**Descrição:**  The shell config file /etc/bash.bashrc is somet# Linux Proxy Hell
Um passo a passo pra sair desse inferno

## Scripts de Ambiente

### MeuProxy.sh

**Fonte:** https://wiki.archlinux.org/index.php/proxy_settings

**Descrição:** O objetivo desse script é reaproveitar o código chamando o script em outros arquivos

**Passo 1** - Crie o arquivo `~/MeuProxy.sh`

Conteúdo do arquivo
```bash
#!/bin/bash

# Sete as variaveis abaixo com os dados do proxy
usu="ProxyUsuario"
pass="ProxySenha"
ip="172.17.1.20"
porta="3128"
proxy="http://$usu:$pass@$ip:$porta"
proxy_excecoes="localhost,127.0.0.1,10.0.0.0/8,.dominio.com.br,172.0.0.0/8,192.0.0.0/8"

# Exportando para as variaveis de ambiente
export http_proxy="$proxy"
export https_proxy="$proxy"
export ftp_proxy="$proxy"
export rsync_proxy="$proxy"
export all_proxy="$proxy"
export no_proxy="$proxy_excecoes"

export HTTP_PROXY="$proxy"
export HTTPS_PROXY="$proxy"
export FTP_PROXY="$proxy"
export RSYNC_PROXY="$proxy"
export ALL_PROXY="$proxy"
export NO_PROXY="$proxy_excecoes"
```

**Passo 2** - Sete o arquivo como executavel
`chmod +x ~/MeuProxy.sh`

### bashrc

**Fonte:** https://help.ubuntu.com/community/EnvironmentVariables

**Descrição:**  The shell config file /etc/bash.bashrc is sometimes suggested for setting environment variables system-wide. While this may work on Bash shells for programs started from the shell, variables set in that file are not available by default to programs started from the graphical environment in a desktop session.

**Arquivos**
- `~/.bashrc` - Altera **APENAS** a configuração do seu usuário
- `/etc/bash.bashrc`- Altera a configuração de **TODOS** os seus usuário

Nesse exemplo, vamos mexer apenas no arquivo do usuário
`$ nano ~/.bashrc`

Adicione a linha abaixo, para chamar o script que você criou anteriormente
```bash
source ~/MeuProxy.sh
```

### /etc/profile.d/MeuProfile.sh

**Fonte:** https://bash.cyberciti.biz/guide/Setting_system_wide_shell_options

**Locais**
- `/etc/profile` - Arquivo
- `/etc/profile.d` - Pasta onde você pode incluir seus scriptsusuario

Neste exemplo, criaremos um script chamado `MeuProfile.sh` e adicionaremos ele na pasta `/etc/profile.d`

**Passo 1** - Abra o arquivo
`$ sudo nano /etc/profile.d/MeuProfile.sh`

**Passo 2** - Adicione a linha abaixo no arquivo
`source ~/MeuProxy.sh`

**Passo 3** - Dê permissão de execução ao arquivo
`$ sudo chmod +x /etc/profile.d/MeuProfile.sh`

## Arquivos de Configuração de Ambiente

### /etc/environment

**Descrição:** is used by the pam_env module and is shell agnostic so scripting or glob expansion cannot be used. The file only accepts variable=value pairs. 

Adicione no final do arquivo `/etc/enviroment` o conteúdo abaixo:

```text
http_proxy="http://ProxyUsuario:ProxySenha@172.17.1.20:3128"
HTTP_PROXY="http://ProxyUsuario:ProxySenha@172.17.1.20:3128"
https_proxy="http://ProxyUsuario:ProxySenha@172.17.1.20:3128"
HTTPS_PROXY="http://ProxyUsuario:ProxySenha@172.17.1.20:3128"
ftp_proxy="http://ProxyUsuario:ProxySenha@172.17.1.20:3128"
FTP_PROXY="http://ProxyUsuario:ProxySenha@172.17.1.20:3128"
all_proxy="http://ProxyUsuario:ProxySenha@172.17.1.20:3128"
ALL_PROXY="http://ProxyUsuario:ProxySenha@172.17.1.20:3128"
no_proxy="localhost,127.0.0.1,10.0.0.0/8,.dominio.com.br,172.0.0.0/8,192.0.0.0/8"
NO_PROXY="localhost,127.0.0.1,10.0.0.0/8,.dominio.com.br,172.0.0.0/8,192.0.0.0/8"
```

## Gerenciadores de Pacotes

### Gerenciador de Pacote - apt

Crie um arquivo de configuração e coloque no diretorio `/etc/apt/apt.conf.d/`

Neste exemplo, chamei o arquivo de `meuproxy`

**Passo 1** - Abra o arquivo
`$ sudo nano /etc/apt/apt.conf.d/meuproxy`

**Passo 2** - Adicione as duas linhas abaixo abaixo:Senha

```text
Acquire::http::Proxy  "http://ProxyUsuario:ProxySenha@172.17.1.20:3128";
Acquire::https::Proxy "http://ProxyUsuario:ProxySenha@172.17.1.20:3128";
```

### Gerenciador de Pacote - dnf

**Passo 1** - Abra o arquivo `/etc/dnf/dnf.conf`

**Passo 2** - Adicione as linhas abaixo
`proxy=http://ProxyUsuario:ProxySenha@172.17.1.20:3128`

Observação: Existe também os parametros `proxy_username` e `proxy_password` mas bastou o `proxy` e colocar o usuario e senha na url

**Importante**:

- Este arquivo **NÃO** aceita substituição de variavel.
- Use apenas texto plano e **SEM** aspas

### wget

Edite um dos arquivos abaixo:

- `/etc/wgetrc` para **TODOS** os usuários do computador
- `~/.wgetrc` **APENAS** paa seu usuário

Conteúdo do arquivo:
```text
https_proxy=http://ProxyUsuario:ProxySenha@172.17.1.20:3128/
http_proxy=http://ProxyUsuario:ProxySenha@172.17.1.20:3128/
ftp_proxy=http://ProxyUsuario:ProxySenha@172.17.1.20:3128/
```

**Importante**
Este arquivo **NÃO** aceita substituição de variavel.
Use apenas texto plano e **SEM** aspas

## Docker

## Docker - Daemon

**Fonte:** https://docs.docker.com/config/daemon/systemd/#httphttps-proxy

**Passo 1** - Crie o diretório abaixo
`sudo mkdir -p /etc/systemd/system/docker.service.d`

**Passo 2** -Crie o arquivo abaixo
`sudo touch /etc/systemd/system/docker.service.d/http-proxy.conf`

**Passo 3** - Edite o arquivo criado anteriormente
`sudo nano /etc/systemd/system/docker.service.d/http-proxy.conf`

```text
[Service]
Environment="HTTP_PROXY=http://USU:PASS@172.17.1.20:3128/"
Environment="HTTPS_PROXY=http://USU:PASS@172.17.1.20:3128/" 
Environment="NO_PROXY=localhost,127.0.0.1,10.0.0.0/8,.dominio.com.br,172.0.0.0/8,192.0.0.0/8"
```

**Passo 4** - Atualize o daemon
`sudo systemctl daemon-reload`### dnf

**Passo 5** - Reinicie o docker
`sudo systemctl restart docker`

**Testando**
`sudo systemctl show --property=Environment docker`

## Docker - Containers
**Fonte:** https://docs.docker.com/network/proxy/#configure-the-docker-client

Edite o arquivo `~/.docker/config.json`
```json
{
    "proxies":
    {
        "default":
        {
            "httpProxy":  "http://USU:PASS*@172.17.1.20:3128",
            "httpsProxy": "http://USU:PASS*@172.17.1.20:3128",
            "noProxy":    "localhost,127.0.0.1,10.0.0.0/8,.dominio.com.br,172.0.0.>
        }
    }
}
```

## Outras Ferramentas

### Maven
**Fonte:** http://maven.apache.org/guides/mini/guide-proxies.html

Edite o arquivo `~/.m2/settings.xml`
Observação: não precissa epecifica a tag `https`, a tag `http` vale para ambos

Conteúdo do arquivo
```xml
<settings>
  <proxies>
   <proxy>
      <id>MeuProxy</id>
      <active>true</active>
      <protocol>http</protocol>
      <host>172.17.1.20</host>
      <port>3128</port>
      <username>ProxyUsuario</username>
      <password>ProxySenha</password>
      <nonProxyHosts>localhost|127.0.0.1|10.0.0.0/8|*.dominio.com.br|172.0.0.0/8|192.0.0.0/8</nonProxyHosts>
    </proxy>
  </proxies>
</settings>
```imes suggested for setting environment variables system-wide. While this may work on Bash shells for programs started from the shell, variables set in that file are not available by default to programs started from the graphical environment in a desktop session.

**Arquivos**
- `~/.bashrc` - Altera **APENAS** a configuração do seu usuário
- `/etc/bash.bashrc`- Altera a configuração de **TODOS** os seus usuário

Nesse exemplo, vamos mexer apenas no arquivo do usuário
`$ nano ~/.bashrc`

Adicione a linha abaixo, para chamar o script que você criou anteriormente
```bash
source ~/MeuProxy.sh
```

### /etc/profile.d/MeuProfile.sh

**Fonte:** https://bash.cyberciti.biz/guide/Setting_system_wide_shell_options

**Locais**
- `/etc/profile` - Arquivo
- `/etc/profile.d` - Pasta onde você pode incluir seus scriptsusuario

Neste exemplo, criaremos um script chamado `MeuProfile.sh` e adicionaremos ele na pasta `/etc/profile.d`

**Passo 1** - Abra o arquivo
`$ sudo nano /etc/profile.d/MeuProfile.sh`

**Passo 2** - Adicione a linha abaixo no arquivo
`source ~/MeuProxy.sh`

**Passo 3** - Dê permissão de execução ao arquivo
`$ sudo chmod +x /etc/profile.d/MeuProfile.sh`

## Arquivos de Configuração de Ambiente

### /etc/environment

**Descrição:** is used by the pam_env module and is shell agnostic so scripting or glob expansion cannot be used. The file only accepts variable=value pairs. 

Adicione no final do arquivo `/etc/enviroment` o conteúdo abaixo:

```text
http_proxy="http://ProxyUsuario:ProxySenha@172.17.1.20:3128"
HTTP_PROXY="http://ProxyUsuario:ProxySenha@172.17.1.20:3128"
https_proxy="http://ProxyUsuario:ProxySenha@172.17.1.20:3128"
HTTPS_PROXY="http://ProxyUsuario:ProxySenha@172.17.1.20:3128"
ftp_proxy="http://ProxyUsuario:ProxySenha@172.17.1.20:3128"
FTP_PROXY="http://ProxyUsuario:ProxySenha@172.17.1.20:3128"
all_proxy="http://ProxyUsuario:ProxySenha@172.17.1.20:3128"
ALL_PROXY="http://ProxyUsuario:ProxySenha@172.17.1.20:3128"
no_proxy="localhost,127.0.0.1,10.0.0.0/8,.dominio.com.br,172.0.0.0/8,192.0.0.0/8"
NO_PROXY="localhost,127.0.0.1,10.0.0.0/8,.dominio.com.br,172.0.0.0/8,192.0.0.0/8"
```

## Gerenciadores de Pacotes

### Gerenciador de Pacote - apt

Crie um arquivo de configuração e coloque no diretorio `/etc/apt/apt.conf.d/`

Neste exemplo, chamei o arquivo de `meuproxy`

**Passo 1** - Abra o arquivo
`$ sudo nano /etc/apt/apt.conf.d/meuproxy`

**Passo 2** - Adicione as duas linhas abaixo abaixo:Senha

```text
Acquire::http::Proxy  "http://ProxyUsuario:ProxySenha@172.17.1.20:3128";
Acquire::https::Proxy "http://ProxyUsuario:ProxySenha@172.17.1.20:3128";
```

### Gerenciador de Pacote - dnf

**Passo 1** - Abra o arquivo `/etc/dnf/dnf.conf`

**Passo 2** - Adicione as linhas abaixo
`proxy=http://ProxyUsuario:ProxySenha@172.17.1.20:3128`

Observação: Existe também os parametros `proxy_username` e `proxy_password` mas bastou o `proxy` e colocar o usuario e senha na url

**Importante**:

- Este arquivo **NÃO** aceita substituição de variavel.
- Use apenas texto plano e **SEM** aspas

### wget

Edite um dos arquivos abaixo:

- `/etc/wgetrc` para **TODOS** os usuários do computador
- `~/.wgetrc` **APENAS** paa seu usuário

Conteúdo do arquivo:
```text
https_proxy=http://ProxyUsuario:ProxySenha@172.17.1.20:3128/
http_proxy=http://ProxyUsuario:ProxySenha@172.17.1.20:3128/
ftp_proxy=http://ProxyUsuario:ProxySenha@172.17.1.20:3128/
```

**Importante**
Este arquivo **NÃO** aceita substituição de variavel.
Use apenas texto plano e **SEM** aspas

## Docker

## Docker - Daemon

**Fonte:** https://docs.docker.com/config/daemon/systemd/#httphttps-proxy

**Passo 1** - Crie o diretório abaixo
`sudo mkdir -p /etc/systemd/system/docker.service.d`

**Passo 2** -Crie o arquivo abaixo
`sudo touch /etc/systemd/system/docker.service.d/http-proxy.conf`

**Passo 3** - Edite o arquivo criado anteriormente
`sudo nano /etc/systemd/system/docker.service.d/http-proxy.conf`

```text
[Service]
Environment="HTTP_PROXY=http://USU:PASS@172.17.1.20:3128/"
Environment="HTTPS_PROXY=http://USU:PASS@172.17.1.20:3128/" 
Environment="NO_PROXY=localhost,127.0.0.1,10.0.0.0/8,.dominio.com.br,172.0.0.0/8,192.0.0.0/8"
```

**Passo 4** - Atualize o daemon
`sudo systemctl daemon-reload`### dnf

**Passo 5** - Reinicie o docker
`sudo systemctl restart docker`

**Testando**
`sudo systemctl show --property=Environment docker`

## Docker - Containers
**Fonte:** https://docs.docker.com/network/proxy/#configure-the-docker-client

Edite o arquivo `~/.docker/config.json`
```json
{
    "proxies":
    {
        "default":
        {
            "httpProxy":  "http://USU:PASS*@172.17.1.20:3128",
            "httpsProxy": "http://USU:PASS*@172.17.1.20:3128",
            "noProxy":    "localhost,127.0.0.1,10.0.0.0/8,.dominio.com.br,172.0.0.>
        }
    }
}
```

## Outras Ferramentas

### Maven
**Fonte:** http://maven.apache.org/guides/mini/guide-proxies.html

Edite o arquivo `~/.m2/settings.xml`
Observação: não precissa epecifica a tag `https`, a tag `http` vale para ambos

Conteúdo do arquivo
```xml
<settings>
  <proxies>
   <proxy>
      <id>MeuProxy</id>
      <active>true</active>
      <protocol>http</protocol>
      <host>172.17.1.20</host>
      <port>3128</port>
      <username>ProxyUsuario</username>
      <password>ProxySenha</password>
      <nonProxyHosts>localhost|127.0.0.1|10.0.0.0/8|*.dominio.com.br|172.0.0.0/8|192.0.0.0/8</nonProxyHosts>
    </proxy>
  </proxies>
</settings>
```
