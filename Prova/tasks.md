# Tasks — Decomposição do trabalho

| ID | Tarefa | Resultado esperado | Dependências |
| --- | --- | --- | --- |
| TASK-01 | Preparar a estrutura da aplicação | Projeto FastAPI configurado, execução na porta 8003 e estrutura de diretórios definida | — |
| TASK-02 | Implementar cadastro e consulta de salas | Endpoints `POST /rooms` e `GET /rooms` funcionando conforme `spec.md` | TASK-01 |
| TASK-03 | Implementar modelo e persistência de reservas | Entidade Reserva e operações de criação, consulta e atualização persistidas | TASK-01 |
| TASK-04 | Implementar realização de reservas | Endpoint `POST /reservations` com validação dos dados e das regras de reserva | TASK-02, TASK-03 |
| TASK-05 | Implementar cálculo de tarifas | Cálculo utilizando tarifa de 500 centavos/hora, frações de 30 minutos e teto diário de 7000 centavos | TASK-04 |
| TASK-06 | Implementar controle de tolerância | Regra de tolerância de 10 minutos implementada conforme `spec.md` | TASK-04 |
| TASK-07 | Implementar verificação de conflitos | Sistema impede reservas conflitantes para a mesma sala | TASK-03, TASK-04 |
| TASK-08 | Implementar cancelamento de reservas | Endpoint `DELETE /reservations/{id}` funcionando e horário liberado após cancelamento | TASK-03, TASK-04 |
| TASK-09 | Criar testes automatizados | Casos de uso, regras de negócio, erros e casos de borda cobertos conforme `tests.md` | TASK-02 a TASK-08 |
| TASK-10 | Revisar rastreabilidade | Verificar correspondência entre `constitution.md`, `spec.md`, `plan.md`, `tasks.md` e `tests.md` | TASK-09 |

## Critério de conclusão

O trabalho será considerado concluído quando todas as tarefas forem
realizadas, os casos de uso e regras definidos em `spec.md` estiverem
implementados, os cenários definidos em `tests.md` estiverem
automatizados e a revisão final confirmar a rastreabilidade entre os
documentos.
