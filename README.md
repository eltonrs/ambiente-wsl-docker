# Ambiente WSL + Docker

Premissas

- A versão do WSL será 2, na qual já está contida na build 18971 ou superior do Windows 10.
- Os comandos serão executados todos como administrador e através do PowerShell.
- Os comandos, tanto do Windows quanto do Linux, serão executados pelo Windows Terminal. A instalação é simples, basta acessar a [loja](https://www.microsoft.com/pt-br/p/windows-terminal/9n0dx20hk701#activetab=pivot:overviewtab) e clicar em _Obter_.

## 1 - Configurações iniciais do WSL

### Habilitar o WSL

```powershell
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
```

### Habilitar a virtualização

```PowerShell
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
```

REINICIAR O PC!!!

### Instalar a atualização do WSL2

```powershell
wsl --update
```
Ou instalar o [aplicativo](https://wslstorestorage.blob.core.windows.net/wslblob/wsl_update_x64.msi).

REINICIAR O WSL!!!

```powershell
wsl --shutdown
```

### Versão 2 do WSL como padrão

```powershell
wsl --set-default-version 2
```

### Arquivo .wslconfig

Na pasta raiz do usuário atual, criar arquivo um arquivo denoninado `.wslconfig`.

Editar esse arquivo e incluir as seguintes entradas:

```ini
[wsl2]
memory=2GB # Limits VM memory in WSL 2 to 4 GB
processors=1 # Makes the WSL 2 VM use two virtual processors
swap=2GB
```

### Verificar instalação do WSL

```powershell
wsl --status
```

# 2 - Instalação do Linux

### Verificar quais distros linux estão disponível de forma online

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

_O * antes do nome indica a distro padrão_

### Instalar uma distro.

Nesse exemplo será instalada a distro do Debian. Para isso, atentemos para o nome (NAME) da distro e então executamos o seguinte comando:

```powershell
wsl --install --distribution Debian
```

Após o download e instalação da distro, será aberto o prompt de comando, já em ambiente Linux, solicitando o nome e senha do usuário:
![image](https://user-images.githubusercontent.com/18177981/133828927-f8c36e44-98c5-4a47-819a-92e15d4a0a0c.png)

PRONTO!!! Linux instalado.

Para conferir a distro em execução, executar o comando:

```powershell
wsl --list --verbose
```

Resultado:

```text
  NAME      STATE           VERSION
* Debian    Running         1
```

Como a instalação foi de uma das distros fornecidas pela Microsoft, as devidas configurações de rede já estão corretas, ou seja, a distro já está com acesso à internet e à rede local. Além disso, a integração entre os sistemas de arquivo também está devidamente configurada:

- Para acessar arquivos do Linux através do Windows Explorer, basta estar no diretório do Linux desejado e executar o comando `explorer.exe .`. Será aberta uma janela do Windows Explorer exibindo o diretório do Linux através do mapeamento de rede que há entre o Windows (host) que a distro (quest).

- Para acessar os arquivos do Windows dentro do Linux, basta acessar quaisquer dos pontos de montagens das unidades do Windows. Para visualizar as unidades do Windows montadas no Linux, execute o comando abaixo no terminal do Linux:

```sh
ls -l /mnt/
```

O resultado será algo assim:

```sh
total 0
drwxrwxrwx 1 root root  512 Sep 17 14:16 c
drwxrwxrwx 1 root root 4096 Sep 17 14:16 d
```

Nesse caso, as unidades C: e D: do Windows estão montadas no ambiente Linux. A partir daqui, basta navegar nas pastas do Windows.

## Bônus

### (WIP) Windows Terminal - Instalação e configuração

## Fontes

https://docs.microsoft.com/en-us/windows/wsl/install-win10#step-2---check-requirements-for-running-wsl-2

https://jayfuconsulting.wordpress.com/2020/11/14/sql-server-2019-docker-wsl-2/
