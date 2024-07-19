# Py Flask com Google Colab e Ngrok

Este projeto demonstra como criar uma aplicação Flask no Google Colab, fazer upload de um arquivo JSON e expor a aplicação usando Ngrok.

## Requisitos

- Conta no Google Colab
- Conta no Ngrok
- Arquivo JSON (`data.json`) que será carregado na aplicação

## Passos para execução

### 1. Upload do Arquivo JSON

Primeiro, faça o upload do seu arquivo `data.json` para o Google Colab:

```python
from google.colab import files

# Faça o upload do arquivo `data.json`
uploaded = files.upload()

# Verifique se o arquivo foi carregado corretamente
import json

with open('data.json') as f:
    data = json.load(f)

print(data)
```

### 2. Instalação das Dependências

Instale as bibliotecas necessárias (`flask`, `flask-ngrok` e `pyngrok`):

```python
!pip install flask flask-ngrok pyngrok
```

### 3. Criação da Aplicação Flask

Crie uma aplicação Flask que carrega o arquivo JSON e define duas rotas:

```python
import json
from flask import Flask, jsonify
from pyngrok import ngrok

app = Flask(__name__)

with open('data.json') as f:
    data = json.load(f)

@app.route('/')
def hello_world():
    return "Hello World!"

@app.route('/index')
def get_data():
    return jsonify(data)

if __name__ == '__main__':
    !ngrok authtoken SEU_AUTHTOKEN_AQUI

    public_url = ngrok.connect(5000)
    print(" * ngrok tunnel \"{}\" -> \"http://127.0.0.1:5000\"".format(public_url))

    app.run()
```

### 4. Executando a Aplicação

Execute a célula de código acima. A aplicação Flask será iniciada e um túnel público será criado usando Ngrok. Você verá um URL público impresso no console, que pode ser usado para acessar sua aplicação a partir da web.

### 5. Acessando as Rotas

- Acesse a rota raiz ("/") para ver a mensagem "Hello World!"
- Acesse a rota "/index" para ver os dados do seu arquivo JSON.

## Observações

- Substitua `SEU_AUTHTOKEN_AQUI` pelo seu token de autenticação do Ngrok.
- Certifique-se de que o arquivo `data.json` está corretamente formatado em JSON.
