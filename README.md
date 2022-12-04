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

```python
from selenium import webdriver
from selenium.webdriver.chrome.service import Service
from webdriver_manager.chrome import ChromeDriverManager
from selenium.webdriver.common.by import By

servico = Service(ChromeDriverManager().install())
navegador = webdriver.Chrome(service = servico)
```

O mais importante no código acima é a importação do webdriver para que seja possível criar um navegador, que no nosso caso estará armazenado na variável 'navegador'.

Importar o Service e o ChromeDriverManager embora não sejam essênciais para a rotina de web scraping, quando inseridos como argumento do webdriver.Chrome (momento de criação do navegador), ele implementa uma rotina de instalação/atualização de versões atuais do selenium, garantindo que o mesmo sempre esteja atualizado e funcional.

A linha de código para importação do comando By também é fundamental para o processo, pois através dela poderemos selecionar os elementos HTML pelo seu ID, Class_name, TAG_NAME, XPATH, entre outras formas.

Agora iremos abrir nossa página web desejada, inserindo a URL como argumento do comando get
```python
navegador.get('https://g1.globo.com/economia/noticia/2022/01/06/veja-os-carros-mais-vendidos-de-2021-por-categoria.ghtml')
```

O site faz o raking de emplacamento por diversas categorias, porém, nesse artigo, vamos trabalhar apenas com a primeira tabela cuja categoria é 'Automóveis Mais Vendidos de 2021' em um Top 20

<img width="449" alt="ranking carros" src="https://user-images.githubusercontent.com/68862907/204960120-8410140e-dcab-4e96-a38e-e0aa20cb0531.png">
Clicando com o botão direito do cursor em qualquer lugar da página e selecionando a opção 'Inspecionar', teremos acesso a guia de desenvolvimento do Chrome, onde a aba 'Elements' nos mostra todo o corpo HTML do site.

<img width="417" alt="html" src="https://user-images.githubusercontent.com/68862907/204960915-9bbc87a0-d589-4d2b-b892-d1436dbe5780.png">

Neste momento iremos dar foco ao nosso ranking de automóveis mais vendidos de 2021, e para isso utilizaremos o recurso Inspecionar Elementos, que esta circulado em vermelho na imagem abaixo.

<img width="417" alt="inspecionar Elementos" src="https://user-images.githubusercontent.com/68862907/204961510-8a8db14c-2156-4b73-8144-2af2df9e5c78.png">

Com a ferramenta, iremos clicar no primeiro carro do nosso ranking para observar o código HTML que será destacado na guia de desenvolvimento.

E agora a estratégia é conseguir obter com o Selenium todo o ranking para ser inserido em uma lista, e para isso existem algumas opções, porém a mais conveniente foi analisar o XPATH do primeiro e último carro do ranking. XPATH é o endereço utilizado para identificar diversas partes da estrutura de uma página HTML, onde iremos observar um padrão na nomenclatura. Perceba que a única diferença entre eles é o último número entre colchetes.

<img width="447" alt="XPATH" src="https://user-images.githubusercontent.com/68862907/204963667-a43f1f4f-4f41-497a-9354-c50a97070320.png">

Dito isso, iremos criar uma lista armazenando os textos com o comando 'append'. Para que seja possível extrair o texto precisamos ao fim da linha de código inserir o parâmetro '.text', garantindo que um valor de texto será selecionado, que no nosso caso é o nome do nosso carro junto a sua quantidade emplacada.

Também é importante dizer que foi necessário inserir a função 'str()' na variável 'i' do loop para garantir que os valores numéricos seriam compreendidos pelo código como uma string.

```python
lista_carros = []

for i in range(1,21):
    elemento = navegador.find_element(By.XPATH,'/html/body/div[2]/main/div[7]/article/div[3]/div[8]/ol/li['+ str(i) +']').text
    lista_carros.append(elemento)
    
print(lista_carros)
```
Gerada a lista de carros, podemos observar que a quantidade de emplacamentos e o nome do veículo estão unidas em uma string única. 

<img width="724" alt="output lista carros" src="https://user-images.githubusercontent.com/68862907/205208517-f817ffc0-f203-47f6-8494-c9455440dc73.png">

A partir de agora a intenção é coletar os valores da lista e criar um dicionário. 

```python
dicionario_carros = {}

for i in range(20):
    split = lista_carros[i].split(':')
    chave_dic = split[0]
    valor_dic = split[1]
    dicionario_carros[chave_dic] = valor_dic
    
print(dicionario_carros)
```

<img width="726" alt="dicionario_carros output" src="https://user-images.githubusercontent.com/68862907/205510785-bfd1e9a7-7887-470a-b0d8-47f687d23058.png">

Devidamente organizadas as chaves e valores do dicionário, agora iniciaremos a etapa de criação do DataFrame, onde vamos utilizar a biblioteca Pandas.

```python
import pandas as pd
````

Criando o Dataframe com o comando 'Dataframe' e visualizando as cinco primeiras linhas

```python
df = pd.DataFrame(list(dicionario_carros.items()))
df.head()
```
<img width="133" alt="df head" src="https://user-images.githubusercontent.com/68862907/205510882-cc8f876d-6485-4214-a88f-0dd585ce92e5.png">

E para finalizar iremos corrigir o nome das colunas do dataframe para algo mais intuitivo e funcional.

```python
df = df.rename(columns = {0:'Carro',1:'Emplacamento'})
df
```

<img width="193" alt="df rename" src="https://user-images.githubusercontent.com/68862907/205510990-7cd13a49-c079-44dd-a521-938793419b6c.png">

### Considerações Finais

Neste artigo o processo de web scraping ocorreu com o uso da biblioteca Selenium, entretanto é importante ressaltar que existem diversas outras formas de realizar a raspagem de dados, como por exemplo a biblioteca BeautifulSoup ou até mesmo o próprio Pandas em situações mais específicas. Por fim também devemos ter cautela com o uso dessas ferramentas, visto que cada vez mais elas tem sido usadas para uma coleta indiscriminada de dados que podem violar diretrizes de determinados sites, causando bloqueios e em casos excepcionais até processos judiciais.

<br /> <br /> <br /> <br /> 

Se conecte comigo no linkedin: https://www.linkedin.com/in/leonardoandradedasilva/

E-mail: leonardo.andrade.work@gmail.com

