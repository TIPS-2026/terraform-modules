# terraform-modules

Módulos Terraform reutilizáveis da plataforma, versionados por tag. Eles são **construídos pela turma**: nada de módulo pronto do registry (pode servir de referência, mas o código é nosso).

📋 **Kanban:** https://github.com/orgs/TIPS-2026/projects/1 · 🏠 **Visão geral:** [giropops-senhas](https://github.com/TIPS-2026/giropops-senhas)

## Donos

| Time | Responsabilidade |
|---|---|
| **Redes** | Rede (VPC, subnets, NAT, endpoints), DNS e certificados |
| **Compute** | Cluster, load balancer, registry de imagens e serviço da aplicação |
| **Runtime** | Automação de release dos módulos |

## O que esperamos que seja entregue

### Redes
- Módulo de **rede**: VPC com subnets públicas e privadas em pelo menos 2 AZs, saída para a internet, NAT configurável (econômico em dev, resiliente em prod) e VPC endpoints opcionais
- **Publicação dos IDs de rede** de forma que outras stacks consumam sem depender do state da rede
- **DNS** da aplicação e **certificado** TLS validado automaticamente

### Compute
- **Cluster** ECS para Fargate, com logs centralizados
- **Registry** de imagens com scan, tags que nunca apontam para outra imagem e limpeza automática
- **Load balancer** público com HTTPS e health check compatível com a aplicação
- Módulo de **serviço** que:
  - rode a aplicação com o **Redis efêmero** junto (sem volume) e sem configuração manual de quem consome
  - fique em subnet privada, acessível só pelo load balancer
  - tenha autoscaling
  - **não desfaça o deploy feito pelo CD** quando a infra for reaplicada

### Todos os módulos
- `README.md` com descrição, inputs, outputs e exemplo
- `examples/` com um uso mínimo funcional
- Testes com `terraform test`
- Versões de Terraform e providers fixadas
- Release por módulo com changelog (`network-vX.Y.Z`, `ecs-service-vX.Y.Z`...)

## Regras

- **Contrato primeiro:** `variables.tf` e `outputs.tf` revisados pelo time consumidor antes da implementação.
- **Módulo não configura provider nem backend:** isso é papel do `infra-live`.
- **Nada fixo no código:** conta, região, ARNs e CIDRs entram por variável.
- **Breaking change = nova major.** Consumidores só avançam de versão por PR.
