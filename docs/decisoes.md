# Registo de decisões

A transcrição de requisitos contém pontos por decidir e afirmações incompatíveis entre si.
Cada decisão, e cada exclusão do que foi deixado de fora do modelo ou do desenho, é registada
em três a cinco linhas, com o formato abaixo.

Não há uma resposta certa fixada à partida. Avalia-se a decisão ter sido tomada conscientemente,
a justificação ser sólida, o modelo ser coerente com ela e as consequências da escolha.

Formato de cada entrada:

    ### D01 — Título curto
    - Tipo: Decisão | Exclusão
    - Questão: o ponto em aberto (ou, numa exclusão, o que poderia ter sido modelado)
    - Decisão: o que foi decidido (ou, numa exclusão, "fica de fora")
    - Justificação: porquê
    - Depende no modelo: o que no modelo depende desta decisão

Os identificadores (D01, D02, ...) não mudam. Ao rever uma decisão, mantenham o identificador
e indiquem o que mudou e porquê.

---

### D01: Modos de registo e pagamento
- Tipo: Decisão
- Questão: a transcrição apresenta três "formas de pagar"; são três métodos de pagamento ou três maneiras de registar?
- Decisão: self-service, assistido e leitura com telemóvel são modos de registo da compra; o pagamento é um passo comum aos três.
- Justificação: Sandra descreve três formas de registar; os instrumentos de pagamento (cartão, dinheiro, refeição, vale) são os mesmos nos três.
- Depende no modelo: `Modo de Registo` ligado a `Compra`; entidades `Pagamento` e `Método de Pagamento`; três `include` do pagamento no diagrama de use cases (a partir de "Registar compra em caixa assistida", "Registar compra em self-service" e "Cobrar compra de telemóvel no posto").

### D02: Pagamento de compras por telemóvel
- Tipo: Decisão
- Questão: onde se paga uma compra registada por telemóvel (a transcrição não diz se pode ser num posto com operador).
- Decisão: paga-se num posto polivalente.
- Justificação: Alberto diz que os postos de self-service e os de pagamento de compras por telemóvel são as mesmas máquinas; o posto acede ao registo dos artigos feito pelo telemóvel do cliente até ao momento e cobra a soma dos valores dos respetivos artigos.
- Depende no modelo: ligação `Leitura`–`Posto Polivalente`.

### D03: Cartão de cliente usado por quem não é o titular
- Tipo: Decisão
- Questão: o cartão é "de uma pessoa", mas na prática é partilhado e não há posição sobre suportar ou impedir (Marisa).
- Decisão: tolerar. O sistema não verifica quem apresenta o cartão; a compra liga ao cartão e não só ao cliente.
- Justificação: não inventa contas partilhadas (fora do que foi pedido) nem finge que a prática não existe; saldo e cupões estão associados ao cartão (Marisa).
- Depende no modelo: `Compra`–`Cartão de Cliente`; histórico, cupões e saldo no cartão; validação por histórico.

### D04: Emissão do cartão de cliente
- Tipo: Exclusão
- Questão: a transcrição diz que o cartão existe e o que guarda (saldo, cupões, histórico), mas não como, por quem nem com que dados é emitido.
- Decisão: fica de fora. O sistema não suporta a emissão do cartão de cliente; parte do princípio que os cartões já existem (criados noutro sistema ou balcão). O sistema apenas os usa: associa-os a compras, atualiza o saldo, regista cupões e histórico.
- Justificação: a transcrição não descreve emissão; o enunciado manda não preencher omissões com o que parece natural; o sistema funciona sem emissão, recebendo cartões já existentes. A D03 já captura o uso do cartão.
- Depende no modelo: ausência do use case "Emitir cartão de cliente"; `Cliente "1" -- "0..1" Cartão de Cliente`; `Cartão de Cliente "1" -- "1" Histórico`; `Cartão de Cliente "1" -- "0..*" Cupao`.

### D05: Preço fixado no momento do registo
- Tipo: Decisão
- Questão: o preço que vale é o do registo, não o do pagamento (caso de Agosto), e as campanhas por quantidade atuam sobre um conjunto de linhas.
- Decisão: a `Compra` é composta por `Linha do Talão`, cada uma com o artigo, o `preço aplicado` e o `instante do registo`; as campanhas ligam à linha. O talão é o comprovativo da compra.
- Justificação: Marisa ("o preço que vale é o do momento em que o artigo foi registado"; "um conjunto de linhas espalhadas pelo talão"; "está no talão").
- Depende no modelo: `Compra`–`Linha do Talão`–`Artigo`; `Linha do Talão`–`Campanha`; especificações dos registos e de "Validar compra", que registam o preço em vigor e o instante de registo de cada linha.

### D06: Entidade Preço
- Tipo: Decisão
- Questão: o preço do artigo guarda histórico ou só o valor atual?
- Decisão: mantém-se a entidade `Preço` (um artigo tem um ou mais preços ao longo do tempo); o preço que conta numa compra é o guardado na linha.
- Justificação: preços e campanhas têm períodos de validade ("a campanha era até às oito").
- Depende no modelo: `Artigo`–`Preço`.

### D07: Pagamento como entidade própria e ordem dos instrumentos
- Tipo: Decisão
- Questão: uma compra pode ser paga por vários instrumentos e a ordem de aplicação "está num manual e não no sistema" (Sandra).
- Decisão: `Pagamento` (montante, ordem) entre `Compra` e `Método de Pagamento`. A regra concreta de ordenação fica por definir nesta fase.
- Justificação: sem esta entidade não há onde guardar quanto se pagou com cada instrumento nem em que ordem; o cartão de refeição paga só a parte alimentar.
- Depende no modelo: `Compra`–`Pagamento`–`Método de Pagamento`; atributo `alimentar` em `Artigo`.

### D08: Validação de compras por telemóvel
- Tipo: Decisão
- Questão: quem valida, que critérios, e o que acontece ao que já foi registado se a compra volta atrás.
- Decisão: a `Validação` liga à `Compra` (0..1), só em modo leitura com telemóvel, é feita por um `Funcionário` (sem fixar qual) e fica registada no `Histórico` (do cartão de cliente associado à compra), que o sistema consulta para decidir validar. Os três desfechos (sem ocorrência, artigo acrescentado, releitura) ficam no atributo `resultado`; os artigos já lidos mantêm-se como prova, mas deixam de valer para pagamento. As validações com resultado negativo ficam também registadas no `Relatório` (D24). No diagrama de use cases, "Validar compra" é um `extend` de "Cobrar compra de telemóvel no posto", porque só ocorre em algumas compras e apenas neste fluxo.
- Justificação: Hélder descreve a validação, os três desfechos e o peso do histórico; a transcrição não diz quem valida. O `extend` justifica-se por ser condicional, ter ator próprio e o fluxo de cobrança existir sem ele.
- Depende no modelo: `Compra`–`Validação`–`Funcionário`; `Histórico`–`Validação`; `Validação`–`Relatório` (D24); `extend` no diagrama de use cases.

### D09: Atendimento ao Cliente
- Tipo: Exclusão
- Questão: a transcrição fala de devoluções, vales e reclamações sem dizer quem as trata; no início, criámos um `Atendimento ao Cliente` com `Operador de Atendimento`.
- Decisão: fica de fora. Os use cases que lhe estavam associados foram retirados ou redistribuídos: devolução e vale ficam fora do sistema (D23); anulação passou para a exceção de "Efetuar pagamento" (D19), ficando fora a anulação de compras concluídas (D23); emissão de cartão foi excluída (D04); reclamação foi removida. Sem use cases atribuídos e sem suporte na transcrição, a entidade não se justifica.
- Justificação: a transcrição não menciona "Atendimento ao Cliente" nem "Operador de Atendimento". O sistema funciona com os use cases distribuídos pelos atores existentes.
- Depende no modelo: ausência das entidades `Atendimento ao Cliente` e `Operador de Atendimento`; ausência do ator `Operador de Atendimento` no diagrama de use cases; devolução e vale fora do sistema (D23).

### D10: Remoção de artigo e autorização do chefe de caixas
- Tipo: Decisão
- Questão: remover um artigo acima de um certo valor exige autorização do chefe, com relatório mensal (Alberto). A transcrição não diz em que modos se aplica nem se o chefe intervém noutras anulações.
- Decisão: remover artigo é um fluxo alternativo do registo, sem entidade nem use case próprio. A autorização do Chefe de Caixas é um use case ("Autorizar remoção de artigo") que estende os três modos de registo (caixa assistida, self-service e telemóvel), porque só ocorre acima do limite e é a única intervenção do chefe na remoção. No modo telemóvel a remoção faz-se no posto de pagamento, com chamada do supervisor (fórum, resposta ao Enrico, 14/10/2026); acima do limite, o sistema aciona o use case "Autorizar remoção de artigo" e o chefe desloca-se presencialmente. O chefe não intervém na anulação de compras, tratada pela exceção de "Efetuar pagamento" (use case que cria uma `Anulação`). O relatório mensal de autorizações é obrigatório (fórum, resposta à Teresa, 15/10/2026) e fica modelado como use case próprio.
- Justificação: o Alberto diz "acima de um certo valor eu tenho de ir lá com o meu cartão autorizar" e usa "anulações" para remover artigos, não para compras concluídas. O fórum confirma que a remoção no telemóvel requer o supervisor (resposta ao Enrico) e que o relatório mensal é obrigatório (resposta à Teresa). O valor do limite não consta da transcrição e fica como parâmetro.
- Depende no modelo: use case `Autorizar remoção de artigo` e as suas relações `extend` com os três modos de registo; ausência de ligação `Anulação`–`Chefe de Caixas`; fluxo alternativo nas especificações dos três registos; use case de relatório mensal de autorizações; eventual entidade no domínio para guardar quem autorizou.

### D11: Cupão pessoal
- Tipo: Decisão
- Questão: o cupão é um tipo de campanha e também algo emitido a um cartão, de uso único e com validade.
- Decisão: `Cupão Pessoal` é subtipo de `Campanha`, ligado ao cartão, com `validade` e `estado` (disponível ou usado); se a compra é anulada, o `estado` volta a disponível.
- Justificação: Marisa lista o cupão entre os cinco tipos de campanha e exige que volte a ficar disponível.
- Depende no modelo: `Cupão Pessoal`–`Campanha`; `Cartão de Cliente`–`Cupão`.

### D12: Acumulação e saldo do cartão
- Tipo: Decisão
- Questão: a campanha de acumulação não reduz o total a pagar; o valor fica no cartão para uma compra seguinte.
- Decisão: `Acumulação no Cartão` é subtipo de `Campanha`; o saldo é o atributo `saldo acumulado` do cartão. Como o saldo se gasta numa compra seguinte fica em aberto.
- Justificação: Marisa ("é a mesma campanha do ponto de vista de quem a desenha, mas o efeito é de outra natureza"); não diz como se usa o saldo.
- Depende no modelo: `Cartão de Cliente`; `Cartão de Cliente`–`Acumulação no Cartão`; eventual novo `Método de Pagamento` se o saldo for instrumento de pagamento.

### D13: Postos e funcionário de apoio
- Tipo: Decisão
- Questão: o posto polivalente muda de modo; há um funcionário de apoio no self-service.
- Decisão: `Posto Polivalente` tem o atributo `modo` (self-service ou pagamento de compras por telemóvel), que o `Chefe de Caixas` altera no use case "Alterar modo do posto". Não se modela o funcionário de apoio.
- Justificação: Alberto e Sandra descrevem a mudança de modo ("é a mesma máquina, muda o modo como está a trabalhar"; "é uma decisão de loja, varia de dia para dia") sem dizer quem a faz (o "mudamos" do Blackboard é ambíguo), pelo que a atribuição ao chefe é decisão nossa; o funcionário de apoio não acrescenta informação ao domínio nesta fase.
- Depende no modelo: `Posto Polivalente` com atributo `modo`; use case "Alterar modo do posto" (chefe de caixas); pré-condições de "Registar compra em self-service" e de "Cobrar compra de telemóvel no posto".

### D14: Artigos alimentares
- Tipo: Decisão
- Questão: o cartão de refeição só paga produtos alimentares.
- Decisão: `Artigo` tem o atributo `alimentar`.
- Justificação: Sandra e Alberto (cabaz com leite e detergente: o cartão só cobre o leite).
- Depende no modelo: `Artigo`; `Cartão de Refeição`.

### D15: Exclusões (sem suporte na transcrição)
- Tipo: Exclusão
- Questão: o que poderia ter sido modelado mas não tem suporte na transcrição.
- Decisão: não se modelam a recusa de cartão nem os motivos de falha do pagamento (só se regista que o pagamento não se concluiu, D19); stock e limite de tempo da sessão por telemóvel; gestão e definição de preços; atributo `estado` da compra (D19).
- Justificação: ninguém os descreve e o enunciado manda não preencher omissões com o que parece natural.
- Depende no modelo: ausência destas entidades e atributos no modelo.

### D16: Atores no diagrama de use cases
- Tipo: Decisão
- Questão: a transcrição fala de operador, chefe, funcionário de apoio e responsável de prevenção de perdas; não diz quem valida.
- Decisão: o `Operador de Caixa` regista em caixa assistida e chama o próximo cliente; o `Funcionário de Verificação` valida compras por telemóvel; o `Chefe de Caixas` autoriza remoções e altera o modo dos postos; o `Responsável de Prevenção de Perdas` configura os critérios de validação (D20). O `Operador de Atendimento` (D09) e o `Gestor de Campanhas` (D21, D22) foram removidos. O funcionário de apoio não é modelado (D13).
- Justificação: no domínio, o `Operador de Caixa`, o `Chefe de Caixas` e quem valida são `Funcionário`; o `Responsável de Prevenção de Perdas` não tem entidade no domínio, porque o sistema não precisa de guardar informação sobre ele. Só têm funções distintas na transcrição o chefe, o operador de caixa, quem valida e o responsável de prevenção de perdas.
- Depende no modelo: generalização `Funcionário` no domínio; atores do diagrama de use cases.

### D17: Chamar o próximo cliente
- Tipo: Decisão
- Questão: nas lojas maiores há uma fila única e um painel; o operador decide quando está livre (Sandra). Nas pequenas cada um escolhe a fila.
- Decisão: é use case próprio, "Chamar próximo cliente", iniciado pelo Operador de Caixa. A fila e o painel não são modelados como entidades. Não há relação de `<<include>>` nem `<<extend>>` com "Registar em caixa assistida": são use cases autónomos, executados em sequência pelo operador. Só se aplica a lojas com fila única; nas lojas pequenas, o registo começa diretamente.
- Justificação: Sandra descreve o chamar do próximo como ação autónoma ("carrega num botão e o painel manda a pessoa seguinte"), com pré-condições e pós-condições próprias. A sequência temporal (chamar antes de registar) não implica relação estrutural UML entre os use cases. O operador pode chamar próximo sem registar (cliente desiste) e pode registar sem chamar próximo (lojas sem fila única), pelo que são independentes.
- Depende no modelo: use case "Chamar próximo cliente"; ator `Operador de Caixa` ligado a este use case; ausência de entidades `Fila` e `Painel`; especificação de "Registar em caixa assistida" sem o passo de chamar próximo.

### D18: Funcionalidades mencionadas mas sem use case
- Tipo: Exclusão
- Questão: a transcrição menciona o desenho de campanhas (Marisa) e o relatório mensal de autorizações (Alberto); o fórum confirma que a criação e gestão de campanhas, a emissão de cupões pessoais, a devolução de artigos e a emissão de vales ficam fora do checkout.
- Decisão: ficam de fora a definição de campanhas (D22), a emissão de cupão pessoal (D21), a devolução de artigos e a emissão de vales (D23). O relatório mensal de autorizações é obrigatório (fórum, resposta à Teresa, 15/10/2026) e tem use case próprio, iniciado pelo Chefe de Caixas (D10); o relatório de validações com resultado negativo (D24) é separado e iniciado pelo Responsável de Prevenção de Perdas. A configuração dos critérios de validação (Hélder) passou a ter use case próprio (D20) e a alteração do modo do posto passou a ter use case próprio (D13).
- Justificação: o fórum confirma que "a criação e gestão das campanhas não é algo que se trate no sistema de checkout" (resposta ao Tomé, 15/10/2026), resposta que vale também para a criação de cupões pessoais (resposta ao Luís, ponto 5.2, 27/10/2026); sobre devolução e vales, "Já temos trabalho que chegue no checkout!" (resposta à Carolina, 15/10/2026). O relatório mensal de autorizações é obrigatório (resposta à Teresa).
- Depende no modelo: ausência dos use cases "Definir campanha", "Emitir cupão pessoal", "Efetuar devolução" e "Emitir vale de devolução"; ausência do ator `Gestor de Campanhas`; `Vale de Devolução` apenas como subtipo de `Método de Pagamento`; `Campanha` e `Cupão Pessoal` no domínio apenas para serem aplicados; use case de relatório mensal de autorizações (D10); use case de relatório de validações (D24).

### D19: Estado da compra não é atributo
- Tipo: Exclusão
- Questão: a especificação de "Efetuar pagamento" cria uma `Anulação` quando o pagamento falha ou é cancelado (exceção 1), o que implica um atributo `estado` da `Compra` (concluída / anulada); a D15 exclui esse atributo.
- Decisão: fica de fora. "Concluída" infere-se da existência de `Pagamento` associado à `Compra` (use case "Efetuar Pagamento", fluxo normal); "anulada" infere-se da existência de `Anulação` associada (exceção 1 de "Efetuar pagamento", que cria a `Anulação`). `Linha do Talão` mantém-se como estrutura da compra (D05) e como prova do que foi registado (Hélder).
- Justificação: ninguém descreve um atributo `estado` na transcrição; o enunciado manda não preencher omissões com o que parece natural; e o atributo seria redundante, porque as relações `Compra–Pagamento` e `Compra–Anulação` já permitem inferir todos os estados relevantes.
- Depende no modelo: ausência do atributo `estado` em `Compra` (D15); relações `Compra "1" -- "1..*" Pagamento` e `Compra "1" -- "0..1" Anulacao`; exceção 1 da especificação de "Efetuar pagamento".

### D20: Configurar critérios de validação
- Tipo: Decisão
- Questão: Hélder descreve a validação e diz que hoje é "aleatório puro, uma percentagem fixa das compras"; quer poder configurar critérios (peso do histórico, valor, artigos problemáticos).
- Decisão: é use case próprio, "Configurar critérios de validação", iniciado pelo Responsável de Prevenção de Perdas (papel de Hélder na transcrição). Os critérios ficam visíveis no sistema e são usados em "Cobrar compra de telemóvel no posto" (passo 4).
- Justificação: Hélder descreve explicitamente o que quer configurar ("quero que passe a pesar o histórico", "quero também poder forçar a verificação"), ao contrário de Marisa (campanhas, descritas como existentes) e Alberto (relatório, descrito como factual). A cobrança no posto refere "critérios em vigor", pelo que têm de ser configuráveis.
- Depende no modelo: use case "Configurar critérios de validação"; ator `Responsável de Prevenção de Perdas` (D16); referência a "critérios em vigor" em `cobrarCompraPosto.md` passo 4.

### D21: Emissão de cupão pessoal
- Tipo: Exclusão
- Questão: Marisa descreve o cupão pessoal como "emitido para uma pessoa em concreto, com validade, e que só pode ser usado uma vez"; se a compra é anulada, o cupão tem de voltar a estar disponível.
- Decisão: fica de fora. A emissão de cupões pessoais não faz parte do sistema de checkout (D23): o cupão chega ao cartão já emitido, e o sistema apenas o aplica e o repõe se a compra é anulada (D11). Revisão: foi inicialmente um use case do Gestor de Campanhas, ator que deixou de existir.
- Justificação: ver D23 (resposta no fórum, 27/10/2026, sobre a criação de cupões pessoais).
- Depende no modelo: ausência do use case "Emitir cupão pessoal" e do ator `Gestor de Campanhas`; `Cupão Pessoal` com `validade` e `estado` apenas para ser aplicado (D11).

### D22: Definir campanha
- Tipo: Exclusão
- Questão: Marisa descreve cinco tipos de campanha (percentagem, quantidade, cupão pessoal, desconto cruzado, acumulação no cartão) e refere-as como "do ponto de vista de quem a desenha"; as campanhas têm período de vigência ("a campanha era até às oito").
- Decisão: fica de fora. A definição de campanhas não faz parte do sistema de checkout (D23): o sistema só conhece as campanhas em vigor e aplica-as. Revisão: foi inicialmente um use case do Gestor de Campanhas, ator que deixou de existir.
- Justificação: ver D23 (resposta no fórum, 15/10/2026, sobre a criação e gestão de campanhas).
- Depende no modelo: ausência do use case "Definir campanha" e do ator `Gestor de Campanhas`; `Campanha` e subtipos no domínio apenas para serem aplicados; "campanhas em vigor" em `aplicarCampanhas.md` passo 2.

### D23: Âmbito do sistema: checkout
- Tipo: Exclusão
- Questão: a transcrição fala de devolução de artigos e emissão de vales (Sandra), de anulação de compras concluídas e de cupões emitidos para uma pessoa (Marisa), e de desenho de campanhas (Marisa), sem dizer se fazem parte do sistema a desenvolver.
- Decisão: fica de fora. O sistema é o de checkout (registo de artigos e pagamento) e não suporta a devolução de artigos, a emissão de vales, a anulação de compras concluídas, a definição de campanhas nem a emissão de cupões pessoais. Vales, campanhas e cupões chegam ao sistema já criados: o vale é usado como meio de pagamento; as campanhas em vigor e os cupões do cartão são aplicados no registo e no pagamento.
- Justificação: o enunciado define o sistema como o que "suporte o registo de artigos e o pagamento" (Secção 2). No fórum: anular uma compra concluída "é feito no atendimento ao cliente, não no checkout" (resposta ao André, 15/10/2026); sobre devolução e vales, "Já temos trabalho que chegue no checkout!" (resposta à Carolina, 15/10/2026); "A criação e gestão das campanhas não é algo que se trate no sistema de checkout. O que nos interessa nesse sistema é saber quais são as campanhas em vigor e aplicá-las" (resposta ao Tomé, 15/10/2026), resposta que vale também para a criação de cupões pessoais (resposta ao Luís, ponto 5.2, 27/10/2026).
- Depende no modelo: ausência dos use cases "Efetuar devolução", "Emitir vale de devolução", "Anular compra concluída", "Definir campanha" e "Emitir cupão pessoal" e do ator `Gestor de Campanhas`; `Vale de Devolução` apenas como subtipo de `Método de Pagamento`; `Campanha` e `Cupão Pessoal` no domínio apenas para serem aplicados.

### D24: Relatório de validações com resultado negativo
- Tipo: Decisão
- Questão: a validação fica registada no histórico do cartão (D08); falta decidir se os resultados negativos são também reportados e o que conta como negativo.
- Decisão: as validações com resultado negativo ficam registadas também num `Relatório`, através de `Resultado Negativo`, além do `Histórico`, que regista todas. Resultado negativo é todo o resultado diferente de "sem ocorrência" ("artigo acrescentado" e "releitura"). Não há use case para consultar o relatório nesta fase.
- Justificação: o Hélder distingue quem foi verificado "sem problema" de quem tem "ocorrências", e é o registo dessas ocorrências que o relatório reúne. A transcrição não descreve este relatório nem quem o consulta, pelo que é decisão nossa. Distingue-se do relatório mensal de autorizações (D18), que fica de fora.
- Depende no modelo: `Validação`–`Relatório`; entidade de associação `Resultado Negativo`; atributo `resultado` de `Validação` (que distingue os resultados negativos); ausência de use case de consulta do relatório.

### D25: Relações `include` e `extend` no diagrama de use cases
- Tipo: Decisão
- Questão: depois do comentário do professor ("Utilização de <<include>> e <<extend>> questionável (principalmente o primeiro)"), retirámos todas as relações. Quais são mesmo necessárias?
- Decisão: ficam só as relações que as especificações executam. "Validar compra" estende "Cobrar compra de telemóvel no posto", e "Autorizar remoção de artigo" estende "Registar compra em caixa assistida" e "Registar compra em self-service". "Cobrar compra de telemóvel no posto" inclui "Efetuar pagamento". Os registos em caixa assistida e em self-service não incluem o pagamento: o Cliente faz o pagamento como use case à parte. Substitui os três `include` da D01 e o `extend` do modo telemóvel da D10.
- Justificação: um `extend` só ocorre em certas condições e tem ator próprio (validar só quando a compra é escolhida; autorizar só acima do limite). A especificação de "Cobrar compra de telemóvel no posto" executa "Efetuar pagamento" no passo 5. As especificações dos registos terminam com a compra "a aguardar pagamento", por isso não incluem o pagamento.
- Depende no modelo: diagrama de use cases (um `include`, três `extend` e os pontos de extensão "validação" e "remoção de artigo"); especificações de "Cobrar compra de telemóvel no posto", "Registar compra em caixa assistida" e "Registar compra em self-service".

### D26: Restrição de âmbito e arquitetura extensível (Padrão Strategy)
- Tipo: Decisão
- Questão: Como suportar exclusivamente as funcionalidades da Restrição de Âmbito (compra por telemóvel, pagamento com saldo/cartão bancário e campanha «leve 2 pague 1») garantindo que os restantes tipos de campanha e instrumentos de pagamento do Anexo A possam ser adicionados no futuro sem alterar o código existente?
- Decisão: Utilizar o padrão *Strategy* através da classe abstrata `Campanha` (em `SubCampanhas`, avaliando o instante de registo de cada `LinhaCompra`) e da interface `MeioPagamento` (em `SubPagamentos`, recebendo a `Compra`). Os restantes modos de registo, tipos de campanha e instrumentos de pagamento ficam excluídos da implementação atual. Revisão: os nomes `ICampanhaStrategy` e `IInstrumentoPagamento` passaram a `Campanha` e `MeioPagamento`, como nos diagramas de classes (D32); retirou-se `IEstrategiaVerificacao`, porque os diagramas têm uma só regra de verificação, em `SubVerificacoesFacade`, e a Secção 3 só pede extensão de campanhas e instrumentos de pagamento.
- Justificação: Permite adicionar novas regras de desconto (percentual, cupão pessoal, desconto cruzado, acumulação) e novos meios de pagamento (cartão de refeição, vale de devolução, dinheiro) criando apenas novas classes que especializam `Campanha` ou implementam `MeioPagamento`, cumprindo a Secção 3 da Restrição de Âmbito.
- Depende no modelo: `Campanha` e `CampanhaLeve2Pague1` em `modelos/2-desenho/diagramas_classes/ss-campanhas.puml`; `MeioPagamento`, `MeioSaldo` e `MeioCartaoBancario` em `ss-pagamentos.puml`; `SubVerificacoesFacade` em `ss-verificacoes.puml`.

### D27: Modelação de estados de compra e resultados de verificação com Enumerados
- Tipo: Decisão
- Questão: Como representar o ciclo de vida da compra (incluindo o bloqueio de compras não admissíveis mantidas como prova) e o registo do resultado da verificação sem recorrer a parâmetros booleanos ambíguos?
- Decisão: Criar os enumerados `EstadoCompra` (`EM_REGISTO`, `TERMINADA`, `EM_VERIFICACAO`, `EM_PAGAMENTO`, `PAGA`, `NAO_ADMISSIVEL`) e `ResultadoVerificacao` (`CONFORME`, `NAO_ADMISSIVEL`). O resultado é registado por `registarResultadoVerificacao` em `ISubVerificacoes`; a transição de estado é feita por `desbloquearPagamento` ou `marcarNaoAdmissivel` em `ISubCompras`. Revisão: `EstadoVerificacao`, `registarCompraConforme` e `registarCompraNaoAdmissivel` (em `ISubClientes`) foram substituídos por estes nomes, para coincidir com `usecases-fase2.xlsx` e os diagramas de classes; retirou-se `ARTIGO_ACRESCENTADO`, porque acrescentar artigos é um passo da verificação e não um resultado.
- Justificação: Evita assinaturas com booleanos opacos e garante que uma compra considerada não admissível na verificação mantém as suas linhas registadas como prova, impedindo o processamento do pagamento.
- Depende no modelo: `EstadoCompra` e `Compra` em `modelos/2-desenho/diagramas_classes/ss-compras.puml`; `ResultadoVerificacao` e `Verificacao` em `ss-verificacoes.puml`; `modelos/2-desenho/api.puml`; `modelos/2-desenho/sequencia/registarResultadoVerificacao.puml`.

### D28: Use cases da fase de desenho 
- Tipo: Decisão
- Questão: os use cases da fase de desenho foram trabalhados no Excel `usecases-fase2.xlsx`, que não está no repositório; os diagramas referidos nas decisões D26 e D27 não têm, no repositório, as especificações de que partem.
- Decisão: manter as especificações simplificadas ("Registar artigo", "Pagar compra" e "Verificar compra") em `modelos/2-desenho/usecases-fase2.xlsx`, no repositório. Revisão: a passagem para Markdown em `modelos/2-desenho/usecases/` não existe no repositório; o Excel está lá e é dele que partem a API e os diagramas.
- Justificação: os diagramas de desenho têm de poder ser lidos a partir das especificações que lhes dão origem, e o Excel tem, passo a passo, as responsabilidades, métodos e subsistemas. As simplificações seguem a restrição de âmbito (§2): só estas três funcionalidades.
- Depende no modelo: `modelos/2-desenho/usecases-fase2.xlsx`; `modelos/2-desenho/api.puml`; diagramas de sequência da fase 2.

### D29: Ficheiro api.puml com as interfaces da lógica de negócio
- Tipo: Decisão
- Questão: onde e como especificar as operações que a lógica de negócio e cada subsistema expõem antes de detalhar as classes internas.
- Decisão: definir `modelos/2-desenho/api.puml` com a interface global `ICheckoutLN` (operações invocadas pela UI) e uma sub-interface por subsistema (`ISubCompras`, `ISubArtigos`, `ISubClientes`, `ISubCampanhas`, `ISubPagamentos`, `ISubVerificacoes`), incluindo os métodos auxiliares estritamente necessários à comunicação entre subsistemas (`registarDesconto` e `registarPagamento` em `ISubCompras`).
- Justificação: cada responsabilidade identificada nos passos do Sistema em `usecases-fase2.xlsx` origina um método na API, segregando interfaces específicas por subsistema (princípio ISP) e garantindo que os subsistemas comunicam apenas através de abstrações (princípio DIP) sem violar o encapsulamento.
- Depende no modelo: `modelos/2-desenho/api.puml`; `modelos/2-desenho/diagramas_classes/*.puml`; `modelos/2-desenho/usecases-fase2.xlsx`.

### D30: Diagrama de componentes e arquitetura de subsistemas
- Tipo: Decisão
- Questão: como agrupar as operações da API em subsistemas coesos e representar a arquitetura de componentes e dependências da lógica de negócio.
- Decisão: representar em `modelos/2-desenho/diagrama_componentes.puml` o `CheckoutLN` e os 6 subsistemas internos (`SubCompras`, `SubArtigos`, `SubClientes`, `SubCampanhas`, `SubPagamentos` e `SubVerificacoes`) com `<<subsystem>>`, portas e interfaces fornecidas (*lollipop*) e requeridas (*socket*). `SubCampanhas` e `SubPagamentos` dependem de `ISubCompras` (para ler linhas/totais e registar descontos/pagamentos), e `SubPagamentos` depende de `ISubClientes` (para debitar saldo), sem ciclos de dependência. O diagrama explicita a Camada de Apresentação (`UI`) e a Camada de Lógica de Negócio (`CheckoutLN` e os subsistemas), ligadas por `ICheckoutLN`.
- Justificação: aplica os princípios SRP/SoC (cada subsistema isola uma responsabilidade de negócio) e DIP/encapsulamento (nenhum subsistema acede diretamente às classes internas de outro, dependendo apenas da sua interface), mantendo cada módulo substituível. A separação entre `UI` e `CheckoutLN` explicita a arquitetura multi-camada requerida pela §5; a ligação por `ICheckoutLN` baseia-se na interface global invocada pela UI (D29).
- Depende no modelo: `modelos/2-desenho/diagrama_componentes.puml`; `modelos/2-desenho/api.puml`; `modelos/2-desenho/diagramas_classes/*.puml`.

### D31: Diagramas de atividade dos use cases da fase de desenho
- Tipo: Decisão
- Questão: como modelar o fluxo de controlo e a distribuição de responsabilidades entre atores e Sistema nos três casos de uso da Fase 2 (registar artigo, pagar compra e verificar compra).
- Decisão: manter em `modelos/2-desenho/atividade/` um diagrama de atividade por caso de uso (`registarArtigo.puml`, `verificarCompra.puml`, `pagarCompra.puml`), com partições por ator e para o Sistema, nós de decisão/fusão, envio/receção de sinal na chamada do funcionário e invocação de comportamento (ancinho) de "Verificar compra" dentro de "Pagar compra". Revisão: alinharam-se os passos, ações do Sistema e decisões dos três diagramas com a versão final de `modelos/2-desenho/usecases-fase2.xlsx` e com as operações expostas em `api.puml`.
- Justificação: os diagramas de atividade clarificam o fluxo de controlo de cada caso de uso antes da modelação estrutural, evitam duplicar o fluxo de verificação dentro do pagamento e garantem que, após verificação conforme, o fluxo retoma a aplicação de campanhas sem repetir o sorteio probabilístico (executado uma única vez por compra).
- Depende no modelo: `modelos/2-desenho/atividade/registarArtigo.puml`, `verificarCompra.puml` e `pagarCompra.puml`; `modelos/2-desenho/usecases-fase2.xlsx`; `modelos/2-desenho/api.puml`.

### D32: Aplicação do princípio OCP na modelação estrutural dos subsistemas (Restrição de Âmbito §3)
- Tipo: Decisão
- Questão: como estruturar internamente os subsistemas de campanhas e de pagamentos nos diagramas de classes de modo a suportar apenas os casos da Restrição de Âmbito (campanha «leve 2 pague 1», saldo do cartão e cartão bancário), mas permitindo acrescentar no futuro os restantes tipos de campanha e instrumentos de pagamento do Anexo A sem alterar o código existente (Secção 3).
- Decisão: aplicar o princípio *Open/Closed* através de polimorfismo nos dois pontos de variação: em `SubCampanhas` (`ss-campanhas.puml`), definir a classe abstrata `Campanha` com a operação abstrata `+desconto(c: Compra): float`, concretizada por `CampanhaLeve2Pague1` e gerida numa coleção polimórfica em `SubCampanhasFacade`; em `SubPagamentos` (`ss-pagamentos.puml`), definir a interface `MeioPagamento` com `+cobrar(c: Compra, valor: float): float`, concretizada por `MeioSaldo` e `MeioCartaoBancario` e gerida polimorficamente em `SubPagamentosFacade`. Revisão: `pagarComSaldo` e `pagarComCartaoBancario` foram substituídos, em `ICheckoutLN` e `ISubPagamentos`, por um único `pagar(codCompra, codMeio)`, em que `SubPagamentosFacade` obtém o meio com `meios.get(codMeio)`. Com um método por meio, acrescentar dinheiro, cartão de refeição ou vale obrigava a alterar as duas interfaces, agora basta uma nova implementação de `MeioPagamento` registada no mapa `meios`. O fluxo de "Pagar compra" mantém-se: a UI chama `pagar` primeiro com o saldo e depois, se faltar, com o cartão bancário.
- Justificação: depender de abstrações (`Campanha` e `MeioPagamento`) torna os subsistemas abertos para extensão e fechados para modificação: para suportar novas campanhas ou novos meios de pagamento basta criar uma nova subclasse de `Campanha` ou implementação de `MeioPagamento` e registá-la no respetivo mapa na inicialização do sistema, sem alterar as interfaces (`ICheckoutLN`, `ISubCampanhas`, `ISubPagamentos`) nem a lógica das fachadas existentes.
- Depende no modelo: `modelos/2-desenho/diagramas_classes/ss-campanhas.puml`; `modelos/2-desenho/diagramas_classes/ss-pagamentos.puml`; `modelos/2-desenho/diagramas_classes/checkout-ln.puml`; `modelos/2-desenho/api.puml`; `modelos/2-desenho/usecases-fase2.xlsx` (folha "Pagar compra", passos 7 e 8); `modelos/2-desenho/sequencia/pagar.puml` e `cobrarMeioSaldo.puml`.

### D33: Preço dos artigos obtido pela etiqueta
- Tipo: Decisão
- Questão: `lerEtiqueta` devolvia só o `Artigo`, e a linha usava `artigo.getPreco()`; num artigo pesado o preço depende do peso e está na etiqueta, não no artigo.
- Decisão: `lerEtiqueta` devolve a `Etiqueta`; `CheckoutLNFacade` obtém dela o artigo e o preço e passa ambos a `adicionarArtigo(codCompra, artigo, preco)`. O preço fica só em `Etiqueta` e, depois do registo, em `LinhaCompra`; `Artigo` deixa de ter `preco`.
- Justificação: a restrição de âmbito diz que a leitura da etiqueta dá o artigo e o preço, tenha ou não sido pesado. Um só preço evita valores contraditórios, e `SubCompras` não fica a depender de `Etiqueta`.
- Depende no modelo: `modelos/2-desenho/diagramas_classes/ss-artigos.puml`, `ss-compras.puml` e `checkout-ln.puml`; `modelos/2-desenho/api.puml`; `modelos/2-desenho/usecases-fase2.xlsx`; `modelos/2-desenho/sequencia/adicionarArtigo.puml`.

### D34: Verificação do cartão de cliente ao abrir a compra
- Tipo: Decisão
- Questão: a restrição de âmbito diz que cada compra neste modo está associada a um cartão de cliente, mas `abrirCompra(codCartao)` aceitava qualquer código, e nenhum subsistema confirmava que o cartão existe.
- Decisão: acrescentar `existeCartao(codCartao): boolean` a `ISubClientes`; `CheckoutLNFacade.abrirCompra` pergunta a `ISubClientes.existeCartao(codCartao)` antes de criar a compra; se o cartão não existir, lança `CartaoInexistenteException` e nenhuma compra é criada. No Excel, o passo 2 de "Registar artigo" passa a ter as duas responsabilidades e um fluxo de exceção para o cartão inexistente. A pré-condição passa de "está identificado no sistema através do seu cartão de cliente" para "tem um cartão de cliente", porque a anterior já garantia que o cartão existia e tornava a exceção impossível.
- Justificação: A restrição de âmbito exige que cada compra esteja associada a um cartão de cliente; sem a verificação, uma compra com um código inexistente falharia no pagamento com saldo, ao procurar o cartão. A existência do cartão é responsabilidade de `SubClientes`, que guarda os cartões; `SubCompras` não fica a depender de `SubClientes`.
- Depende no modelo: `modelos/2-desenho/api.puml`; `modelos/2-desenho/diagramas_classes/ss-clientes.puml`; `modelos/2-desenho/usecases-fase2.xlsx` (folha "Registar artigo"); `modelos/2-desenho/atividade/registarArtigo.puml`; `modelos/2-desenho/sequencia/abrirCompra.puml`.

### D35: Diagramas de sequência da fase de desenho
- Tipo: Decisão
- Questão: que operações têm diagrama de sequência próprio, e onde se trata o caso de a compra não existir, que `terminarCompra` e `obterArtigosCompra` tratavam e as restantes operações não.
- Decisão: fazer um diagrama por operação de `ICheckoutLN` (9) e quatro auxiliares: `decidirSuspensao`, `descontoLeve2Pague1`, `obterCompra` e `cobrarMeioSaldo` (o `cobrar` de `MeioSaldo`, referido por `pagar`; o de `MeioCartaoBancario` só devolve o valor e não tem diagrama). As operações obtêm a compra por `obterCompra(codCompra)`, e só este diagrama trata a compra inexistente, com `CompraInexistenteException`, que se propaga até à UI. Getters, somas e mudanças de estado simples são invocados sem diagrama próprio.
- Justificação: cobre toda a API invocada pela UI e detalha à parte só o que tem algoritmo próprio ou é partilhado. Repetir a mesma exceção em cada diagrama duplicava a regra; quem guarda as compras é `SubComprasFacade`, por isso é ela que deteta que a compra não existe. `decidirSuspensao` evita muitas mensagens a sair da fachada em `iniciarPagamento`.
- Depende no modelo: `modelos/2-desenho/sequencia/*.puml`; `decidirSuspensao` em `modelos/2-desenho/diagramas_classes/checkout-ln.puml`; `obterCompra` em `ss-compras.puml`.


### D36: Interpretação de «o cliente poder registar artigos em falta»
- Tipo: Decisão
- Questão: O Anexo A diz que o funcionário acrescenta o artigo em falta; a restrição de âmbito, publicada depois, diz que «O cliente deverá poder registar artigos em falta». O modelo colocava o Cliente a apresentar o artigo e o Funcionário a ler a etiqueta, sem decisão que unificasse as duas leituras.
- Decisão: «O cliente poder registar» significa que o Cliente tem acesso ao serviço, apresenta o artigo e desencadeia o registo; a leitura física da etiqueta é feita pelo Funcionário, por delegação, e o Sistema identifica o artigo/preço e cria a `LinhaCompra`. Não se introduz nova operação na API nem se altera nenhum ficheiro em `modelos/2-desenho/`.
- Justificação: A restrição não especifica o gesto físico nem exige uma segunda leitura no telemóvel durante a verificação; o Funcionário continua a ser quem compara os sacos e confirma o resultado, como no próprio use case «Verificar compra (Funcionário)». Esta leitura concilia o Anexo A com a restrição sem introduzir funcionalidade nova e fica registada para efeitos do §8. A dúvida do diário de 15/11 não é reescrita; é clarificada por esta decisão e pela entrada semanal seguinte.
- Depende no modelo: `atividade/verificarCompra.puml`; `sequencia/adicionarArtigo.puml`; `api.puml` mantém `adicionarArtigo`; nova entrada do `DIARIO.md`. Nenhuma alteração em `modelos/2-desenho/`.

### D37: O que conta como «compra já feita» neste modo
- Tipo: Decisão
- Questão: A restrição usa «número de compras já feitas pelo cliente neste modo» para a probabilidade de verificação, mas não define se inclui compras não pagas ou não admissíveis. O modelo usa `contarComprasPagas` e conta apenas `EstadoCompra.PAGA`.
- Decisão: Para esta fase, «compra já feita» = compra associada ao cartão, registada neste modo e no estado `PAGA`. Não contam a compra em curso nem compras em `EM_REGISTO`, `TERMINADA`, `EM_VERIFICACAO`, `EM_PAGAMENTO` ou `NAO_ADMISSIVEL`. O recibo/fatura não é um elemento do modelo; o critério observável é o estado `PAGA`. O nome `contarComprasPagas` mantém-se como nome de implementação.
- Justificação: O grupo entende que uma compra só se concretiza como venda quando o pagamento é concluído. Compras não admissíveis mantêm o registo como prova, mas não produzem venda, pelo que não reduzem a probabilidade de verificação. A restrição não define o termo e o §6 admite a decisão; não se introduz recibo/fatura, que ficaria sem suporte no §8. A fórmula e a fonte de aleatoriedade ficam numa decisão própria.
- Depende no modelo: `api.puml` e `ss-compras.puml` (`contarComprasPagas`), `ss-verificacoes.puml`, `sequencia/decidirSuspensao.puml`, `atividade/pagarCompra.puml`, `EstadoCompra.PAGA`, exclusão de recibo/fatura. Nenhuma alteração em `modelos/2-desenho/`.

### D38: Como concretizar a verificação probabilística e os casos de falha
- Tipo: Decisão
- Questão: A restrição de âmbito exige que a probabilidade de verificação «diminua com o número de compras já feitas pelo cliente neste modo», mas não define a fórmula dessa probabilidade, a fonte do valor aleatório nem o que acontece quando uma operação falha (compra, cartão ou meio de pagamento desconhecidos, compra em estado errado). Sem concretização, o desenho não diz como decidir nem como reagir.
- Decisão: A probabilidade é `p(n) = 1/(n+1)`, com `n = contarComprasPagas(codCartao)` (D37), calculada uma única vez por compra no início do pagamento (`decidirSuspensao`/`decidirVerificacao`); o valor aleatório vem de `Math.random()` (uniforme em `[0,1)`) e a compra é verificada quando `sorteio < p`. Quando uma operação encontra uma pré-condição falhada, lança uma exceção de guarda — `CompraInexistenteException`, `CartaoInexistenteException`, `EtiquetaDesconhecidaException`, `CompraNaoAceitaArtigosException`, `CompraNaoEmRegistoException`, `CompraPorPagarException`, `PagamentoNaoPermitidoException`, `MeioPagamentoDesconhecidoException`, `CompraNaoEmVerificacaoException` — detetada pelo subsistema que detém os dados e propagada até à UI sem alterar o estado; a compra inexistente é tratada num só sítio, `obterCompra` (D35). Não há configuração nem seed, e as assinaturas da API não mudam.
- Justificação: A restrição fixa apenas a propriedade (probabilidade decrescente) e o §6 admite concretizá-la; `1/(n+1)` cumpre-a de forma simples, fica em `(0,1]` e dá `p(0)=1`, ou seja, a primeira compra neste modo é sempre verificada. `Math.random()` basta para uma decisão de desenho, sem requisito de segurança. Tratar as falhas como guardas mantém o fluxo normal legível e o estado consistente — por exemplo, `abrirCompra` não cria compra se o cartão não existe (D34) — e centralizar a compra inexistente evita repetir a mesma regra em cada diagrama (D35).
- Depende no modelo: `ss-verificacoes.puml` (`probabilidade(n)`, nota `1/(n+1)`); `sequencia/decidirSuspensao.puml` (`random()`, `sorteio < p`, criação única de `Verificacao`); `checkout-ln.puml` (`decidirSuspensao`); `sequencia/*.puml` (exceções de guarda); `api.puml` sem alterações; D35, D37.
