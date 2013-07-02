Valor-da-Construcao-API
=======================

API de integração - www.valordaconstrucao.com.br

Para realizar a integração foi criado um webservice onde será feito o post do arquivo CSV com as informações dos produtos.

O nome deste arquivo será formado pelo "CNPJ" da empresa + Data de Geração do Arquivo + Hora de Geração do Arquivo, separados por "_", no seguinte formato:

Ex:

  CNPJ : 12.123.123/0001-12 (serão utilizados apenas numeros)
  Data de Geração do Arquivo : 24/04/2013 (será utilizada em formato YYYYMMDD)
  Hora de Geração do Arquivo : 16:30 (será utilizada em formato HHMM)

Nome do arquivo enviado -> 12123123000112_20132404_1630.csv

Uma vez recebido e validado o arquivo, o mesmo irá entrar em uma "fila" de processamento.

O arquivo csv será importado inicialmente para uma "base temporária". Após validação automatizada e sem a necessidade de intervenção humana, os registros serão finalmente atualizados no banco de dados real da aplicação. Será feita uma atualização ("update") para os "produtos+loja" já existente, registrando a data/hora da atualização, e os produtos inexistentes serão criados no banco ("insert").

O envio do arquivo deve ser feito utilizando o método HTTP POST na url http://valor-da-construcao.herokuapp.com/scom.xml passando os seguintes parâmetros:

products_file (arquivo CSV); tipo: file.

A autenticação é feita através do HTTP BASIC AUTHENTICATION, passando como parametros o usuário e senha fornecidos pela equipe do valor da construção.

Após executar o envio do arquivo o sistema retornará um XML com o status do envio. 

Se o arquivo for sincronizado com sucesso ele retornará nesse XML a lista de produtos, o status de cada produto (cadastrado ou não) e os possíveis erros em cada um dos produtos da lista.

Se houver algum erro o sistema retornará uma mensagem com o erro no retorno XML.

