# Use Case: Chamar próximo cliente

- **Descrição:** O operador de caixa indica que está livre e o sistema encaminha para o seu posto o cliente seguinte da fila.
- **Cenários:** Nas lojas maiores há uma fila única e um painel indica ao cliente para que caixa deve seguir. Quando um operador fica livre, carrega num botão e o painel manda a pessoa seguinte para aquele número. Quem decide que está livre é o operador, não o sistema (Sandra).
- **Pré-condição:** A loja tem fila única e há pelo menos um cliente na fila.
- **Pós-condição:** O cliente seguinte da fila foi encaminhado para o posto do operador e já não está na fila.

**Fluxo normal:**
1. Ator (Operador de Caixa) indica que está livre.
2. Sistema retira da fila o cliente seguinte.
3. Sistema encaminha o cliente para o posto do operador.