# PagSeguro Doações 
## Aceite doações únicas ou recorrentes via PagSeguro
### E com apenas 4 linhas de código

## Instalação e configuração
### 1. Adicione o código abaixo em qualquer página
```html
<div id="doar-pagseguro" data-receiveremail="testepagseguro2@ricardomartins.net.br" data-title="Contribua com nosso projeto" data-logo=""></div>

<script src="https://cdn.jsdelivr.net/gh/r-martins/doacao-pagseguro@1/dist/js/chunk-vendors.js"></script>
<script src="https://cdn.jsdelivr.net/gh/r-martins/doacao-pagseguro@1/dist/js/app.js"></script>
<link href="https://cdn.jsdelivr.net/gh/r-martins/doacao-pagseguro@1/dist/css/app.css" rel="stylesheet">
```
### 2. Substitua o e-mail recebedor
Modifique o e-mail em `data-receiveremail`. <br/>Coloque o e-mail da conta PagSeguro que receberá os pagamentos.

### 3. Autorize a conta PagSeguro no modelo de aplicação
[Clique aqui](https://pagseguro.ricardomartins.net.br/autorizar.html) para autorizar a aplicação de "PagSeguro Integrações/Ricardo Martins".<br/>
A autorização é gratuita e você passará a pagar menos taxas em pagamentos realizados com o Widget.<br/>
Saiba mais [aqui](https://pagseguro.ricardomartins.net.br/compare.html).

### Pronto!
Você está pronto para receber doações únicas ou recorrentes via PagSeguro.<br/>
Se tiver dúvidas ou problemas, acesse nossa [central de ajuda](https://pagsegurotransparente.zendesk.com/hc/pt-br). <br/>Você também pode entrar em contato com nosso suporte por lá.

# Demonstração
Você pode ver uma demonstração ao vivo [aqui](https://pagseguro.ricardomartins.net.br/doar-com-pagseguro.html).
<br/>Veja um screenshot abaixo: <br/>
<img src="https://user-images.githubusercontent.com/191149/153778187-0275a7a8-18e3-4eb6-af93-b4672ea8ac6c.png" width="300"/>

Você pode personalizar logo, cores e visual. Este é apenas um modelo.

# Outras propriedades
`data-logo`

Personalize com o URL do logo da sua empresa

`data-title`

Personalize com uma frase para o título do seu widget. Ela aparece logo abaixo do logo

`data-receiveremail`

Preencha com o e-mail da sua conta PagSeguro. Lembre-se que você deve autorizar nossa aplicação antes.

