# Casos de Teste V-lab

## 1. Validação de Valores e Cálculos (RN-01)

| ID | Cenário de Teste | Entrada (Payload/Ação) | Resultado Esperado | Resultado obtido |
|----|------------------|------------------------|--------------------|------------------|
| CT-01 | Criar abastecimento com litragem inválida | litros: 0 ou litros: -5 | HTTP 400 | ✅ |
| CT-02 | Criar abastecimento com preço unitário excessivo | valor_litro: 50.01 | HTTP 400 | ❌ |
| CT-03 | Tentar injetar valor total pelo cliente | Enviar total_pago: 10.00 no POST | API deve ignorar o valor enviado e calcular o total correto no backend | ❌ |
| CT-04 | Validar precisão monetária | Abastecimento válido | O campo total_pago deve possuir exatamente 2 casas decimais | ✅ |

## 2. Integridade Referencial e Auditoria (RN-02)

| ID | Cenário de Teste | Entrada (Payload/Ação) | Resultado Esperado | Resultado obtido |
|----|------------------|------------------------|--------------------|------------------|
| CT-05 | Criar abastecimento com IDs inexistentes | motorista_id ou veiculo_id inválidos | HTTP 400 com mensagem clara de erro | ✅ |
| CT-06 | Excluir abastecimento com reporte ativo | DELETE em ID reportado nos últimos 30 dias | HTTP 409 (Conflict)| ❌ |

## 3. Privacidade e Exposição de Dados (RN-03)

| ID | Cenário de Teste | Entrada (Payload/Ação) | Resultado Esperado | Resultado obtido |
|----| -----------------|------------------------|--------------------|------------------|
| CT-07 | Validar máscara de CPF (LGPD) | GET em qualquer endpoint de abastecimento | CPF retornado no formato XXX.XXX.XXX-XX | ❌ |
|CT-08 | Validar exibição da placa | GET em qualquer endpoint de abastecimento | Placa do veículo exibida integralmente (sem máscara) | ✅ |

## 4. Filtros, Paginação e Consistência (RN-04)

| ID | Cenário de Teste | Query | Resultado Esperado | Resultado Obtido |
|----| -----------------|-------|--------------------|------------------|
| CT-09 | Filtro de UF Case-Insensitive | ?uf=pe, ?uf=PE, ?uf=Pe | Os três filtros devem retornar o mesmo conjunto de resultados | ❌ |
| CT-10 | Filtro de Combustível Case-Insensitive | ?combustivel=GASOLINA ou ?combustivel=gasolina | Resultados idênticos independentemente da caixa | ❌ |
| CT-11 | Ordem cronológica inválida | data_inicio > data_fim | HTTP 400 com mensagem explicativa | ❌ |
| CT-12 | Consistência de totais na paginação | -- |Aplicar filtros de UF ou período total e total_pages devem refletir apenas os itens filtrados | ❌ |
| CT-13 | Acesso a página inexistente | ?page=999 (além do limite real) | HTTP 200 com data: [] (lista vazia) | ✅ |

## 5. Comportamento de Métodos HTTP (RN-05)

| ID | Cenário de Teste | Entrada (Payload/Ação) | Resultado Esperado | Resultado Obtido |
|----| -----------------|------------------------|--------------------|------------------|
| CT-14 | DELETE em recurso inexistente | DELETE /abastecimentos/99999 | HTTP 404 (não 500) | ❌ |
| CT-15 | PUT em recurso inexistente | PUT /abastecimentos/99999 | HTTP 404 (sem criação silenciosa) | ❌ |
| CT-16 | GET em recurso inexistente | GET /abastecimentos/99999 | HTTP 404 com payload { "error": "...", "code": "NOT_FOUND" } | ✅ |

## 6. Sistema de Reporte (RN-06)
| ID | Cenário de Teste | Entrada (Payload/Ação) | Resultado Esperado | Resultado Obtido | 
|----|------------------|------------------------|--------------------|------------------|
| CT-17 | Múltiplos reportes no mesmo registro | Criar 2 reportes para o mesmo ID |Cada reporte deve gerar um protocolo único | ✅ |
| CT-18 | Limite de caracteres na descrição | Enviar descrição < 10 ou > 500 caracteres | HTTP 400 | ✅ |
| CT-19 | Integridade pós-reporte | Consultar registro após reportar erro | O abastecimento original deve estar inalterado | ✅ |

## 7. Consistência Temporal e Coerência (RN-07 e RN-08)
| ID | Cenário de Teste | Entrada (Payload/Ação) | Resultado Esperado | Resultado Obtido |
|----| -----------------|------------------------|--------------------|------------------|
| CT-20 | Data futura proibida | data > 1 hora do servidor | HTTP 400 | ❌ |
| CT-21 | Limite histórico de data | data anterior a 01/01/2020 | HTTP 400 | ❌ |
| CT-22 | Abastecimento de veículo elétrico | Veículo tipo "ELETRICO" + combustível convencional | HTTP 400 | ✅ |
