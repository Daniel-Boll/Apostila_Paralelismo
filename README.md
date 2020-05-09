# <h1 align="center">Apostila de Paralelismo (C++ & OpenMP)</h1>

> O intuito desse repositório é de fazer um passo a passo de como conseguir construir
> algoritmos que se utilizam da parelelização da ***libgomp*** e também analisar suas 
> performances de acordo com o uso de tempo da CPU.

---

## Sumário

1. <a href="#Ferramentas">Ferramentas</a><br>
  1.1 <a href="#VSC">Visual Studio Code</a><br>
  1.2 <a href="#VSC2">Visual Studio Community 2019</a>
----

<div id="Ferramentas"></div>

## 1. Ferramentas

Antes de começarmos iremos precisar de algumas ferramentas para facilitar e possibilitar o desenvolvimento dos algoritmos e da análise:
 
<div id="VSC"></div>

### 1.1 VSCode [(Visual Studio Code)](https://github.com/microsoft/vscode)

A primeira ferramenta de edição de texto que estaremos utilizando é o [Visual Studio Code](https://code.visualstudio.com/) que pode ser baixada clicando ali ou com os seguintes passos.

----

#### Caso Windows
Com a seguinte linha de comando caso você tenha o chocolatey em sua máquina.
```console
foo@bar:~$ choco install vscode
```

----

#### Caso Linux

01. Primeiro, atualize o índice de pacotes e instale as dependências digitando:
```console
foo@bar:~$ sudo apt update
foo@bar:~$ sudo apt install software-properties-common apt-transport-https wget
```

02. Em seguida, importe a chave Microsoft GPG usando o seguinte comando wget:
```console
foo@bar:~$ wget -q https://packages.microsoft.com/keys/microsoft.asc -O- | sudo apt-key add -
```
E ative o repositório do Visual Studio Code digitando:
```console
foo@bar:~$ sudo add-apt-repository "deb [arch=amd64] https://packages.microsoft.com/repos/vscode stable main"
```

03. Depois que o repositório apt estiver ativado, instale a versão mais recente do Visual Studio Code com:
```console
foo@bar:~$ sudo apt update
foo@bar:~$ sudo apt install code
```

____

Caso opte pelo download direto do site, é só clicar em ***Download for Windows/Linux***

<p align="center">
  <img src="https://github.com/Daniel-Boll/Apostila_Paralelismo/blob/master/Imagens/Apostila_1.png">
</p>

____

<div id="VSC2"></div>

### 1.2 Visual Studio Community 2019 [(Visual Studio Code)](https://github.com/microsoft/vscode)

{...}

[0] https://code.visualstudio.com/<br>
[1] https://linuxize.com/post/how-to-install-visual-studio-code-on-ubuntu-18-04/
