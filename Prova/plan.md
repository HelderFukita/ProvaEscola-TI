# Plan — Arquitetura e decisões

## 1. Stack proposta

- Python 3.11 como linguagem principal.
- FastAPI para implementação da API HTTP.
- Pydantic para validação e serialização dos dados de entrada e saída.
- PostgreSQL para persistência dos dados.
- SQLAlchemy para acesso ao banco de dados.
- Pytest para testes automatizados.
- HTTPX para testes dos endpoints da API.
- Docker para padronização do ambiente de execução.

A aplicação deverá ser executada utilizando a porta `8003`, conforme
definido na especificação.

---

## 2. Organização proposta

A aplicação será organizada em camadas, separando a interface HTTP,
as regras de negócio e a persistência dos dados.

Estrutura proposta:

- `app/main.py` — inicialização da aplicação FastAPI.
- `app/routers/` — definição dos endpoints HTTP.
- `app/schemas/` — modelos de entrada e saída da API.
- `app/services/` — regras de negócio e casos de uso.
- `app/repositories/` — acesso e persistência dos dados.
- `app/models/` — entidades persistidas no banco.
- `tests/` — testes automatizados da aplicação.

Fluxo principal:

`Router → Service → Repository → Banco de dados`

As respostas devem retornar pelo caminho inverso:

`Banco de dados → Repository → Service → Router → Cliente`

---

## 3. Responsabilidades

### Routers

Responsáveis por:

- receber requisições HTTP;
- validar o formato básico da entrada;
- chamar os serviços correspondentes;
- retornar os códigos HTTP definidos na especificação.

### Schemas

Responsáveis por:

- definir os dados esperados nas requisições;
- validar tipos e formatos;
- definir o formato das respostas JSON.

### Services

Responsáveis por:

- executar os casos de uso;
- aplicar as regras de negócio;
- verificar disponibilidade da sala;
- detectar conflitos de reservas;
- calcular duração e valor da reserva;
- aplicar frações de cobrança;
- aplicar o teto diário;
- aplicar a regra de tolerância;
- controlar criação e cancelamento das reservas.

### Repositories

Responsáveis por:

- consultar salas;
- criar salas;
- consultar reservas;
- criar reservas;
- atualizar o estado das reservas;
- persistir e recuperar dados.

### Models

Representam as entidades persistidas, principalmente:

- Sala;
- Reserva.

### Tests

Responsáveis por verificar:

- contrato dos endpoints;
- regras de negócio;
- limites;
- casos de erro;
- cálculo da tarifa;
- conflitos;
- cancelamento e liberação do horário.

---

## 4. Decisões técnicas relevantes

### DT-01 — Separação entre API e regras de negócio

As regras de negócio não devem ser implementadas diretamente nos
routers.

**Justificativa:** isso facilita testes, manutenção e rastreabilidade
entre os casos de uso e sua implementação.

### DT-02 — Valores monetários em centavos

Os valores monetários serão tratados como inteiros em centavos.

**Justificativa:** evita problemas de precisão associados ao uso de
números de ponto flutuante em cálculos monetários.

### DT-03 — Cálculo de cobrança centralizado

O cálculo da tarifa será centralizado no serviço responsável pela
cobrança da reserva.

Esse serviço deverá utilizar:

- `TARIFA_HORA_CENTAVOS = 500`;
- `FRACAO_MINUTOS = 30`;
- `TETO_DIARIO_CENTAVOS = 7000`.

**Justificativa:** evita duplicação das regras de cobrança e permite
testar isoladamente os limites da tarifa.

### DT-04 — Tratamento de datas e horários

Datas e horários serão tratados como valores com informação de fuso
horário e normalizados antes das comparações.

**Justificativa:** evita inconsistências na comparação dos períodos
das reservas.

### DT-05 — Verificação de conflito no serviço

A validação de conflitos será realizada antes da criação da reserva.

**Justificativa:** garante que duas reservas incompatíveis para a mesma
sala não sejam aceitas pela aplicação.

### DT-06 — Cancelamento por alteração de estado

O cancelamento deverá alterar o estado da reserva em vez de remover
necessariamente seu registro da persistência.

**Justificativa:** permite preservar o histórico da reserva e identificar
que ela foi cancelada.

### DT-07 — Injeção do relógio

A obtenção do horário atual deverá ser abstraída de forma que possa ser
controlada durante os testes.

**Justificativa:** permite testar de forma determinística as regras que
dependem de data e hora.

---

## 5. Estratégia de verificação
Os testes serão organizados de forma a validar cada caso de uso
definido na especificação.

A estratégia deverá incluir:

1. Testes de contrato HTTP, verificando endpoints, entradas, respostas
   e códigos de status.

2. Testes das regras de negócio de reserva.

3. Testes dos limites de cobrança, especialmente:
   - frações de 30 minutos;
   - tarifa por hora;
   - teto diário de `7000` centavos;
   - tolerância de `10` minutos.

4. Testes de conflito entre reservas da mesma sala.

5. Testes de reservas em salas diferentes.

6. Testes de cancelamento e posterior liberação do horário.

7. Testes de dados inválidos e recursos inexistentes.

Os testes devem contemplar situações antes, no limite e depois dos
limites definidos pelas regras de negócio.

---

## 6. Riscos e pontos de atenção

### Risco 1 — Cálculo incorreto da tarifa

Erros de arredondamento ou interpretação das frações de 30 minutos
podem gerar valores incorretos.

**Mitigação:** centralizar o cálculo e testar os limites das frações.

### Risco 2 — Erros de fuso horário

Comparações incorretas de datas e horários podem permitir reservas
indevidas ou gerar conflitos incorretos.

**Mitigação:** utilizar datas com fuso horário e normalizar os valores
antes das comparações.

### Risco 3 — Reservas conflitantes

Duas solicitações podem tentar ocupar o mesmo período.

**Mitigação:** realizar validação de conflito antes da criação da
reserva e garantir consistência na persistência.

### Risco 4 — Interpretação incorreta da tolerância

A regra `TOLERANCIA_MINUTOS = 10` depende do contexto definido pela
especificação.

**Mitigação:** manter a regra centralizada e cobrir seus limites nos
testes.

### Risco 5 — Divergência entre documentação e implementação

Alterações nas regras podem não ser refletidas nos documentos
relacionados.

**Mitigação:** manter rastreabilidade entre `spec.md`, `plan.md`,
`tasks.md` e `tests.md`.