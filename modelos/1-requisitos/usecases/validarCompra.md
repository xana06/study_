# Use Case: Validar compra

- **Descrição:** Um funcionário confirma os artigos físicos de uma compra registada com telemóvel face ao que está registado.
- **Cenários:** Quando a compra chega ao posto e o sistema decide validá-la, aparece uma mensagem, o pagamento fica suspenso e vem um funcionário confirmar os artigos (Hélder). O resultado pode ser tudo conforme, uma diferença pequena de boa-fé ou uma diferença que obriga a reler a compra.
- **Pré-condição:** Está em curso a cobrança de uma compra registada com telemóvel e o sistema determinou que ela requer validação.
- **Pós-condição:** A compra está confirmada, o resultado ficou registado no histórico do cartão de cliente e a compra está disponível para pagamento.

**Fluxo normal:**
1. Sistema suspende o pagamento e chama um funcionário.
2. Ator (Funcionário de Verificação) confirma os artigos físicos contra os artigos registados.
3. Ator (Funcionário de Verificação) indica que está tudo conforme.
4. Sistema regista a validação como "sem ocorrência" no histórico do cartão de cliente.
5. Sistema desbloqueia a compra para pagamento.

**Fluxo alternativo (1):** [Diferença pequena, de boa-fé] (passo 2)
1a.1. Ator (Funcionário de Verificação) deteta um artigo que não foi lido e acrescenta-o à compra.
1a.2. Sistema adiciona o artigo, com o preço em vigor nesse momento e o instante de registo, e recalcula o total.
1a.3. Sistema regista a validação como "artigo acrescentado" no histórico do cartão de cliente.
1a.4. Regressa ao passo 5.

**Fluxo de exceção (1):** [Diferença inexplicável] (passo 2)
1e.1. Ator (Funcionário de Verificação) indica que a compra não é válida.
1e.2. Sistema regista a validação como "releitura" no histórico do cartão de cliente.
1e.3. Sistema mantém os artigos registados como prova, mas deixa de os considerar para pagamento.
1e.4. Sistema repõe como disponíveis os cupões associados à compra.
1e.5. O cliente tem de registar a compra de novo numa caixa assistida (use case Registar compra em caixa assistida).