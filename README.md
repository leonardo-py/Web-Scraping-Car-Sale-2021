# Web Scraping de Venda de Carros em 2021
### Introdução
Este artigo tem a intenção de documentar o processo de web scraping de um ranking de venda de carros de 2021, com a posterior elaboração de um dataset dos valores obtidos.

Informações Iniciais sobre o projeto:
- A linguagem de programação utilizada é Python
- A IDE utilizada para o processo foi o Jupyter Notebook
- O brower escolhido foi o Google Chrome
- As bibliotecas utilizadas no processo são Selenium, WebDriver Manager e Pandas

Comando para instalação das bibliotecas:

- pip install selenium
- pip install webdriver-manager
- pip install pandas

Segue abaixo o link da página web utilizada:

https://g1.globo.com/economia/noticia/2022/01/06/veja-os-carros-mais-vendidos-de-2021-por-categoria.ghtml

### Mãos à Obra
Vamos começar importando uma série de bibliotecas e módulos que serão importantes para o desenvolvimento do projeto.
<img width="625" alt="importação" src="https://user-images.githubusercontent.com/68862907/204959004-bf871bbd-f3e6-4485-87e8-f526f715870a.png">

O mais importante no código acima é a importação do webdriver para que seja possível criar um navegador, que no nosso caso estará armazenado na variável 'navegador'.

Importar o Service e o ChromeDriverManager embora não sejam essênciais para a rotina de web scraping, quando inseridos como argumento do webdriver.Chrome (momento de criação do navegador), ele implementa uma rotina de instalação/atualização de versões atuais do selenium, garantindo que o mesmo sempre esteja atualizado e funcional.

A linha de código para importação do comando By também é fundamental para o processo, pois através dela poderemos selecionar os elementos HTML pelo seu ID, Class_name, TAG_NAME, XPATH, entre outras formas.

Agora iremos abrir nossa página web desejada, inserindo a URL como argumento do comando get
<img width="639" alt="navegador get" src="https://user-images.githubusercontent.com/68862907/204959401-9dd7f789-fbe2-4d96-a236-f9e7dc66d9c3.png">

O site faz o raking de emplacamento por diversas categorias, porém, nesse artigo, vamos trabalhar apenas com a primeira tabela cuja categoria é 'Automóveis Mais Vendidos de 2021' em um Top 20

<img width="449" alt="ranking carros" src="https://user-images.githubusercontent.com/68862907/204960120-8410140e-dcab-4e96-a38e-e0aa20cb0531.png">
Clicando com o botão direito do cursor em qualquer lugar da página e selecionando a opção 'Inspecionar', teremos acesso a guia de desenvolvimento do Chrome, onde a aba 'Elements' nos mostra todo o corpo HTML do site.

<img width="417" alt="html" src="https://user-images.githubusercontent.com/68862907/204960915-9bbc87a0-d589-4d2b-b892-d1436dbe5780.png">

Neste momento iremos dar foco ao nosso ranking de automóveis mais vendidos de 2021, e para isso utilizaremos o recurso Inspecionar Elementos, que esta circulado em vermelho na imagem abaixo.

<img width="417" alt="inspecionar Elementos" src="https://user-images.githubusercontent.com/68862907/204961510-8a8db14c-2156-4b73-8144-2af2df9e5c78.png">

Com a ferramenta, iremos clicar no primeiro carro do nosso ranking para observar o código HTML que será destacado na guia de desenvolvimento.

E agora a estratégia é conseguir obter com o Selenium todo o ranking para ser inserido em uma lista, e para isso existem algumas opções, porém a mais conveniente foi analisar o XPATH do primeiro e último carro do ranking. XPATH é o endereço utilizado para identificar diversas partes da estrutura de uma página HTML, onde iremos observar um padrão na nomenclatura. Perceba que a única diferença entre eles é o último número entre colchetes.

<img width="447" alt="XPATH" src="https://user-images.githubusercontent.com/68862907/204963667-a43f1f4f-4f41-497a-9354-c50a97070320.png">

Dito isso, iremos criar uma lista armazenando os textos com o comendo 'append'. Para que seja possível extrair o texto precisamos ao fim da linha de código inserir o parâmetro '.text', garantindo que um valor de texto será selecionado, que no nosso caso é o nome do nosso carro junto a sua quantidade emplacada.

Também é importante dizer que foi necessário inserir a função 'str()' na variável 'i' do loop para garantir que os valores numéricos seriam compreendidos pelo código como uma string.

![listacarros](https://user-images.githubusercontent.com/68862907/205207168-88fc3e76-b87a-4e6d-91e5-0f13e33b1b0e.png)

Gerada a lista de carros, podemos observar que a quantidade de emplacamentos e o nome do veículo estão unidas em uma string única. 

<img width="724" alt="output lista carros" src="https://user-images.githubusercontent.com/68862907/205208517-f817ffc0-f203-47f6-8494-c9455440dc73.png">

A partir de agora a intenção é coletar os valores da lista e criar um dicionário. 

<img width="720" alt="dicionariocarros" src="https://user-images.githubusercontent.com/68862907/205208814-eac95bd9-ff6e-4ad1-a285-9f424d28fa7f.png">

Devidamente organizadas as chaves e valores do dicionário, agora iniciaremos a etapa de criação do DataFrame, onde vamos utilizar a biblioteca Pandas.

<img width="118" alt="import pandas" src="https://user-images.githubusercontent.com/68862907/205209410-10659396-e4cd-427f-8550-8b47d9d9d28d.png">

Criando o Dataframe com o comando 'Dataframe' e visualizando as cinco primeiras linhas

<img width="304" alt="dataframe" src="https://user-images.githubusercontent.com/68862907/205209903-d0d39776-d0c8-4333-a3f7-f4fd5363f397.png">

E para finalizar iremos corrigir o nome das colunas do dataframe para algo mais intuitivo e funcional.

<img width="322" alt="ranking final" src="https://user-images.githubusercontent.com/68862907/205210281-26c2bea1-56a0-4f2f-880c-d5495d050149.png">

### Considerações Finais

Neste artigo o processo de web scraping ocorreu com o uso da biblioteca Selenium, entretanto é importante ressaltar que existem diversas outras formas de realizar a raspagem de dados, como por exemplo a biblioteca BeautifulSoup ou até mesmo o próprio Pandas em situações mais específicas. Por fim também devemos ter cautela com o uso dessas ferramentas, visto que cada vez mais elas tem sido usadas para uma coleta indiscriminada de dados que podem violar diretrizes de determinados sites, causando bloqueios e em casos excepcionais até processos judiciais.

<br /> <br /> <br /> <br /> 

Se conecte comigo no linkedin: https://www.linkedin.com/in/leonardoandradedasilva/

E-mail: leonardo.andrade.work@gmail.com

