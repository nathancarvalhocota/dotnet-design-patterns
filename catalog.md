# Catálogo de padrões em .NET

Os sinais acham candidatos. Confirme lendo o código antes de afirmar o padrão.

Injeção de dependência e async/await não entram: estão em todo código .NET e não rendem nota.

## Criação

| Padrão | Ideia | No próprio .NET | Sinais no código | Exagero quando |
|---|---|---|---|---|
| Factory Method | Um método decide qual implementação criar | `IHttpClientFactory`, `ILoggerFactory` | `static ... Create(...)`, `switch` que devolve `new X()`, classes `*Factory` | Só existe um tipo concreto: `new` direto resolve |
| Abstract Factory | Cria uma família de objetos que combinam entre si | `DbProviderFactory` (conexão, comando, parâmetro do mesmo banco) | Interface com vários `Create*` relacionados | Só existe uma família |
| Builder | Monta um objeto complexo passo a passo e entrega no fim | `StringBuilder`, `UriBuilder`, `WebApplication.CreateBuilder`, `ConfigurationBuilder` | Métodos que devolvem `this` (fluent) e terminam em `.Build()` | Poucos parâmetros obrigatórios: construtor, `record` ou `init` resolve |
| Singleton | Uma única instância compartilhada | `services.AddSingleton<T>()`, `Lazy<T>` | Construtor `private` com `static Instance`, `AddSingleton` | O DI já controla o lifetime; `static` esconde a dependência e atrapalha teste |
| Prototype | Cria cópia de um objeto existente | `record` com `with`, `ICloneable` | `x with { ... }`, `Clone()` | Objeto imutável que nunca precisa de cópia alterada |

## Estrutura

| Padrão | Ideia | No próprio .NET | Sinais no código | Exagero quando |
|---|---|---|---|---|
| Adapter | Traduz uma interface na que o cliente espera | `StreamReader` (de `Stream` para `TextReader`) | Classe `: IEsperada` que recebe um tipo não relacionado no construtor e converte as chamadas | Uma chamada só: converter no lugar resolve |
| Decorator | Embrulha um objeto com o mesmo contrato e acrescenta comportamento | `BufferedStream`, `GZipStream`, `DelegatingHandler` | Classe `: IX` que recebe `IX` no construtor e delega; `services.Decorate<IX, Y>()`; `AddHttpMessageHandler` | O comportamento vale sempre e para um único uso: cabe no próprio serviço |
| Facade | Uma entrada simples para um subsistema complicado | `File.ReadAllText` (esconde `FileStream` e `StreamReader`) | Serviço que orquestra vários outros e expõe poucos métodos | Wrapper 1:1 que só repassa chamadas |
| Proxy | Mesmo contrato, controla acesso, carga tardia ou chamada remota | `DispatchProxy`, lazy-loading proxies do EF Core, clients HTTP gerados (proxy remoto) | Classe `: IX` que guarda `IX` e checa permissão, cache ou rede antes de delegar | O controle nunca muda: fica no próprio objeto |
| Composite | Trata uma árvore e suas folhas do mesmo jeito | Árvore de componentes do Blazor, `XElement`, `Expression` | Tipo com coleção de filhos do mesmo tipo e método recursivo | Estrutura plana, sem aninhamento real |

## Comportamento

| Padrão | Ideia | No próprio .NET | Sinais no código | Exagero quando |
|---|---|---|---|---|
| Strategy | Algoritmo trocável sem mudar quem usa | `IComparer<T>`, `IEqualityComparer<T>`, o lambda do `OrderBy` | Interface com várias implementações escolhidas em runtime; parâmetro `Func<...>` | `switch` de poucos casos que não cresce, ou um delegate já resolve |
| Observer | Avisa interessados quando algo muda | `event`/`EventHandler`, `IObservable<T>`, `INotifyPropertyChanged`, `EventCallback` do Blazor | `event`, `+=`, `Subscribe(`, `OnChange(` | Um único ouvinte fixo: chamada direta é mais clara |
| Command | A ação vira objeto: enfileirar, desfazer, registrar | `ICommand` (WPF/MAUI), requests do MediatR | Classe com os dados da ação e um `Execute`/`Handle`; fila ou histórico de operações | A ação roda na hora e nunca é enfileirada nem desfeita |
| Template Method | A base define o roteiro e as subclasses preenchem etapas | `ComponentBase` (`OnInitialized`, `OnParametersSetAsync`), `BackgroundService.ExecuteAsync` | `abstract class` com método concreto que chama `protected abstract` ou `virtual` | Uma subclasse só, ou composição resolve sem herança |
| Chain of Responsibility | O pedido passa por uma corrente; cada elo trata ou repassa | Middleware do ASP.NET Core, corrente de `DelegatingHandler`, filtros do MVC | `RequestDelegate next`, `await next(`, `app.Use(` | Um único tratamento fixo |
| State | O comportamento muda conforme o estado, uma classe por estado | Não tem exemplo clássico na BCL | Classes de estado com a mesma interface; o mesmo `switch` sobre um enum de status repetido em vários métodos é candidato | Poucos estados e transições simples: um `switch` resolve |
| Iterator | Percorre uma coleção sem expor como ela é guardada | `IEnumerable<T>`, `IAsyncEnumerable<T>`, `yield return` | `yield return`, implementação de `GetEnumerator` | Lista pequena já materializada |
| Mediator | Objetos falam por um intermediário em vez de se conhecerem | `IMediator` do MediatR | `IMediator.Send`, `IRequestHandler<,>` | Duas classes que só se chamam: chamada direta é mais clara |
| Visitor | Operação nova sobre uma hierarquia sem mexer nas classes | `ExpressionVisitor`, `CSharpSyntaxWalker` do Roslyn | Métodos `Visit*`/`Accept` | Hierarquia pequena: `switch` com pattern matching sobre os tipos resolve |
| Null Object | Objeto que não faz nada no lugar de `null` | `NullLogger.Instance`, `Stream.Null` | `Null*`/`Empty*` estáticos que implementam a interface | Quem chama precisa saber que está vazio |

## Arquitetura comum em .NET

| Padrão | Ideia | No próprio .NET | Sinais no código | Exagero quando |
|---|---|---|---|---|
| Repository | Esconde o acesso a dados atrás de métodos do domínio | `DbSet<T>` do EF Core já faz esse papel | `I*Repository` com consultas nomeadas pelo domínio | Repositório genérico sobre o EF que só repassa o `DbSet` |
| Unit of Work | Acumula as mudanças e grava tudo junto | `DbContext` + `SaveChanges` do EF Core | Change tracking e um único `SaveChanges` por operação | Unit of Work próprio em cima de um `DbContext`, que já é um |
| Options | Configuração tipada injetada | `IOptions<T>`, `IOptionsSnapshot<T>`, `IOptionsMonitor<T>` | `services.Configure<T>(...)`, `IOptions<` | Valor que nunca muda entre ambientes: `const` resolve |
| Specification | Regra de filtro como objeto reutilizável e combinável | `Expression<Func<T, bool>>` | Classes `*Specification`, membros que devolvem `Expression<Func<...>>` | Filtro usado uma vez: `Where` no lugar resolve |
| Result | Sucesso ou erro como retorno, sem exceção | `int.TryParse(s, out var n)` | `Try*(... out ...)`, `Result<T>`, `IsSuccess` | A falha é realmente excepcional: exceção é mais clara |
