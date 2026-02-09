---
agent: agent
description: 'Execute testes pytest com cobertura, descubra linhas sem cobertura e aumente a cobertura para 100%.'
---

O objetivo e que os testes cubram todas as linhas de codigo.

Gere um relatorio de coverage com:

pytest --cov --cov-report=annotate:cov_annotate

Se voce estiver verificando a cobertura de um modulo especifico, pode especifica-lo assim:

pytest --cov=your_module_name --cov-report=annotate:cov_annotate

Voce tambem pode especificar testes especificos para rodar, por exemplo:

pytest tests/test_your_module.py --cov=your_module_name --cov-report=annotate:cov_annotate

Abra o diretorio cov_annotate para ver o codigo-fonte anotado.
Havera um arquivo por arquivo de fonte. Se um arquivo tiver 100% de cobertura de source, significa que todas as linhas estao cobertas por testes, entao voce nao precisa abrir o arquivo.

Para cada arquivo com menos de 100% de cobertura de teste, encontre o arquivo correspondente em cov_annotate e revise-o.

Se uma linha comecar com ! (ponto de exclamacao), significa que a linha nao foi coberta por testes.
Adicione testes para cobrir as linhas ausentes.

Continue rodando os testes e melhorando a cobertura ate que todas as linhas estejam cobertas.
