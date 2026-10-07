# Tests — Cenários de verificação (TDD)

Estes cenários descrevem resultados observáveis do Sistema de Reservas
de Salas de Estudo. Cada cenário deve ser convertido em um teste
automatizado durante a implementação. Os IDs dos requisitos apontam
para os casos de uso e regras definidos em `spec.md`.

| ID | Requisito | Cenário e resultado esperado | Tipo |
| --- | --- | --- | --- |
| TEST-01 | UC1 | Cadastrar sala com nome e capacidade válidos retorna `201`; a resposta contém ID único, nome e capacidade informados. | Feliz |
| TEST-02 | UC1 | Cadastrar sala sem nome ou sem capacidade retorna `400`. | Erro |
| TEST-03 | UC1 | Cadastrar sala com capacidade igual a zero ou negativa retorna `400`. | Borda |
| TEST-04 | UC2 | Consultar salas com salas cadastradas retorna `200` e uma lista contendo as salas cadastradas. | Feliz |
| TEST-05 | UC2 | Consultar salas sem nenhuma sala cadastrada retorna `200` com lista vazia. | Borda |
| TEST-06 | UC3 | Criar reserva por horário para uma sala existente e período válido retorna `201` e informa os dados da reserva e o valor calculado. | Feliz |
| TEST-07 | UC3 | Criar reserva sem um dos campos obrigatórios ou com dados inválidos retorna `400`. | Erro |
| TEST-08 | UC3 | Criar reserva para uma sala inexistente retorna `404`. | Erro |
| TEST-09 | UC3 | Criar reserva com horário de término igual ao horário de início retorna `400`. | Borda |
| TEST-10 | UC3 | Criar reserva com horário de término anterior ao horário de início retorna `400`. | Borda |
| TEST-11 | UC3 | Criar reserva com duração exatamente igual a `30` minutos é aceita e retorna `201`. | Borda |
| TEST-12 | UC3 | Criar reserva com duração de `60` minutos é aceita e retorna `201`. | Feliz |
| TEST-13 | UC3 | Criar reserva com duração que não respeita a fração de `30` minutos deve ser rejeitada conforme a regra definida na `spec.md`. | Borda |
| TEST-14 | UC3 | Criar uma reserva cujo período se sobrepõe a outra reserva da mesma sala retorna `409`. | Borda |
| TEST-15 | UC3 | Criar uma reserva iniciando exatamente no horário de término de outra reserva da mesma sala é aceita quando os períodos forem adjacentes. | Borda |
| TEST-16 | UC3 | Criar reservas no mesmo período para salas diferentes é permitido e retorna `201`. | Borda |
| TEST-17 | UC3 | Criar uma reserva para o dia inteiro, em uma sala disponível, retorna `201` e considera o período integral definido pela especificação. | Feliz |
| TEST-18 | UC3 | Reserva de `60` minutos utiliza a tarifa de `500` centavos por hora e retorna `500` centavos como valor calculado. | Regra de negócio |
| TEST-19 | UC3 | Reserva de `90` minutos calcula o valor utilizando as frações de `30` minutos conforme a regra de cobrança. | Borda |
| TEST-20 | UC3 | Valor acumulado de cobrança exatamente igual a `7000` centavos respeita o teto diário e é aceito. | Borda |
| TEST-21 | UC3 | Quando o cálculo ultrapassar `7000` centavos, o valor considerado não deve ultrapassar o teto diário. | Borda |
| TEST-22 | UC3 | Datas e horários informados em formato ISO 8601 com fuso horário válido são aceitos. | Feliz |
| TEST-23 | UC3 | Datas ou horários em formato inválido retornam `400`. | Erro |
| TEST-24 | UC4 | Consultar uma reserva existente retorna `200` e seus dados. | Feliz |
| TEST-25 | UC4 | Consultar uma reserva inexistente retorna `404`. | Erro |
| TEST-26 | UC4 | Cancelar uma reserva existente retorna `204` sem corpo na resposta. | Feliz |
| TEST-27 | UC4 | Tentar cancelar uma reserva já cancelada retorna `404`. | Borda |
| TEST-28 | UC4 | Após cancelar uma reserva, o período anteriormente ocupado pode ser reservado novamente, desde que as demais regras sejam atendidas. | Borda |
| TEST-29 | UC3 | Uma nova reserva não deve entrar em conflito com uma reserva cancelada. | Borda |

## Cobertura requisito → testes

| Parte da spec | Cenários |
| --- | --- |
| UC1 — Cadastrar sala | TEST-01–TEST-03 |
| UC2 — Consultar salas | TEST-04–TEST-05 |
| UC3 — Realizar reserva | TEST-06–TEST-23 |
| UC4 — Cancelar reserva | TEST-24–TEST-29 |
| Regras de tarifa | TEST-18–TEST-21 |
| Regras de conflito | TEST-14–TEST-16, TEST-28–TEST-29 |
| Regras de data e horário | TEST-09–TEST-13, TEST-22–TEST-23 |

## Observação sobre TDD

Este arquivo documenta os comportamentos que devem ser verificados.
Durante o desenvolvimento, cada cenário deverá ser convertido em um
teste automatizado.

No ciclo de TDD, o teste deve inicialmente falhar, o comportamento
deve ser implementado e o teste deve ser executado novamente para
confirmar o resultado esperado.

A documentação dos cenários não substitui os testes automatizados
quando estes forem exigidos durante a implementação.