---
agent: 'agent'
tools: ['githubRepo', 'github', 'get_me', 'get_pull_request', 'get_pull_request_comments', 'get_pull_request_diff', 'get_pull_request_files', 'get_pull_request_reviews', 'get_pull_request_status', 'list_pull_requests', 'request_copilot_review']
description: 'Liste meus pull requests no repositorio atual'
---

Pesquise o repo atual (usando #githubRepo para informacoes do repo) e liste quaisquer pull requests que encontrar (usando #list_pull_requests) que estejam atribuidos a mim.

Descreva o proposito e os detalhes de cada pull request.

Se um PR estiver aguardando alguem revisar, destaque isso na resposta.

Se houver falhas de checks no PR, descreva-as e sugira possiveis correcoes.

Se nao houve review do Copilot, ofereca solicitar uma usando #request_copilot_review.
