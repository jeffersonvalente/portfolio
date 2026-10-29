# Case: IaC para AKS com Módulos Terraform e Pipeline Azure DevOps

**Repo:** [jeffersonvalente/deployAksTerraform](https://github.com/jeffersonvalente/deployAksTerraform)

---

## Problema

Um cliente precisava provisionar clusters AKS de forma repetível entre ambientes (dev, staging, prod) sem que cada ambiente virasse um fork independente com configurações divergindo ao longo do tempo.

O processo anterior era manual: portal Azure, cliques, configurações anotadas em documento Word. Qualquer novo ambiente levava horas e introduzia diferenças sutis.

---

## Decisões técnicas

**Módulos Terraform parametrizados**
Um módulo base para o cluster AKS recebe variáveis de ambiente: tamanho dos node pools, versão do Kubernetes, VNET, SKU do load balancer. O código é o mesmo; o que muda são os valores.

**State remoto no Azure Storage**
`terraform.tfstate` no Blob Storage com lock via leasing. Elimina conflito entre execuções paralelas e mantém histórico.

**Pipeline Azure DevOps**
`terraform plan` automático em PRs, com output comentado no pull request. `terraform apply` só roda na branch main, com aprovação manual para prod.

**Separação de workspaces**
Cada ambiente tem seu próprio workspace Terraform. Não há risco de um `apply` de dev afetar prod por acidente.

**Variáveis sensíveis via Key Vault**
Secrets (service principal, tokens) lidos do Azure Key Vault no pipeline, nunca hardcoded no repositório.

---

## Resultado

| Antes | Depois |
|---|---|
| Provisionamento manual no portal | `terraform apply` via pipeline |
| Horas por novo ambiente | ~15 minutos end-to-end |
| Configurações divergentes | Um módulo, todos os ambientes |
| Secrets em variáveis de pipeline | Key Vault integrado |
| Sem rastreabilidade | Histórico no state + PR aprovado |

---

## Stack

Terraform · Azure Kubernetes Service · Azure DevOps Pipelines · Azure Storage (state) · Azure Key Vault
