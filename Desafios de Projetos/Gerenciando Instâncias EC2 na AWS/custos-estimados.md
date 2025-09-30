# 💰 Estimativa de Custos - AWS Services

## 🎯 Free Tier Overview

A AWS oferece o **Free Tier** por 12 meses para novos clientes, além de serviços sempre gratuitos.

### ✅ Serviços Incluídos no Free Tier (12 meses)

| Serviço | Limites Gratuitos | Duração |
|---------|-------------------|---------|
| **EC2** | 750 horas/mês de t2.micro ou t3.micro | 12 meses |
| **EBS** | 30 GB de armazenamento (gp2 ou gp3) | 12 meses |
| **RDS** | 750 horas/mês de db.t2.micro, db.t3.micro ou db.t4g.micro | 12 meses |
| **S3** | 5 GB de armazenamento padrão + 20.000 Get + 2.000 Put | 12 meses |
| **Lambda** | 1 milhão de requisições + 400.000 GB-segundos | Sempre gratuito |
| **DynamoDB** | 25 GB de armazenamento + 25 unidades de leitura/escrita | Sempre gratuito |

## 💵 Custos Estimados - Cenário 1: EC2 + RDS + EBS

### Especificações
- **EC2**: t2.micro (1 instância, 24/7)
- **EBS**: 2 volumes de 10 GB (gp3)
- **RDS**: db.t3.micro MySQL (24/7)
- **Snapshots**: 20 GB
- **Data Transfer**: 10 GB/mês

### Cálculo Mensal

#### EC2 Instância
```
t2.micro On-Demand (fora do Free Tier)
= $0.0116/hora × 730 horas
= $8.47/mês
```

#### EBS Volumes
```
2 volumes × 10 GB × $0.08/GB-mês (gp3)
= $1.60/mês
```

#### EBS Snapshots
```
20 GB × $0.05/GB-mês
= $1.00/mês
```

#### RDS
```
db.t3.micro × $0.017/hora × 730 horas
= $12.41/mês

Storage: 20 GB × $0.115/GB-mês
= $2.30/mês
```

#### Data Transfer
```
Primeiros 10 GB: Gratuito
= $0.00/mês
```

### 📊 Total Estimado: **$25.78/mês**

> ⚠️ **Dentro do Free Tier**: $0.00/mês pelos primeiros 12 meses!

## 💵 Custos Estimados - Cenário 2: S3 + Lambda + DynamoDB

### Especificações
- **S3**: 50 GB de armazenamento
- **Lambda**: 5 milhões de requisições/mês (200ms cada)
- **DynamoDB**: 10 GB de armazenamento + requisições moderadas
- **Data Transfer**: 15 GB/mês

### Cálculo Mensal

#### S3 Storage
```
Primeiros 50 GB × $0.023/GB-mês
= $1.15/mês
```

#### S3 Requests
```
50.000 PUT requests × $0.005/1000
= $0.25/mês

500.000 GET requests × $0.0004/1000
= $0.20/mês
```

#### Lambda
```
Requisições:
(5.000.000 - 1.000.000 free) × $0.20/1M
= $0.80/mês

Compute:
5M × 0.2s × 512 MB / 1024 = 500.000 GB-s
(500.000 - 400.000 free) × $0.0000166667
= $1.67/mês
```

#### DynamoDB
```
Storage: 
(10 GB - 25 GB free) = $0.00/mês (dentro do free tier)

Read/Write Units: $0.00/mês (dentro do free tier)
```

#### Data Transfer
```
Primeiros 15 GB: Gratuito
= $0.00/mês
```

### 📊 Total Estimado: **$4.07/mês**

> ⚠️ **Com Free Tier**: ~$2.50/mês (Lambda e S3 têm cobranças residuais)

## 🔍 Breakdown Detalhado - EC2 Pricing

### Tipos de Instância (On-Demand, us-east-1)

| Tipo | vCPU | Memória | $/hora | $/mês (730h) | Uso Recomendado |
|------|------|---------|--------|--------------|-----------------|
| t2.micro | 1 | 1 GB | $0.0116 | $8.47 | Desenvolvimento, testes |
| t2.small | 1 | 2 GB | $0.023 | $16.79 | Apps pequenos |
| t2.medium | 2 | 4 GB | $0.0464 | $33.87 | Apps médios |
| t3.micro | 2 | 1 GB | $0.0104 | $7.59 | Performance melhor que t2 |
| t3.small | 2 | 2 GB | $0.0208 | $15.18 | Uso geral |
| m5.large | 2 | 8 GB | $0.096 | $70.08 | Produção, cargas balanceadas |
| c5.large | 2 | 4 GB | $0.085 | $62.05 | Compute-intensive |

### Modelos de Pricing

#### 1. On-Demand
- ✅ Sem compromisso
- ✅ Pague pelo uso
- ❌ Mais caro

#### 2. Reserved Instances (1-3 anos)
- ✅ Até 72% de desconto
- ✅ Previsibilidade de custos
- ❌ Requer compromisso

**Exemplo t2.micro:**
```
On-Demand: $8.47/mês
Reserved (1 ano): $4.96/mês (-41%)
Reserved (3 anos): $3.14/mês (-63%)
```

#### 3. Spot Instances
- ✅ Até 90% de desconto
- ✅ Ideal para workloads flexíveis
- ❌ Pode ser interrompido

**Exemplo t2.micro:**
```
Spot Price: ~$0.0035/hora = $2.56/mês (-70%)
```

#### 4. Savings Plans
- ✅ Até 72% de desconto
- ✅ Mais flexível que Reserved
- ❌ Requer compromisso de gasto

## 💾 EBS Pricing Detalhado

| Tipo | $/GB-mês | IOPS | Throughput | Uso Recomendado |
|------|----------|------|------------|-----------------|
| **gp3** | $0.08 | 3.000 base (provisionável) | 125 MB/s | Uso geral moderno |
| **gp2** | $0.10 | 3 IOPS/GB (até 16.000) | 250 MB/s | Uso geral legado |
| **io2** | $0.125 + IOPS | Provisionável | Alta | Aplicações críticas |
| **st1** | $0.045 | - | 500 MB/s | Big Data, throughput |
| **sc1** | $0.015 | - | 250 MB/s | Cold storage, baixo acesso |

### Snapshots
```
Snapshots: $0.05/GB-mês
Incremental: Apenas mudanças são cobradas
```

## 🗄️ RDS Pricing Detalhado

### Instâncias MySQL/PostgreSQL (On-Demand, us-east-1)

| Tipo | vCPU | RAM | $/hora | $/mês | Free Tier |
|------|------|-----|--------|-------|-----------|
| db.t3.micro | 2 | 1 GB | $0.017 | $12.41 | ✅ 750h/mês |
| db.t3.small | 2 | 2 GB | $0.034 | $24.82 | ❌ |
| db.t3.medium | 2 | 4 GB | $0.068 | $49.64 | ❌ |
| db.m5.large | 2 | 8 GB | $0.192 | $140.16 | ❌ |

### Storage RDS
```
General Purpose (gp3): $0.115/GB-mês
Provisioned IOPS (io1): $0.125/GB-mês + $0.10/IOPS
Magnetic (deprecated): $0.10/GB-mês

Backup Storage:
- Igual ao tamanho do DB: Gratuito
- Excedente: $0.095/GB-mês
```

### Multi-AZ
```
Dobra o custo da instância
db.t3.micro Multi-AZ: $0.034/hora = $24.82/mês
```

## ☁️ S3 Pricing Detalhado

### Storage Classes

| Classe | $/GB-mês | Retrieval | Uso Recomendado |
|--------|----------|-----------|-----------------|
| **Standard** | $0.023 | Instantâneo | Dados frequentes |
| **Intelligent-Tiering** | $0.023 + $0.0025/1000 obj | Auto | Padrões de acesso imprevisíveis |
| **Standard-IA** | $0.0125 | $0.01/GB | Acesso infrequente |
| **One Zone-IA** | $0.01 | $0.01/GB | Dados não críticos |
| **Glacier Instant** | $0.004 | $0.03/GB | Arquivos com acesso rápido |
| **Glacier Flexible** | $0.0036 | Varia | Arquivos (minutos-horas) |
| **Glacier Deep Archive** | $0.00099 | $0.02/GB | Arquivos de longo prazo |

### Requests Pricing
```
PUT, COPY, POST, LIST: $0.005/1000 requisições
GET, SELECT: $0.0004/1000 requisições
DELETE, CANCEL: Gratuito
```

### Data Transfer
```
IN para S3: Gratuito
OUT para internet:
- Primeiros 10 TB: $0.09/GB
- Próximos 40 TB: $0.085/GB
- Acima de 150 TB: $0.05/GB
```

## ⚡ Lambda Pricing Detalhado

### Compute Pricing
```
Requisições:
- Primeiro 1 milhão/mês: Gratuito
- Acima: $0.20 por 1 milhão

Duração (GB-segundo):
- Primeiros 400.000 GB-s/mês: Gratuito
- Acima: $0.0000166667/GB-s
```

### Exemplos de Custo Lambda

**Cenário 1: API Leve**
```
10 milhões de requisições/mês
Cada requisição: 128 MB, 100ms

Requisições: (10M - 1M) × $0.20/1M = $1.80
Compute: 10M × 0.1s × 128MB/1024 = 125.000 GB-s
(125.000 - 400.000 free) = $0.00

Total: $1.80/mês
```

**Cenário 2: Processamento Pesado**
```
1 milhão de requisições/mês
Cada requisição: 1.024 MB (1 GB), 5 segundos

Requisições: (1M - 1M) = $0.00 (dentro do free tier)
Compute: 1M × 5s × 1GB = 5.000.000 GB-s
(5M - 400k) × $0.0000166667 = $76.67

Total: $76.67/mês
```

## 📊 DynamoDB Pricing

### On-Demand Mode
```
Write: $1.25 por 1 milhão de requisições
Read: $0.25 por 1 milhão de requisições
Storage: $0.25/GB-mês (acima de 25 GB free tier)
```

### Provisioned Mode
```
1 WCU (Write Capacity Unit): $0.00065/hora = $0.47/mês
1 RCU (Read Capacity Unit): $0.00013/hora = $0.09/mês
Storage: $0.25/GB-mês
```

### Comparação On-Demand vs Provisioned

**Exemplo: 10 milhões de leituras + 2 milhões de escritas/mês**

**On-Demand:**
```
Reads: 10M × $0.25/1M = $2.50
Writes: 2M × $1.25/1M = $2.50
Total: $5.00/mês
```

**Provisioned (distribuído uniformemente):**
```
RCUs necessárias: ~4 RCUs
WCUs necessárias: ~1 WCU

4 RCUs × $0.09 = $0.36
1 WCU × $0.47 = $0.47
Total: $0.83/mês
```

> 💡 Provisioned é ~84% mais barato para cargas previsíveis!

## 🌐 Data Transfer Pricing

### Internet (Outbound)
```
Primeiros 10 TB/mês: $0.09/GB
Próximos 40 TB/mês: $0.085/GB
Próximos 100 TB/mês: $0.07/GB
Acima de 150 TB/mês: $0.05/GB
```

### Entre Regiões AWS
```
us-east-1 ↔ us-west-2: $0.02/GB
us-east-1 ↔ eu-west-1: $0.02/GB
```

### Dentro da Mesma Região
```
Entre AZs: $0.01/GB (entrada e saída)
Mesma AZ: Gratuito (usando IPs privados)
```

## 💡 Dicas para Reduzir Custos

### 1. Use Reserved Instances
- ✅ Economize até 72% em EC2 e RDS
- ✅ Ideal para workloads previsíveis
- ✅ Compromisso de 1 ou 3 anos

### 2. Aproveite o Free Tier
- ✅ 12 meses gratuitos para novos clientes
- ✅ Alguns serviços sempre gratuitos (Lambda, DynamoDB)
- ✅ Planeje projetos de aprendizado dentro dos limites

### 3. Right-Sizing
- ✅ Use instâncias menores quando possível
- ✅ t3/t4g são mais eficientes que t2
- ✅ Monitore com CloudWatch e ajuste

### 4. Auto Scaling
- ✅ Escale para baixo fora dos horários de pico
- ✅ Use Spot Instances para workloads tolerantes a falhas
- ✅ Configure políticas baseadas em métricas

### 5. Storage Optimization
- ✅ Use S3 Lifecycle Policies
- ✅ Mova dados antigos para Glacier
- ✅ Delete snapshots e AMIs não utilizados
- ✅ Use gp3 em vez de gp2 (20% mais barato)

### 6. Serverless First
- ✅ Lambda elimina custos de servidor ocioso
- ✅ DynamoDB On-Demand evita provisionamento excessivo
- ✅ API Gateway + Lambda = infraestrutura escalável e barata

### 7. Monitore e Alerte
- ✅ Configure AWS Budgets
- ✅ Ative Cost Explorer
- ✅ Crie alertas de billing
- ✅ Use tags para rastreamento de custos

### 8. Clean-Up Regular
- ✅ Delete recursos não utilizados
- ✅ Pare instâncias de desenvolvimento à noite
- ✅ Remova volumes EBS desanexados
- ✅ Limpe buckets S3 antigos

## 📈 Calculadoras e Ferramentas

### AWS Pricing Calculator
```
https://calculator.aws/
```
- Estime custos antes de provisionar
- Compare cenários
- Exporte orçamentos

### AWS Cost Explorer
```
Console AWS → Billing → Cost Explorer
```
- Analise gastos históricos
- Identifique tendências
- Encontre oportunidades de economia

### AWS Budgets
```
Console AWS → Billing → Budgets
```
- Defina orçamentos mensais
- Receba alertas de gastos
- Monitore uso do Free Tier

## 📊 Exemplo Real: Startup Tech

### Arquitetura
- 3x EC2 t3.small (web servers)
- 1x RDS db.t3.small Multi-AZ
- 100 GB S3 Standard
- 50 GB EBS gp3 por instância
- 5 milhões Lambda requests/mês
- 20 GB DynamoDB
- ALB (Application Load Balancer)
- 500 GB data transfer out

### Cálculo Mensal
```
EC2 (3× t3.small): $45.54
RDS Multi-AZ: $24.82 + $2.30 storage = $27.12
EBS (150 GB gp3): $12.00
S3 (100 GB): $2.30
Lambda: $2.50
DynamoDB: $0.00 (free tier)
ALB: $18.40 + $0.72 (LCU) = $19.12
Data Transfer (500 GB): $45.00

Total: $153.58/mês
```

### Com Otimizações
```
EC2 Reserved (1 ano): $26.73 (-41%)
RDS Reserved (1 ano): $15.90 (-41%)
Auto Scaling (50% economia horários baixos): $13.37
S3 Intelligent-Tiering (30% economia): $1.61
EBS gp3 otimizado: $10.00

Total Otimizado: $111.73/mês (-27%)
Economia anual: $502.20
```

## 🎯 Conclusão

### Custos Típicos por Fase

| Fase | Arquitetura | Custo Mensal |
|------|-------------|--------------|
| **Aprendizado** | Free Tier máximo | $0 - $5 |
| **Desenvolvimento** | t2/t3.micro + básicos | $10 - $30 |
| **Startup MVP** | t3.small + RDS + S3 | $50 - $150 |
| **Produção Pequena** | Multi-AZ + Auto Scaling | $200 - $500 |
| **Produção Média** | HA + Scaling + CDN | $500 - $2000 |

### Recomendações Finais

1. **Comece Pequeno**: Use Free Tier ao máximo
2. **Meça Sempre**: Ative Cost Explorer desde o dia 1
3. **Automatize**: Use Terraform/CloudFormation para reproduzibilidade
4. **Reserve**: Para workloads previsíveis, Reserved Instances são essenciais
5. **Revise**: Análise mensal de custos e otimizações

---

*Preços referentes a us-east-1 (N. Virginia) em setembro de 2025. Valores podem variar por região.*

**⚠️ Importante:** Sempre consulte a [AWS Pricing Page](https://aws.amazon.com/pricing/) oficial para valores atualizados.