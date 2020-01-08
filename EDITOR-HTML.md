# Editor de HTML

Criar um tema customizado na minestore é possível e muito fácil.
Você só precisa ter conhecimentos básicos de HTML, CSS, Javascript e um pouco de lógica de programação.

Palavras-chave para as seções especiais. As mesmas só podem ser utilizadas quando editando sua própria seção.

Lista de seções
- [Lista de Produtos](#lista-de-produtos)
- [Produto](#produto)
- [Carrinho de compras](#carrinho-de-compras)
- [Página não encontrada](#pagina-nao-encontrada)
- [Avisos](#avisos)
- [Imagens](#imagens)


## [Lista de Produtos](#lista-de-produtos)

Página que exibe uma lista dos produtos de uma categoria específica, de uma busca, ou de todos os produtos da loja.

- `{{#list_page}}` Tag de abertura da página de listagem de produtos.
- `{{#each products}}` Iterar sobre os produtos da lista.
- `{{#with primary_image}}` Imagem principal do produto.
- `{{#each images}}` Iterar sobre todas as imagens do produto.
- `{{this}}` URL da imagem aberta.
- `{{title}}` Título do produto.
- `{{price_with_currency}}` Preço do produto.
- `{{original_price_with_currency}}` Preço promocional do produto, quando há alguma promoção ativa.
- `{{#if in_stock}}` Verifica se o produto tem estoque. Aceita {{else}} para indicar que o produto está fora de estoque.
- `{{paginate}}` Paginação da lista.
- `{{#search_page}}` Se a lista for resultado de uma busca.
- `{{search_term}}` Termo usado na busca.

Estrutura exemplo:

```handlebars
{{#list_page}}
  {{#search_page}}<!-- se estiver numa página de resultado de busca -->
    {{search_term}}<!-- exibe o termo que foi pesquisado -->
  {{/search_page}}
  {{#each products}} <!-- faz um loop por todos produtos -->
    <a class="product {{#if out_of_stock}}out_of_stock{{/if}}" href="{{url}}"><!-- verifica se o produto está fora de estoque, se tiver, adiciona a classe 'out_of_stock' -->
      <div class="image">
        {{#with primary_image}} <!-- retorna a imagem principal do produto -->
          <img alt="{{title}}" data-at2x='{{resize_image this "940x940#"}}' src='{{resize_image this "470x470#"}}'>
        {{/with}}
      </div>
      <div class="info">
        {{title}} <!-- retorna o título do produto-->
        {{#if in_stock}} <!-- verifica se o produto tem estoque -->
          {{original_price_with_currency}} <!-- exibe preço original fora da promoção -->
          {{price_with_currency}} <!-- exibe o preço promocianal -->
        {{/if}}
        {{#if out_of_stock}} Fora de estoque {{/if}}
      </div>
    </a>
  {{/each}}
  {{{paginate}}} <!-- retorna paginação dos produtos -->
{{/list_page}}
```

## [Produto](#produto)

Página que exibe os detalhes de um produto.

- `{{#product_page}}` Abertura da tag da página do produto.
- `{{#with product}}` Abertura da tag do produto.
- `{{url}}` URL da página do produto.
- `{{#with primary_image}}` Imagem principal do produto.
- `{{#each images}}` Iterar sobre todas as imagens do produto.
- `{{this}}` URL da imagem aberta.
- `{{#add_to_cart}}` Abertura da tag com as informações do produto.
- `{{variations_select}}` Seletor de variações do produto.
- `{{price_with_currency}}` Preço do produto.
- `{{original_price_with_currency}}` Preço promocional do produto, quando há alguma promoção ativa.
- `{{#if in_stock}}` Verifica se o produto tem estoque. Aceita `{{else}}` para indicar que o produto está fora de estoque.
- `{{#add_to_cart_button}}` Botão para adicionar o produto no carrinho. É exibido apenas se o produto estiver em estoque. Conteúdo do bloco define o texto do botão.
- `{{description}}` Descrição do produto.

Estrutura exemplo:

```handlebars
{{#product_page}}
  {{#with product}}
    {{url}}
    {{#with primary_image}}
      <img alt="{{title}}" src="{{this}}">
    {{/with}}
    {{#each images}}
      <img alt="{{title}}" src="{{this}}">
    {{/each}}
    {{title}}
    {{#add_to_cart}}
      {{variations_select}}
      <div>
        <span>De: {{original_price_with_currency}}</span>
        <span>Por: {{price_with_currency}}</span>
        {{#if in_stock}} Produto em estoque {{else}} Produto fora de estoque {{/if}}
      </div>
      {{#add_to_cart_button}} Adicionar ao Carrinho {{/add_to_cart_button}}
      {{{description}}}
    {{/add_to_cart}}
  {{/with}}
{{/product_page}}
```

## [Carrinho de compras](#carrinho-de-compras)

Página que exibe o carrinho e o botão de fechamento de pedido.

- `{{#cart_page}}` Abertura da tag da página do carrinho.
- `{{information}}` Exibe informações sobre o carrinho.
- `{{#with cart}}` Exibe o carrinho.
- `{{#cart_form_tag}}` Abertura da tag do carrinho.
- `{{#each line_items}}` Iterar sobre os produtos do carrinho.
- `{{title}}` Título do produto.
- `{{#remove_line_item_link}}` Link para remover item do carrinho. Conteúdo do bloco define o texto do link.
- `{{#link_to_product}}` URL do produto.
- `{{unit_price}}` Preço da unidade do produto. Se o produto estiver com preço promocional, a tag para exibir pro preço original é `{{ unit_price_without_discount }}`
- `{{quantity_input_field}}` Quantidade de unidades do produto.
- `{{total_price}}` Valor total da quantidade do produto indicado quando dentro do bloco `{{#each line_items}}` ou valor total do carrinho quando dentro do bloco `{{#cart_form_tag}}`.
- `{{#submit_cart_form_tag}}` Botão para fechar pedido. Conteúdo do bloco define o texto do botão.
- `{{ shipping_fields }}` Renderiza o formulário para cálculo de frete no carrinho.
- `{{#if is_shipping_calculated}}` Verifica se o frete já foi calculado no carrinho.
- `{{ shipping_price }}` Preço do frete calculado.
- `{{#if has_discount}}` Verifica se tem algum desconto por promoção ou promocode.
- `{{ subtotal_price }}` Retorna o preço total do carrinho sem desconto.
- `{{ total_discount_price }}` Retorna o valor total de descontos no carrinho.

Estrutura exemplo:

```handlebars
{{#cart_page}}
  {{information}}
  {{#with cart}}
    {{#cart_form_tag}}
      {{#each line_items}}
        {{title}}
        {{#remove_line_item_link}} Remover item {{/remove_line_item_link}}
        {{#link_to_product}} {{/link_to_product}}
        de: {{ unit_price_without_discount }} por: {{unit_price}} {{quantity_input_field}} {{total_price}}
      {{/each}}
      {{ shipping_fields }}
      {{#if is_shipping_calculated}}
        {{ shipping_price }}
      {{/if}}
      {{#if has_discount}}
        {{ subtotal_price }}
        {{ total_discount_price }}
      {{/if}}
      {{total_price}}
      {{#submit_cart_form_tag}}Finalizar{{/submit_cart_form_tag}}
    {{/cart_form_tag}}
  {{/with}}
{{/cart_page}}
```

## [Página não encontrada](#pagina-nao-encontrada)

Conteúdo de página não encontrada.

- `{{#not_found_page}}` Define o conteúdo exibido quando o usuário tenta acessar uma página que não existe.

Estrutura exemplo:

```handlebars
{{#not_found_page}}
  <div class="alert"> Página não encontrada </div>
{{/not_found_page}}
```

Tags usadas para compor o menu, como links para as páginas da loja e campo de busca.

- `{{#each categories}}` Itera sobre as categorias.
- `{{#each children}}` Itera sobre as subcategorias.
- `{{#each pages}}` Itera sobre as páginas customizadas.
- `{{url}}` URL da categoria, subcategoria ou página.
- `{{label}}` Nome da categoria ou subcategoria.
- `{{title}}` Título da página.
- `{#if is_current}}` Renderiza o conteúdo do bloco se a página da categoria ou subcategoria estiver sendo exibida.
- `{{products_url}}` URL da página com todos os produtos da loja.
- `{{cart_url}}` URL do carrinho.
- `{{#if cart_has_items}}` Renderiza o conteúdo do bloco se o carrinho possuir items.
- `{{cart_count}}` Número de ítens no carrinho.
- `{{search_form}}` Exibe formulário de busca de produtos.

```handlebars
<ul class="nav">
  <li class="{{#if all_products_page}}selected{{/if}}">
    <a href="{{products_url}}">produtos</a>
  </li>
  <li style="list-style: none">{{#each pages}}</li>
  <li class="{{#if is_current}}selected{{/if}}">
    <a href="{{url}}">{{title}}</a>
  </li>
  <li style="list-style: none">{{/each}}</li>
  <li class="cart {{#is_cart_page}}selected{{/is_cart_page}}">
    <a href="{{cart_url}}">carrinho
      {{#if cart_has_items}}
        ({{cart_count}})
      {{/if}}
    </a>
  </li>
  <li>{{{search_form}}} <i class="fa fa-search" id="search-button"></li>
  </li>
</ul>
```

## [Avisos](#avisos)

Controles de exibição de imagens no editor de temas da minestore.

- `{{minestore_messages}}` Tag necessária para informar ações do usuários e quaisquer problemas ocorridos.
- `{{information}}` Tag que deve ser incluída dentro da `{{#cart_page}}` para informações sobre o carrinho.

## [Imagens](#imagens)

Informações para o usuário e o administrador da loja.

- `{{resize_image imagem "larguraxaltura"}}` Você pode redimensionar qualquer imagem usando a tag `resize_image`. A imagem pode ser uma URL externa com aspas, um indicador de uma tag pai aberta com `this` ou uma imagem dinâmica inserida pelo editor de temas com a tag `theme.nome_da_tag`. O segundo parâmetro larguraxaltura define o tamanho da imagem e como ela é cortada.

Exemplos de usos da ferramenta de redimensionamento

- '400x300' Redimensionar mantendo a proporção.
- '400x300!' Forçar redimensionamento na proporção indicada.
- '400x' Redimensionar largura, mantendo a proporção.
- 'x300' Redimensionar altura, mantendo a proporção.
- '400x300<' Redimensionar apenas se a imagem for menor do que o indicado.
- '400x300>' Redimensionar apenas se a imagem for maior do que o indicado.
- '50x50%' Redimensionar largura e altura para 50%.
- '400x300#' Redimensionar, cortar se necessário para manter a proporção (gravidade central).
- '400x300#ne' Redimensionar, cortar se necessário para manter a proporção (gravidade nordeste).
- '400x300+50+100' Cortar 400×300 do ponto 50 de largura e 100 de altura.

Aplicação das tags

```handlebars
<!-- Renderiza a logo do Google para que caiba dentro de uma área de 400x300px. -->
{{resize_image "https://www.google.com/images/srpr/logo11w.png" "400x300"}}
<!-- Renderiza a logo do tema para que tenha 400px de largura. -->
{{resize_image theme.logo "400x"}}
<!-- Renderiza a imagem principal de um produto para que tenha 300px de altura. -->
{{#with primary_image}}
  <img src="{{resize_image this "x300"}}">
{{/with}}
```
