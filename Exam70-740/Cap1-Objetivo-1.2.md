# Objetivo 1.2: Instalar e configurar o Nano Server
- Não suporta 32bits
- Sem Area de trabalho remota
- Administrado por PowerShell (WinRM) e WMI
- [Dicas Nano](https://www.ntweekly.com/category/nano-server/)

## Determinar cenários de uso apropriados e requisitos do Nano Server
- Serviços de nuvem, hyper-v, cluster, DNS, IIS, Scale-out File Server.
- Aplicativos para nuvem sem GUI, container.
- Pode-se usar algumas ferramentas MMC para gerenciamento.
- Seu uso é limitado, mas viável para nuvem e escalável.
- Microsoft executou mais de 3.400 VMs, com 128 MB de RAM cada, em um único servidor com oito processadores de 20 núcleos e 1 terabyte de memória.

## [Instale o Nano Server](https://docs.microsoft.com/pt-br/windows-server/get-started/getting-started-with-nano-server)
- Não há assistente de instalação. :-)
- Criado através de um VHD.
- Diretório imagem Server 2016 "NanoServer\NanoServer.win".
- Módulo PowerShell em subdiretório "NanoServerImageGenerator".
- Pacotes com funções em subdiretório "Packages".

### Crie uma imagem do Nano Server
- Abra o PowerShell com direitos administrativos
- Monte a ISO do Server ou a adicione na VM
- Atribuir senha ao Nano Server


### Associe a um domínio
- Ter acesso ao domínio com permissões de criação de contas
- Parâmetro "DomainName", para associar ao AD
- Parâmetro "ReuseDomainNode", para reutilizar uma conta do AD

### Crie uma VM de nano Server
- VM versão 1 suporta .vhd e .vhdx
- VM versão 2 suporta somente .vhdx

### Implemente funções e recursos no Nano Server
- Funções são pacotes .cab
- Opções (Compute) são pacotes (Microsoft-NanoServer-Compute-Package)
- Editar imagens existentes "Edit-NanoServerImage"

## Gerenciar e configurar o Nano Server
- Nano Server Recovery Console
- Sem suporte a mouse, teclado numérico, CapsLk, NumLk :-)

### Configure o endereço IP de um Nano Server
- Padrão é DHCP Client


## Gerenciar o Nano Server remotamente usando o Windows PowerShell
- New-PSSession, Enter-PSSession, Get-PSSession, Exit-PSSession ...
- Server 2016 PowerShell Completo
- Nano Server PowerShell Core mesmo do Desktop "$PSVersionTable"
- [PowerShell Nano](https://docs.microsoft.com/en-us/windows-server/get-started/powershell-on-nano-server)

## Prática Objetivo 1.2

### Crie uma imagem do Nano Server
```
# Importe o Modulo
Import-Module E:\NanoServer\NanoServerImageGenerator -Verbose

# Criando imagem Nano Server
$NanoName = ''

New-NanoServerImage -Edition Standard -DeploymentType Guest -MediaPath E:\ `
    -BasePath .\Base -TargetPath ".\NanoServers\$NanoName.vhdx" -ComputerName $NanoName

# Criar uma VM Geração 2 no HYPERV2 via GUI
```

### Associe a um domínio
```
# Com arquivo blob
djoin /provision /domain class.local /machine NanoServer /savefile NanoServer-blob.txt

# Criando imagem Nano Server e adicionando ao dominio com arquivo blob

New-NanoServerImage -Edition Standard -DeploymentType Guest -MediaPath E:\ `
    -BasePath .\Base -TargetPath ".\NanoServers\$NanoName.vhdx" -ComputerName $NanoName `
    -DomainBlobPath .\NanoServer-blob.txt -Package Microsoft-NanoServer-IIS-Package

# Com parâmetro "DomainName"
# Criando imagem Nano Server e adicionando ao dominio
New-NanoServerImage -Edition Standard -DeploymentType Guest -MediaPath E:\ `
    -BasePath .\Base -TargetPath ".\NanoServers\$NanoName.vhdx" -ComputerName $NanoName `
    -DomainName class.local
```

### Implemente funções e recursos no Nano Server
```
# Criando imagem Nano Server com IIS Service e adicionando ao dominio
New-NanoServerImage -Edition Standard -DeploymentType Guest -MediaPath E:\ `
    -BasePath .\Base -TargetPath ".\NanoServers\$NanoName.vhdx" -ComputerName $NanoName `
    -DomainName class.local -Package Microsoft-NanoServer-IIS-Package
```

### Configure o endereço IP de um Nano Server
```
# Criando imagem Nano Server com IIS Service e adicionando ao dominio
$NanoIPAddress = '10.100.0.?'

New-NanoServerImage -Edition Standard -DeploymentType Guest -MediaPath E:\ `
    -BasePath .\Base -TargetPath ".\NanoServers\$NanoName.vhdx" -ComputerName $NanoName `
    -DomainName class.local -Package Microsoft-NanoServer-IIS-Package
    -InterfaceNameOrIndex ethernet -ipv4address $NanoIPAddress `
    -ipv4subnetmask 255.255.255.0 -ipv4gateway 10.100.0.1 -ipv4dns 10.100.0.5
```

### Gerenciar o Nano Server remotamente usando o Windows PowerShell
```
Set-Item wsman:\localhost\client\trustedhosts *

$Creds = Get-Credential -\administrator
$NanoIPAddress = ''

Enter-PSSession -ComputerName $NanoIPAddress -Credential $Creds

Exit-PSSession
```