```
**Título:** XSS Refletido em site.com/renderHTML Resulta em Tomada de Conta

## Resumo:
É possível para um atacante explorar uma vulnerabilidade XSS Refletida em `https://site.com/renderHTML` para executar código JavaScript arbitrário no navegador da vítima e comprometer o Token de Acesso armazenado na chave `access_token` do LocalStorage.

## Descrição:
Um atacante pode especificar código que deve ser renderizado no parâmetro `HTMLCode` para o endpoint `/renderHTML`.
`https://site.com/renderHTML?HTMLCode=<script>alert(document.domain)</script>`.

Este código será refletido no DOM:
`<html>Aqui está seu código: <script>alert(document.domain)</script></html>`

Assim, se um atacante forçar uma vítima a navegar até essa URL, o atacante pode forçar o código JavaScript a ser executado no navegador da vítima sob a origem `site.com`.

Usando isso, é possível para um atacante extrair e exfiltrar a chave `access_token` do LocalStorage usando o seguinte exploit:
`https://site.com/renderHTML?HTMLCode=<script>alert(localStorage.getItem("access_token")</script>`

O que demonstra o acesso e roubo do `access_token` - o token usado para autenticação nesta aplicação.

## Passos Para Reproduzir:
1. Faça login na aplicação como um usuário normal faria (para colocar `access_token` no LocalStorage).
2. Visite `https://site.com/renderHTML?HTMLCode=<script>alert(localStorage.getItem("access_token")</script>` e note que seu `access_token` foi roubado.

## Material de Apoio/Referências:

##Impacto:
É possível usar essa vulnerabilidade para executar JavaScript arbitrário controlado pelo atacante no navegador da vítima sob a origem `site.com`.
Usando isso, somos capazes de demonstrar Tomada de Conta ao exfiltrar o `access_token` que é usado para autenticação. Ao mostrar que controlamos isso, mostramos que podemos sequestrar a conta da vítima e ganhar controle completo. Somos capazes de ler e modificar todos os dados na conta da vítima.

```
