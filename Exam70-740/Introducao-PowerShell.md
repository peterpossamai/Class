# Introdução ao PowerShell

## Links recomendados
- https://powershell.org/
- https://powershellexplained.com/
- https://docs.microsoft.com/en-us/powershell/
- https://dmitrysotnikov.wordpress.com/

## Sobre o PowerShell
- PowerShell é uma command-line shell
- PowerShell é um ambiente script
- PowerShell é um engine de automação
- Shells tradicionais lidam somente com strings
- PowerSheel é baseado em objetos
    ```
    "Hello World".GetType()
    ```
- Formato de comandos **Verbo-Substantivo** Ex.: ```Get-Help```
- Auto completar com TAB

## Ambientes PowerShell
- **PowerShell** - PowerShell.exe
- **Windows PowerShell ISE** - PowerShell_ISE.exe
- [**Visual Estudio Code**](https://code.visualstudio.com/)

## Categoria de Comandos
### Cmdlets
- Implementado de uma classe .NET do [SDK de desenvolvimento PowerShell](https://docs.microsoft.com/en-us/powershell/developer/windows-powershell)
- Carregado de uma DLL na inicialização do PowerShell
- Formato Verbo **Verbo-Substantivo**
    ```
    Get-Command -CommandType Cmdlet
    ```

### Functions
- Bloco de código de scritp PowerShell
    ```
    function PorraToda {
        "Função da Porra Toda"
    }
    PorraToda
    ```
- Fica na memória da execução do Shell e descartado no fechamento
    ```
    Get-Command -CommandType Function
    ```

### Alias
- Apelidos dado a Functions e Cmdlets
    ```
    Get-Command -CommandType Alias
    ```

### Application
- Aplicativos internos contidos no Path ```$Env:Path```
    ```
    Get-Command -CommandType Application
    ```

## Parâmetros e Pipeline
- Parêmetros posicional e nominal, auto-completar com TAB
- Nem tudo aceita o Pipeline (Opção foreach)
- Pipeline processa um a um "|"

## Como realizar comentários em PowerShell

- Para comentar uma linha, use cerquilha
    ```
    #Lorem Ipsum è un testo segnaposto utilizzato nel settore della tipografia
    ```
- Para comentar um bloco, siga o exemplo:
    ```
    <#
    Lorem Ipsum è un testo segnaposto utilizzato nel settore della tipografia e della stampa. Lorem Ipsum è considerato il testo segnaposto standard sin dal sedicesimo secolo
    #>
    ```

## Variáveis
- Toda variável é um tipo de objeto PowerShell
- Comandos para manipular variável
    ```
    Get-Command -Noun Variable | Format-Table -Property Name,Definition -AutoSize -Wrap

    New-Variable -Name MinhaVariavel -Value "Parangarico"
    Set-Variable -Name MinhaVariavel -Value "Parangaricotirimirruaru"
    Get-Variable -Name MinhaVariavel
    Clear-Variable -Name MinhaVariavel
    Remove-Variable -Name MinhaVariavel
    ```
- Manipulação por atribuição
    ```
    $MinhaVariavel = "Parangarico"
    $MinhaVariavel
    $MinhaVariavel += "tirimirruaru"
    $MinhaVariavel
    ```
- Lendo variável
```
$MinhaVariavel = Read-Host "Digita a palavra cabalistica"
```
- Variáveis do cmd.exe "$env"
    ```
    Get-ChildItem env:
    $env:SystemRoot
    $env:MinhaVariavel="Parangaricotirimirruaru"
    $env:MinhaVariavel
    ```
## Uso de aspas e aspas simples
- aspas expandem variaveis
    ```
    $PalavraCabalistica = "Parangaricotirimirruaru"
    "$PalavraCabalistica"
    ```
- aspas simples são literais
    ```
    $PalavraCabalistica = "Parangaricotirimirruaru"
    '$PalavraCabalistica'
    ```
- Crase é o caractere de escape "`"
    ```
    "A `$PalavraCabalistica é $PalavraCabalistica"
    ```

## Formatação e saida
- [About Redirection](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_redirection)
- Uso através do Pipeline
- Format-Table [-AutoSize] [-Wrap]
- Format-List
- Out-File
- Out-GridView
- Out-File
- Out-Null

## Trabalhando com tipos

## String
- Metodos
    ```
    "Hello".ToLower()
    "Hello".ToUpper()
    "Hello".EndsWith('lo')
    "Hello".StartsWith('he')
    "Hello".toLower().StartsWith('he')
    "Hello".Contains('l')
    "Hello".LastIndexOf('l')
    "Hello".IndexOf('l')
    "Hello".Substring(3)
    "Hello".Substring(1,2)
    "Hello".Insert(3, "INSERTED")
    "Hello".Length
    "Hello".Replace('l', 'x')
    "Server1,Server2,Server3".Split(',')
    " remove space at ends ".Trim()
    " remove space at ends ".Trim(' rem')
    ```
- Expansão $()
    ```
    "(2+2)"
    "$(2+2)"
    ```
- Concatenação "+"
    ```
    "Nois"+" que "+"ta"
    ```
- Multiplição "*"
    ```
    "#"+"-"*10
    ```

## Here-String
- Aspas
    ```
    $PalavraCabalistica = "Parangaricotirimirruaru"
    @"
    PalavraCabalistica é $PalavraCabalistica
    "@
    ```
- Aspas simples, sem expanção de variaveis
    ```
    $PalavraCabalistica = "Parangaricotirimirruaru"
    @'
    PalavraCabalistica é $PalavraCabalistica
    '@
    ```

## Trabalhando com Datas
- O cmdlet Get-Date retorna a data atual e manipula datas

    ```Get-Date```

- A opção -Uformat , podemos determina o modo que ele nos retornara a data, por exemplo.

    ```Get-Date -Uformat "%Y-%m-%d"```

- Para converter uma string para um objeto de date

    ```Get-Date "10/12/2012 4:30"```

- Comparar duas datas.

    ```
    $DateA = Get-Date "05/05/2012"
    $DateB = Get-Date "06/05/2012"
    $DateA -gt $DateB
    $DateB -gt $DateA
    ```

- Adicionando ou retirando tempo

    ```
    $DateA = Get-Date
    $DateA.AddMinutes(480)
    $DateA.AddMinutes(-90)
    ```

## Continuação: [Donda PowerShell wiki](https://github.com/danieldonda/PowerShell/wiki)

## [PowerShell Galary](https://www.powershellgallery.com/)
- https://docs.microsoft.com/en-us/powershell/gallery/overview

## [Sample scripts for system administration](https://docs.microsoft.com/en-us/powershell/scripting/samples/sample-scripts-for-administration?view=powershell-5.1)

## [About](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/?view=powershell-5.1)

