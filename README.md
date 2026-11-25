# DSS 2026/2027 — Grupo 04

Repositório do trabalho prático de Desenvolvimento de Sistemas de Software
(Licenciatura em Engenharia Informática, Universidade do Minho).

## Grupo

| Nº | Nome | Utilizador GitHub |
|----|------|-------------------|
|a104180|Pedro Rebocho Amaral de Almeida Miranda|sk1w4lker|
|a107672|Daniel Roettger Barroso|daniibarroso|
|a110903|Alexandra Cerqueira da Luz das Neves|xana06|
|a106838|Alexandre Rei Mateus|AlexrMateus|
|a110770|Eduardo Gabriel Esteves Adão|EduardoAdao1|

## Como compilar e executar

_A preencher até à entrega final._

## Organização do repositório

| Pasta / ficheiro           | Conteúdo                                                          |
|----------------------------|-------------------------------------------------------------------|
| `DIARIO.md`                | Diário do projecto (uma entrada por semana)                       |
| `docs/decisoes.md`         | Registo de decisões e exclusões                                   |
| `docs/enunciado/`          | Enunciado, restrição de âmbito e alteração de requisitos (anexos) |
| `docs/relatorio/`          | Documento técnico final                                           |
| `docs/ia/`                 | Conversas com ferramentas de IA generativa                        |
| `modelos/1-requisitos/`    | Análise de requisitos                                             |
| `modelos/2-desenho/`       | Desenho e modelação conceptual                                    |
| `modelos/3-implementacao/` | Modelos da solução efectivamente implementada                     |
| `src/`                     | Implementação                                                     |

## Regras (resumo; o enunciado é que manda)

- Os modelos UML são mantidos em PlantUML (`.puml`). Uma imagem não substitui o ficheiro fonte.
- O diário escreve-se semana a semana, num commit por entrada, e não se reescreve.
- Cada decisão sobre um ponto em aberto, e cada exclusão, fica registada em `docs/decisoes.md`.
- A utilização de IA generativa é declarada no diário, e as conversas ficam em `docs/ia/`.
- Quando forem emitidos, juntem a restrição de âmbito e a alteração de requisitos a `docs/enunciado/`.
- O estado do repositório avaliado em cada entrega é o do último commit antes da hora limite
  (datas no enunciado). Sugere-se marcar cada entrega com uma tag:
  `entrega-requisitos`, `entrega-desenho`, `entrega-final`.

## Verificações automáticas

Em cada push que toque em `modelos/` ou em `DIARIO.md`, o GitHub Actions verifica que:

- todos os ficheiros `.puml` compilam;
- não há imagens em `modelos/` sem ficheiros `.puml` na mesma pasta;
- o `DIARIO.md` só recebeu linhas novas (nenhuma linha antiga foi alterada ou apagada).
