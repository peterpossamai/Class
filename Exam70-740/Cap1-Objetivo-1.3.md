# Ob##etivo 1.3: Criar, gerenciar e fazer a manutenção de imagens para implantação

## Planejar a virtualização do Windows Server
- Custo baixo
- Varias VMs em um único Server
- Compatibilidade de hardware
- Datacenters menores
- Possibilidade de upgrade
- Provisionamento
- Eficiência
- Tempo de atividade
- Manutenção
- Recuperação de desastres
- Teste
- Aplicativos isolados
- Migração para a nuvem
- Retorno sobre o investimento

### Quais servidores você deve migrar primeiro?
- Baixo risco -> Não crucial -> Maior uso -> Crucial para os negócios

### Como você migrará servidores físicos para servidores virtuais?
- P2V
- [Disk2Vhd](https://docs.microsoft.com/en-us/sysinternals/downloads/disk2vhd)

## Planejar implantações de Linux e FreeBSD
- Suportado (Microsoft da suporte)
- FreeBSD Integration Services (BIS)

## Avaliar cargas de trabalho de virtualização usando o Microsoft Assessment and Plan-ning (MAP) Toolkit
- Vide página 62

## Atualizar imagens com patches, hotfixes e drivers
- DISM.exe
- Arquivos .VHD, .VHDX e .WIM
- Adicionar e remover drivers de dispositivos
- Adicionar e remover pacotes de idioma
- Adicionar e remover pacotes de atualização
- Adicionar e remover arquivos e pastas
- Ativar ou desativar recursos do sistema operacional
- [Executar arquivos de resposta](https://www.windowsafg.com/)
- Adicionar ou remover pacotes de aplicativos
- [Intruções de captura Windows Imagem .WIM](https://docs.microsoft.com/pt-br/windows-hardware/manufacture/desktop/capture-and-apply-an-image)
- [Video DISM](https://www.youtube.com/watch?v=5PQL1##gCGbA)

## Instalar funções e recursos em imagens offline
- Verificar imagem
```
# Instanciando variáveis
$ImageFile = "D:\DISM\IMAGEM\install.wim"
$MountDir = "D:\DISM\MOUNT"
$PackageFile = "D:\DISM\PACKAGES\WindowsTH-RSAT_WS_1803-x64.msu"

dism /get-imageinfo /imagefile:$ImageFile
Get-WindowsImage -ImagePath $ImageFile
```
- Montando uma imagem
```
dism /mount-image /imagefile:$ImageFile /index:5 /mountdir:$MountDir
Mount-WindowsImage -ImagePath $ImageFile -Path $MountDir -Index 5
```
- Lista nomes das features
```
dism /image:$MountDir /get-features
Get-WindowsOptionalFeature -Path $MountDir
```
- Ativa determinada feature
```
dism /image:$MountDir /enable-feature /featurename:Microsoft-Hyper-V [/all]
Enable-WindowsOptionalFeature -Path $MountDir -FeatureName Microsoft-Hyper-V [-All]
```
- Desativa determinada feature
```
dism /image:$MountDir /disable-feature /featurename:WindowsMediaPlayer
Disable-WindowsOptionalFeature -Path $MountDir -FeatureName WindowsMediaPlayer
```
- Instala um pacote ou atualização .CAB ou .MSU
```
dism /image:$MountDir /add-package /packagepath:$PackageFile
Add-WindowsPackage -Path $MountDir -PackagePath $PackageFile
```
- Lista pacotes em formato de tabela
```
dism /image:$MountDir /Format:Table /get-packages
Get-WindowsPackage -Path $MountDir | Format-Table -AutoSize
```
- Salva alterações e desmonta a imagem
```
dism /unmount-image /mountdir:$MountDir [/commit] [/discard]
Dismount-WindowsImage -Path $MountDir [-Save] [-Discard]
```

## Gerenciar e fazer a manutenção do Windows Server Core, de imagens do Nano Server, e de VHDs usando o Windows PowerShell
- Vide página 79

