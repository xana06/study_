# Use Case: Registar compra em self-service

- **Descrição:** O cliente regista os seus artigos num posto de self-service para preparar a compra para pagamento.
- **Cenários:** O cliente faz tudo sozinho: passa os artigos ele próprio e finaliza o registo ali; a fila é para arranjar posto (Sandra). Os artigos de peso também se podem registar no self-service, "com mais paciência" (Alberto).
- **Pré-condição:** O posto está livre e a funcionar em modo self-service.
- **Pós-condição:** A compra está registada e a aguardar pagamento.

**Fluxo normal:**
1. Ator (Cliente) regista um artigo.
2. Sistema identifica o artigo e adiciona-o à compra, com o preço em vigor nesse momento e o instante de registo.
3. Os passos 1 e 2 repetem-se para cada artigo do cliente.
4. Ator (Cliente) indica que não há mais artigos a registar.
5. Sistema aplica as campanhas à compra, apresenta o total a pagar e aguarda a realização do pagamento.

**Fluxo alternativo (1):** [Artigo de peso] (passo 1)
1a.1. Ator (Cliente) coloca o artigo na balança do posto.
1a.2. Sistema lê o peso, calcula o valor ao preço em vigor nesse momento e adiciona o artigo à compra, com o instante de registo.
1a.3. Regressa ao passo 3.

**Fluxo alternativo (2):** [Cliente desiste de um artigo de valor não superior ao limite] (passo 3)
2a.1. Ator (Cliente) seleciona o artigo a remover.
2a.2. Sistema remove o artigo e recalcula a compra.
2a.3. Regressa ao passo 3.

**Fluxo alternativo (3):** [Cliente desiste de um artigo de valor superior ao limite] (passo 3)
3a.1. Ator (Cliente) seleciona o artigo a remover.
3a.2. Sistema executa o use case Autorizar remoção de artigo.
3a.3. Sistema remove o artigo e recalcula a compra.
3a.4. Regressa ao passo 3.

**Fluxo alternativo (4):** [Chefe de caixas recusa a remoção] (passo 3a.2)
4a.1. Sistema mantém o artigo na compra.
4a.2. Regressa ao passo 3.