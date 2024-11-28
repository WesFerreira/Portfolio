### Wesley Ferreira da Silva

### Introdução

Meu nome é Wesley Ferreira da Silva, tenho 29 anos e sou graduando do 6º semestre de Banco de Dados pela FATEC - Prof. Jessen Vidal. Ingressei na Fatec no segundo semestre de 2021, onde obtive meu primeiro contato com a programação.

Durante a minha formação adquiri diversos conhecimentos em Hard skills e aprimorei diversas soft skills. Com a Aprendizagem por Projetos Integrador (API), tive a oportunidade de colocar em prática os conteúdos estudados em sala de aula quando conquistei meu primeiro trabalho na área da tecnologia na Stealth Mode, atuando com tecnologias como Java, Framework Spring, Mensageria com RabbitMQ e banco de dados DB2(IBM) .

Atualmente sou Desenvolvedor de Software na consultoria Mutant, atuando com tecnologias como Java, Framework Quarkus, Mensageria com RabbitMQ e banco de dados não relacional MongoDB. 

<p align="center">
<img src="https://avatars.githubusercontent.com/u/88842443?v=4" widht="203" height="250">
</p>

## Contatos

<a href="https://www.linkedin.com/in/wesley-ferreira-405440207/" target="_blank"><img src="https://img.shields.io/badge/LinkedIn-0077B5?style=for-the-badge&logo=linkedin&logoColor=white" target="_blank"></a>
 <a href="https://github.com/WesFerreira" target="_blank"><img src="https://img.shields.io/badge/GitHub-100000?style=for-the-badge&logo=github&logoColor=white" alt="GitHub" target="_blank"></a>
 <br>

## Projeto 1: 2º Semestre de 2021
### Empresa Parceira: FATEC (PROJETO INTERNO)

<p align="center">
<img src="https://github.com/WesFerreira/Portfolio/blob/main/Imagem/fatec.png?raw=true" widht="40" height="100">
</p>

## Descrição do projeto
O projeto proposto pela facudade, visa analisar os dados oficiais da COVID-19 no Estado de São Paulo e apresentá-los de maneira clara e contextualizada aos usuários, por meio de visualizações gráficas e/ou não-gráficas. A solução desenvolvida foi um sistema desktop em Python, utilizando os arquivos CSV disponibilizados pelo governo contendo dados estatísticos da COVID, permitia aos usuários visualizar gráficos informativos que apresentavam estatísticas detalhadas sobre o vírus.
## Técnologias usadas
### Python
Python foi a escolha predominante para o desenvolvimento da aplicação devido à sua sintaxe clara, simplificada e sua vasta gama de bibliotecas e frameworks que suportam diversas áreas, incluindo análise de dados. Sua simplicidade, versatilidade e suporte facilitaram significativamente o processo de criação do projeto. Uma das bibliotecas que mais utilizamos no projeto foi a do Pandas que nos fornece ferramentas para análise e manipulação de dados, que no nosso projeto foi manipulado os dados por csv.

## Contribuições pessoais


<details>
  <summary>Rastreador de dados de um arquivo CSV</summary>

- Verifica se um arquivo CSV (caso_full.csv) está presente em um diretório específico. Se não estiver, chama uma função atualizar() do módulo update.
- Lê o arquivo CSV em um DataFrame do Pandas (pd), renomeia as colunas e faz várias manipulações nos dados.
- Cria várias variáveis com diferentes valores extraídos do DataFrame, como total de mortes em São Paulo, novos casos, total de mortes em novembro, listas de cidades, anos e meses.
- Gera um novo DataFrame chamado df com as operações e filtros.
- Imprime o DataFrame resultante.

  ```python

  class tracker:
    arquivo = os.path.expanduser('~\Documents\caso_full.csv')  # Local na pasta documentos onde o arquivo estará

    if os.path.isfile(arquivo):
        print("Arquivo encontrado.")
        pass
    else:
        from update import atualizar
        print("Arquivo não encotrado, realizando o download...")
        atualizar()

        """Verifica se o arquivo csv existe, caso a condição seja falsa, irá realizar o download do arquivo."""

    file = pd.read_csv(arquivo)
    """Executa a leitura do arquivo.csv - "file" é apenas o nome que dei a variável."""

    df = pd.DataFrame(data=file,
                      columns=['city', 'date', 'state', 'last_available_deaths', 'place_type',
                               'new_deaths', 'new_confirmed',
                               'ast_available_confirmed'])  # transforma o arquivo csv em um dataframe e seleciona as colunas

    df = df.rename(
        columns={"city": "cidade", 'state': 'estado',
                 'last_available_deaths': 'mortes confirmadas', 'place_type': 'tipo',
                 'new_deaths': 'mortes', 'new_confirmed': 'confirmados', 'date': 'data', 'ast_available_confirmed': 'novos casos'})
    """Renomeia as colunas"""

    df = df.loc[df['estado'] == 'SP'].loc[
        df['tipo'] == 'city']
    """Filtra apenas o estado de SP na coluna 'estado', e apenas o cálculo por tipo "state"""

    df = df.fillna(value="")
    """altera os valores de NaN para valor em branco."""

    df = df.drop_duplicates()  # retira valores duplicados.
    df['data'] = pd.to_datetime(df['data'])  # cria uma coluna de data
    df['ano'] = pd.DatetimeIndex(df['data']).year  # cria uma coluna de ano
    df['mes'] = pd.DatetimeIndex(df['data']).month  # cria uma coluna de mes
    df['mes_nome'] = df['data'].dt.strftime('%B')  # transforma o numero da coluna 'mes' para nome do mes
    df['mes_ano'] = df['mes_nome'].astype(str) + "-" + df['ano'].astype(str)  # concatena mes e ano
    df['dia'] = pd.DatetimeIndex(df['data']).day
    df['chave'] = df['cidade'] + df['mes_nome'] + df['ano'].astype(str)

    '''Variáveis'''

    dia_1 = date.today() - timedelta(days=1)
    dia_1.strftime("%Y-%m-%d")

    total_mortes_sp = df['mortes'].loc[df['estado'] == 'SP'].loc[
        df['data'] == str(dia_1)].sum()  # Soma somente as mortes do estado de SP

    novos_casos = df['novos casos'].loc[df['estado'] == 'SP'].loc[df['data'] == str(dia_1)].sum()  # Soma os casos novos

    total_mortes_dia = df['mortes'].loc[df['mes_nome'] == 'novembro'].sum()

    lista_cidades = df['cidade'].loc[df['estado'] == 'SP'].loc[df['cidade'] != ''].drop_duplicates().sort_values() \
        .tolist()

    lista_ano = ['2020', '2021']

    lista_meses = ['Todos', 'janeiro', 'fevereiro', 'março', 'abril', 'maio', 'junho', 'julho', 'setembro', 'outubro', 'novembro', 'dezembro']

    """Transforma os valores em lista, e coloca em ordem alfabética."""

    total_mortes_cidade = df[['cidade', 'mortes']].groupby('cidade').sum().sort_values(by='mortes', ascending=False) \
        .iloc[:10]

    # Ordena as cidades de forma decrescente e mostra as 10 cidades com maior numero de morte

    lista_mes = df['mes_ano'].drop_duplicates().tolist()
    print(df)


  ```
</details>

<details>
  <summary>Mostrando dados específicos para o usuário</summary>

  ```python

 """Calcula a data atual - 1 para saber a quantidade de novas mortes """

        self.valor_mortes_sp = tracker.df['mortes'].sum()

        """ Total de óbitos no estado de São Paulo"""

        self.valor_total_confirmados = tracker.df['confirmados'].sum()

        """Total de casos confirmados no estado de São Paulo"""

        self.mortes = QLabel("Número total de mortes: " + str(self.valor_cidade), self)
        self.mortes_dia = QLabel("Novas mortes: " + str(self.valor_dia), self)
        self.mortes_sp = QLabel("Total de óbitos no estado de São Paulo: " + str(self.valor_mortes_sp), self)
        self.total_confirmados = QLabel(
            "Total de casos confirmados no estado de São Paulo: " + str(self.valor_total_confirmados), self)

  ```
</details>

## Aprendizado efetivo

### Hard Skills

  - **Desenvolvimento de aplicação desktop**: sei fazer com ajuda
  - **Manipulação de dados**: sei fazer com ajuda
  - **Gerar gráficos**: sei fazer com ajuda

### Soft Skills

  #### Comunicação:
  Com o primeiro semestre totalmente online por conta da pandemia, nosso contato com professores e colegas foram um pouco mais complicado pelo fato de ser online, então tivemos que utilizar de ferramentas para se comunicar, entrando em sintonia e conseguindo entregar o desafio proposto.

<br>

[Projeto no GitHub](https://github.com/RenanRhoads/GRUPO-04---FATEC-BD)

<br><br>

## Projeto 2: 1º Semestre de 2022


  ### Empresa Parceira: DOM ROCK

<p align="center">
<img src="https://github.com/WesFerreira/Portfolio/blob/main/Imagem/domRock.png?raw=true" widht="40" height="100">
</p>

## Descrição do projeto
O projeto em questão almeja criar uma plataforma robusta para ativação e gestão de clientes, focalizada na otimização do uso dos recursos oferecidos pela empresa. Essa iniciativa visa integrar telas de cadastro que alimentam um banco de dados central, armazenando informações cruciais para a personalização e ativação dos clientes na plataforma Dom Rock. A estratégia concentra-se na entrada precisa de dados, considerando parâmetros específicos de cada cliente para alocar os recursos de forma eficiente.

Além disso, a abordagem adotada contempla a avaliação criteriosa do consumo esperado de recursos, levando em conta variáveis como o volume de dados a serem manipulados e o número de usuários envolvidos. Isso se traduz na estimativa acurada dos recursos necessários para cada cliente, garantindo uma alocação adequada e maximizando a utilização dos recursos disponíveis.

Para complementar, o sistema é desenvolvido para gerar relatórios detalhados e oferecer consultas facilitadas, fornecendo insights valiosos sobre o uso dos recursos e o desempenho dos clientes na plataforma. Essa solução integral não só simplifica a ativação dos clientes, mas também permite uma administração eficiente e informada dos recursos da empresa na plataforma Dom Rock.
## Técnologias usadas
### Java
A aplicação foi desenvolvido utilizando a linguagem Java e desenvolvendo projeto desktop. A escolha do Java permitiu a construção eficiente da aplicação, aproveitando as vantagens e recursos oferecidos.

### PostgreSQL
Utilizamos o PostgreSQL como banco de dados para alocar e gerenciar todos os dados referente as etapas dos processos de cadastros e separação de categoria. Optamos pelo PostgreSQL devido à sua versatilidade, interface simplificada e funcionalidades expostas de forma dedutiva, o que proporcionou uma excelente usabilidade e facilitou o gerenciamento do banco de dados. Além disso, a escolha do PostgreSQL foi impulsionada pela sua alta eficiência e desempenho, permitindo o armazenamento tranquilo e eficaz dos milhões de dados importados. A capacidade do PostgreSQL de lidar com grandes volumes de dados e seu suporte a consultas complexas foram fundamentais para garantir o processamento rápido e preciso das informações.


## Contribuições pessoais

<details>
  <summary>Tela do primeiro processo do cliente</summary>
    <p align="center">
    <img src="https://github.com/WesFerreira/Portfolio/blob/main/Imagem/01%20-%20Tela.jpg?raw=true" widht="550" height="500">
    </p>
</details>

<details>
  <summary>Tela do segundo processo do cliente</summary>
    <p align="center">
    <img src="https://github.com/WesFerreira/Portfolio/blob/main/Imagem/02%20-%20Tela.jpg?raw=true" widht="550" height="500">
    </p>
</details>

<details>
  <summary>Tela do terceiro processo do cliente</summary>
    <p align="center">
    <img src="https://github.com/WesFerreira/Portfolio/blob/main/Imagem/03%20-%20Tela.jpg?raw=true" widht="550" height="500">
    </p>
</details>

## Aprendizado efetivo

### Hard Skills

  - **Orientação a objetos**: sei fazer com ajuda
  - **Funções em Java**: sei fazer com ajuda
  - **Integração com banco de dados**: sei fazer com ajuda
  - **Consultas personalizadas com SQL**: sei fazer com ajuda

### Soft Skills

  #### Aprendizado Contínuo: 
  O estudo constante foi essencial ao me deparar pela primeira vez com Java, precisando aprender rapidamente sobre orientação a objetos e suas particularidades para aplicá-la no proposta da empresa parceira. 

  #### Adaptabilidade: 
  Trabalhando em uma equipe recém-formada, precisei me adaptar rapidamente às dinâmicas de trabalho e ao novo ambiente. A flexibilidade foi crucial para enfrentar os desafios e mudanças ao longo do projeto, o que nos permitiu manter o foco nos resultados e entregar uma solução que atendesse às expectativas do cliente de maneira ágil.
  
</details>

<br>

[Projeto no GitHub](https://github.com/CarcaraTec/Dom-Rock)

<br><br>

## Projeto 3: 2º Semestre de 2022

  ### Empresa Parceira: IACIT Soluções tecnológicas S.A.

<p align="center">
<img src="https://github.com/WesFerreira/Portfolio/blob/main/Imagem/iacit.png?raw=true" widht="40" height="100">
</p>

## Descrição do projeto
Foi proposto o desenvolvimento de um sistema que atenda aos seguintes requisitos funcionais: cadastro de estações, cadastro de estados e regiões, importação de dados meteorológicos de arquivos CSV fornecidos pela empresa IACIT e geração de relatórios. Além disso, o sistema precisa ser uma aplicação web com interface amigável, implementada em linguagem Java e utilizar um banco de dados relacional.

Para atender aos requisitos não funcionais exigidos, o sistema deve ser capaz de importar e pesquisar os dados disponibilizados pelo Instituto Nacional de Meteorologia (INMET) por meio de seu site, integrando-os ao banco de dados. A aplicação web deve fornecer um mecanismo de filtro para que os usuários possam filtrar os registros por datas, regiões, estados, localidades e tipos de dados. As informações filtradas devem ser apresentadas em gráficos e cards, proporcionando uma visualização clara e intuitiva. Além disso, a aplicação deve permitir a exportação de relatórios com base nas consultas realizadas.
## Técnologias usadas
### Java
O back-end da aplicação foi desenvolvido utilizando a linguagem Java em conjunto com o framework Spring Boot que é uma estrutura projetada para simplificar a criação de aplicativos web em Java, fornecendo um conjunto abrangente de ferramentas e bibliotecas que facilitam o desenvolvimento. A escolha do Java e do Spring Boot permitiu a construção eficiente do back-end da aplicação, aproveitando as vantagens e recursos oferecidos por essa poderosa combinação de tecnologias.

### PostgreSQL
Utilizamos o PostgreSQL como banco de dados para alocar e gerenciar todos os dados meteorológicos, incluindo informações das estações e regiões. Optamos pelo PostgreSQL devido à sua versatilidade, interface simplificada e funcionalidades expostas de forma dedutiva, o que proporcionou uma excelente usabilidade e facilitou o gerenciamento do banco de dados. Além disso, a escolha do PostgreSQL foi impulsionada pela sua alta eficiência e desempenho, permitindo o armazenamento tranquilo e eficaz dos milhões de dados meteorológicos importados. A capacidade do PostgreSQL de lidar com grandes volumes de dados e seu suporte a consultas complexas foram fundamentais para garantir o processamento rápido e preciso das informações meteorológicas.

### HTML, CSS, JavaScript
A criação da interface gráfica da aplicação envolveu a combinação das linguagens de marcação HTML e CSS com a linguagem de programação JavaScript. O HTML desempenhou um papel fundamental ao estabelecer a estrutura básica da página da web, definindo os elementos e a organização dos conteúdos. Por sua vez, o CSS foi responsável por adicionar estilo e aparência à página, definindo cores, fontes, layouts e outros aspectos visuais.

A linguagem de programação JavaScript foi amplamente utilizada para adicionar interatividade à interface. Por meio do JavaScript, os usuários puderam realizar ações como interagir com elementos da página, exibir e ocultar informações dinamicamente, validar dados inseridos e até mesmo enviar requisições assíncronas para atualizar os dados em tempo real.

A combinação dessas três tecnologias permitiu o desenvolvimento de uma interface amigável e intuitiva para os usuários da aplicação. Eles puderam visualizar facilmente os dados meteorológicos, navegar entre diferentes seções e gerar relatórios personalizados de forma eficiente. A interface foi projetada para fornecer uma experiência fluida, com transições suaves e elementos responsivos que se adaptam a diferentes dispositivos e tamanhos de tela.

Além disso, a aplicação fez uso da biblioteca JavaScript chamada Charts, que desempenhou um papel fundamental na apresentação dos dados meteorológicos. Essa biblioteca ofereceu recursos avançados para a criação de gráficos e visualizações, permitindo uma representação visual clara e compreensível dos dados. Os gráficos foram projetados para serem interativos, fornecendo aos usuários uma maneira intuitiva de explorar e analisar os dados meteorológicos em diferentes perspectivas.

## Contribuições pessoais
No projeto em questão fiquei responsável pelo desenvolvimento do front-end da aplicação.

Segue alguns dos códigos que desenvolvi.

<details>
  <summary> Cadastros com JavaScript </summary>
  
  ```javascript
  
  function cadastrar(){
    fetch("http://localhost:8080/usuarios",
    {
        headers: {
            'Accept': 'application/json',
            'Content-Type': 'application/json'
        },
        method: "POST",
        body: JSON.stringify({
            nome: Inome.value,
            email: Iemail.value,
            senha: Isenha.value
        })
    })
    .then(function(res) {console.log(res) })
    .catch(function(res) {console.log(res) })
  };

  function limpar(){
      Inome.value = "";
      Iemail.value = "";
      Isenha.value = "";
  }

  formulario.addEventListener('submit', function(event){
      event.preventDefault();

      cadastrar();
      limpar();
  });
  
  ```
  
 O código apresentado é uma função JavaScript que lida com o cadastro de usuários em um servidor local </br>
    A função cadastrar() é definida para realizar uma requisição POST ao servidor local (http://localhost:8080/usuarios) usando o método fetch(). Essa função possui as seguintes configurações:

Define o cabeçalho da requisição com o tipo de conteúdo aceito (Accept) e o tipo de conteúdo enviado (Content-Type) como JSON.
Define o método da requisição como POST.
Envia os dados do usuário como um objeto JSON no corpo da requisição usando JSON.stringify(). Os dados incluem nome, email e senha.
O resultado da requisição é tratado por meio das funções then() e catch(). Se a requisição for bem-sucedida, o resultado é impresso no console usando console.log(res). Caso contrário, se ocorrer algum erro, também é exibido no console.

A função limpar() é definida para limpar os campos de entrada após o cadastro. Ela define os valores dos campos Inome, Iemail e Isenha como vazios.

Um "ouvinte de evento" é adicionado ao formulário usando addEventListener() para capturar o evento de envio (submit). A função de retorno de chamada é executada quando o evento ocorre. Essa função realiza as seguintes ações:

Chama a função event.preventDefault() para evitar o envio tradicional do formulário.
Chama a função cadastrar() para enviar os dados do usuário ao servidor.
Chama a função limpar() para limpar os campos do formulário após o cadastro.
</details>  


<details>
  <summary> Requisição para obter o IP do cliente com Javascript </summary>

  ```javascript
  
$(function() {
  $.getJSON("https://api.ipify.org?format=jsonp&callback=?",
    function(json) {
      const formulario = document.querySelector("unlog");

      function cadastrarLog(){

        fetch("http://localhost:8080/log",
          {
          headers: {
              'Accept': 'application/json',
              'Content-Type': 'application/json'
          },
          method: "POST",
          body: JSON.stringify({
              email: "email",
              ip: json.ip,
              status: "deslogado"
          })
        })

      .then(function(res) {console.log(res) })
      .catch(function(res) {console.log(res) })
      };


    unlog.addEventListener('submit', function(event){
    event.preventDefault();
    cadastrarLog();
    });
  });
});
  
```

A arquitetura desse código segue uma abordagem funcional, utilizando jQuery para fazer uma requisição assíncrona para obter o endereço IP do cliente por meio da API "https://api.ipify.org".

O código envolve a função "$()" para garantir que o código seja executado somente após o carregamento completo da página. Em seguida, é feita uma chamada à função $.getJSON() para obter o endereço IP.

Dentro da função de retorno de chamada da $.getJSON() o código define uma função chamada cadastrarLog(), que é executada quando o formulário "unlog" é submetido. Essa função realiza uma requisição POST para "http://localhost:8080/log" para registrar o evento de deslogamento do usuário.

Os dados são enviados como um objeto JSON no corpo da requisição, contendo o email, o endereço IP obtido anteriormente e o status "deslogado".

Após a requisição POST, são tratadas as respostas da promessa utilizando os métodos .then() e .catch(), que registram o resultado no console.

Por fim, o código adiciona um evento de escuta ao formulário "unlog", que é acionado quando o formulário é submetido. O evento é prevenido de realizar a ação padrão e, em vez disso, chama a função cadastrarLog().

A arquitetura desse código é relativamente simples, fazendo uso de bibliotecas externas (jQuery) e APIs para obter informações do cliente e registrar eventos no servidor. O objetivo é registrar o deslogamento do usuário, capturando o email e o endereço IP, e enviando esses dados para o servidor por meio de uma requisição POST.  
</details>

## Aprendizado efetivo

### HARD SKILLS 
  - **Desenvolvimento de aplicações REST**: sei fazer com ajuda
  - **Integração com banco de dados**: sei fazer com ajuda
  - **Programação orientada a objetos**: sei fazer com ajuda
  - **Consumo de API Rest**: sei fazer com ajuda
  - **Funções em JavaScript**: sei fazer com ajuda
  - **Manipulação de variáveis com JavaScript**: sei fazer com ajuda

### SOFT SKILLS

  #### Inteligência Emocional
  Durante o trabalho em equipe, enfrentei momentos de pressão que exigiram muita inteligência emocional. Busquei manter a calma mesmo diante de prazos apertados e conflitos pontuais, evitando que as emoções interferissem nas minhas ações ou decisões. Quando percebi que algum colega estava sobrecarregado ou frustrado, procurei ser compreensivo e oferecer apoio, ajudando a aliviar o clima e fortalecer o espírito de colaboração.

[Projeto no GitHub](https://github.com/CarcaraTec/IACIT)

<br><br>

## Projeto 4: 1º Semestre de 2023

  ### Empresa Parceira: EMBRAER

<p align="center">
<img src="https://github.com/WesFerreira/Portfolio/blob/main/Imagem/embraer.png?raw=true" widht="100" height="200">
</p>

## Descrição do projeto

O desafio foi desenvolver uma plataforma que permitisse aos usuários consultar os itens instalados nas aeronaves, além de incluir funcionalidades para verificar e editar os itens instalados ou aplicáveis a determinados "chassis", com base em uma base de dados fornecida.

A solução implementada consistiu em um sistema que armazenava todas as regras de composição dos itens em um banco de dados. Ao consultar um número de chassi, o sistema recuperava as regras de composição correspondentes no banco de dados e exibia os itens compatíveis com o chassi na tela para o usuário.

## Técnologias usadas
### Java e Spring
A infraestrutura de back-end da aplicação foi construída integralmente em Java, aproveitando as vantagens oferecidas pelo robusto framework Spring Boot. Esta escolha estratégica permitiu aos desenvolvedores a agilidade e praticidade necessárias para a criação de aplicativos web em Java, graças ao conjunto abrangente de ferramentas e bibliotecas disponibilizadas pelo Spring Boot.

Dentro desse contexto, o Spring e o Spring Data foram utilizados para desenvolver a lógica das regras de negócio. Essa lógica foi essencial para determinar o status dos itens, classificando-os como instalados, instaláveis ou não instaláveis com base nos chassis pesquisados. O uso do Spring Data possibilitou consultas otimizadas ao banco de dados, garantindo uma eficiência significativa no tratamento das lógicas e condições no back-end.

Essa abordagem reforça não apenas a utilização da linguagem Java para a construção do back-end, mas também destaca a aplicação estratégica do framework Spring, capacitando a aplicação a executar suas funções de maneira eficaz, ágil e com uma gestão eficiente das regras de negócio.
### Vue.js
Para o front-end do sistema, adotamos o Vue.js, um framework JavaScript que se destaca pela sua estrutura baseada na criação de componentes reutilizáveis. Essa abordagem arquitetural facilitou significativamente o desenvolvimento da plataforma, especialmente devido à semelhança entre determinadas partes visuais da interface.

O Vue.js é reconhecido como um framework progressivo de código aberto para o desenvolvimento web. Sua arquitetura centrada em componentes oferece uma abordagem modular que simplifica a construção de aplicativos complexos de maneira eficiente. Essa escolha não apenas facilitou a criação da interface, mas também possibilitou uma estrutura flexível e adaptável para o desenvolvimento contínuo da plataforma.
### Oracle Cloud
Para o armazenamento de dados, nossa escolha recaiu sobre o Oracle Autonomous Database, um banco de dados relacional baseado na nuvem. Optamos por esse sistema devido à sua capacidade de ser acessado pela internet, o que proporcionou uma acessibilidade, praticidade e flexibilidade excepcionais para consulta dos dados. Além disso, sua robusta estrutura ofereceu um nível elevado de segurança, não apenas controlando o acesso ao banco, mas também garantindo a consistência dos dados por meio de backups automáticos, mitigando perdas e simplificando a recuperação em casos de falhas operacionais.


## Contribuições pessoais


## Aprendizado efetivo

### HARD SKILLS 
  - **Vue.js**: sei fazer com ajuda
  - **Java**: sei fazer com ajuda
  - **Spring boot**: sei fazer com ajuda

### SOFT SKILLS 
  #### Trabalho em equipe:
  A harmonia entre os membros permitiu que todos aprendessem e compartilhassem conhecimentos sobre novas tecnologias, contribuindo para o sucesso da implementação.
  
  #### Comunicação:
  A troca de experiências e a boa comunicação foram essenciais para garantir que toda a equipe aproveitasse ao máximo as capacidades da base de dados. Isso promoveu um ambiente colaborativo e eficiente, fortaleceu os laços entre os membros e permitiu o alcance dos objetivos de forma mais eficaz.

<br>

[Projeto no GitHub](https://github.com/CarcaraTec/Embraer)

<br><br>

## Projeto 5: 2º Semestre de 2023

<p align="center">
<img src="https://github.com/WesFerreira/Portfolio/blob/main/Imagem/oracle.png?raw=true" widht="10" height="30">
</p>

## Descrição do projeto
O desafio foi desenvolver uma plataforma online abrangente para a gestão eficiente de restaurantes, integrando funcionalidades como painéis de controle, gráficos, relatórios, além da administração de pessoal, fornecedores e inventário. O objetivo é ajudar os proprietários a lidar com questões críticas, como controle de custos, gestão de funcionários e estoque, visando à melhoria das operações.

A solução implementada consistiu em um sistema web que consulta dados armazenados no banco de dados, realiza cálculos para gerar insights valiosos e os apresenta de forma intuitiva por meio de gráficos, tabelas e cards. A plataforma foi organizada em seções dedicadas, permitindo uma visualização clara do desempenho de funcionários, pratos, vendas e inventário, facilitando a tomada de decisões estratégicas pelos gestores.

## Técnologias usadas
### Java e Spring
Responsável por disponibilizar uma API REST com mapeamento de rotas. Foram criados endpoints CRUD para interação com os dados de pratos, funcionários e estoque, permitindo o consumo dessas informações pelo frontend.

### Vue.js
Possibilitou a componentização dos elementos da interface, facilitando sua reutilização. As propriedades de condicional (v-if) e looping (v-for) permitiram a exibição dinâmica dos dados de maneira eficiente.

### Oracle Autonomous Database
Escolhido pela segurança do acesso via wallet e pela funcionalidade de backups automáticos. Esse sistema permitiu o consumo de dados de qualquer ambiente conectado à internet, proporcionando maior flexibilidade e segurança na gestão das informações.

## Contribuições pessoais
   Como Product Owner (PO), refinei minhas habilidades em ambientes ágeis, utilizando a metodologia Scrum. Minha principal responsabilidade foi definir e comunicar as prioridades do produto, garantindo que as tarefas estivessem alinhadas com as necessidades dos clientes e objetivos do negócio. A priorização das atividades foi essencial para o sucesso do projeto, permitindo ajustes rápidos e eficazes conforme o feedback dos stakeholders.

<details>
  <summary>Entendimento das Necessidades do Cliente</summary>
   Como Product Owner (PO), meu primeiro passo foi compreender profundamente as necessidades do cliente e os objetivos do produto. Isso envolveu uma comunicação eficaz com os stakeholders e a coleta contínua de feedback. Com uma visão clara das expectativas, conseguindo estabelecer prioridades que agregaram valor real ao cliente.
</details>

<details>
  <summary>Gestão do Product Backlog</summary>
O Product Backlog é uma lista ordenada de funcionalidades, requisitos e melhorias que precisaram ser implementadas no produto. Eu, junto à equipe, fomos responsável por criar, manter e priorizar esse backlog, atribuindo valor de negócio a cada item e considerando seu impacto no sucesso do produto.
</details>

<details>
  <summary>Planejamento e Execução do Sprint</summary>
Nas reuniões de planejamento do Sprint, colaborei com a equipe para definir as tarefas que iriam compor o Sprint Backlog. A priorização das tarefas foi baseada no Product Backlog, comigo como PO selecionando as mais importantes para o Sprint atual, enquanto a equipe discutiu detalhes e estimou o esforço necessário para sua conclusão.
</details>

## Aprendizado efetivo

### HARD SKILLS 
  - **Gestão de Backlog**: sei fazer com ajuda
  - **Metodologia Ágil**: sei fazer com ajuda
  - **Definição de Métricas**: sei fazer com ajuda

### SOFT SKILLS 

  #### Comunicação Eficaz:
  Foi de extrema importância para garantir que todos estivessem alinhados. Procurei me expressar de forma clara e objetiva, compartilhando ideias de maneira organizada e garantindo que as informações fossem compreendidas por todos. Além disso, dei espaço para que meus colegas também contribuíssem e sempre validei se as mensagens estavam sendo interpretadas corretamente. 

  #### Empatia:
  Busquei me colocar no lugar dos meus colegas para entender melhor suas perspectivas e preocupações. Houve momentos em que as opiniões divergiam, mas procurei ouvir ativamente cada ponto de vista antes de expressar o meu. Isso ajudou a criar um ambiente de confiança e respeito, onde todos se sentiram à vontade para contribuir

  

<br>

[Projeto no GitHub](https://github.com/CarcaraTec/Cloud-Kitchen-Oracle)

<br><br>

## Projeto 6: 1º Semestre de 2024


<p align="center">
<img src="https://github.com/WesFerreira/Portfolio/blob/main/Imagem/imagemGeosistema.png?raw=true" widht="80" height="150">
</p>


## Descrição do projeto
O projeto consistiu em desenvolver uma plataforma web de inteligência artificial capaz de analisar e interpretar os sentimentos expressos por clientes em avaliações online de hotéis, proporcionando insights detalhados e contextualizados.

A solução integra tecnologia de ponta em machine learning para classificar os sentimentos em categorias como neutro, positivo ou negativo, permitindo uma visão clara e abrangente sobre o desempenho dos hotéis. Os dados, armazenados em um banco de dados não relacional, são organizados e exibidos por meio de uma interface rica em funcionalidades, incluindo mapas interativos, gráficos de tendências, e filtros para cidades, datas, hotéis e características específicas dos hóspedes.

Além disso, a plataforma fornece insights geograficamente contextualizados, permitindo a visualização dos dados em diferentes perspectivas e possibilitando que os gestores tomem decisões mais informadas sobre o atendimento e as necessidades dos clientes.

## Técnologias usadas
### Java e Spring Boot
Java e Spring Boot foram usados para desenvolver a camada de segurança e o módulo de gerenciamento de usuários, garantindo conformidade com a LGPD. Essa combinação facilitou o tratamento seguro dos dados dos usuários.

### Python e Flask
Python e Flask foram utilizados para o desenvolvimento da API back-end. Com a biblioteca scikit-learn, também foi possível treinar modelos de machine learning para a classificação de sentimentos.

### MongoDB
O MongoDB foi escolhido como banco de dados não relacional para armazenar dados de sentimentos. Sua flexibilidade de schema permitiu armazenar registros classificados e informações adicionais.

### MySQL
O MySQL, um banco de dados relacional, foi utilizado para o gerenciamento de dados de usuários e logs de acesso, garantindo a integridade dos dados e o relacionamento entre eles.

### Vue.js
Vue.js foi usado no front-end para criar dashboards e gráficos interativos. Sua capacidade de componentização simplificou o desenvolvimento de elementos visuais dinâmicos para a exibição de insights.

## Contribuições pessoais
Neste projeto, assumi um papel central no desenvolvimento da parte de Security utilizando Java com framework Spring Security. 
Sendo assim criando perfis específicos para cada funcionalidade, geração de Token uilizando JWT, validações de tokens e Criptografia de dados sensiveis

<details>
  <summary>Gerando Token</summary>

```java
    public String generateToken(User user){
        try{
            Algorithm algorithm = Algorithm.HMAC256(secret);
            String token = JWT.create()
                    .withIssuer("imagem_backend_security")
                    .withSubject(user.getUsername())
                    .withExpiresAt(genExpirationDate())
                    .sign(algorithm);
            return token;
        } catch (JWTCreationException exception) {
            throw new RuntimeException("Error while generating token", exception);
        }
    }
    private Instant genExpirationDate(){
        return LocalDateTime.now().plusMinutes(30).toInstant(ZoneOffset.of("-03:00"));
    }
```
</details>

<details>
  <summary>Validando Token</summary>

```java
  public String validateToken(String token){
        try {
            Algorithm algorithm = Algorithm.HMAC256(secret);
            return JWT.require(algorithm)
                    .withIssuer("imagem_backend_security")
                    .build()
                    .verify(token)
                    .getSubject();
        } catch (JWTVerificationException exception){
            return "";
        }
    }
```
</details>


<details>
  <summary>Criptografar e Descriptografar dados</summary>

```java
  private static final String ALGORITHM = "AES";
    private static final String TRANSFORMATION = "AES";
    public static String encrypt(String input, SecretKey key) throws Exception {
        Cipher cipher = Cipher.getInstance(TRANSFORMATION);
        cipher.init(Cipher.ENCRYPT_MODE, key);
        byte[] encryptedBytes = cipher.doFinal(input.getBytes());
        return Base64.getEncoder().encodeToString(encryptedBytes);
    }
    public static SecretKey generateKey() throws Exception {
        KeyGenerator keyGen = KeyGenerator.getInstance(ALGORITHM);
        keyGen.init(256);
        return keyGen.generateKey();
    }

  public static String decrypt(String input, SecretKey key, byte[] iv) throws Exception {
        Cipher cipher = Cipher.getInstance(TRANSFORMATION);
        IvParameterSpec ivParameterSpec = new IvParameterSpec(iv);
        cipher.init(Cipher.DECRYPT_MODE, key, ivParameterSpec);
        byte[] decryptedBytes = cipher.doFinal(Base64.getDecoder().decode(input));
        return new String(decryptedBytes);
    }
```
</details>

## Aprendizado efetivo

### HARD SKILLS 
  - **Java**: sei fazer com autonomia
  - **Spring**: sei fazer com autonomia
  - **MySQL**: sei fazer com autonomia

### SOFT SKILLS 
  #### Responsabilidade:
  Como único desenvolvedor responsável pelo Security, assumi o compromisso da entrega dos resultados dentro dos prazos definidos. O Security desempenhava um papel crucial na nossa aplicação definindo de o usuário é um usuário apto a utilizar a aplicação, se o mesmo tem permissão para tal ação e total confiabilidade nos dados lidando com a lei LGPD

  #### Colaboração: 
  Trabalhei em conjunto com a equipe, contribuindo com ideias, compartilhando recursos e assumindo responsabilidades para garantir o sucesso do projeto.
<br>

[Projeto no GitHub](https://github.com/CarcaraTec/Imagem-api6sem)

<br><br>
