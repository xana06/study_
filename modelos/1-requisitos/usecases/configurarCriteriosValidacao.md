# Use Case: Configurar critérios de validação

- **Descrição:** O responsável de prevenção de perdas define os critérios que determinam se uma compra registada com telemóvel é sujeita a validação.
- **Cenários:** Hoje a escolha é aleatória, uma percentagem fixa das compras. Hélder quer que pese o histórico do cartão de cliente, e poder forçar a validação acima de um valor ou quando o cabaz tem artigos problemáticos. Sandra quer que a validação seja rara, porque cada uma custa três minutos parados com o cliente à espera.
- **Pré-condição:** Existem critérios de validação em vigor (inicialmente, uma percentagem fixa de compras).
- **Pós-condição:** Os novos critérios estão registados e passam a ser usados em Cobrar compra de telemóvel no posto.

**Fluxo normal:**
1. Ator (Responsável de Prevenção de Perdas) indica que pretende alterar os critérios de validação.
2. Ator (Responsável de Prevenção de Perdas) define a percentagem de compras sujeitas a validação.
3. Sistema regista os critérios e passa a aplicá-los.

**Fluxo alternativo (1):** [Peso do histórico do cartão de cliente] (passo 2)
1a.1. Ator (Responsável de Prevenção de Perdas) define como as validações sem ocorrência e as ocorrências do histórico do cartão de cliente aumentam ou reduzem a probabilidade de validação.
1a.2. Regressa ao passo 3.

**Fluxo alternativo (2):** [Validação forçada por valor] (passo 2)
2a.1. Ator (Responsável de Prevenção de Perdas) define o valor da compra acima do qual a validação é sempre requerida.
2a.2. Regressa ao passo 3.

**Fluxo alternativo (3):** [Validação forçada por artigos] (passo 2)
3a.1. Ator (Responsável de Prevenção de Perdas) indica os artigos que, estando na compra, forçam a validação.
3a.2. Regressa ao passo 3.