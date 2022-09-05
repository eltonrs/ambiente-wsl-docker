# Ambiente WSL + Docker

Premissas

- A versão do WSL será 2, na qual já está contida na build 18971 ou superior do Windows 10.
- Os comandos serão executados todos como administrador e através do PowerShell.
- Os comandos, tanto do Windows quanto do Linux, serão executados pelo Windows Terminal. A instalação é simples, basta acessar a [loja](https://www.microsoft.com/pt-br/p/windows-terminal/9n0dx20hk701#activetab=pivot:overviewtab) e clicar em _Obter_.

## 1 - Configurações iniciais do WSL

### 1.1 - Habilitar o WSL

```powershell
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
```

### 1.2 - Habilitar a virtualização

```PowerShell
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
```

REINICIAR O PC!!!

### 1.3 - Instalar a atualização do WSL2 (necessário)

```powershell
wsl --update
```
Ou instalar o [aplicativo](https://wslstorestorage.blob.core.windows.net/wslblob/wsl_update_x64.msi).

REINICIAR O WSL!!!

```powershell
wsl --shutdown
```

### 1.4 - Versão 2 do WSL como padrão

```powershell
wsl --set-default-version 2
```

### 1.5 - Arquivo .wslconfig

Na pasta raiz do usuário atual, criar arquivo um arquivo denoninado `.wslconfig`.

Editar esse arquivo e incluir as seguintes entradas:

```ini
[wsl2]
memory=2GB # Limits VM memory in WSL 2 to 4 GB
processors=1 # Makes the WSL 2 VM use two virtual processors
swap=2GB
```

### 1.6 - Verificar instalação do WSL

```powershell
wsl --status
```

# 2 - Instalação do Linux

### 2.1 - Verificar quais distros linux estão disponível de forma online

```powershell
wsl --list --online
```

O resultado será algo como esse:

```text
  NAME            FRIENDLY NAME
* Ubuntu          Ubuntu
  Debian          Debian GNU/Linux
  kali-linux      Kali Linux Rolling
  openSUSE-42     openSUSE Leap 42
  SLES-12         SUSE Linux Enterprise Server v12
  Ubuntu-16.04    Ubuntu 16.04 LTS
  Ubuntu-18.04    Ubuntu 18.04 LTS
  Ubuntu-20.04    Ubuntu 20.04 LTS
```

### 2.2 - Instalar uma distro.

Nesse exemplo será instalada a distro do Debian. Para isso, atentemos para o nome (NAME) da distro e então executamos o seguinte comando:

```powershell
wsl --install --distribution Debian
```

Após o download e instalação da distro, será aberto o prompt de comando, já em ambiente Linux, solicitando o nome e senha do usuário:
![image](https://user-images.githubusercontent.com/18177981/133828927-f8c36e44-98c5-4a47-819a-92e15d4a0a0c.png)

PRONTO!!! Linux instalado.

2.3 - Para conferir a distro em execução, executar o comando:

```powershell
wsl --list --verbose
```

_O * antes do nome indica a distro padrão. Para alterar a versão para a distro que que acabou de instalar, execute o seguinte comando:_

```powershell
wsl --set-default Debian
```

Execute novamente o comando do item 2.3. Resultado:

```text
  NAME      STATE           VERSION
* Debian    Running         1
```

# 3 - Acessando o Linux

Basicamente, há duas formas de acessar o Linux:

## 3.1 - PowerShell do Windows

Com a distro padrão configurada, basta executar o seguinte comando:

```powershell
wsl
```

O Linux será inicializado e o prompt de comando do PowerShell será alternado para o bash do Linux:
![image](https://user-images.githubusercontent.com/18177981/134011730-01a3b6f6-1d56-4df5-a9c5-7af050990174.png)

## 3.2 - Janela de comando própria

Com o Windows Terminal aberto, basta acessar o menu de perfis e clicar no perfil desejado:
![image](https://user-images.githubusercontent.com/18177981/134012050-513b2997-fa46-4454-aa8b-034d01001dfd.png)

Será criada uma aba com o bash do Linux:
![image](https://user-images.githubusercontent.com/18177981/134012180-e64eaaa6-dacb-4bae-af26-b6bea4a12ddc.png)

## 3.3 - Para ambos os modos de acesso

Para sair do bash do Linux, basta executar:

```sh
exit
```

Será feito o logout. Nesse momento, o Linux continua em execução e consumindo recursos:
![image](https://user-images.githubusercontent.com/18177981/134013005-e8fe536d-ff30-4880-b55d-d4c2e82ae88e.png)

Para *desligar* o Linux, basta executar:

```powershell
wsl --shutdown
```

## 3.4 - Acessando arquivos entre Windows x Linux

Como a instalação foi de uma das distros fornecidas pela Microsoft, as devidas configurações de rede já estão corretas, ou seja, a distro já está com acesso à internet e à rede local. Além disso, a integração entre os sistemas de arquivo também está devidamente configurada:

### 3.4.1 - Arquivos do Linux no Windows

Para acessar arquivos do Linux através do Windows (através do Windows Explorer), basta estar no diretório do Linux desejado e executar o comando `explorer.exe .`. Será aberta uma janela do Windows Explorer exibindo o diretório do Linux através do mapeamento de rede que há entre o Windows (host) que a distro (quest). Exemplo:

No Linux:
![image](https://user-images.githubusercontent.com/18177981/134013876-05de6949-dfbf-432a-918a-ee56b6b6f5d2.png)

No Windows:
![image](https://user-images.githubusercontent.com/18177981/134013923-69d1d534-b5ef-4108-ba10-5ecdf2f5ad08.png)

### 3.4.2 - Arquivos do Windows no Linux

Para acessar os arquivos do Windows dentro do Linux, basta acessar quaisquer dos pontos de montagens das unidades do Windows. Para visualizar as unidades do Windows montadas no Linux, execute o comando abaixo no terminal do Linux:

```sh
ls -l /mnt/
```

O resultado será algo assim:

```sh
total 0
drwxrwxrwx 1 root root  512 Sep 15 14:16 c
drwxrwxrwx 1 root root 4096 Sep 15 14:16 d
```

Nesse caso, as unidades C: e D: do Windows estão montadas no ambiente Linux. A partir daqui, basta navegar nas pastas do Windows.

Exemplo: **acessando a pasta _home_ do usuário do Windows.**

Para isso, navegue até a pasta home do usuário do Windows a partir do ponto de montagem da unidade na qual o Windows foi instalada, nesse caso, no C:.
![image](https://user-images.githubusercontent.com/18177981/134014777-05138a11-3ecd-44e0-8176-50419e38f0a7.png)

Será exibido algo assim:
![image](https://user-images.githubusercontent.com/18177981/134014889-431614d7-a1c6-4543-98d5-0d9d7cd90b4a.png)

# 4 - Instalação do Docker

A instalação do Docker será feita na distro Linux que acabou de instalar, nesse caso, será necessário acessar o ambiente Linux.

Uma vez dentro do prompt de comando da distro Linux, seguir os tutoriais abaixo:

Instalação Docker para distro Debian: [link](https://docs.docker.com/engine/install/debian/).

Após a instalação, checar se o serviço do Docker está em execução. Para isso, executar:

```sh
sudo service docker status
```

Deve retornar algo como:
```text
Docker is running.
```

Se retornar algo como:
```text
Docker is not running ... failed!
```

Basta executar o comando abaixo para levantar o serviço do Docker:

```sh
sudo service docker start
```

Instalação Docker Compose para Linux: [link](https://docs.docker.com/compose/install/#install-compose-on-linux-systems).

Como o Docker-Compose instalado, basta navegar até o diretório que está o arquivo docker*.yml e executar o comando:

```sh
docker-compose up -d
```

Será exibido algo como:
```text
Starting mongo-server       ... done
Starting sqlserver-database ... done
Starting rabbitmq-server    ... done
Starting redis-server       ... done
```

## Bônus

### 1 Otimizando o espaço em disco

cd C:\Users\<nome do usuário>\AppData\Local\Packages\TheDebianProject.DebianGNULinux_76v4gfsz19hv4\

**TheDebianProject.DebianGNULinux_76v4gfsz19hv4** para distribuições Debian
**CanonicalGroupLimited.UbuntuonWindows_79rhkp1fndgsc** para distribuições Ubuntu

```powershell
optimize-vhd -Path .\ext4.vhdx -Mode full
```

Ao longo do tempo, a 

### 2 Desinstalar o Hyper-V (CUIDADO!!! Não faça isso se deseja manipular as VMs manualmente)

Como o WSL/WSL2 utiliza uma arquitetura própria, o Hyper-V, que geralmente está habilitado para funcionamento correto de VMs (VirtualBox e/ou VM Ware), pode ser desabilitado. Para isso, navegue até _Ativar ou desativar recursos do Windows_, procure pelo **Hyper-V**, desmarque-o e clique em **Ok**.

![image](https://user-images.githubusercontent.com/18177981/134016357-2aa7b06f-ec80-4af2-aa7f-cc6257f84d4b.png)

### 3 (WIP) Windows Terminal - Instalação e configuração

## Fontes

https://github.com/codeedu/wsl2-docker-quickstart

https://docs.microsoft.com/en-us/windows/wsl/install-win10#step-2---check-requirements-for-running-wsl-2

https://jayfuconsulting.wordpress.com/2020/11/14/sql-server-2019-docker-wsl-2/

https://docs.docker.com/compose/install/#install-compose-on-linux-systems

https://docs.microsoft.com/pt-br/windows/wsl/about

https://github.com/timvisee/docker-compose-mssql

https://docs.microsoft.com/pt-br/sql/linux/quickstart-install-connect-docker?view=sql-server-ver15&pivots=cs1-bash

https://docs.docker.com/compose/reference/

Erro no Debian:

https://github.com/microsoft/WSL/discussions/4872
