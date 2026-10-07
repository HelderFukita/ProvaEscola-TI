# Sistema de Reservas de Salas de Estudo

Este projeto utiliza uma abordagem orientada por especificação para
descrever o comportamento, as decisões técnicas, as tarefas e os
cenários de verificação do Sistema de Reservas de Salas de Estudo.

A documentação foi organizada para manter rastreabilidade entre os
requisitos do sistema, as decisões técnicas, o trabalho necessário e
os testes.

## Arquivos

- `constitution.md` — define os princípios e regras gerais que devem
  ser respeitados durante todo o projeto.

- `spec.md` — define o comportamento esperado do sistema, incluindo
  casos de uso, entradas, regras de negócio, critérios de aceitação,
  respostas, erros, requisitos não funcionais e limites do escopo.

- `plan.md` — apresenta a arquitetura proposta, tecnologias,
  organização da aplicação, responsabilidades e decisões técnicas.

- `tasks.md` — decompõe o trabalho necessário para implementar a
  solução e define as dependências entre as tarefas.

- `tests.md` — descreve os cenários de verificação que devem ser
  automatizados para validar os comportamentos definidos na
  especificação.

- `README.md` — apresenta a organização da documentação e orienta
  a leitura dos arquivos.

## Ordem de leitura e produção

A documentação deve ser lida e produzida na seguinte ordem:

1. `constitution.md`
2. `spec.md`
3. `plan.md`
4. `tasks.md`
5. `tests.md`

O `README.md` pode ser utilizado como ponto inicial para compreender
a organização do projeto.

A sequência representa a relação entre os documentos:

`Constitution → Spec → Plan → Tasks → Tests`

A `constitution.md` estabelece os princípios gerais.

A `spec.md` transforma o problema em comportamentos observáveis e
critérios de aceitação.

O `plan.md` define como a solução será organizada tecnicamente para
atender à especificação.

O `tasks.md` transforma o planejamento em trabalho executável.

O `tests.md` define como verificar se os comportamentos especificados
foram implementados corretamente.

## Limite entre os documentos

### Constitution

Define princípios e regras gerais aplicáveis ao projeto como um todo.

Não deve conter regras específicas de funcionamento das reservas,
como valores de tarifa ou limites particulares do domínio.

### Spec

Define o que o sistema deve fazer.

Deve conter casos de uso, entradas, regras de negócio, critérios de
aceitação, respostas, erros, limites e requisitos do sistema.

Não deve definir detalhes de implementação que pertençam ao
planejamento técnico.

### Plan

Define como a solução será organizada tecnicamente.

Pode conter tecnologias, arquitetura, componentes, responsabilidades,
decisões técnicas e justificativas.

Não deve alterar ou redefinir os requisitos estabelecidos na `spec.md`.

### Tasks

Define o trabalho necessário para construir a solução.

Cada tarefa deve possuir um resultado esperado e, quando necessário,
suas dependências.

Não deve introduzir novos requisitos de negócio.

### Tests

Define como os comportamentos da `spec.md` serão verificados.

Os cenários devem ser rastreáveis aos casos de uso e regras da
especificação, incluindo casos normais, erros e limites.

Os testes não devem criar regras de negócio que não estejam definidas
na `spec.md`.

## Rastreabilidade

As informações devem permanecer rastreáveis entre os documentos:

`Spec → Plan → Tasks → Tests`

Uma alteração em uma regra de negócio deve ser refletida nos
documentos afetados, mantendo a consistência da documentação.

O projeto deve evitar duplicação ou contradição entre os documentos.