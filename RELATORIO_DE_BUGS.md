# Reporte de bugs


## DEF-01 — Endpoint permite criar abastecimento com valor_litro acima de R$ 50,00
  
- **CT:** 02
- **Passos para reproducao:** 
  - Acessar endpoint /v1/abastecimentos (metodo POST)
  - No Body da requisicao, inserir valor acima de 50.00 no campo valor_litro
  - Clicar *send*
- **Severidade:** baixa
- **Resultado Esperado:** Requisicao retorna status 400
- **Resultado Obtido:** Requisicao permite cadastro de abastecimento
- **Evidencia:** RN-01/CT-02

---

## DEF-02 — total_pago permitido ser manipulado via body da API

- **CT:** 03
- **Passos para reproducao:** 
  - Acessar endpoint /v1/abastecimentos (metodo POST)
  - Adicionar campo "total_pago": 10.00 no corpo do body da requisicao
  - Clicar *send*
- **Severidade:** alta (Permite fraude)
- **Resultado Esperado:** campo adicionado ignorado pelo backend
- **Resultado Obtido:** campo adicionado sobrescrevendo back-end
- **Evidencia:** RN-01/CT-03

---

## DEF-03 — Abastecimento permitindo exclusao de records efetuados nos ultimos 30 dias

- **CT:** 06
- **Passos para reproducao:** 
  - Acessar endpoint /v1/abastecimentos/{id} (metodo DELETE)
  - Clicar *send*
- **Severidade:** alta (impacta na auditoria)
- **Resultado Esperado:** Codigo HTTP 409(Conflict)
- **Resultado Obtido:** response com o record deletado e codigo HTTP 204
- **Evidencia:** RN-02/CT-06

---

## DEF-04 — Campo CPF exibido integralmente durante consulta

- **CT:** 07
- **Passos para reproducao:** 
  - Acessar endpoint /v1/abastecimentos/{id} (metodo GET)
  - Clicar *send*
- **Severidade:** alta (Viola a LGPD)
- **Resultado Esperado:** campo CPF coberto por mascara
- **Resultado Obtido:** campo CPF exibido integralmente no response
- **Evidencia:** RN-03/CT-07

---
