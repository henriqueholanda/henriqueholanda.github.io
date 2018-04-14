---
title: "Refactoring PHP - Introduce Null Object"
date: 2018-04-14T19:00:00-02:00
draft: true
comments: false
preview: true
---

Refatorar código é uma tarefa que está presente diáriamente em nossas vidas.
Os Design Patters estão aí para nos ajudar a deixar nossos códigos com mais 
facilidade de entendimento e organização.

Mas nem todos os Design Patterns são muito conhecidos e utilizados.

Um bom exemplo disso é o **Null Object**.

O Null Object é um pattern de proteção, ou seja, ele serve para proteger nosso
código de possíveis `BUGs`.

### Um erro de bilhões de dólares
Ao pesquisar sobre o uso de referência nula à objetos, descobri que  Tony Hoare, 
o criador do ALGOL diz que a implementação da referência nula à um objeto 
é um erro de bilhões de dólares, pois a mais de 40 anos várias empresas tiveram vários 
BUGS em seus sistemas por causa de um objeto nulo não previsto.

### Propoósito

O Null Object serve para encapsular a ausência de um objeto através de um substituto 
que ofereça um comportamento padrão adequado ao domínio.

### Problema
Quem nunca esqueceu de verificar se um objeto ou variável está com valor NULL que atire a 
primeira pedra.
Esse é um problema muito comum e que pode causar grandes transtornos.

Existem várias formas de proteger seu código contra esse problema, a mais comum que 
encontramos é o uso de `ifs`:
```php
    if ($var != null) {
        $var->doSomething();
    }
```

Essa solução pode muito bem resolver nosso problema, mas acaba gerando diversos outros:
 
 - Muita duplicidade de código;
 - Dificuldade no entendimento do código;
 - Objetos com diversas responsabilidades;

Além disso, a facilidade de um desenvolvedor esquecer de implementar esse `if` é muito grande
e normalmente, o resultado de um null check `$var == null` é **fazer nada** ou retornar um **valor padrão**.

# **... to be continue ...**
