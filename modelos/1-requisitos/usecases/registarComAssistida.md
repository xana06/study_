# Use Case: Registar compra em caixa assistida

- **Descrição:** O operador de caixa regista os artigos de um cliente num posto assistido para preparar a compra para pagamento.
- **Cenários:** O cliente põe as compras no tapete, uma pessoa da loja passa os artigos e disponibiliza a compra para liquidação (Sandra). Os artigos de peso pesam-se na própria caixa, e a remoção de um artigo acima de um certo valor exige a autorização do chefe de caixas (Alberto).
- **Pré-condição:** Há um cliente a ser atendido num posto assistido.
- **Pós-condição:** A compra está registada e a aguardar pagamento.

**Fluxo normal:**
1. Ator (Operador de Caixa) regista um artigo do cliente.
2. Sistema identifica o artigo e adiciona-o à compra, com o preço em vigor nesse momento e o instante de registo.
3. Os passos 1 e 2 repetem-se para cada artigo do cliente.
4. Ator (Operador de Caixa) indica que não há mais artigos a registar.
5. Sistema aplica as campanhas à compra, apresenta o total a pagar e aguarda a realização do pagamento.

**Fluxo alternativo (1):** [Artigo de peso] (passo 1)
1a.1. Ator (Operador de Caixa) coloca o artigo na balança da caixa.
1a.2. Sistema lê o peso, calcula o valor ao preço em vigor nesse momento e adiciona o artigo à compra, com o instante de registo.
1a.3. Regressa ao passo 3.

**Fluxo alternativo (2):** [Cliente desiste de um artigo de valor não superior ao limite] (passo 3)
2a.1. Ator (Operador de Caixa) seleciona o artigo a remover.
2a.2. Sistema remove o artigo e recalcula a compra.
2a.3. Regressa ao passo 3.

**Fluxo alternativo (3):** [Cliente desiste de um artigo de valor superior ao limite] (passo 3)
3a.1. Ator (Operador de Caixa) seleciona o artigo a remover.
3a.2. Sistema executa o use case Autorizar remoção de artigo.
3a.3. Sistema remove o artigo e recalcula a compra.
3a.4. Regressa ao passo 3.

**Fluxo alternativo (4):** [Chefe de caixas recusa a remoção] (passo 3a.2)
4a.1. Sistema mantém o artigo na compra.
4a.2. Regressa ao passo 3.