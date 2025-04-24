# Case Técnico — Provisionamento de AKS com Terraform e Azure DevOps

Este case mostra como estruturei uma prova de conceito (PoC) para provisionamento automatizado de clusters AKS utilizando Terraform, Azure DevOps e GitHub — com o objetivo de demonstrar a viabilidade técnica de um pipeline completo de infraestrutura como código.

O projeto foi inspirado em desafios reais enfrentados durante minha atuação com Azure e AKS, e estruturado como uma PoC funcional para demonstrar práticas de automação e provisionamento com foco em reusabilidade e escalabilidade.

## Objetivo

Criar uma PoC realista e funcional de provisionamento de clusters AKS totalmente automatizado, usando ferramentas que já faziam parte do dia a dia técnico do time (Azure + Azure DevOps).  
A ideia era demonstrar que o processo podia ser seguro, simples e reaproveitável — eliminando a prática manual comum na criação de clusters.

## Desafios técnicos

Durante o desenvolvimento, enfrentei alguns pontos-chave:

- Configuração detalhada de pipelines no Azure DevOps (integração com GitHub + organização dos stages)
- Separação e reutilização de módulos Terraform
- Estruturação de variáveis de ambiente e parametrização segura
- Padronização de variáveis e nomenclatura para ambientes e recursos

Tudo isso teve que ser pensado considerando que no futuro outros clusters poderiam ser provisionados pela mesma base.

## Decisões técnicas

- AKS foi escolhido por já ser parte do stack usado no contexto original.
- Terraform foi priorizado ao invés do Bicep, por já existir conhecimento interno — o que permitiria escalar a adoção com menos fricção.
- Azure DevOps foi mantido por já ser o pipeline padrão do time, facilitando a integração com permissões já definidas.

Essas decisões não foram apenas técnicas — foram baseadas em contexto organizacional, barreiras culturais e facilidade de adoção futura.

## Aplicabilidade real em time técnico

Apesar de ter nascido como uma PoC, a estrutura foi pensada desde o início para ser aproveitada por qualquer membro do time — sem depender da minha presença.

A modularização com Terraform, a parametrização via biblioteca de variáveis e os templates YAML do pipeline foram organizados de forma que:

- Um novo cluster pode ser criado com apenas alguns ajustes mínimos
- A estrutura pode ser reutilizada em múltiplos ambientes (dev, hmg, prd) sem reescrever código
- Toda a automação pode ser entendida por outro devops ou engenheiro de plataforma com leitura simples

Essa visão ajudou o time a visualizar o valor da automação além do script — como algo que sustenta operação e escala.

Mesmo não sendo levada à produção por decisões organizacionais externas, a estrutura foi elogiada pela simplicidade e replicabilidade.

## Resultado entregue

- Pipeline funcional para provisionamento de clusters AKS
- Uso de variáveis parametrizadas por ambiente
- Estrutura modular reaproveitável
- Documentação mínima com instruções de uso

## Aprendizados

- Foi o primeiro projeto onde me desafiei a criar uma entrega técnica pensando também em evangelização futura
- A estrutura criada serviu de base para outros testes e futuros projetos de automação

Se eu fosse refazer hoje, com a visão que tenho agora:

- Implementaria um backend remoto com controle de estado em storage externo (ex: Azure Storage Account) para preparar o pipeline para uso real em produção
- Incluiria workspaces ou organização explícita por ambientes (`dev`, `hmg`, `prd`) já no código
- Criaria scripts auxiliares de execução (`deploy.sh`, `destroy.sh`) para facilitar onboarding de novos membros
- Adicionaria exemplos de rollback seguro e destruição controlada da infraestrutura
- Enriqueceria a documentação técnica nos próprios arquivos `.tf` e nos YAMLs de pipeline, com comentários explicando as decisões

Esses pontos aumentariam a clareza, a reusabilidade e a preparação da estrutura para times maiores ou múltiplos clusters simultâneos.

## Repositório com código

[https://github.com/jeffersonvalente/deployAksTerraform](https://github.com/jeffersonvalente/deployAksTerraform)

## Contato

[LinkedIn](https://www.linkedin.com/in/jefferson-hoy-valente/)
