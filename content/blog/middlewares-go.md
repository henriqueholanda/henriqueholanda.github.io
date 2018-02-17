---
title: "Middlewares em Go"
date: 2018-02-17T17:29:20-02:00
draft: false
comments: true
---

Quando criamos aplicações web em Go, o primeiro code smell que nos deparamos é a duplicidade de código. Sempre precisamos realizar algumas tarefas em cada endpoint como gravar um log, autenticar usuário, iniciar uma transação no NewRelic, etc.

A forma que mais me deixa confortável pra resolver esse tipo de problema é criar Middlewares.

Para exemplificar isso vamos criar uma simples API, com a seguinte estrutura:

```
/controllers
    - index.go
/middlewares
    - authentication.go
    - notFound.go
api.go
```

Vamos começar com nosso controller:

{{< gist henriqueholanda b1a9dad66a915829fedf841b1a842b13 >}}

Como podemos ver, definimos a estrutura dele para receber um objeto que implementa a interface Handler, e implementamos a função ServeHTTP com o código de nosso controller, que neste caso irá retornar um texto na nossa response. Todas as nossas controllers deverão seguir essa mesma estrutura, modificando somente o corpo da função ServeHTTP.

A seguir iremos criar nossos middlewares, um para tratar páginas não encontradas e retornar 404, e outro para validar um token JWT e autorizar a requisição.

{{< gist henriqueholanda f0f847058440738b2c587953030e3a0a >}}

{{< gist henriqueholanda 92551b77dda65d3ca2cba5ad4170c2d7 >}}

Agora vamos criar nosso arquivo principal, onde vamos juntar os middlewares, e iniciar nosso servidor.

{{< gist henriqueholanda dabf15675e282de46e0e102e7ee4a59a >}}

Esse é nosso arquivo principal, ele é o primeiro a ser executado, e nele temos todas as rotas, que são um http.Handle. O primeiro parâmetro é nosso endpoint, e o segundo precisa ser uma implementação da interface Handler, que no nosso caso, passamos nossos middlewares.

Cada middleware, recebe o próximo middleware a ser executado, até chegar no nosso controller. E podemos definir quais middlewares serão executados em cada endpoint de nossa aplicação, como nesse exemplo em que no endpoint "/" não precisamos fazer a autenticação do token JWT.

Na execução desse código, quando você acessar http://localhost:8085/teste por exemplo, que é uma rota inválida, ele vai começar passar por todas as rotas, para verificar se faz um match com alguma definida, e neste caso, ao passar na primeira rota e entrar no middleware de NotFound, será retornado um status code 404 na resposta da requisição, e não executará os próximos middlewares e nem a controller.

Bom, esse foi um simples exemplo de como implementar middlewares em Go. Dúvidas, questionamentos ou qualquer tipo de feedback serão sempre muito bem-vindos. 😉