# Use Case: Autorizar remoção de artigo

- **Descrição:** O chefe de caixas autoriza, com o seu cartão, a remoção de um artigo da compra quando o valor ultrapassa o limite.
- **Cenários:** O cliente desiste de um artigo. Acima de um certo valor o chefe tem de ir lá com o cartão autorizar, porque não é o operador que decide, e no fim do mês há um relatório disso (Alberto).
- **Pré-condição:** Durante o registo de uma compra foi pedida a remoção de um artigo cujo valor ultrapassa o limite.
- **Pós-condição:** O chefe de caixas decidiu sobre a remoção e, se a autorizou, a autorização ficou registada.

**Fluxo normal:**
1. Sistema suspende a remoção e pede a autorização do chefe de caixas.
2. Ator (Chefe de Caixas) apresenta o seu cartão.
3. Sistema valida o cartão do chefe de caixas.
4. Ator (Chefe de Caixas) confirma a remoção.
5. Sistema regista a autorização (chefe de caixas, artigo, valor e momento) e autoriza a remoção do artigo.

**Fluxo alternativo (1):** [Chefe de caixas recusa a remoção] (passo 4)
1a.1. Ator (Chefe de Caixas) recusa a remoção.
1a.2. Sistema mantém o artigo na compra.

**Fluxo de exceção (1):** [Cartão sem permissão de chefe de caixas] (passo 3)
1e.1. Sistema rejeita o cartão e mantém o pedido de autorização.
1e.2. Regressa ao passo 2.