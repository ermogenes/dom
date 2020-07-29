# Ambiente de execução JavaScript

Códigos JavaScript são executados em um ambiente como um navegador (Chorme, Firefox), um servidor (Node, Deno) ou qualquer outro tipo de plataforma.

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

💡 Veja/navegue pela estrutura de um objeto qualquer usando `console.dir` e `console.log`.

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

## Prinipais tipos de nós

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

Use:
- `document.getElementById('myId')` para obter o elemento de `id` igual a `myId`, não importa em que parte do documento.

🍌 É criada automaticamente uma variável global para cada elemento com o atributo `id`, com o nome equivalente. Seu uso não é recomendado pois se outra variável for criada com o mesmo nome ela possui precedência.

🍌🍌 Se um `id`não é único, todas as técnicas de obtenção por `id` se tornam imprevisíveis.

## Obtendo elementos usando seletores CSS

- `elem.querySelectorAll(cssSelector)` obtém uma coleção **estática** dos elementos dentro de `elem` que atendem `cssSelector`;
- `elem.querySelector(cssSelector)` obtém o primeiro elemento dentro de `elem` que atende `cssSelector`;
- `elem.matches(cssSelector)` verifica se algum elemento em `elem` atende `cssSelector`;
- `elem.contains(outroElem)` verifica se `outroElem` é descendente de `elem`;
- `elem.closest(cssSelector)` obtém o ancestral mais próximo de `elem` que atende `cssSelector` (incluíndo possivelmente `elem`).

## Outros métodos

Podem ser chamados no singular (`getElement_`) para obter o primeiro elemento, ou no plural (`getElements_`) para obter uma coleção **viva** com todos os elementos.

- `elem.getElementsByTagName(tag)` obtém elementos pela _tag_ (`"*"` para qualquer _tag_);
- `elem.getElementsByClassName(class)` obtém elementos que contenham a class indicada;
- `document.getElementsByName(name)` obtém elementos (em todo o documento) pelo atributo `name`.