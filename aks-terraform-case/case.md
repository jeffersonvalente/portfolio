# Case Técnico — Provisionamento de AKS com Terraform e Azure DevOps

Este case mostra como estruturei uma prova de conceito (PoC) para provisionamento automatizado de clusters AKS utilizando Terraform, Azure DevOps e GitHub — com o objetivo de demonstrar a viabilidade técnica de um pipeline completo de infraestrutura como código.

O projeto foi construído a partir da replicação de um ambiente real utilizado na empresa em que eu trabalhava, e teve como foco apresentar a solução para um cliente que estava iniciando a jornada de automação e Kubernetes.

## Objetivo

Criar uma PoC realista e funcional de provisionamento de clusters AKS totalmente automatizado, usando ferramentas que o cliente já utilizava em sua stack (Azure + Azure DevOps).  
A ideia era demonstrar que o processo podia ser seguro, simples e reaproveitável — eliminando a prática manual que o time usava até então para subir clusters na mão.

## Desafios técnicos

Durante o desenvolvimento, enfrentei alguns pontos-chave:

- Configuração detalhada de pipelines no Azure DevOps (integração com GitHub + organização dos stages)
- Separação e reutilização de módulos Terraform
- Estruturação de variáveis de ambiente e parametrização segura
- Necessidade de um backend remoto (bucket) para armazenar o terraform.tfstate
- Padrão de nomeação para ambientes e recursos

Tudo isso teve que ser pensado considerando que no futuro outros clusters poderiam ser provisionados pela mesma base.

## Decisões técnicas

- AKS foi escolhido por já ser parte do stack da empresa e do cliente.
- Terraform foi priorizado ao invés do Bicep, por já existir conhecimento interno (dois devops do cliente já utilizavam Terraform) — o que permitiria escalar a adoção com menos fricção.
- Azure DevOps foi mantido por já ser o pipeline padrão tanto da consultoria quanto do cliente, e facilitar a integração com permissões já definidas.

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
- Se eu fosse refazer, daria mais atenção à forma de apresentação e à modularização dos pipelines

## Repositório com código

[https://github.com/jeffersonvalente/deployAksTerraform](https://github.com/jeffersonvalente/deployAksTerraform)

## Contato

[LinkedIn](https://www.linkedin.com/in/jefferson-hoy-valente/)
