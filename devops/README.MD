# DEVOPS

<p align="center">
 <a href="#versionamento">Versionamento</a> •
 <a href="#rastreabilidade">Rastreabilidade</a> • 
 <a href="#teste">Testes</a> • 
 <a href="#ci">CI</a> •
 <a href="#da">Deploy Automático</a>
</p>


# Documentação <a id="doc"></a>


### Versionamento: <a id="versionamento"></a>

VER

### Rastreabilidade: <a id="rastreabilidade"></a>

Rastreabilidade

### Testes: <a id="teste"></a>

Para a realização dos testes foram elaborados diferentes estratégias de processos de acordo com a necessidade de cada tipo de serviço. O projeto possui vários serviços ativos que são divididos, basicamente, em serviços Node e Python, há também um repositório em código C de modelo para dispositivos IOT.
Tendo isso em vista, se faz necessário, ocasionalmente, divergir nas estratégias dos processos de testes entre os serviços.
O foco definido pela equipe para os testes foram os testes de integração. E para isso, foi definido um integrante para ser o "tester" da aplicação toda. 
Os processos foram divididos da seguinte forma:
- Serviços Node backend (estações métricas e autenticação);
- Serviço Node frontend;
- Serviços Python backend (tratamento e recepção de dados);

#### Processo de teste Node backend:

O processo começa com o ínicio de alguma feature, logo deve-se respeitar os padrões estabelecidos de commits e etc. Assim que o desenvolvedor terminar a adição e fizer o push de sua branch, seguindo o fluxo de testes, o tester entrará em ação e assim, ele deve ir na pasta "test" presente na raiz dos repositórios, e seguir ou estipular o caminho dos arquivos alterados na pasta "src", porém nesta pasta "test". Após isso, ele deve fazer os testes de integração referentes às adições realizadas. Terminando o teste, deve-se rodar o seguinte comando na raiz do projeto: "npm run test"; E então será iniciado um script que rodará todos os testes disponíveis. Se tudo der "OK" então o tester poderá fazer o merge de suas alterações com a branch "develop". Ao final da sprint, antes de juntar a "develop" com a "prod" os testes devem ser rodados novamente para garantir que de fato não há problemas e só então deverá ser feito o merge com a branch de produção.

#### Bibliotecas utilizadas:
- jest;
- supertest;
- ts-jest;
- @types do jest e supertest;

Essas libs foram escolhidas pois o Jest é um framework simples que, dentre outras coisas, providencia métodos para organizar os testes a serem criados, já o Supertest é uma biblioteca que simula a aplicação REST inteira, permitindo que sejam feitas requisições e etc, sem que seja preciso rodar a aplicação tornando muito útil para o ambiente de teste.

#### Especificações:

Cada teste possui um escopo que deve ser respeitado e cada escopo é definido pela função "describe(" ", () => {})" que possuirá uma string explicando esse escopo. Cada teste deve ser feito dentro do "describe(" ", () => {})" numa função "it(" ", () => {})" que também terá uma string para descrever seu escopo. No caso de um teste abrir um novo escopo, deve-se criar uma nova função "describe(" ", () => {})" para ele. Vale ressaltar que toda função de teste deve possuir uma asserção de um valor esperado.

#### Processo de teste Node frontend:

O processo de teste para o frontend segue exatamente os mesmos passos que o Node backend, exceto por um fator determinante; Após a criação da feature e a criação dos testes estipulados para ela, antes de rodar o comando para iniciar os testes, deve-se ir ao arquivo "package.json" na raiz do projeto e alterar a linha 4: "type: module"; para "type: commonjs", ao fazer isso, deve-se rodar o comando de testes normalmente (npm run test) e se tudo der "OK", é preciso voltar ao arquivo "package.json" e desfazer as alterações para só então realizar o merge das branches.

#### Bibliotecas utilizadas:
- jest;
- ts-jest;
- @types do jest;

Foi escolhido o Jest pois ele é um framework que cria um ambiente de testes virtual para rodar os scripts descritos pelos testes e providencia métodos para organizar os testes a serem criados. O que o torna muito útil especialmente para testes de integração.

#### Especificações:

Cada teste possui um escopo que deve ser respeitado e cada escopo é definido pela função "describe(" ", () => {})" que possuirá uma string explicando esse escopo. Cada teste deve ser feito dentro do "describe(" ", () => {})" numa função "it(" ", () => {})" que também terá uma string para descrever seu escopo. No caso de um teste abrir um novo escopo, deve-se criar uma nova função "describe(" ", () => {})" para ele. Vale ressaltar que toda função de teste deve possuir uma asserção de um valor esperado.

#### Processo de teste Python backend:

Para o processo dos serviços em python é necessário gerar um arquivo de teste seguindo a nomenclatura (test_ + nome arquivo) de forma que o nome do arquivo faça referência à operação específica que ele testerá, esse mesmo nome deve ser aplicado a uma pasta dentro da pasta "test", é preciso também criar um arquivo vazio chamado "__init__.py" em cada pasta pois isso é um padrão requirido para que a lib encontre os testes. Após desenvolvido os testes, deve-se abrir o terminal dentro da pasta "test" e rodar o seguinte comando: "python -m unittest". Se tudo der "OK" então já é possível fazer o merge com a branch develop. Antes de mergear com a branch prod é necessário rodar os testes novamente para garantir que está tudo okay.

#### Bibliotecas utilizadas:
- unittest (padrão do python);

O motivo da escolha dessa lib é por sua simplicidade e eficiencia já que não é necessário instalar dependências pois ela já vem com o python e sua estruturação de testes é bastante intuitiva e rápida.

#### Especificações:

O escopo de cada teste é definido pelo arquivo criado, ou seja, se há um arquivo "app.py" que precisa ter seus métodos testados, todos os testes referentes a eles devem ficar no arquivo "test_app.py". Dentro de cada arquivo de teste deve haver uma classe "TestApp(unittet.TestCase)", e cada teste deve representar uma função dentro da classe. O nome dessas funções deve descrever o escopo daquele teste específico. Vale ressaltar que toda função de teste deve possuir uma asserção de um valor esperado.

#### Fluxo Geral dos Testes

Testes de integração geralmente envolvem partes do código em que mais de um desenvolvedor esteve trabalhando. Isso implica que há a necessidade de estabelecer certas condições para que os testes possam ser criados. Portanto, durante a fase de levantamento das features e requisitos a serem trabalhados deve-se também estipular quais features devem ser terminadas para que os testes de integração referente à elas, sejam elaborados, essa definição será marcada como uma tarefa do tipo "(TESTE)". Esse é o caso para os testes iniciais, após as features em questão forem terminadas, então o tester já estará pronto para pegar a tarefa, realizar os testes numa branch "(feature/teste_tal_coisa)", seguir o processo definido praquele serviço, e enfim, finalizar a tarefa e sua branch. Para o caso dos testes iniciais já terem sido criados e, eventuais features que não haviam sido planejadas previamente, surjam, então o tester (ou o/os próprio/os dev/devs) deverá abrir uma nova tarefa "(TESTE)" e implementar os testes das funcionalidades que abordam aquela feature, e então seguir o mesmo esquema do caso anterior.

## CI: <a id="ci"></a>

## Processo de CI

Este documento descreve o processo de Integração Contínua (CI) para o nosso projeto.

## Pipeline CI

### Builds Automáticos

- A cada PR aberto ou merge em qualquer uma das branches (`develop`, `staging`, `prod`), o projeto é automaticamente buildado.
- Testes de Integração são executados para garantir que nada foi quebrado.
- Nas branchs `develop` e `prod` as imagens Docker são atualizadas para refletir as mudanças.

### Processo do Job da `develop`

1. **Trigger**:
   - Desenvolvedores fazem commits e push do código para o repositório.

2. **Build e Testes**:
   - Dependências instaladas.
   - Código é compilado.
   - Testes de integração são executados.

3. **Criação de Imagem Docker**:
   - Uma imagem Docker é criada a partir do código atualizado.
   - A imagem é testada para garantir que ela funciona conforme o esperado.

4. **Push da Imagem Docker**:
   - A imagem Docker é enviada para um repositório de contêiner (Docker Hub).

### Processo do Job da `staging`

1. **Trigger**:
   - Desenvolvedores fazem commits e push do código para o repositório.

2. **Build e Testes**:
   - Depêndencias Instaladas
   - Código é compilado.
   - Testes integração são executados.


### Processo do Job da `prod`

1. **Trigger**:
   - Desenvolvedores fazem commits e push do código para o repositório.

2. **Build e Testes**:
   - Depêndencias Instaladas.
   - Código é compilado.
   - Testes de integração são executados.

3. **Criação de Imagem Docker**:
   - Uma imagem Docker é criada a partir do código atualizado.
   - A imagem é testada para garantir que ela funciona conforme o esperado.

4. **Push da Imagem Docker**:
   - A imagem Docker é enviada para um repositório de contêiner (Docker Hub).



## Jobs das branchs develop, staging, prod

#### Exemplo de `job` da branch `develop`

```yaml
name: CI

on:
  push:
    branches:
      - develop

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v2

      - name: Set up Node.js
        uses: actions/setup-node@v2
        with:
          node-version: '20'

      - name: Install dependencies
        run: npm install

      - name: Run build
        run: npm run build

      - name: Build and tag Docker image for Development
        run: |
          docker build -t jonasrsribeiro/lw-controller-gerenciamento-estacoes-metricas -f Dockerfile.dev .
          docker tag jonasrsribeiro/lw-controller-gerenciamento-estacoes-metricas:latest jonasrsribeiro/lw-controller-gerenciamento-estacoes-metricas:develop

      - name: Log in to Docker Hub
        run: echo "${{ secrets.DOCKERHUB_TOKEN }}" | docker login -u jonasrsribeiro --password-stdin

      - name: Push Docker image to Docker Hub
        run: |
          docker push jonasrsribeiro/lw-controller-gerenciamento-estacoes-metricas:latest
          docker push jonasrsribeiro/lw-controller-gerenciamento-estacoes-metricas:develop
```



### Deploy Automático: <a id="da"></a>

Deploy automático
