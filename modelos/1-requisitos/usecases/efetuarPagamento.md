# Use Case: Efetuar pagamento

- **Descrição:** O cliente paga uma compra com um ou mais instrumentos de pagamento.
- **Cenários:** Cartão e dinheiro são o óbvio; o cartão de refeição só paga produtos alimentares; o vale de devolução perde o que sobrar (Sandra). Uma compra pode ser paga por mais do que um instrumento. O preço que vale é o do momento em que o artigo foi registado, também para efeitos de campanha (Marisa). Uma compra anulada por um problema no pagamento deixou o cliente sem o cupão, que devia ter voltado a estar disponível (Marisa).
- **Pré-condição:** A compra está registada e aguarda pagamento.
- **Pós-condição:** A compra está paga na totalidade e concluída, o valor a acumular foi creditado no cartão de cliente e os cupões usados ficaram consumidos.

**Fluxo normal:**
1. Sistema apresenta o total a pagar.
2. Ator (Cliente) identifica o cartão de cliente, se tiver.
3. Sistema aplica à compra as campanhas em vigor no instante de registo de cada artigo e apresenta o total a pagar.
4. Ator (Cliente) paga com um instrumento de pagamento (cartão ou dinheiro), pela ordem que a regra de aplicação dos instrumentos em vigor determinar.
5. Sistema regista o valor pago, que cobre a totalidade da compra.
6. Sistema credita no cartão de cliente o valor a acumular apurado pelas campanhas de acumulação, consome os cupões usados e conclui a compra.

**Fluxo alternativo (1):** [Valor pago inferior ao valor em falta] (passo 4)
1a.1. Sistema regista o valor pago e apresenta o valor em falta.
1a.2. Regressa ao passo 3.

**Fluxo alternativo (2):** [Pagamento com cartão de refeição] (passo 3)
2a.1. Ator (Cliente) apresenta o cartão de refeição.
2a.2. Sistema determina o valor dos produtos alimentares da compra que ainda está por pagar e debita o menor entre esse valor e o saldo do cartão.
2a.3. Regressa ao passo 4.

**Fluxo alternativo (3):** [Pagamento com vale de devolução de valor não superior ao valor em falta] (passo 3)
3a.1. Ator (Cliente) apresenta o vale de devolução.
3a.2. Sistema regista como pago o valor integral do vale e invalida-o.
3a.3. Regressa ao passo 4.

**Fluxo alternativo (4):** [Pagamento com vale de devolução de valor superior ao valor em falta] (passo 3)
4a.1. Ator (Cliente) apresenta o vale de devolução.
4a.2. Sistema regista como pago o valor em falta e invalida o vale por completo. O remanescente perde-se.
4a.3. Regressa ao passo 4.

**Fluxo de exceção (1):** [Pagamento não se conclui, por falha ou por cancelamento] (passo 3 ou 4)
1e.1. Sistema regista que o pagamento não se concluiu.
1e.2. Sistema repõe como disponíveis os cupões associados à compra.
1e.3. Sistema anula a compra.