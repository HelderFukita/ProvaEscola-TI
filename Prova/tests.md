# Tests — Cenários de verificação (TDD)

Estes cenários descrevem resultados observáveis. Cada um deve ser convertido em um teste automatizado durante a implementação. Os IDs de requisito apontam para casos de uso e regras de `spec.md`.

| ID | Requisito | Cenário e resultado esperado | Tipo |
| --- | --- | --- | --- |
| TEST-01 | UC1 | Cadastrar sala com nome e capacidade válidos retorna `201`; resposta contém ID positivo, nome normalizado e capacidade enviada. | Feliz |

## Cobertura requisito → testes

| Parte da spec | Cenários |
| --- | --- |
| UC1 — Cadastro de sala | TEST-01–TEST-03 |

## Observação sobre TDD

Este arquivo documenta quais comportamentos devem ser verificados. TDD, no ciclo de desenvolvimento, normalmente significa escrever um teste automatizado, observar que ele falha antes da implementação, implementar o comportamento e rodar o teste novamente. Se a avaliação exigir somente documentação, a entrega é a descrição dos cenários; não se deve confundir este documento com o código executável dos testes.
