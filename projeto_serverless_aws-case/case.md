# Case: Arquitetura Serverless na AWS com Terraform

**Repo:** [jeffersonvalente/projeto_serverless_aws](https://github.com/jeffersonvalente/projeto_serverless_aws)

---

## Problema

Precisava de uma API funcional na AWS com custo proporcional ao uso — zero quando ocioso, escala automática em pico — sem gerenciar instâncias EC2 ou cluster de containers.

O objetivo era também ter toda a infraestrutura como código, reproduzível em qualquer conta AWS sem intervenção manual.

---

## Decisões técnicas

**Lambda + API Gateway**
Lambda para a lógica de aplicação: paga por execução, escala sem configuração, sem servidor para patchar. API Gateway expõe os endpoints REST com autenticação e throttling configuráveis.

**RDS (Postgres) para persistência**
Serverless não significa sem banco. RDS Postgres gerenciado cobre backup automático, failover e manutenção sem overhead operacional.

**VPC com subnets privadas**
Lambda e RDS ficam em subnets privadas. API Gateway é o único ponto de entrada público. Lambda acessa o RDS via VPC sem passar pela internet.

**Terraform completo**
Lambda, API Gateway, RDS, VPC, subnets, security groups, IAM roles — tudo declarado em Terraform. Criar um ambiente do zero é `terraform init && terraform apply`.

**Variáveis de ambiente via SSM Parameter Store**
Secrets e configurações da Lambda lidos do SSM em tempo de execução. Sem credenciais no código ou nas variáveis de ambiente do console.

---

## Resultado

| Antes | Depois |
|---|---|
| EC2 rodando 24/7 | Lambda: paga só o que usa |
| Setup manual na conta | `terraform apply` end-to-end |
| Banco exposto ou em EC2 | RDS gerenciado em subnet privada |
| Secrets no console ou no código | SSM Parameter Store |
| Ambiente difícil de recriar | Reproduzível em qualquer conta AWS |

---

## Stack

AWS Lambda · API Gateway · RDS Postgres · VPC · SSM Parameter Store · Terraform · IAM
