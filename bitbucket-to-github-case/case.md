# Automação de Migração de Repositórios do Bitbucket para o GitHub

Este case documenta a criação de um script em Python para automatizar a migração de repositórios do Bitbucket para o GitHub — com preservação completa de histórico de commits, branches e nomes originais.

A solução foi desenvolvida para atender uma situação crítica de transição de plataforma sob prazo apertado, onde migrar manualmente mais de 270 repositórios era inviável técnica e operacionalmente.

## Contexto

A empresa para a qual presto consultoria utilizava repositórios hospedados no Bitbucket de sua fornecedora. Quando a fornecedora decidiu descontinuar o uso da plataforma, surgiu a necessidade urgente de migrar todos os repositórios para um GitHub próprio da empresa consultada.

Esse processo precisava ser feito com rastreabilidade completa, sem perda de histórico e dentro de um prazo de aproximadamente um mês.

## Objetivo

Evitar a perda de informações e o retrabalho manual na migração de centenas de repositórios, garantindo:

- Preservação de branches, histórico de commits e nomes
- Organização e rastreio da origem
- Capacidade de filtrar e priorizar o que seria migrado

## Desafios técnicos

Durante o desenvolvimento, enfrentei diversos pontos críticos:

- A API do Bitbucket exigia controle de paginação para workspaces grandes
- O Bitbucket possui o conceito de "projetos", que não existe no GitHub — isso exigiu um contorno lógico
- Autenticação por App Password, controle de permissão por SSH
- Existência de repositórios que não deveriam ser migrados, o que exigiu a criação de filtros dinâmicos por sufixo ou nome

## Decisões técnicas

A linguagem escolhida foi Python pela facilidade de leitura, curva de adoção acessível e compatibilidade com ferramentas como a GitHub CLI. Além disso, garante que o script possa ser facilmente adaptado e mantido por outros membros do time, se necessário.

A CLI oficial do GitHub (`gh`) foi usada para facilitar a criação automatizada de repositórios privados com autenticação segura.

## Resultado entregue

- Script funcional capaz de clonar qualquer repositório Bitbucket e enviá-lo para o GitHub
- Preservação de todos os históricos, branches e nomes
- Capacidade de aplicar filtros para selecionar quais repositórios migrar
- Migração controlada e segura, feita repositório por repositório

O script foi validado em ambiente real, com feedback imediato da equipe: **a automação poupou tempo, evitou erros manuais e garantiu uma transição muito mais segura do que seria possível manualmente.**

## Aprendizados

- A lógica para lidar com projetos e nomes inconsistentes no Bitbucket exigiu bastante tratamento e refinamento
- Se fosse refazer, buscaria otimizar a performance da execução para migrar em lotes maiores ou com paralelismo controlado
- Essa entrega mostrou o impacto real de uma automação bem pensada em um cenário que mistura urgência, escala e transição técnica

## Repositório com código

[https://github.com/jeffersonvalente/bitbucket-to-github-migration](https://github.com/jeffersonvalente/bitbucket-to-github-migration)

## Contato

[LinkedIn](https://www.linkedin.com/in/jefferson-hoy-valente/)
