# Case Técnico — Alerta Cego: Detecção de Falhas Invisíveis com New Relic, Kubernetes e GitHub Actions

Este case mostra como simulei, detectei e automatizei a resposta a um tipo de falha crítica que, na prática, **ninguém via acontecer** — mesmo em produção.  
A origem foi um incidente real em uma aplicação de sincronização, onde o processo interno parava de executar **sem gerar erros, travamentos ou qualquer sinal de alerta**. O pod seguia em execução, os logs chegavam como `INFO`, e o sistema simplesmente deixava de funcionar como deveria.

A partir dessa experiência, estruturei uma **PoC funcional** que simula esse cenário e demonstra como é possível detectar falhas silenciosas com **alertas baseados em padrões de log textual**, sem precisar alterar a aplicação. Tudo isso com **rastreabilidade de deploy**, **dashboards** e **alertas automatizados** — usando Kubernetes local, GitHub Actions e New Relic.

## Objetivo

Demonstrar, de forma prática e reprodutível, como detectar **falhas operacionais silenciosas** sem alterar o código da aplicação, sem exigir instrumentação APM e sem depender de logs em nível `ERROR`.

A proposta foi **transformar um erro invisível em algo rastreável e acionável**, usando o que já existia no ambiente: logs em `INFO`, padrões de texto previsíveis e uma estratégia de observabilidade leve, porém funcional.

## Desafios Técnicos

Durante o desenvolvimento, os principais pontos de atenção foram:

- Simular uma falha realista com status HTTP 200 (sem stacktrace)
- Gerar logs com a palavra **“error” em nível `INFO`**
- Garantir ingestão correta dos logs pelo **agente do New Relic**
- Criar **alertas baseados em texto** (`LIKE '%error%'`) com NRQL
- Marcar automaticamente os **deploys com commit hash e evento customizado**
- Automatizar todo o fluxo com **GitHub Actions, Docker e Kubernetes local (Docker Desktop)**

## Decisões Técnicas

**New Relic** foi utilizado por já estar presente no stack original durante o incidente.  
**GitHub Actions** foi mantido pela simplicidade e familiaridade do time.  
**Kubernetes local** foi escolhido para garantir reprodutibilidade sem dependências externas.

As principais decisões técnicas foram:

- Aplicação em **Python + Flask**, com duas rotas:
  - `GET /ok`: retorna 200 OK e gera log `INFO`
  - `GET /fail`: retorna 200 OK, mas gera um log `INFO` com a palavra “error”
- A imagem é **buildada localmente**, com `imagePullPolicy: Never`
- O `entrypoint.sh` aplica `envsubst` para gerar dinamicamente o `newrelic.ini`
- O **deploy automatizado** via GitHub Actions inclui **rastreabilidade por commit hash**
- Dashboard e política de alerta são criados via script usando a **API GraphQL do New Relic**
- **Eventos customizados** do tipo `Deployment` são enviados automaticamente para correlação no dashboard

Essas decisões foram feitas com foco em **simplicidade, reuso e adoção imediata**.

## Aplicabilidade real em time técnico

Mesmo sendo uma PoC, a estrutura foi pensada para uso real por qualquer time que:

- Precisa **monitorar aplicações legadas ou sem cobertura APM**
- Enfrenta **falhas operacionais sem stacktrace ou log `ERROR`**
- Depende de **crons, sincronizações ou jobs silenciosos**
- Precisa validar alertas com base em logs existentes, **sem alteração no código**

Também pode ser usada para:

- Testar **estratégias de observabilidade baseadas em log**
- Treinar times de **SRE/DevOps** sobre falhas difíceis de identificar
- Servir como referência para **automação mínima de alerta, deploy e rastreabilidade**

## Resultado entregue

- Aplicação funcional simulando **falha silenciosa com log `INFO`**
- Pipeline **GitHub Actions** com build e deploy local
- Logs visíveis no **New Relic**, detectados por busca textual
- **Dashboard NRQL versionado em JSON**
- Política de alerta criada via **script com a API GraphQL**
- Evento custom `Deployment` enviado com commit hash
- Estrutura **modular e reproduzível**

## Aprendizados

Este projeto mostrou que é possível gerar **visibilidade de produção sem alterar a aplicação**.  
A falha estava invisível — mas bastou uma **query bem definida** e **rastreio de deploy** para tornar o erro visível e acionável.

Possíveis evoluções:

- Parsing de logs multiline com **Grok**
- Simulação de payloads variáveis
- Integração com **Slack, Opsgenie**
- **Dashboards comparativos** por período
- Simulação de **degradação progressiva**

## Repositório com código

[github.com/jeffersonvalente/silent-failure-detection-newrelic](https://github.com/jeffersonvalente/silent-failure-detection-newrelic)

## Contato

[LinkedIn — Jefferson Hoy Valente](https://www.linkedin.com/in/jefferson-hoy-valente/)
