# Case: Um VPS Barato Virado Plataforma de Engenharia Privada

**Repo:** [jeffersonvalente/one-vps-platform](https://github.com/jeffersonvalente/one-vps-platform)

---

## Problema

Precisava de um ambiente pessoal para hospedar projetos, rodar experimentos e atender clientes freelance sem depender de planos cloud caros ou ambientes compartilhados de empresa.

O desafio era fazer isso com custo mensal baixo, sem abrir portas expostas na internet e com a mesma disciplina de GitOps e observabilidade que aplico em ambientes produtivos.

---

## Decisões técnicas

**k3s em vez de k8s completo**
K3s roda num VPS de 2 vCPU / 4 GB RAM sem problema. Mantém compatibilidade com manifests Kubernetes padrão mas sem o overhead do control plane full.

**Tailscale para acesso remoto**
Nenhuma porta exposta na internet. Todo acesso passa pela VPN mesh do Tailscale: SSH, kubectl, dashboards internos. Se o IP do VPS mudar, não afeta nada.

**ArgoCD para GitOps**
Estado do cluster vive no repositório. Qualquer mudança passa por PR. Rollback é reverter o commit. Sem `kubectl apply` manual.

**SigNoz para observabilidade**
OpenTelemetry nativo, self-hosted, sem enviar dados para fora. Logs, métricas e traces centralizados no mesmo lugar.

**Terraform + Ansible**
Terraform provê o VPS e recursos de rede. Ansible configura o sistema operacional e instala k3s. Separação clara entre infra e configuração.

**Isolamento por cliente**
Cada projeto ou cliente roda em namespace separado com ResourceQuota e NetworkPolicy. Um namespace não enxerga os outros.

---

## Resultado

| Antes | Depois |
|---|---|
| Ambiente local ou cloud cara | VPS ~R$80/mês |
| Acesso por IP exposto | Tailscale, zero porta pública |
| Deploy manual | ArgoCD GitOps |
| Logs espalhados | SigNoz centralizado |
| Setup na mão a cada rebuild | Terraform + Ansible idempotente |

O ambiente está em produção para uso pessoal e freelance. Rebuild completo leva menos de 30 minutos.

---

## Stack

k3s · ArgoCD · SigNoz · Tailscale · Terraform · Ansible · Kubernetes NetworkPolicy
