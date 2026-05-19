# Plano de testes V-lab

## 1. Escopo de Testes

Validação funcional e de integridade de todos os endpoints da API de abastecimentos, garantindo que as regras de negócio (RN-01 a RN-08) sejam aplicadas rigorosamente.

  - Fluxo completo de CRUD (`Criação, Consulta, Atualização e Exclusão`).

  - Validação de campos obrigatórios e limites de valores (`litros, preço unitário, datas`).

  - Cálculo automático do total_pago pelo backend.

  - Integridade referencial de motoristas e veículos.

  - Regras de privacidade (`máscara de CPF`).

  - Mecanismos de filtragem (`case-insensitive`) e paginação consistente.

  - Sistema de reporte de erros e geração de protocolos.

  - Consistência temporal (`restrições de datas passadas e futuras`).

  - Validação de compatibilidade de combustível para veículos elétricos.

## 2. Estratégia de Teste

A abordagem será focada em testes de caixa-preta via API, utilizando as seguintes técnicas:
- Testes Funcionais Positivos e Negativos: Validar se a API retorna sucesso para dados válidos e os códigos de erro corretos (`HTTP 400, 404, 409`) para violações de regras.

- Análise de Valor de Limite: Testar as fronteiras de campos como valor_litro (`até R$ 50,00`), data (`limite de 2020 e data futura)` e o tamanho da descricao no reporte (`10 a 500 caracteres`).

- Testes de Regressão: Garantir que a criação de um reporte não altere o registro de abastecimento original.

- Testes de Idempotência e Métodos: Verificar se chamadas repetidas ou em IDs inexistentes mantêm o comportamento esperado (`ex: DELETE retornar 404 se o recurso não existe`).

- Validação de Resposta (Payload): Conferir se a máscara do CPF segue o padrão `XXX.XXX.XXX-XX` em todos os retornos que envolvam dados do motorista.

## 3. Riscos Identificados
Com base nas criticidades apresentadas, os principais riscos do projeto são:

- Risco de Integridade de Dados: O sistema permitir o envio do campo total_pago pelo cliente, ignorando o cálculo interno do backend, o que geraria divergências financeiras.

- Risco de Segurança/Privacidade (LGPD): Falha na aplicação da máscara do CPF em algum endpoint de consulta, expondo dados sensíveis de motoristas.

- Risco de Auditoria: Exclusão acidental de abastecimentos que possuem reportes ativos nos últimos 30 dias, comprometendo rastreios legais.

- Risco de Negócio (Incompatibilidade): Permissão de abastecimento com combustível fóssil em veículos elétricos por falha na validação do tipo de veículo.

- Risco de Consistência: Filtros e paginação retornarem dados inconsistentes, como apresentar um total de páginas que não condiz com os filtros de UF ou combustível aplicados.

- Risco Temporal: Aceitação de registros com datas retroativas ao início do sistema (antes de 2020) ou datas futuras, o que invalidaria relatórios gerenciais.
