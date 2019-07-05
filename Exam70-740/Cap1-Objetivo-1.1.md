# Objetivo 1.1: Instalar, fazer o upgrade e migrar servidores e cargas de trabalho

## Determine requisitos de instalação do Windows Server 2016
- Que edição do Windows Server 2016 você deve instalar?
- Standard, Datacenter ...
  - Que opção de instalação você deve usar?
- Desktop, Core, Nano
  - Quais funções e recursos o servidor precisa?
- AD CS, Hyper-V, DNS, DHCP ...
  - Que estratégia de virtualização você deve usar?
- Datacenter VMs ilimitadas
- Replicas de volumes

## Requisitos mínimos de hardware
- Processador: 1.4-GHz 64 bits
- RAM: 512 MB ECC para Server Core, 2 GB ECC para servidor com Desktop Experience
- Espaço em disco: Mínimo de 32 GB em uma unidade SATA ou compatível
- Adaptador de rede: Ethernet, com throughout em gigabits
- Monitor: Super VGA (1024 x 768) ou resolução superior
- Teclado e mouse (ou outro dispositivo apontador compatível)
- Acesso à Internet
- Sem suporte a IDE

## Limites máximos de hardware e virtualização
- Processadores: Um servidor host dará suporte a até 512 processadores lógicos (LPs,
logical processors) se o Hyper-V estiver instalado.
- Processadores de máquinas virtuais: Até 240 por máquina virtual.
- Memória: Até 24 terabytes por servidor host e até 12 terabytes por máquina virtual.
- Tamanho do VHDX: Até 64 terabytes.
- Máquinas virtuais: Até 1.024 por servidor host.

### ENTENDENDO OS LPS
- Intel hyperthreading duas threads por nucleo
- Função Hyper-V considera cada thread da Intel
- Sem função Hyper-V só considera o núcleo

## Determine edições apropriadas do Windows Server 2016 por cargas de trabalho
- Quais funções e recursos?
- Quais licenças?
- Máquinas virtuais ou físicas?
- Tendência atual servidores executando única função (Core)

### Windows Server 2016 Datacenter
- Projetado para virtualização
- OSEs ilimitadas (Operating System Environments)
- Storage Spaces Direct
- Gerenciamento de aramzenamento tipo Storage, usando discos baratos e camadas com SSD.
  - Storage Replica
- Replicação de volumes usando SMB 3.0, síncrona e assíncrona
  - Máquinas virtuais blindadas

### [Comparativo entre versões](https://docs.microsoft.com/en-us/windows-server/get-started/2016-edition-comparison)

### Windows Server 2016 Standard
- Permite dois OSEs (Host + VM)
- Standard OSE físico exclusivo para hyper-v permite duas VMs
- Sem recursos de armazenamento
- A partir de 10 VMs considere o uso do Datacenter

### Windows Server 2016 Essentials
- Sem opção Server Core
- único OSE
- 25 usuários e 50 dispositivos

### Windows Server 2016 Multipoint Premium Server
- licenciamento acadêmico
- Acesso por varios usuários

### Windows Storage Server 2016 Server
- Disponível apenas por canais OEM

### Windows Hyper-V Server 2016
- Gratuito
- Sem interface gráfica
- Função Hyper-V

### ENTENDENDO OS OSEs
- Operating System Environments
- Instâncias em execução
- Físico ou Virtual
- Edição Standard uma VM

## Instale o Windows Server 2016
- Observação Drivers e Imagens ISO
- Windows Deployment Services (WDS)

## Instale recursos e funções do Windows Server 2016
- Funções (Roles): executar tarefas específicas
- Recursos (Features): componentes menores (apps)
- Recomendação de uso limitado de funções
- Intalado por Server Manager
- Intalado por PowerShell (Install-WindowsFeature)
- PowerShell não distingue Role de Features

## Instale e configure o Windows Server Core
- Versão sem inteface gráfica
- No 2012 era possível instalar ou remover interface gráfica
- Conservação de recursos de hardware
- Menor espaço em disco
- Menos atualizações
- Superfície de ataque reduzida

## Gerencie instalações do Windows Server Core usando o Windows PowerShell, a linha de comando e recursos de gerenciamento remoto

### Use o PowerShell remoto
- WinRM ativado por padrão
- New-PSSession, Enter-PSSession, Get-PSSession

### Use o Server Manager remotamente
- RSAT (Remote Server Administration Tool)

### Use snap-ins do MMC remotamente
- Run --> mmc.exe
- Liberar DCOM no firewall windows
- Customize uma MMC com ferramentas de usuo diário

## Implemente o Desired State Configuration (DSC) do Windows PowerShell para estabelecer e manter a integridade de ambientes instalados
- [Microsoft DSC](https://docs.microsoft.com/pt-br/powershell/dsc/overview/overview)
- [Donda](https://danieldonda.com/2015/02/11/instalar-e-configurar-o-windows-powershell-desired-state-configuration-dsc/)
- Recurso do Windows PowerShell que usa arquivos de script para aplicar,
  monitorar e manter uma configuração específica do sistema.
- [Configurations](https://docs.microsoft.com/pt-br/powershell/dsc/configurations/configurations)
- Script PowerShell com blocos de "nodes"(hosts) e "resources"
- [Resources](https://docs.microsoft.com/pt-br/powershell/dsc/resources/resources)
- Blocos de configuração com atributos a serem usados nas "configurations"
- [Local Configuration Manager (LCM)](https://docs.microsoft.com/pt-br/powershell/dsc/managing-nodes/metaconfig)
- Mecanismo aplicador das configurações no cliente
  - Servidos por SMB ou HTTP
  - Implantação PULL (extração) ou PUSH (envio)
  - Idempotentes (aplicados repetidamente sem erros)
  - Modulo PowerShell DSC Client "PSDesiredStateConfiguration"

### Crie scripts de configuração DSC
- Paradigma [declarativo](https://medium.com/alexandre-malavasi/descomplicando-programa%C3%A7%C3%A3o-imperativa-declarativa-e-reativa-a481baa87742), o foco não está no “como” e sim no “que”
- Scripts compilados na extenção .mof
- Pra cada nó declarado é gerado um arquivo .mof

### Implante configurações DSC
- Servidor SMB ou [WEB OData](https://docs.microsoft.com/en-us/powershell/developer/webservices/creating-a-management-odata-web-service)
  - Arquitetura PULL ou PUSH
  - PULL:
- Script no cliente com URL do Server
- Tarefa agendada
- LCM faz download do arquivo e aplica
  - PUSH:
- A partir do Server
- CMDLET Start-DscConfiguration, Path para pasta com arquivos .mof

## Faça upgrades e migrações de servidores e cargas de trabalho básicas do Windows Server 2008 e Windows Server 2012 para o Windows Server 2016
- Upgrade, instalar nova versão em versão existente
- Migração, transferir aplicações, serviços e dados para um novo server
- Microsoft remomenda migrar

### Faça o upgrade de servidores
- Leva tempo
- Problemas de incompatibilidade
- Na verdade uma migração interna
- Nova pasta Windows
- Migra aplicativos e configurações
- Importa perfis
- Importa registro
- Atualiza drivers
- Se possível crie uma imagem antes

### Caminhos de atualização
- Versões:
  - Direta somente a partir do 2012, demais em etapas
- Edições:
  - Standard para Standard
  - Datacenter para Standard, não ao contrário
- Opções de instalação:
  - Upgrades entre Core e GUI não são suportados, em nenhuma direção.
  - Upgrades entre Nano e outras opções também não são suportados.
- Plataformas:
  - Upgrades de 32 para 64 bits nunca tiveram suporte
  - 2008 foi a última versão em 32 bits
- Idiomas:
  - Upgrades não são suportados, independentemente da versão.
  - Estações de trabalho:
  - Upgrades não são suportados, independentemente da versão.

### Prepare-se para o upgrade
- Verifique a compatibilidade de hardware
- Remova o NIC Teaming
- Verifique o espaço em disco
- Confirme se o software está assinado
- Desinstalar aplicativos ou drivers não assinados antes.
  - Verifique a compatibilidade dos aplicativos
- What Needs Your Attention
- Crie um inventário dos softwares e procure atualizações
- Testar todos os aplicativos em relação à compatibilidade com o Windows Server 2016
  - Instale atualizações do Windows
  - Verifique a funcionalidade do computador
- Verifique os Logs do Windows
  - Faça um backup completo
  - Compre o Windows Server 2016 correto

### Execute uma instalação de upgrade
- Keep personal files and apps (Upgrade)
- Nothing (Clean Install)

### Migre funções
- Entre versões
- Minimo Windows Server 2003 SP2
  - Entre plataformas
- x86 ou x64
  - Entre edições
  - Entre máquinas físicas e virtuais
  - Entre opções de instalação
- Core e GUI 2016
  - Não entre idiomas diferentes e a
  - Não entre Core 2008 e Core 2016, não possui .NET
  - Tranferência de funções individualmente

### Instale as Windows Server Migration Tools
- Composto por 5 CMDLETS PowerShell
- Export-SmigServerSetting
  - Exporta recursos e configurações
- Get-SmigServerFeature
  - Exibe lista de recursos que podem ser migrados
- Import-SmigServerSetting
  - Importa recursos e configurações
- Receive-SmigServerData
  - Recebe arquivos, pastas, permissões e compartilhamentos
  - O cmdlet Send-SmigServerData deve estar em execução no servidor de origem.
- Send-SmigServerData
  - Transfere arquivos, pastas, permissões e compartilhamentos, da origem para o destino.
  - O cmdlet Receive-SmigServerData deve estar em execução no servidor de destino.
  - É uma feature "migration"
- Install-WindowsFeature migration
  - Path, C:\Windows\system32\ServerMigrationTools
- Exemplo, SmigDeploy.exe /package /architecture amd64 /os WS12R2 /path c:\temp
  - Copiar arquivos gerados e executar no servidor origem como ADM
  - Não um padrão no procedimento, guias são fornecidos pela Microsoft

## Determine o modelo de ativação apropriado para a instalação do servidor
[Dica: Live Licenciamento](https://youtu.be/oFZfpG6GsXU)
- Chaves de Ativação Múltipla (MAK, Multiple Activation Keys)
- Serviço de Gerenciamento de Chaves (KMS, Key Management Service)
- Ativação Baseada no Active Directory

### MAK
- Pode adicionar mais licenças a mesma MAK
- Pode incorporar uma chave a uma ISO ou script ("slmgr.vbs")
- Ativação independentemente
- Direto na internet
  - Ativação única
  - Proxy de MAK
- VAMT, Volume Activation Management Tool
- Usado por sistemas sem acesso a internet

### Serviço de Gerenciamento de Chaves
- Plataforma cliente/servidor
- Host KMS deve ter acesso a internet
- Recomendado para grandes redes
- Tem autoridade de ativar e renovar os clientes

### Limitações do KMS
- Host KMS Windows 10 só ativa desktops
- Limite de ativação, 5 servers ou 25 desktops (contador)
- Mantem cache de duas vezes seu total, 50 exemplo
- Microsoft não recomenda seu uso abaixo de 50
- Ativação expira em 180 dias (intervalo de validade de ativação)
- Tentativa de renovação a cada 7 dias

### Instale um host KMS
- É uma feature do server 2016 "VolumeActivation"
- Chave de Host KMS Microsoft
- Porta TCP 1688 Firewall Windows
- Hosta KMS cria um registro DNS SRV

### Configure clientes KMS
- Windows 10 Enterprise usam KMS por padrão
- Demais GVLKs, generic volume licensing keys

### Ativação baseada no Active Directory
- Mesma feature do server 2016 "VolumeActivation"
- A partir do Server 2012 e Windows 8
- Active Directory e nivel funcional 2012
- AD 2008 R2 expandir AD Adprep.exe

### Use a Ativação Automática de Máquina Virtual (AVMA)
- Hosts Hyper-V 2012 R2 Datacenter e 2016 Datacenter
- VMs ativadas automaticamente, Datacenter, Standard e Essentials
- Ativação mantida mesmo se migrada para outros HOSTs
- Ativar máquinas virtuais em locais remotos
- Ativar máquinas virtuais com ou sem uma conexão à Internet
- Ativação sem direito administrativo a VM
- "slmgr /ipk AVMAkey"


## Prática Objetivo 1.2

- Instalar o Windows Server Datacenter Core na VM FILES
- Insatalar Roles e Features via PowerShell
