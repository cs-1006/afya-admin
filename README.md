# Afya Admin — Dashboard com Blazor WebAssembly e MudBlazor

## Identificação

| | |
|---|---|
| **Aluno(a)** | Cauã Ferreira Sales Silva |
| **Matrícula** | 2628617 |
| **Faculdade** | São Lucas Campus II |
| **Curso** | Ciência da Computação |
| **Disciplina** | Programação de Sistemas Web |
| **Professor(a)** | Liluyoud Cury Lacerda |
| **Semestre** | 2026.2 |

## Objetivo do projeto

O objetivo desta atividade foi colocar em prática maior parte dos conhecimentos de aulas anteriores, especialmente as de html e webassembly, para criar uma aplicação com dados ficticios representando uma plataforma da afya. 

Essa pagina conta com irformaçoes como atividades e projetos recentes, distribuiçao de clientes e receita da instituiçao. Outras paginas que aparecem na barra lateral e botoes nao funcionam, pois nao era o foco da tarefa realizada.

## Tecnologias utilizadas

- .NET 10 / Blazor WebAssembly
- MudBlazor 9

## Como executar

Passo a passo para outra pessoa clonar e rodar o projeto:

```bash
git clone https://github.com/seu-usuario/afya-admin.git
cd afya-admin
dotnet watch
```
.net versão 10.0.203

## Telas

### Tema claro
![Dashboard — tema claro](docs/prints/tema-claro.png)

### Tema escuro
![Dashboard — tema escuro](docs/prints/tema-escuro.png)

### Versão mobile
![Dashboard — celular](docs/prints/mobile.png)

### HTML gerado (DevTools)
![Inspeção do HTML no DevTools](docs/prints/devtools.png)

Explique em poucas linhas o que o print do DevTools mostra: qual componente você inspecionou, qual HTML ele gerou e quais classes apareceram.

## Estrutura do projeto

<img width="116" height="149" alt="image" src="https://github.com/user-attachments/assets/cef6a32f-2473-4b41-b874-2ada7641127d" />

* Components: Armazena os componentes visuais e reutilizáveis da interface (como cartões, botões e gráficos).
* Data: Contém as classes de modelo de dados (*records*), serviços de dados e ficheiros de *mock data*.
* Layout: Guarda as estruturas de layout mestre da aplicação, tais como a barra superior (`MudAppBar`) e a gaveta lateral (`NavMenu`).
* Pages: Reúne os componentes roteáveis (com a diretiva `@page`) que representam as páginas e ecrãs acessíveis por URL.
* wwwroot: Armazena os ficheiros estáticos e públicos da aplicação, como imagens, estilos CSS, scripts JavaScript e o `index.html`.

## Componentes criados

| Componente | Responsabilidade | Parâmetros que recebe |
|---|---|---|
| `DashboardCard` | Servir como wrapper base flexível e padronizado para os cartões do painel | Titulo, Subtitulo, HeaderAction, ChildContent, Class, Elevation |
| `KpiCard` | Exibir indicadores-chave de desempenho (KPIs) com variação percentual, ícone em fundo pastel e minigráfico de tendência | Titulo, Valor, Variacao, Icone, Cor, DadosGrafico |
| `CabecalhoPagina` | Exibir o título principal do ecrã, subtítulo explicativo e guardar ações ou filtros do topo | Titulo, Subtitulo, ChildContent |
| `SeletorPeriodo` | Controlar o filtro de intervalo temporal dos dados visualizados no painel | Valor, ValorChanged (suporte a @bind-Valor) | 
| `GraficoReceita` | Renderizar o gráfico de linhas interativo para comparar a receita realizada em relação às metas | DadosReceita |
| `GraficoDistribuicaoClientes` | Exibir o gráfico de rosca com a divisão por segmento de clientes e totalizador SVG central | DadosDistribuicao, TotalClientes |
| `PerformanceProjetos` | Mostrar o nível de conclusão dos projetos através de barras de progresso lineares | Projetos |
| `AtividadesRecentes` | Exibir a linha do tempo / feed de eventos com as últimas interações registadas no sistema | Atividades |
| `ProjetosRecentes` | Renderizar a tabela de dados com o histórico, estados e valores dos projetos recentes | Projetos |

## O que aprendi

1. Como uma aplicação Blazor WebAssembly inicia no navegador? Qual é o papel do `index.html`, da `<div id="app">` e do `Program.cs`?
 
 O navegador descarrega primeiro o index.html, que carrega o runtime do WebAssembly e os scripts do Blazor. A <div id="app"> funciona como o ponto onde a interface vai ser feita. Program.cs inicializa as configurações da aplicação e manda renderizar o componente principal dentro dessa div.

2. Qual é a diferença entre um **Layout**, uma **Page** e um **Component** neste projeto? Dê um exemplo de cada.

O Layout é a moldura fixa que envolve a aplicação, como o MainLayout.razor que mantém a barra superior e o menu lateral. A Page é a página que muda conforme o endereço, por exemplo a Index.razor associada à rota @page "/". Já o Component é um bloco visual reutilizável que se pode colocar dentro das páginas ou layouts, como o KpiCard.razor

3. O que é um `RenderFragment` e como o `DashboardCard` usa esse recurso para ser reutilizado por vários cards?

O RenderFragment é um tipo especial do Blazor que representa um pedaço de HTML a ser renderizado dinamicamente. O DashboardCard usa um RenderFragment para funcionar como um "slot". Assim, o cartão mantém a sua moldura, sombra e cabeçalho padrão, sem ter de duplicar código.

4. Como funciona o `@bind-Valor` no `SeletorPeriodo`? Qual é o papel do `ValorChanged`?

O @bind-Valor é o atalho do Blazor para realizar ligação de dados bidirecional. Quando o utilizador altera a opção no SeletorPeriodo, o componente dispara o evento ValorChanged para notificar a página pai de que o estado mudou, atualizando a variável vinculada de forma automática.

5. Por que os dados ficam na pasta `Data`, separados dos componentes? Que vantagem isso traz se, no futuro, os dados vierem de uma API?

Guardar os dados na pasta Data separa as responsibilidades da aplicaçao, garantindo que os componentes visuais apenas se preocupem em desenhar a interface. A grande vantagem é que, se decidires ligar uma API no futuro, basta alterar o serviço de dados na camada Data para procurar as informações no servidor, sem precisar de tocar numa única linha de código visual dos teus componentes.

6. Como o `MudGrid` com `xs`, `sm` e `lg` faz os cards de KPI se reorganizarem em telas de tamanhos diferentes?

O MudGrid usa um sistema responsivo flexível dividido em 12 colunas. Ao definires propriedades como xs="12", sm="6" e lg="3", dizes ao layout quantos blocos o card deve ocupar: no telemóvel (xs), o card ocupa as 12 colunas; no tablet (sm), ocupa 6; e em ecrãs grandes (lg), ocupa 3 colunas.

7. Como foi possível estilizar a página inteira sem escrever CSS? Explique o papel do tema (`MudTheme`) e das classes utilitárias.

Foi possível graças ao ecossistema do MudBlazor, que substitui o CSS tradicional por parâmetros e classes utilitárias. O MudTheme centraliza as definições globais de cores, modo claro/escuro, fontes e cantos arredondados num só local, enquanto os utilitários do MudBlazor tratam do alinhamento e dos espaçamentos diretamente na marcação do Blazor.

8. Por que o namespace do projeto é `afya_admin` e não `afya-admin`?

Isto acontece devido às regras de sintaxe da linguagem C#. Os namespaces em C# seguem as regras de identificadores de código, onde o hífen (-) é um operador reservado para subtração e não pode ser usado no nome de classes ou namespaces. Por isso, ao criar o projeto, o .NET substitui automaticamente hífenes por underscores (_).

## Dificuldades e soluções

Algumas parte do codigo no pdf de referencia estavam cortatos ou incompletos. Facilmente resolvido prestando atençao em indentaçao e formato de comandos de blazor.

Ao abrir a aplicaçao usando dotnet watch era possivel ver um erro no posicionamento do 'clientes' abaixo do 1.842(total de clientes). Resolvido diminuindo o y de 58 para 52.
