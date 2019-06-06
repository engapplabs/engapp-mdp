## EngApp - Mobile Development Pattern

##  Prefácio
Seja bem vindo a esta documentação, nela, vamos exibir como nós, desenvolvedores mobile da EngApp, organizamos, criamos e montamos nossos softwares utilizando o [Flutter](http://flutter.dev "Flutter")

Exibiremos nosso guia de boas práticas para padronizar nosso desenvolvimento e consequentemente espalhar para os que ainda não tenham um rumo ou estão começando nessa área e querem um norte para tal.

Então, vamos lá.

## Introdução

Se você chegou até esse documento é porque desenvolve software para mobile, usando especificamente o Flutter, que é nossa stack oficial para tal.

Então, vamos começar. Vamos passar pelos seguintes tópicos:

- Estrutura do projeto
- Diferença entre funções e classes e como escrever as mesmas
- Declaracao de variaveis
- Gerenciamento de estado

**OBS**: Essa documentação aqui será nossa base inicial, nada impede que ela seja alterada conforme fomos descobrimos maneiras mais fáceis, melhores de fazer o que fazemos atualmente.

**OBS 2**: Caso você tenha um outro padrão para organizar, não se prenda a este conteúdo, aqui você irá encontrar como a Eng App organiza e por fim, padroniza seus projetos.

## **Estrutura do projeto**

Quando se fala em “estrutura do projeto”, quero dizer como organizar seus arquivos, suas pastas, sub-pastas, sub-arquivos, enfim, como seu projeto deverá se organizar daqui pra frente. Segue exemplo:

### Pastas

![Folder](https://i.imgur.com/mbV52nU.png)

Sempre vamos iniciar, dentro da pasta **lib**, com a pasta **src**, dentro dela, seguimos com a criação de nossos componentes, criando a pasta **components**, dentro teremos, por exemplo, a **login component** e dentro de cada **"algum componente"**, teremos sua pasta de **Utils** com o arquivo: **nome_do_componente_pricipal_utils.dart** assim como nosso arquivo para gerenciar nossos requests de API.

No nosso arquivo de Utils, vamos separar nossos componentes/widgets do componente principal. Segue exemplo:

### Componentes

![Componente](https://i.imgur.com/tV23Fjw.png "Componente")

Com isso, mantemos nosso código limpo e conciso, e no componente **Utils** referente ao principal, estarão nossos widgets desenvolvidos.

No nosso arquivo de **network**, é onde ficarão todas as conexões necessárias daquele componente, por exemplo, no **network_login.dart** teremos apenas a requisição para fazer o login, e assim por diante com os demais componentes do projeto.

## Função, Classe ou Variável Local?

Como você já deve saber, existem diversas maneiras de se declarar um componente no Flutter, esses 3 tipos são os mais utilizados (Função, Classe e Variável Local).

Nenhum dos 3 está errado, todos executam de maneira correta o que o usuário digitou, porém, diversos jeitos de se declarar a mesma coisa pode ficar confuso e até mesmo de difícil manutenção futuramente, tanto para você como para outro programador que venha pegar seu código. Vamos a alguns exemplos.

#### Função

    Widget functionWidget({ Widget child}) {
      	return Container(child: child);
     }

#### Classe

    class ClassWidget extends StatelessWidget {
      final Widget child;
    
      const ClassWidget({Key key, this.child}) : super(key: key);
    
      @override
      Widget build(BuildContext context) {
        return Container(
          child: child,
        );
      }
    }

Percebemos aqui que ambas fazem o que foi solicitado, eles criam um `Container` e chamam um `child` como widget filho. No papel o que ocorre é exatamente isso, porém, o framework desconhece funções, mas reconhece classes.

Nossa árvore de widgets ficaria assim no primeiro exemplo:

    Container
      Container

E no segundo:

    ClassWidget
      Container
        ClassWidget
          Container

E isso faz uma diferença enorme em relação a como o framework se comporta quando temos que atualizar um componente.

Aqui está uma lista de itens comparando o uso de funções com classe:

1. Classes:
    * Otimização na performance (const constructor, operator== override, rebuild mais granular)
    * Possuem hot-reload
    * Podemos vê-las no Widget Inspector (debugFillProperties)
    * Podemos atribuir chaves
    * Podemos usar seu próprio contexto
    * Garantem que todos os widgets são usados da mesma forma
    * Garantem que se trocarmos entre dois layouts diferentes, o método dispose vai funcionar corretamente e irá liberar os recursos (as funções geralmente utilizam estado anterior)
2. Funções: 
    * Possuem menos código
    * ?

Podemos ver claramente e com mais detalhe agora.

Não estamos proibindo o uso de funções, porém, para declarar componentes visuais, temos a preferência e será sempre nossa escolha número #1 o uso de classes.

Utilizamos funções para declarar métodos como de requisições, alguma verificação, entre outros.

**OBS**: Não recomendamos o uso de declarar variáveis locais por não ser tão legível quanto uma classe quando se vai observar um código

## Variáveis

Já vimos até então a nossa recomendação para estruturar seu projeto e como criar da melhor forma seus componentes (Widgets). Outro assunto bastante importante é no tocante a como devemos declarar nossas variáveis, sejam elas simples, como declarar um inteiro, ou string. Seja ao nomear suas funções e até mesmo suas classes.

Como já dizia Robert C. Martin, autor do livro "[Codigo Limpo](https://www.amazon.com.br/Código-Limpo-Habilidades-Práticas-Software/dp/8576082675 "Codigo Limpo")", declara que devemos pensar bem no nome de nossas variáveis, não importa se vamos demorar ou não para dar um nome, mas esse nome tem que ser o mais claro e de preferência que dê significado ao que você quer que execute, seguindo, claro, o princípio da responsabilidade única no qual aquela classe, aquela função vai executar apenas uma única tarefa e nada além disso.

Vamos a alguns exemplos:

Ao decorrer da nossa jornada, já vimos vários exemplos de variáveis como esse daqui:

`var x;`

Até então, pra quem está criando o código, pode ser até que não veja tanto problema essa variável com o nome "x", afinal, ele mesmo está sabendo como utilizar ela, seja para armazenar algum inteiro, uma string.

No entanto, conforme o tempo vai passando, você começa a desenvolver mais seu projeto e acaba esquecendo aquela variável x, futuramente, quando se é necessário dar a manutenção, você possivelmente vai ficar perdido, tentando descobrir o que aquela variável x está fazendo ali e qual seu uso.

Isso é algo que Robert C Martin e nós da EngApp **reprovamos**, consideramos isso como "bad practice", jamais, em hipótese alguma permitiremos o uso de variáveis desse tipo.

O código que devemos escrever, é a nossa comunicação com outros profissionais, se futuro programador ou ate mesmo voce nao entender o que está acontecendo com seu código, como você poderá cuidar dele? Até mesmo entender ele? Impossível dessa maneira.

Sempre há um porém, é esse porém se dá caso sua aplicação seja pra mostrar algo rápido pro cliente ou até mesmo pra vc ver como está sua aplicação.

Se seu projeto estiver tomando maiores proporções, crescendo cada vez, tira um dia livre para refatorar essas variáveis. Você pode até achar que é perda de tempo fazer isso, tudo bem, opinião sua, mas torca entao pra nao ter que dar manutenção no seu código futuramente :)
