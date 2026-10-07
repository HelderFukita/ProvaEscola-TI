# Spec — Sistema de Reservas de Salas de Estudo

## 1. Objetivo

O sistema deve permitir o gerenciamento de reservas de salas de estudo,
incluindo as operações necessárias para consultar salas, realizar reservas
e aplicar as regras de utilização e cobrança definidas para o sistema.

## 2. Termos e regras comuns

### Sala
Espaço disponível para realização de reservas.

### Reserva
Registro de utilização de uma sala em determinado período.

### Tarifa
A tarifa de utilização da sala é definida por
`TARIFA_HORA_CENTAVOS = 500`.

### Fração de cobrança
A cobrança deve considerar a constante
`FRACAO_MINUTOS = 30`.

### Teto diário
O valor máximo diário de cobrança é definido por
`TETO_DIARIO_CENTAVOS = 7000`.

### Tolerância
O sistema possui uma tolerância de
`TOLERANCIA_MINUTOS = 10`, aplicada conforme as regras de utilização
da reserva.

## 3. Interface e formato de resposta

A aplicação será disponibilizada por uma API HTTP.

As requisições e respostas devem utilizar JSON, exceto quando o
endpoint definir uma resposta sem corpo.

Os recursos da API devem utilizar nomes no plural.

As propriedades JSON devem utilizar `camelCase`.

### Formato de erro

Os erros devem possuir um código identificável e uma mensagem
compreensível para o usuário, sem exposição de detalhes internos.

## 4. Casos de uso

### UC1 — Cadastrar sala

**Operação:** `POST /rooms`

**Entrada:**
- `name`
- `capacity`

**Regras e critérios de aceitação:**
1. O nome da sala é obrigatório.
2. O nome da sala não pode ser vazio.
3. A capacidade deve ser um número inteiro positivo.
4. Quando os dados forem válidos, a sala deve ser cadastrada.
5. A sala criada deve possuir um identificador único.

**Sucesso:**
- HTTP `201`.
- Retornar os dados da sala criada.

**Erros:**
- Dados inválidos → HTTP `400`.

---

### UC2 — Consultar salas

**Operação:** `GET /rooms`

**Entrada:**
- Nenhuma.

**Regras e critérios de aceitação:**
1. O sistema deve retornar as salas cadastradas.
2. Cada sala deve informar seu identificador, nome e capacidade.
3. Quando não houver salas cadastradas, o sistema deve retornar uma lista vazia.

**Sucesso:**
- HTTP `200`.
- Retornar a lista de salas em JSON.

---

### UC3 — Realizar reserva de sala

**Operação:** `POST /reservations`

**Entrada:**
- `roomId`
- `date`
- `startAt`
- `endAt`
- `fullDay`

**Regras e critérios de aceitação:**
1. A sala informada deve existir.
2. A reserva deve possuir uma data válida.
3. A reserva pode ser realizada por horário ou para o dia inteiro.
4. Quando realizada por horário, o horário de início deve ser anterior ao horário de término.
5. A duração da reserva deve respeitar a `FRACAO_MINUTOS = 30`.
6. O sistema deve calcular o valor da reserva utilizando a `TARIFA_HORA_CENTAVOS = 500`.
7. O valor diário não pode ultrapassar o `TETO_DIARIO_CENTAVOS = 7000`.
8. O sistema deve aplicar a `TOLERANCIA_MINUTOS = 10` conforme a regra definida para utilização da reserva.
9. Uma sala não pode possuir duas reservas com períodos conflitantes.
10. Reservas de salas diferentes não devem ser consideradas conflitantes.
11. Uma reserva válida deve receber um identificador único.
12. O valor calculado da reserva deve ser armazenado em centavos.

**Sucesso:**
- HTTP `201`.
- Retornar:
  - identificador da reserva;
  - identificador da sala;
  - data;
  - horário de início;
  - horário de término;
  - valor da reserva;
  - status da reserva.

**Erros:**
- Dados inválidos → HTTP `400`.
- Sala inexistente → HTTP `404`.
- Horário conflitante → HTTP `409`.

**Variações:**

**Reserva por horário**
- O usuário informa o horário de início e término.
- O sistema calcula a duração e o valor correspondente.

**Reserva de dia inteiro**
- O usuário seleciona a opção de dia inteiro.
- O sistema considera o período integral definido para a utilização da sala.
- O valor é calculado conforme as regras de cobrança.

---

### UC4 — Cancelar reserva

**Operação:** `DELETE /reservations/{id}`

**Entrada:**
- `id` da reserva.

**Regras e critérios de aceitação:**
1. A reserva informada deve existir.
2. Uma reserva já cancelada não pode ser cancelada novamente.
3. Ao cancelar uma reserva, o período anteriormente ocupado deve ser liberado.
4. Após o cancelamento, outra reserva poderá utilizar o mesmo período, desde que as demais regras sejam atendidas.

**Sucesso:**
- HTTP `204`.
- A resposta não deve possuir corpo.

**Erros:**
- Reserva inexistente → HTTP `404`.
- Reserva já cancelada → HTTP `404`.

## 5. Fora do escopo

Não fazem parte desta especificação:

1. Autenticação e gerenciamento de credenciais de usuários.

2. Gerenciamento de funcionários, administradores ou permissões de acesso.

3. Processamento efetivo de pagamentos ou integração com gateways de pagamento.

4. Cadastro, alteração ou gerenciamento das tarifas pelo usuário.
   Os valores utilizados no cálculo são definidos pela especificação.

5. Envio de notificações por e-mail, SMS ou outros canais.

6. Relatórios financeiros ou relatórios gerenciais avançados.

7. Integração com sistemas externos.

8. Controle físico de acesso às salas, como catracas, fechaduras
   eletrônicas ou dispositivos IoT.


## 6. Requisitos não funcionais

1. A aplicação deve ser disponibilizada por meio de uma API HTTP.

2. O serviço deve utilizar a porta `8003`.

3. As requisições e respostas devem utilizar JSON, exceto respostas
   que não possuam corpo.

4. As propriedades dos objetos JSON devem utilizar a convenção
   `camelCase`.

5. Os endpoints devem seguir o padrão REST e utilizar nomes de
   recursos no plural.

6. Datas e horários devem utilizar o padrão ISO 8601 com informação
   de fuso horário.

7. Os identificadores de salas e reservas devem ser únicos.

8. Os valores monetários devem ser representados em centavos.

9. Os erros da API devem utilizar um formato padronizado, contendo
   código e mensagem compreensível.

10. A aplicação deve manter separação entre as regras de negócio
    e as decisões técnicas de implementação.

11. As regras de negócio devem possuir testes correspondentes,
    incluindo os limites e casos de borda relevantes.