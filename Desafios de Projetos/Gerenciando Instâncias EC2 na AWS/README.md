# 🚀 Gerenciando Instâncias EC2 na AWS

## 📋 Sobre o Projeto

Este repositório documenta minha jornada de aprendizado sobre **gerenciamento de instâncias EC2 na AWS**, desenvolvido como parte do desafio prático da DIO (Digital Innovation One). O objetivo é consolidar conhecimentos em computação em nuvem, demonstrando a aplicação prática dos conceitos estudados.

## 🎯 Objetivos de Aprendizagem

- ✅ Compreender a arquitetura e funcionamento das instâncias EC2
- ✅ Configurar e gerenciar volumes EBS (Elastic Block Store)
- ✅ Integrar EC2 com outros serviços AWS (RDS, S3, Lambda, DynamoDB)
- ✅ Implementar boas práticas de segurança e gerenciamento
- ✅ Documentar processos técnicos de forma estruturada

## 🏗️ Arquiteturas Implementadas

### 1️⃣ Arquitetura EC2 com RDS e EBS

Esta arquitetura demonstra uma aplicação web típica hospedada na AWS:

```
Usuario → EC2 (Instância Principal)
           ↓
           ├── RDS (Banco de Dados)
           ├── D-EBS (Volume de Dados)
           └── E-EBS (Volume Extra)
```

**Componentes:**
- **EC2**: Instância computacional principal
- **RDS**: Amazon Relational Database Service para armazenamento estruturado
- **EBS Volumes**: Armazenamento em bloco persistente (D-EBS e E-EBS)

**Fluxo de Dados:**
1. Usuário acessa a aplicação via interface
2. EC2 processa as requisições
3. Dados persistentes são armazenados no RDS
4. Arquivos e dados de aplicação ficam nos volumes EBS

### 2️⃣ Arquitetura Serverless com S3, Lambda e DynamoDB

Arquitetura orientada a eventos para processamento de arquivos:

```
Usuario → S3 (Upload via AWS CLI)
           ↓
           S3 → Lambda (Trigger automático)
                 ↓
                 DynamoDB (Armazenamento NoSQL)
```

**Componentes:**
- **S3**: Armazenamento de objetos/arquivos
- **Lambda**: Processamento serverless com suporte a múltiplas linguagens (Node.js, .NET Core, Python)
- **DynamoDB**: Banco de dados NoSQL de alta performance

**Fluxo de Dados:**
1. Usuário faz upload de arquivo para S3 via AWS CLI
2. S3 dispara trigger para função Lambda
3. Lambda processa o arquivo (pode ser Node.js, Python ou .NET Core)
4. Dados processados são armazenados no DynamoDB

## 💡 Principais Aprendizados

### Instâncias EC2
- **Tipos de instâncias**: T2, T3 (burstable), M5 (uso geral), C5 (otimizado para compute)
- **Configuração de segurança**: Security Groups, Key Pairs, IAM Roles
- **Pricing models**: On-Demand, Reserved Instances, Spot Instances
- **Auto Scaling**: Configuração de grupos para alta disponibilidade

### Volumes EBS
- **Tipos de volume**: 
  - gp3/gp2 (SSD de uso geral)
  - io2/io1 (SSD provisionado para alta IOPS)
  - st1 (HDD otimizado para throughput)
  - sc1 (HDD frio para dados acessados raramente)
- **Snapshots**: Backup incremental de volumes
- **Encryption**: Criptografia at-rest para segurança de dados

### Integração com RDS
- **Engines suportados**: MySQL, PostgreSQL, MariaDB, Oracle, SQL Server
- **Multi-AZ deployment**: Alta disponibilidade com replicação síncrona
- **Read Replicas**: Escalabilidade de leitura
- **Automated backups**: Ponto de recuperação de até 35 dias

### Arquitetura Serverless
- **Event-driven architecture**: Redução de custos com cobrança por execução
- **Lambda layers**: Reutilização de código e bibliotecas
- **DynamoDB streams**: Reação a mudanças em tempo real
- **S3 lifecycle policies**: Otimização de custos de armazenamento

## 🛠️ Tecnologias e Serviços Utilizados

| Serviço | Descrição | Caso de Uso |
|---------|-----------|-------------|
| **Amazon EC2** | Computação virtual escalável | Hospedagem de aplicações web |
| **Amazon RDS** | Banco de dados relacional gerenciado | Armazenamento de dados estruturados |
| **Amazon EBS** | Armazenamento em bloco | Volumes persistentes para EC2 |
| **Amazon S3** | Armazenamento de objetos | Arquivos, backups, hosting estático |
| **AWS Lambda** | Computação serverless | Processamento orientado a eventos |
| **Amazon DynamoDB** | Banco NoSQL gerenciado | Dados de alta performance e escalabilidade |
| **AWS CLI** | Interface de linha de comando | Automação e scripting |

## 📚 Conceitos Importantes

### Security Groups
Firewall virtual que controla o tráfego de entrada e saída das instâncias EC2:
- Regras baseadas em protocolo, porta e origem/destino
- Stateful: tráfego de retorno é automaticamente permitido
- Podem ser compartilhados entre múltiplas instâncias

### Elastic IPs
Endereços IPv4 estáticos projetados para computação em nuvem dinâmica:
- Permanece associado à sua conta até ser liberado
- Pode ser rapidamente remapeado para outra instância
- Ideal para manter endereços IP consistentes

### IAM Roles para EC2
Permissões temporárias e rotacionadas automaticamente:
- Sem necessidade de armazenar credenciais na instância
- Segue o princípio do menor privilégio
- Facilita integrações com outros serviços AWS

### Tags
Sistema de metadados para organização e billing:
- Facilita gerenciamento de recursos em larga escala
- Permite criação de relatórios de custo detalhados
- Essencial para governança em ambientes corporativos

## 🔧 Comandos Úteis AWS CLI

### EC2
```bash
# Listar instâncias
aws ec2 describe-instances

# Iniciar instância
aws ec2 start-instances --instance-ids i-1234567890abcdef0

# Parar instância
aws ec2 stop-instances --instance-ids i-1234567890abcdef0

# Criar snapshot de volume EBS
aws ec2 create-snapshot --volume-id vol-1234567890abcdef0 --description "Backup diário"
```

### S3
```bash
# Upload de arquivo
aws s3 cp arquivo.txt s3://meu-bucket/

# Sincronizar diretório
aws s3 sync ./local-folder s3://meu-bucket/remote-folder

# Listar objetos
aws s3 ls s3://meu-bucket/
```

### Lambda
```bash
# Listar funções
aws lambda list-functions

# Invocar função
aws lambda invoke --function-name minhaFuncao output.txt

# Atualizar código da função
aws lambda update-function-code --function-name minhaFuncao --zip-file fileb://function.zip
```

## 🎓 Boas Práticas Implementadas

1. **Segurança em Camadas**
   - Security Groups restritivos
   - Chaves SSH protegidas
   - IAM Roles com permissões mínimas necessárias

2. **Alta Disponibilidade**
   - Distribuição em múltiplas AZs
   - Auto Scaling configurado
   - RDS Multi-AZ para redundância

3. **Otimização de Custos**
   - Uso de Spot Instances quando apropriado
   - Reserved Instances para cargas previsíveis
   - S3 Lifecycle Policies para arquivamento

4. **Monitoramento e Logging**
   - CloudWatch para métricas
   - CloudTrail para auditoria
   - Alertas configurados para eventos críticos

5. **Backup e Recuperação**
   - Snapshots automáticos de EBS
   - RDS automated backups
   - Testes periódicos de restore

## 📁 Estrutura do Repositório

```
.
├── README.md                          # Este arquivo
├── images/                            # Diagramas e capturas de tela
│   ├── arquitetura-ec2-rds.png
│   └── arquitetura-s3-lambda.png
├── scripts/                           # Scripts de automação
│   ├── deploy-ec2.sh
│   ├── backup-ebs.sh
│   └── lambda-deploy.py
└── docs/                              # Documentação adicional
    ├── configuracao-inicial.md
    ├── troubleshooting.md
    └── custos-estimados.md
```

## 🔗 Recursos Úteis

### Documentação Oficial AWS
- [EC2 User Guide](https://docs.aws.amazon.com/ec2/)
- [RDS Documentation](https://docs.aws.amazon.com/rds/)
- [S3 Documentation](https://docs.aws.amazon.com/s3/)
- [Lambda Developer Guide](https://docs.aws.amazon.com/lambda/)
- [DynamoDB Documentation](https://docs.aws.amazon.com/dynamodb/)

### Ferramentas e Calculadoras
- [AWS Pricing Calculator](https://calculator.aws/)
- [AWS Well-Architected Tool](https://aws.amazon.com/well-architected-tool/)
- [AWS CLI Command Reference](https://awscli.amazonaws.com/v2/documentation/api/latest/index.html)

### Cursos e Certificações
- AWS Certified Solutions Architect - Associate
- AWS Certified Developer - Associate
- AWS Certified SysOps Administrator - Associate

## 🤝 Contribuições

Este é um projeto de aprendizado pessoal, mas sugestões e feedbacks são sempre bem-vindos! Sinta-se livre para:
- Abrir issues com dúvidas ou sugestões
- Propor melhorias na documentação
- Compartilhar suas próprias experiências



## 🙏 Agradecimentos

- **Digital Innovation One (DIO)** pela oportunidade de aprendizado
- **AWS** pela documentação completa e recursos educacionais
- Comunidade dev brasileira pelo suporte e compartilhamento de conhecimento

---

⭐ **Gostou do projeto? Deixe uma estrela!**

📚 **Continue aprendendo**: Este é apenas o começo da jornada na AWS. Próximos passos incluem ECS, EKS, e arquiteturas mais complexas!

*Última atualização: Setembro 2025*