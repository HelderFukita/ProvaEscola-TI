# Constitution — Sistema de Reservas de Salas de Estudo

## Propósito

Estabelecer os princípios e regras gerais que devem ser respeitados
na definição, implementação e validação do Sistema de Reservas de
Salas de Estudo.

As regras deste documento devem orientar os demais artefatos do
projeto, garantindo consistência, rastreabilidade e previsibilidade
do comportamento do sistema.

## Princípios

1. A API deve seguir o padrão REST.

2. As requisições e respostas devem utilizar JSON.

3. Os nomes das propriedades JSON devem utilizar `camelCase`.

4. Os recursos da API devem utilizar nomes no plural.

5. Mensagens destinadas aos usuários devem estar em português.

6. Erros devem possuir um código identificável e uma mensagem
   compreensível, sem exposição de detalhes internos.

7. Toda regra de negócio deve possuir pelo menos um cenário de
   teste correspondente.

8. Os testes devem contemplar os limites definidos pelas regras
   de negócio, incluindo situações imediatamente antes, no limite
   e imediatamente depois, quando aplicável.

9. As regras de negócio devem ser mantidas separadas das decisões
   técnicas de implementação.

10. Os requisitos devem manter rastreabilidade entre especificação,
    planejamento, tarefas e testes.

## Como resolver conflitos
