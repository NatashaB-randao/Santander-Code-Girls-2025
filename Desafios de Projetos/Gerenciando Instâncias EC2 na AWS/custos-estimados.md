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
| **sc
