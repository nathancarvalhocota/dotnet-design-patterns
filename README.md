# dotnet-design-patterns

Skill do Claude Code para estudar design patterns em C#/.NET no código real que você está lendo.

- **Nota de estudo:** ao explicar, revisar ou escrever C#/.NET para você, o Claude aponta um padrão do código em questão: onde está, por que ajuda ali e quando seria dispensável. No máximo uma nota por resposta, só para padrão ainda não explicado na conversa.
- **Busca:** `/dotnet-design-patterns onde tem Strategy em src/?` ou `/dotnet-design-patterns quais padrões aparecem em <pasta>?`.

O catálogo (`catalog.md`) lista os padrões com exemplos do próprio .NET, sinais para achar no código e quando são exagero.

## Instalação

1. Instale a skill globalmente:

   ```
   npx skills add nathancarvalhocota/dotnet-design-patterns -g -a claude-code
   ```

   Ou clone direto: `git clone https://github.com/nathancarvalhocota/dotnet-design-patterns ~/.claude/skills/dotnet-design-patterns`.

2. Para a nota de estudo aparecer sem você pedir, adicione esta linha ao `~/.claude/CLAUDE.md`:

   ```
   Estudo de design patterns: em resposta direta a mim que leia, explique, revise ou escreva C#/.NET, carregue a skill `dotnet-design-patterns` e siga a seção "Nota de estudo" dela.
   ```

   Sem ela, a skill só entra quando chamada com `/dotnet-design-patterns` ou quando o Claude decide carregá-la pela descrição.

## Atualizar e remover

```
npx skills update dotnet-design-patterns -g
npx skills remove dotnet-design-patterns -g
```

Ao remover, apague também a linha do `~/.claude/CLAUDE.md`.
