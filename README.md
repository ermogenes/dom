# Ambiente de execução JavaScript

Códigos JavaScript são executados em um ambiente como um navegador (Chrome, Firefox), um servidor (Node, Deno) ou qualquer outro tipo de plataforma.

Quando executado em um navegador, é criada uma estrutura de objetos chamada BOM (_Browser Object Model_) que pode ser manipulada livremente.

## Objeto `window`

É a raiz da estrutura. Contém informações sobre a janela e métodos para sua manipulação. É também o _container_ para todos os objetos globais criados pela aplicação.

Contém, entre outras coisas, os objetos:

- `console`, que provê acesso ao console do navegador;
- `history`, que provê acesso ao histórico do navegador;
- `location`, que provê acesso ao console do navegador;
- `navigator`, que provê informações sobre a localização (URL) atual;
- `screen`, que provê informações sobre a tela do usuário;
- `frames`, que contém os _frames_ do documento;
- `document`, que contém o documento atual, seguindo o modelo DOM.

💡 Veja/navegue pela estrutura de um objeto qualquer usando `console.dir` (para explorar suas propriedades) e `console.log` (para ver sua árvore no DOM).

# DOM - _Document Object Model_

Estrutura de objetos [padronizada pelo WHATWG](https://dom.spec.whatwg.org/) como ponto de entrada comum para todo o conteúdo do documento.

O objeto raiz é `document`. Abaixo dele existe tudo que foi _parseado_ do documento HTML de origem, incluíndo textos dentro e fora de _tags_, comentários e _scripts_.

Exemplo:

```html
<!DOCTYPE HTML>
<html>
<head>
  <title>Título do documento</title>
</head>
<body>
  Conteúdo do documento
</body>
</html>
```

Nós na estrutura do DOM:

```
DOCTYPE: html
  HTML
    HEAD
      #text: \n, 2 espaços
      TITLE
        #text: Título do documento
      #text: \n
    #text: \n
    BODY
      #text: Conteúdo do documento
```

Ref.: Live DOM Viewer: http://software.hixie.ch/utilities/js/live-dom-viewer/

Para garantir a integridade do DOM, algumas autocorreções são feitas:

- Nós `html` e `body` (e `tbody`) são criadas caso não existam;
- Espaços e quebras de linha antes de `head` são ignorados;
- Todo conteúdo após `body` é movido para dentro.

🍌 As ferramentas de desenvolvedor do navegador ocultam nós que só contenham espaços e/ou quebras de linha (por simplicidade), mas eles existem no DOM.

## Principais tipos de nós

- Documento: nó raiz;
- Elemento: nós contendo as _tags_ do documento;
- Texto: nós de conteúdo textual;
- Comentário: nós contendo os comentários do documento (que podem ser processados).

## Obtendo elementos do DOM

- `document.documentElement`: contém a _tag_ `html`
- `document.head`: contém a _tag_ `head`
- `document.body`: contém a _tag_ `body`, ou `null` se chamado dentro de `head` (_ainda não foi lido pelo navegador_).

## Navegando pela estrutura do DOM

Alguns termos importantes:

- filhos (_children_): todos os elementos diretamente dentro de um elemento, incluíndo textos e comentários;
- descendentes (_descendants_): todos os elementos e os filhos de seus elementos, até o final da árvore;
- pai (_parent_): elemento diretamente acima na árvore;
- ancestrais (_ancestors_): elementos na mesma cadeia de pais;
- irmãos (_siblings_) elementos com o mesmo pai.

Todos os nós | Somente elementos | Descrição
--- | --- | ---
`childNodes` | `children` | coleção viva, somente-para-leitura, dos filhos (_use `for..of` para iterar_)
`childNodes[n]` | `children[n]` |  _n_-ésimo filho
`firstChild` | `firstElementChild` | primeiro filho
`lastChild` | `lastElementChild` | último filho
`parentNode` | `parentElement` | o pai (`document.documentElement.parentNode === document` mas `document.documentElement.parentElement === null`)
`nextSibling` | `previousElementSibling` | o próximo irmão
`previousSibling` | `nextElementSibling` | o irmão anterior

Use:

- `hasChildNodes()` para saber se o elemento possui filhos;
- `length` para obter o número de filhos.

Há alguns auxiliares específicos para tabelas:

- `<table>` possui `rows`, `caption`, `tHead`, `tFoot` e `tBodies`;
- `<thead>`, `tfoot` e `tbody` possuem `rows`;
- `tr` possui `cells` (`<td>`'s e `<th>`'s), `rowIndex` (em relação à tabela) e `sectionRowIndex` (em relação ao à seção);
- `<td>`'s e `<th>`'s possuem `cellIndex` (em relação ao `<tr>` pai).

## Obtendo elementos usando o atributo `id`

Use `document.getElementById('myId')` para obter o elemento de `id` igual a `myId`, não importa em que parte do documento.

🍌 É criada automaticamente uma variável global para cada elemento com o atributo `id`, com o nome equivalente. Seu uso não é recomendado pois se outra variável for criada com o mesmo nome ela possui precedência.

🍌🍌 Se um `id`não é único, todas as técnicas de obtenção por `id` se tornam imprevisíveis.

## Obtendo elementos usando seletores CSS

- `elem.querySelectorAll(cssSelector)` obtém uma coleção **estática** dos elementos dentro de `elem` que atendem `cssSelector`;
- `elem.querySelector(cssSelector)` obtém o primeiro elemento dentro de `elem` que atende `cssSelector`;
- `elem.matches(cssSelector)` verifica se algum elemento em `elem` atende `cssSelector`;
- `elem.contains(outroElem)` verifica se `outroElem` é descendente de `elem`;
- `elem.closest(cssSelector)` obtém o ancestral mais próximo de `elem` que atende `cssSelector` (incluíndo possivelmente `elem`).

## Outros métodos para obtenção de elementos

Podem ser chamados no singular (`getElement_`) para obter o primeiro elemento, ou no plural (`getElements_`) para obter uma coleção **viva** com todos os elementos.

- `elem.getElementsByTagName(tag)` obtém elementos pela _tag_ (`"*"` para qualquer _tag_);
- `elem.getElementsByClassName(class)` obtém elementos que contenham a classe indicada;
- `document.getElementsByName(name)` obtém elementos (em todo o documento) pelo atributo `name`.

## Nós

Tudo no DOM são nós. Eles seguem a seguinte hierarquia de classes:

* `Object`: classe base do JavaScript.
  * `EventTarget`: classe base abstrata, para suporte a eventos.
    * `Node`: classe base abstrata com os _getters_ de navegação.
      * `Text`: nós de texto.
      * `Comment`: nós de comentário.
      * `Element`: elementos do DOM, com implementações da navegação.
        * `SVGElement`: _tags_ SVG.
        * `XMLElement`: _tags_ XML.
        * `HTMLElement`: _tags_ HTML.
          * `HTMLBodyElement`: _tag_ `body`.
          * `HTMLInputElement`: _tag_ `input`.
          * `HTMLAnchorElement`: _tag_ `a`.
          * ...
      * ...

## Conteúdo de elementos

Podemos obter e alterar o conteúdo de um elemento usando `innerHTML` (conteúdo e de seus descendentes) e `outerHTML` (incluindo também a si próprio).

* Os valores retornados e gravados são strings.
* Em uma gravação, o antigo elemento é removido e o novo gravado (recarregando imagens, perdendo seleção em campos de texto, etc.).
* O navegador processa a autocorreção;
* Os `script`'s não são executados.

💡 Para nós de texto ou comentários, use `data`.

Para obter somente o texo, sem as _tags_, use `textContent`. Gravar usando essa propriedade garante que nenhuma _tag_ será injetada.

Para obter o valor alterável pelo usuário em elementos de formulário, use `value`.

## Visibilidade

O atributo `hidden` possui a mesma especificação de `style=display:none`.

Ex:

```html
<div id="elem">Elemento piscante</div>
<script>
  setInterval(() => elem.hidden = !elem.hidden, 1000);
</script>
```

## Obtendo atributos não-padronizados

Não são criadas propriedades automaticamente, mas podem ser acessadas usando `hasAttribute(attr)`, `getAttribute(attr)`, `setAttribute(attr, valor)`, `removeAttribute(attr)` e iterados usando a coleção `attributes`.

Atributos `data-*` são preferíveis, e estarão disponíveis em `dataset`.

Ex.:

```html
<body data-especie-relacionada="Elefante">
<script>
  alert(document.body.dataset.especieRelacionada); // Elefante
</script>
```

## Criando elementos

Criação:

* `document.createElement(tag)` cria um elemento do tipo adequado para a tag informada;
* `document.createTextNode(texto)` cria um elemento de texto.

Ex.:

```js
let boxAlerta = document.createElement('div');
boxAlerta.className = "alerta";
boxAlerta.innerHTML = "<strong>Atenção!</strong> Fique esperto!";
```

Gera em `boxAlerta`:

```html
<div class="alerta"><strong>Atenção!</strong> Fique esperto!</div>
```

## Inserindo elementos

Para inserir elementos, escolha um nó base e chame o método adequado passando outro elemento, uma lista de elementos (cada um em um argumento) ou uma (ou mais) string(s).

* `no.append`: como último elemento dentro do nó.
* `no.prepend`: como primeiro elemento do nó;
* `no.before`: como o último elemento anterior ao nó;
* `no.after`: como primeiro elemento após o nó;
* `no.replaceWith`: substitui o nó, na mesma posição.

Strings serão inseridas de forma segura, como em `textContent`. Para inserir como tags a serem processadas, use:

* `no.insertAdjacentHTML("beforebegin", string)`: na mesma posição de `before`;
* `no.insertAdjacentHTML("afterbegin", string)`: na mesma posição de `prepend`;
* `no.insertAdjacentHTML("beforeend", string)`: na mesma posição de `append`;
* `no.insertAdjacentHTML("afterend", string)`: na mesma posição de `after`.

Também podem ser usados `insertAdjacentText` e `insertAdjacentElement`.

## Removendo um elemento

Use `elem.remove()`.

## Clonando elementos

Para criar cópias idênticas de um elemento:

* `elem.cloneNode(false)`: clonagem rasa.
* `elem.cloneNode(true)`: clonagem profunda.

## Estilos com `style`

Usando `style`:

* `elem.style.top` obtém/altera o conteúdo do estilo `top` (use as unidades na string, como em `'20px'`).
* `elem.style.cssText` obtém/altera **todo** o estilo do objeto, na sintaxe CSS (e aceita `!important`).

Todas as propriedades são nomeadas usando _camelCase_. Onde houver um `-`, o próximo caracter é maiúsculo.

💡 Gravar `""` faz o navegador resetar o elemento.

Só deve ser utilizado quando envolver cálculos. Em outra situações, usar `class`.

## Estilos com `class`

* O atributo `class` está disponível em `elem.className`, como uma string.
* Para manipular a lista de classes, use `classList` (iterar com `for...of`):
  * `add("classe")` adiciona `"classe"`;
  * `remove("classe")` remove `"classe"`;
  * `toggle("classe")` adiciona/remove `"classe"`, conforme o caso;
  * `contains("classe")` verifica se `"classe"` existe em `classList`.

## Estilos computados/resolvidos

O valor de `elem.style` reflete o estilo atribuído ao elemento.

Para obter o valor correto considerando toda a cascata de estilos, já resolvido na unidade padrão para os navegadores,use `getComputedStyle(elemento, [pseudo])`.

🍌 Não use propriedades de atalho, como `margin` ou `padding`, e sim `marginTop` ou `paddingLeft`. Não há padronização entre navegadores.

🍌🍌 O JavaScript não tem acesso à pseudoclasse `:visited`, por privacidade.

## Geometria de elementos

Veja esse [exemplo](https://javascript.info/size-and-scroll#sample-element):

```html
<div id="example">
  ...Text...
</div>
<style>
  #example {
    width: 300px;
    height: 200px;
    border: 25px solid #E8C48F;
    padding: 20px;
    overflow: auto;
  }
</style>
```

Desconsiderando a margem (que não faz parte do elemento), a geometria do elemento é:

![Geometria do elemento](https://javascript.info/article/size-and-scroll/metric-css.svg)

* A medida da barra de rolagem conta! Ela é descontada de `width` para obter a medida disponível para o conteúdo.
* O conteúdo pode vazar para o `padding-bottom`, em caso de `overflow`.

As propriedades abaixo indicam como obter essas medidas, em `px`.

![Propriedades de medição](https://javascript.info/article/size-and-scroll/metric-all.svg)

Em relação ao pai:

![Propriedades de medição em relação ao pai](https://javascript.info/article/size-and-scroll/metric-offset-parent.svg)

* `offsetParent`, `offsetLeft` e `offsetTop` são calculados em relação ao pai (de acordo com `position`, ou `td`/`th`, ou `table`, ou `body`);
* `offsetParent` é `null` se:
  * o elemento está `hidden`;
  * em elementos com `position: fixed`;
  * nos elementos `body` e `html`.

Tamanho completo do elemento:

![Propriedades de medição](https://javascript.info/article/size-and-scroll/metric-offset-width-height.svg)

* Todas as propriedades de geometria são nulas para elementos `hidden`.

Medidas internas ao elemento:

![Medidas internas](https://javascript.info/article/size-and-scroll/metric-client-left-top.svg)

* Na prática, a medida da borda.
* `clientLeft` pode conter o tamanho da barra de rolagem, para textos RTL:

![Medidas internas - RTL](https://javascript.info/article/size-and-scroll/metric-client-left-top-rtl.svg)

Tamanho interno:

![Tamanho interno](https://javascript.info/article/size-and-scroll/metric-client-width-height.svg)

* Não inclui barras de rolagem e bordas, mas inclui `padding`:

![Tamanho interno, sem padding](https://javascript.info/article/size-and-scroll/metric-client-width-nopadding.svg)

Tamanho da área de atuação da rolagem:

![Área de rolagem](https://javascript.info/article/size-and-scroll/metric-scroll-width-height.svg)

![Área já rolada](https://javascript.info/article/size-and-scroll/metric-scroll-top.svg)

* `scrollLeft` e `scrollTop` podem ser ajustados para rolar o conteúdo.

🍌 Usar `getComputedStyle` para obter medidas não é recomendado, pois `box-sizing`, medidas `auto` e diferenças de implementação de barras de rolagem quebram o JavaScript.

---

_Disclaimer_: este conteúdo é um resumo pessoal do assunto com muitos trechos adaptados de [The Modern JavaScript Tutorial](https://javascript.info/) (CC-BY-NC-SA), do qual respeita os [termos de uso](https://javascript.info/terms).