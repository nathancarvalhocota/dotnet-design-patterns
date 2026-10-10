# dotnet-design-patterns

Skill para Claude Code, Codex e outros agentes compatíveis com Agent Skills, para estudar design patterns em C#/.NET no código real que você está lendo.

- **Nota de estudo:** ao explicar, revisar ou escrever C#/.NET para você, o agente aponta um padrão do código em questão: onde está, por que ajuda ali e quando seria dispensável. No máximo uma nota por resposta, só para padrão ainda não explicado na conversa.
- **Busca:** peça onde um padrão aparece ou quais padrões uma pasta usa. O agente confirma as ocorrências no código e explica o papel de cada uma.

O catálogo ([catalog.md](catalog.md)) lista os padrões com exemplos do próprio .NET, sinais para achar no código e quando são exagero. Todos os agentes usam os mesmos arquivos [SKILL.md](SKILL.md) e `catalog.md`.

## Instalação com `npx skills`

Requer Node.js/npm e Git. Escolha o agente, ou instale para os dois:

```sh
# Claude Code
npx skills add nathancarvalhocota/dotnet-design-patterns -g -a claude-code

# Codex
npx skills add nathancarvalhocota/dotnet-design-patterns -g -a codex

# Claude Code e Codex
npx skills add nathancarvalhocota/dotnet-design-patterns -g -a claude-code -a codex
```

`-g` instala para o usuário, em todos os projetos. Para instalar apenas no projeto atual, rode o comando na raiz dele e omita `-g`.

O instalador mantém uma cópia compartilhada e, por padrão, cria links para os agentes que precisam deles. Use `--copy` se preferir cópias independentes. Consulte os [agentes e caminhos suportados pelo instalador](https://github.com/vercel-labs/skills#supported-agents) para outros agentes; para um agente que lê `.agents/skills`, também é possível usar a instalação manual abaixo.

## Instalação manual com Git

Escolha um destino reconhecido pelo seu agente:

| Agente | Global (usuário) | Somente no projeto |
| --- | --- | --- |
| Claude Code | `~/.claude/skills/dotnet-design-patterns` | `.claude/skills/dotnet-design-patterns` |
| Codex (documentação atual) | `~/.agents/skills/dotnet-design-patterns` | `.agents/skills/dotnet-design-patterns` |
| Outros agentes que leem `.agents/skills` | `~/.agents/skills/dotnet-design-patterns` | `.agents/skills/dotnet-design-patterns` |

Os comandos abaixo funcionam em Bash, Git Bash e PowerShell. `$HOME` é a pasta do usuário; no Windows, normalmente `C:\Users\<usuario>`.

```sh
# Claude Code: instalação global
git clone https://github.com/nathancarvalhocota/dotnet-design-patterns.git "$HOME/.claude/skills/dotnet-design-patterns"

# Codex e agentes que leem .agents: instalação global compartilhada
git clone https://github.com/nathancarvalhocota/dotnet-design-patterns.git "$HOME/.agents/skills/dotnet-design-patterns"
```

Para instalar somente no projeto, escolha um dos comandos na raiz dele:

```sh
# Claude Code
git clone https://github.com/nathancarvalhocota/dotnet-design-patterns.git .claude/skills/dotnet-design-patterns

# Codex e agentes que leem .agents
git clone https://github.com/nathancarvalhocota/dotnet-design-patterns.git .agents/skills/dotnet-design-patterns
```

### Codex em `.codex/skills`

Alguns ambientes e o instalador integrado do Codex usam `$CODEX_HOME/skills` (por padrão, `~/.codex/skills`). Se o seu ambiente carrega skills desse local, também pode instalar ali:

```sh
git clone https://github.com/nathancarvalhocota/dotnet-design-patterns.git "$HOME/.codex/skills/dotnet-design-patterns"
```

Com `CODEX_HOME` personalizado, use a pasta `skills/dotnet-design-patterns` dentro dele. Para novas instalações compartilhadas, prefira `.agents/skills`, conforme a [documentação do Codex](https://learn.chatgpt.com/docs/build-skills#where-codex-loads-local-skills). Escolha um único destino para o Codex, evitando duas instalações com o mesmo nome no seletor.

A pasta instalada precisa conter `SKILL.md` e `catalog.md` juntos. Instalar em `.agents` atende aos agentes que procuram skills nesse caminho; outros agentes podem exigir configuração própria.

## Uso

No Claude Code:

```text
/dotnet-design-patterns onde tem Strategy em src/?
/dotnet-design-patterns quais padrões aparecem em src/Services/?
```

No Codex:

```text
$dotnet-design-patterns onde tem Strategy em src/?
$dotnet-design-patterns quais padrões aparecem em src/Services/?
```

Em outros agentes, use o seletor de skills disponível ou peça para carregar `dotnet-design-patterns` e localizar os padrões na pasta desejada. Se o agente não descobrir a skill, indique o caminho de `SKILL.md`. Reinicie a sessão se a instalação ainda não aparecer.

## Nota de estudo automática

O agente pode selecionar a skill pela descrição. Para pedir a nota de estudo de forma consistente, adicione a instrução abaixo ao arquivo de instruções apropriado:

| Agente | Instruções globais | Instruções do projeto |
| --- | --- | --- |
| Claude Code | `~/.claude/CLAUDE.md` | `CLAUDE.md` |
| Codex | `~/.codex/AGENTS.md` (ou `$CODEX_HOME/AGENTS.md`) | `AGENTS.md` |
| Outros agentes | Conforme a configuração do agente | `AGENTS.md`, se suportado |

```text
Estudo de design patterns: em resposta direta a mim que leia, explique, revise ou escreva C#/.NET, carregue a skill `dotnet-design-patterns` e siga a seção "Nota de estudo" dela.
```

A instalação não edita esses arquivos automaticamente. Sem essa instrução, você ainda pode invocar a skill explicitamente, e agentes com seleção automática podem carregá-la pela descrição.

## Atualizar e remover

Se instalou com `npx skills`:

```sh
npx skills update dotnet-design-patterns -g
npx skills remove dotnet-design-patterns -g
```

Para instalações somente no projeto, use `npx skills update dotnet-design-patterns -p` e `npx skills remove dotnet-design-patterns` na raiz dele.

Se clonou manualmente, atualize com `git -C <pasta-da-skill> pull --ff-only`. Para remover, exclua a pasta que você clonou.

Ao desinstalar a skill de um agente, apague também a linha de nota automática do arquivo de instruções em que você a adicionou: `CLAUDE.md` para Claude Code, `AGENTS.md` para Codex ou o arquivo equivalente para outros agentes. Use o arquivo global ou do projeto, conforme a instalação. Se o arquivo for compartilhado com agentes que continuam usando a skill, mantenha a instrução para eles.

## Referências de instalação

- [Skills no Claude Code](https://code.claude.com/docs/en/skills)
- [Skills no Codex](https://learn.chatgpt.com/docs/build-skills)
- [CLI `skills`](https://github.com/vercel-labs/skills)
