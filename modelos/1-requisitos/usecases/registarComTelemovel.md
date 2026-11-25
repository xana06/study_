# Use Case: Registar compra com telemóvel

- **Descrição:** O cliente regista os artigos com o telemóvel, à medida que os tira da prateleira, e dá a compra por terminada para a pagar mais tarde num posto.
- **Cenários:** O cliente começa quando entra, percorre a loja, lê os artigos e mete-os diretamente no saco; pode demorar cinco minutos ou uma hora (Sandra). Os artigos de peso exigem pesar na secção e ler a etiqueta, senão não se registam (Alberto).
- **Pré-condição:** O cliente está identificado no sistema.
- **Pós-condição:** A compra está registada e terminada pelo cliente, a aguardar cobrança num posto.

**Fluxo normal:**
1. Ator (Cliente) inicia a compra ao entrar na loja.
2. Sistema abre uma compra associada ao cliente.
3. Ator (Cliente) lê um artigo que tira da prateleira.
4. Sistema adiciona o artigo à compra, com o preço em vigor nesse momento e o instante de registo, e atualiza o total provisório.
5. Os passos 3 e 4 repetem-se para cada artigo que o cliente coloca no saco.
6. Ator (Cliente) indica que terminou a compra.
7. Sistema guarda a compra como terminada, a aguardar cobrança num posto.

**Fluxo alternativo (1):** [Artigo de peso] (passo 3)
1a.1. Ator (Cliente) pesa o artigo na balança da secção, que imprime uma etiqueta com o código e o preço já calculado.
1a.2. Ator (Cliente) lê a etiqueta.
1a.3. Sistema adiciona o artigo à compra com o preço da etiqueta, que é o do momento da pesagem, e com o instante de registo.
1a.4. Regressa ao passo 5.

**Fluxo alternativo (2):** [Cliente desiste de um artigo] (passo 5)
2a.1. Ator (Cliente) remove o artigo da compra.
2a.2. Sistema remove o artigo e recalcula o total provisório.
2a.3. Regressa ao passo 5.

**Fluxo alternativo (3):** [Artigo de peso lido sem etiqueta] (passo 3)
3a.1. Ator (Cliente) lê um artigo de peso sem ter lido a respetiva etiqueta.
3a.2. Sistema não regista o artigo.
3a.3. Regressa ao passo 5.

