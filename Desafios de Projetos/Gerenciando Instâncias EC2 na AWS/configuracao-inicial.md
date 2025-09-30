# ⚙️ Configuração Inicial - Passo a Passo

## 📋 Pré-requisitos

Antes de começar, certifique-se de ter:

- ✅ Conta AWS criada e verificada
- ✅ AWS CLI instalado na sua máquina
- ✅ Conhecimentos básicos de Linux/terminal
- ✅ Editor de texto ou IDE (VS Code recomendado)

## 🔐 1. Configurando AWS CLI

### Instalação

**Windows:**
```bash
msiexec.exe /i https://awscli.amazonaws.com/AWSCLIV2.msi
```

**macOS:**
```bash
curl "https://awscli.amazonaws.com/AWSCLIV2.pkg" -o "AWSCLIV2.pkg"
sudo installer -pkg AWSCLIV2.pkg -target /
```

**Linux:**
```bash
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install
```

### Configuração de Credenciais

1. Acesse o Console AWS → IAM → Users → Seu usuário
2. Clique em "Security credentials"
3. Crie uma Access Key
4. No terminal, execute:

```bash
aws configure
```

Preencha as informações:
```
AWS Access Key ID: [sua-access-key]
AWS Secret Access Key: [sua-secret-key]
Default region name: us-east-1
Default output format: json
```

### Verificação
```bash
aws sts get-caller-identity
```

## 🖥️ 2. Criando sua Primeira Instância EC2

### Via Console AWS

1. **Acesse EC2 Dashboard**
   - Console AWS → Services → EC2

2. **Launch Instance**
   - Click em "Launch Instance"
   - Nome: `MinhaPrimeiraInstancia`

3. **Escolha a AMI**
   - Amazon Linux 2023 AMI (Free Tier elegível)

4. **Tipo de Instância**
   - t2.micro (Free Tier elegível)

5. **Key Pair**
   - Create new key pair
   - Nome: `minha-chave-ec2`
   - Type: RSA
   - Format: .pem (Linux/Mac) ou .ppk (Windows/PuTTY)
   - **Download e guarde em local seguro!**

6. **Network Settings**
   - Crie um Security Group permitindo:
     - SSH (22) - Seu IP
     - HTTP (80) - 0.0.0.0/0
     - HTTPS (443) - 0.0.0.0/0

7. **Storage**
   - 8 GB gp3 (Free Tier)

8. **Launch Instance**

### Via AWS CLI

```bash
# Criar key pair
aws ec2 create-key-pair \
  --key-name minha-chave-ec2 \
  --query 'KeyMaterial' \
  --output text > minha-chave-ec2.pem

# Ajustar permissões (Linux/Mac)
chmod 400 minha-chave-ec2.pem

# Criar security group
aws ec2 create-security-group \
  --group-name meu-sg \
  --description "Security group para minha EC2"

# Adicionar regra SSH
aws ec2 authorize-security-group-ingress \
  --group-name meu-sg \
  --protocol tcp \
  --port 22 \
  --cidr 0.0.0.0/0

# Lançar instância
aws ec2 run-instances \
  --image-id ami-0c55b159cbfafe1f0 \
  --count 1 \
  --instance-type t2.micro \
  --key-name minha-chave-ec2 \
  --security-groups meu-sg
```

## 🔌 3. Conectando à Instância EC2

### Linux/Mac
```bash
ssh -i "minha-chave-ec2.pem" ec2-user@[IP-PUBLICO-DA-INSTANCIA]
```

### Windows (PowerShell)
```powershell
ssh -i "minha-chave-ec2.pem" ec2-user@[IP-PUBLICO-DA-INSTANCIA]
```

### Windows (PuTTY)
1. Converta .pem para .ppk usando PuTTYgen
2. Configure a conexão no PuTTY com o arquivo .ppk

## 💾 4. Criando e Anexando Volume EBS

### Via Console
1. EC2 → Elastic Block Store → Volumes
2. Create Volume
   - Type: gp3
   - Size: 10 GB
   - AZ: mesma da sua EC2
3. Actions → Attach Volume
4. Selecione sua instância

### Via CLI
```bash
# Criar volume
aws ec2 create-volume \
  --volume-type gp3 \
  --size 10 \
  --availability-zone us-east-1a

# Anexar à instância
aws ec2 attach-volume \
  --volume-id vol-xxxxxxxxx \
  --instance-id i-xxxxxxxxx \
  --device /dev/sdf
```

### Formatar e Montar (dentro da EC2)
```bash
# Listar discos
lsblk

# Formatar
sudo mkfs -t ext4 /dev/xvdf

# Criar ponto de montagem
sudo mkdir /mnt/meu-volume

# Montar
sudo mount /dev/xvdf /mnt/meu-volume

# Montar automaticamente no boot (adicionar ao /etc/fstab)
echo "/dev/xvdf /mnt/meu-volume ext4 defaults,nofail 0 2" | sudo tee -a /etc/fstab
```

## 🗄️ 5. Configurando RDS

### Via Console
1. RDS → Create database
2. **Engine**: MySQL ou PostgreSQL
3. **Templates**: Free tier
4. **DB instance identifier**: meu-database
5. **Master username**: admin
6. **Master password**: [senha-forte]
7. **DB instance class**: db.t3.micro
8. **Storage**: 20 GB gp3
9. **Connectivity**:
   - VPC: default
   - Security group: criar novo ou usar existente
   - Public access: No (recomendado)
10. Create database

### Conectar da EC2 ao RDS
```bash
# Instalar cliente MySQL
sudo yum install mysql -y

# Conectar
mysql -h [RDS-ENDPOINT] -u admin -p
```

## 📦 6. Configurando S3 Bucket

### Via Console
1. S3 → Create bucket
2. **Bucket name**: meu-bucket-unico-12345
3. **Region**: us-east-1
4. **Block Public Access**: manter ativado
5. Create bucket

### Via CLI
```bash
# Criar bucket
aws s3 mb s3://meu-bucket-unico-12345

# Upload de arquivo
aws s3 cp arquivo.txt s3://meu-bucket-unico-12345/

# Listar conteúdo
aws s3 ls s3://meu-bucket-unico-12345/
```

## ⚡ 7. Criando Função Lambda

### Via Console
1. Lambda → Create function
2. **Author from scratch**
3. **Function name**: minhaFuncaoLambda
4. **Runtime**: Python 3.12
5. Create function

### Código Exemplo (Python)
```python
import json

def lambda_handler(event, context):
    print("Evento recebido:", json.dumps(event))
    
    # Processar evento do S3
    if 'Records' in event:
        for record in event['Records']:
            bucket = record['s3']['bucket']['name']
            key = record['s3']['object']['key']
            print(f"Arquivo {key} foi adicionado ao bucket {bucket}")
    
    return {
        'statusCode': 200,
        'body': json.dumps('Processado com sucesso!')
    }
```

### Configurar Trigger S3
1. Add trigger → S3
2. Bucket: selecione seu bucket
3. Event type: All object create events
4. Add

## 🎯 8. Configurando DynamoDB

### Via Console
1. DynamoDB → Create table
2. **Table name**: MinhaTabela
3. **Partition key**: id (String)
4. **Sort key** (opcional): timestamp (Number)
5. Create table

### Via CLI
```bash
aws dynamodb create-table \
  --table-name MinhaTabela \
  --attribute-definitions \
    AttributeName=id,AttributeType=S \
    AttributeName=timestamp,AttributeType=N \
  --key-schema \
    AttributeName=id,KeyType=HASH \
    AttributeName=timestamp,KeyType=RANGE \
  --billing-mode PAY_PER_REQUEST
```

## ✅ Checklist Final

- [ ] AWS CLI instalado e configurado
- [ ] Instância EC2 criada e acessível
- [ ] Volume EBS anexado e montado
- [ ] RDS configurado e testado
- [ ] S3 bucket criado
- [ ] Função Lambda operacional
- [ ] DynamoDB table criada
- [ ] Integrações testadas

## 🔍 Próximos Passos

1. Configure CloudWatch para monitoramento
2. Implemente backups automáticos
3. Configure Auto Scaling
4. Estude sobre VPCs e sub-redes
5. Explore IAM policies mais detalhadas

## 🆘 Problemas Comuns

### Não consigo conectar via SSH
- Verifique se o Security Group permite porta 22
- Confirme se está usando o IP público correto
- Verifique permissões do arquivo .pem (deve ser 400)

### Lambda não dispara com S3
- Verifique se a Lambda tem permissões para S3
- Confirme se o trigger está configurado corretamente
- Cheque os logs no CloudWatch

### Erro ao anexar EBS
- Instância e volume devem estar na mesma AZ
- Volume não pode estar anexado a outra instância

---

*Este guia cobre os fundamentos. Consulte a documentação oficial da AWS para configurações avançadas.*