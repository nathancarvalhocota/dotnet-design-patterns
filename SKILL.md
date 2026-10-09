---
name: dotnet-design-patterns
description: Use quando ler, explicar, revisar ou escrever código C#/.NET (incluindo Blazor e ASP.NET Core) em que um design pattern seja reconhecível, ou quando o usuário perguntar onde um padrão aparece no código, quais padrões um arquivo, pasta ou projeto usa, ou se vale aplicar um padrão.
---

# Design patterns em .NET (estudo)

## Visão geral

Serve ao estudo do usuário: ligar o nome de um padrão ao código real que ele está vendo, com o porquê e o custo. São dois modos: a nota de estudo, que sai junto de qualquer resposta, e a busca, quando o usuário pede.

Catálogo, com os sinais para reconhecer cada padrão: `catalog.md`, nesta pasta.

## Nota de estudo

Escreva a nota quando todas as condições valerem:

- A resposta vai direto para o usuário. Mensagem para outro agente (relatório, resposta a quem delegou a tarefa), commit, PR, spec, comentário de código e arquivo do projeto não levam nota.
- O padrão está no código que esta resposta leu, editou ou discutiu. Não procure padrões fora disso.
- Dá para apontar a estrutura que caracteriza o padrão (coluna "Sinais" do catálogo), não só um nome de classe sugestivo.
- O padrão ainda não foi explicado nesta conversa. Não guarde em memória o que já foi explicado: em outra conversa, pode explicar de novo.

No máximo uma nota por resposta. Com mais de um candidato, escolha o que mais ajuda a entender o código discutido.

### Formato

No fim da resposta, separada do resto, em citação. Três partes em texto livre, até ~80 palavras:

1. Nome do padrão, `arquivo:linha` e os objetos que o formam.
2. O que o padrão faz nesse trecho e por que ajuda aqui.
3. Quando seria dispensável ou o que custa.

> **Padrão: Decorator** em `RetryHandler.cs:12` (`RetryHandler : DelegatingHandler`, registrado com `AddHttpMessageHandler`)
> Envolve o handler HTTP real e acrescenta retry sem que o `HttpClient` nem quem o chama saibam. Ajuda porque o retry vira uma peça plugável, reaproveitada por vários clients.
> Seria dispensável com um único client e um único ponto de chamada: um laço de retry ali resolveria.

## Busca

Quando o usuário pergunta onde um padrão aparece ou quais padrões um trecho usa:

1. Use os sinais do catálogo para achar candidatos. Em pasta grande ou projeto inteiro, delegue a varredura a um subagent.
2. Confirme cada candidato lendo o código: sinal de busca sozinho não prova o padrão.
3. Responda agrupado por padrão, uma linha por ocorrência: `arquivo:linha`, objetos envolvidos e o papel do padrão ali.
4. Sem ocorrência: diga onde procurou e cite padrões parecidos que apareceram.

Na busca, cada ocorrência é uma linha da lista, não uma nota de estudo.
