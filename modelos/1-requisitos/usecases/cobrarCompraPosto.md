# Use Case: Cobrar compra de telemóvel no posto

- **Descrição:** O cliente que registou a compra com o telemóvel dirige-se a um posto, onde a compra é recuperada, eventualmente validada, e paga.
- **Cenários:** Quando o cliente chega ao posto, este vai buscar a compra e o que faz é cobrar (Sandra). Só nestas compras há validação, porque nada nem ninguém viu o que foi para o saco (Hélder). Os postos de self-service e os de pagamento de compras por telemóvel são as mesmas máquinas, em modos diferentes (Alberto).
- **Pré-condição:** O cliente tem uma compra terminada, registada com telemóvel, e está num posto a funcionar em modo de pagamento de compras por telemóvel.
- **Pós-condição:** A compra está paga e concluída.

**Fluxo normal:**
1. Ator (Cliente) indica o posto no qual pretende pagar a sua compra.
2. Sistema obtém a compra terminada do cliente.
3. Sistema executa o use case Aplicar campanhas.
4. Sistema determina, segundo os critérios de validação em vigor, que a compra não requer validação.
5. Sistema executa o use case Efetuar pagamento.

**Fluxo alternativo (1a):** [Compra sujeita a validação] (passo 3)
1a.1. Sistema determina que a compra requer validação.
1a.2. Sistema executa o use case Validar compra.
1a.3. Regressa ao passo 4.

**Fluxo de exceção (1e):** [Validação com diferença inexplicável] (passo 1a.2)
1e.1. A compra deixa de valer para pagamento, nos termos do fluxo de exceção de Validar compra.
1e.2. O caso de uso termina sem pagamento. O cliente tem de registar a compra de novo numa caixa assistida (use case Registar compra em caixa assistida).

**Fluxo de exceção (2e):** [Pagamento não se conclui] (passo 4)
2e.1. A compra fica anulada, nos termos do fluxo de exceção de Efetuar pagamento.