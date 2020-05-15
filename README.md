# <h1 align="center">Apostila de Paralelismo (C++ & OpenMP)</h1>

> O intuito desse repositório é de fazer um passo a passo de como conseguir construir
> algoritmos que se utilizam da parelelização da ***libgomp*** e também analisar suas 
> performances de acordo com o uso de tempo da CPU.

---

## Sumário
1. <a href="#Ferramentas">Ferramentas</a><br>
  1.1 <a href="#VSC2">Visual Studio Community 2019</a><br>
  1.2 <a href="#MGW">MinGW</a>
2. <a href="#PIDE">Preparando a IDE</a><br>
3. <a href="#Algoritmos">Algoritmos</a><br>
  3.1 <a href="#AG">Algoritmo Genético</a><br>
  3.2 <a href="#RS">Recozimento Simulado</a><br>
3. <a href="#Ref">Referências</a><br>
----

<div id="Ferramentas"></div>

## 1. Ferramentas

Antes de começarmos iremos precisar de algumas ferramentas para facilitar e possibilitar o desenvolvimento dos algoritmos e da análise:

<div id="VSC2"></div>

### 1.1 Visual Studio Community 2019 [(Visual Studio Code)](https://github.com/microsoft/vscode)

Utilizaremos eventualmente uma IDE mais completa que um simples editor de texto para possibilitar algumas análises de código que necessitam de arquivos gerados durante a compilação do algoritmo. Para isso utilizaremos a IDE Visual Studio em sua versão ***Community 2019*** que pode ser encontrada [aqui](https://visualstudio.microsoft.com/pt-br/vs/).

<p align="center">
  <img src="https://github.com/Daniel-Boll/Apostila_Paralelismo/blob/master/Imagens%20apostila/Apostila_2.png">
</p>

_____

<div id="MGW"></div>

### 1.2 MinGW

MinGW (Minimalist GNU for Windows) é um compilador é um ambiente de desenvolvimento minimalista para aplicativos nativos do Microsoft Windows. Sistemas linux geralmente contem o GCC nativamente. A instalação e configuração do MinGW pode ser obtida atráves [deste link](https://terminaldeinformacao.com/2015/10/08/como-instalar-e-configurar-o-gcc-no-windows-mingw/) em português, e [deste](http://www.codebind.com/cprogramming/install-mingw-windows-10-gcc/) em inglês. 

Finalmente para verificar se ocorreu tudo certo digite *gcc* no cmd/terminal e deverá ver o seguinte.

```console
foo@bar:~$ gcc
gcc: fatal error: no input files
compilation terminated.
```
_____

<div id="PIDE"></div>

## 2. Preparando a IDE

Primeiro abra o *Visual Studio Community 2019*, assim que abrir irá se deparar com uma tela de introdução. Clique em **Criar um projeto**.

<p align="center">
  <img src="https://github.com/Daniel-Boll/Apostila_Paralelismo/blob/master/Imagens%20apostila/Apostila_3.png">
</p>

Em seguida a IDE irá lhe oferecer alguns templates de projetos, escolha **Aplicativo de Console** e clique em próximo ou aperte enter.

<p align="center">
  <img src="https://github.com/Daniel-Boll/Apostila_Paralelismo/blob/master/Imagens%20apostila/Apostila_4.png">
</p>

Escolha um nome, estarei utilizando Genetic Algorithm, e aperte enter. Irá abrir um código que deve printar um *Hello World!*, aperte CTRL+F5 e deverá ver

```console
Hello World!

Pressione qualquer tecla para fechar esta janela...
```

Agora em **Projeto** -> **Propriedades de (Nome do seu projeto)...** achará a opção C/C++, clique e abrirá mais opções dentro dessa.

<p align="center">
  <img src="https://github.com/Daniel-Boll/Apostila_Paralelismo/blob/master/Imagens%20apostila/Apostila_5.png">
</p>

Em **linguagem** marque **Abra o suporte MP** como **Sim (/openmp)** e em **Linha de Comando** adicione em opções adicionais o seguinte: ***/Zc:twoPhase-*** conforme as imagens mostram

<p align="center">
  <img src="https://github.com/Daniel-Boll/Apostila_Paralelismo/blob/master/Imagens%20apostila/Apostila_6.png">
  <img src="https://github.com/Daniel-Boll/Apostila_Paralelismo/blob/master/Imagens%20apostila/Apostila_7.png">
</p>

E está tudo pronto para começarmos.
_____

<div id="Algoritmos"></div>

## 3. Algoritmos

Para construir os algoritmos utilizaremos a linguagem de programação C++, para [saber mais](https://pt.wikipedia.org/wiki/C%2B%2B).

<div id="AG"></div>

### 3.1 Algoritmo Genético

Algoritmos Genéticos (AG ou GA) foram idealizados e experimentados por John Holland na década de 1970; estes algoritmos têm por base os princípios da seleção natural propostos por Darwin (HOLLAND, 1975). Eles empregam uma estratégia de busca paralela e estruturada, mas aleatória, que é voltada em direção ao reforço da busca de pontos de “alta aptidão”. Os indivíduos são representados genotipicamente por vetores binários, onde cada elemento de um vetor denota a presença (1) ou ausência (0) de uma determinada característica: o seu genótipo. Os elementos podem ser combinados formando as características reais do indivíduo, ou o seu fenótipo. Teoricamente, esta representação é independente do problema, pois uma vez encontrada a representação em vetores binários, as operações padrão podem ser utilizadas, facilitando o seu emprego em diferentes classes de problemas. Ou seja, basicamente o algoritmo a partir das exaustivas rotinas de testes vai classificando os indivíduos com base em seu desempenho, os indivíduos que melhor se saírem passam seus genes avante com algumas mutações gerando outros e assim por diante até que se chegue ao resultado esperado (CARVALHO, 2019).

O problema que escolhi resolver e aplicar o *Algoritmo Genético* foi proposto por [Ayran Andrade Sampaio](https://github.com/AyranAndrade) podem checar a ideia original [aqui](https://github.com/AyranAndrade/Hello-World-Algoritmo-Genetico). Resumidamente se baseia em um algoritmo que consiga evoluir uma cadeia de caracteres aleatórios até a cadeia desejada. Eu peguei a ideia do problema que ele fez e algumas partes de sua estrutura e comecei a fazer o algoritmo. Note que ele não segue todas as regras pra um AG perfeito há alguns detalhes que podem facilitar o processo e coisas que fiz a mais para me ajudar, conforme o processo explico melhor.

Eu dividi o código em dois arquivos, um *main.cpp* e outro *GeneticAlgorithm.cpp*. Irei passando trecho a trecho do código devido ao seu tamanho, mas pode achá-lo completo [aqui](https://github.com/Daniel-Boll/Apostila_Paralelismo/source/GeneticAlgorithm/).

Agora em seu projeto criado no *Visual Studio* deverá ter um arquivo com o mesmo nome do projeto em **Arquivos de Origem**. No meu caso como criei o projeto chamado GeneticAlgorithm nos meus arquivos de origem encontro **GeneticAlgorithm.cpp**.

<p align="center">
  <img src="https://github.com/Daniel-Boll/Apostila_Paralelismo/blob/master/Imagens%20apostila/Apostila_8.png">
</p>

Iremos criar mais um arquivo de origem que será nosso *main* o **main.cpp**. Para isso clicamos em cima de **Arquivos de Origem** -> **Adicionar** -> **Novo Item** ou apertamos **CTRL+SHIFT+A** após ter clicado no diretório que quisermos adicionar.

<p align="center">
  <img src="https://github.com/Daniel-Boll/Apostila_Paralelismo/blob/master/Imagens%20apostila/Apostila_9.png">
</p>

Então selecione Arquivo do C++ (.cpp) e nomeie-o **main.cpp**

<p align="center">
  <img src="https://github.com/Daniel-Boll/Apostila_Paralelismo/blob/master/Imagens%20apostila/Apostila_10.png">
</p>

Irá abrir um código vazio, iremos inserir o seguinte código dentro dele.

```cpp
/*
GeneticAlgorithm.cpp

--- Alcançar uma cadeia de caracteres utilizando Algoritmo Genético ---

@autor: Daniel Carlos Chaves Boll
GITHUB: https://github.com/daniel-boll


data de início: 28/07/2019
data de termino: 03/08/2020


Editor: Visual Studio Code

Ideia de Ayran Andrade Sampaio. GITHUB: https://github.com/AyranAndrade
*/

#include "cstdio"
#include "cstring"
#include <chrono>
#include <thread>
#include "GeneticAlgorithm.h"

int main() {
    GeneticAlgorithm ga = GeneticAlgorithm("frasedesejadaalcancar"); // Construtor w/ params 
    // GeneticAlgorithm ga = GeneticAlgorithm(); // Construtor padrão "hello world"
    return 0;
}
```

Irá indicar alguns erros, devido ao fato de não termos o cabeçalho do código GeneticAlgorithm que é **GeneticAlgorithm.h**, iremos criá-lo em **Arquivos de Cabeçalho** -> **Adicionar** -> **Novo Item**, dessa vez selecionando **Arquivos de Cabeçalho (.h)**.

Assim que feito, colocaremos o seguinte código dentro.

```cpp
#pragma once
#include <vector>
#include <string>
#include <cstdio>
#include <fstream>

class GeneticAlgorithm {
public:
    int n = 100, generation = 0;
    int nthreads = 2;
    std::string phrase;
    std::vector <std::string> population;
    std::vector <std::string> populationUp;


    GeneticAlgorithm(); // Construtor default
    GeneticAlgorithm(std::string phrase);
    void initializePopulation(); // Inicializa a população
    int fitness(std::string population); // Avalia a população
    void elitism(); // Seleciona os 20 melhores individuos e adiciona outros 10
    void evolPop(); // Fica evoluindo a população até o resultado esperado
    std::string crossOver(std::string father, std::string mother, int state); // Cruza a população
    void mutation(); // Altera algum caracter da palavra
    int best(int state); // Printa o melhor atual
    ~GeneticAlgorithm(); // Destrutor
};
```

Agora os erros devem ter sumido, iremos agora ao **GeneticAlgorithm.cpp** onde a mágica irá acontecer. Após apagar tudo o que tinha previamente sido criado pela IDE colocaremos

{...}
____

<div id="RS"><div>
  
### 3.2 Recozimento Simulado

O Recozimento Simulado (RS ou SA) tem sua origem na analogia entre o processo físico do resfriamento de um metal em estado de fusão e o problema de otimização. Metropolis, em 1953, introduziu um algoritmo simples para simular o sistema de átomos em equilíbrio numa determinada temperatura. Em cada passo do algoritmo são dados deslocamentos aleatórios em cada átomo. O SA consiste em primeiro “fundir” o sistema a ser otimizado a uma temperatura elevada e depois em reduzir a temperatura até que o sistema “congele” e não ocorra nenhuma melhora no valor da função objetivo. Em cada temperatura a simulação deve ser executada num número tal de vezes que o estado de equilíbrio seja atingido. A sequência de temperaturas e o número de rearranjos tentados em cada temperatura para o equilíbrio representam o esquema de SA (SOEIRO et al., 2019).


_____

<div id="Ref"></div>

## X. Referências

[0] https://code.visualstudio.com/<br>
[1] https://linuxize.com/post/how-to-install-visual-studio-code-on-ubuntu-18-04/<br>
[2] https://visualstudio.microsoft.com/pt-br/vs/<br>
[3] https://terminaldeinformacao.com/2015/10/08/como-instalar-e-configurar-o-gcc-no-windows-mingw/
[4] http://www.codebind.com/cprogramming/install-mingw-windows-10-gcc/
[5] https://pt.wikipedia.org/wiki/C%2B%2B

____

Carvalho, A. P. L. F. Algoritmos Genéticos. Disponível em: http://conteudo.icmc.usp.br/ pessoas/andre/research/genetic/. Acesso em: 30/04/2019.

Holland, J. H. (1975). Adaptation in natural and artificial systems. Ann Arbor: University of Michigan Press.

Soeiro, F. J. C. P, Becceneri, J. C., Silva Neto, A. J. (2009). Recozimento Simulado (Simulated Annealing). In Técnicas de inteligência computacional inspiradas na natureza: aplicação em problemas inversos em transferência radiativa (pp. 35-42). 
