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

