#!/bin/bash

cat > README.md << 'EOF'
# **💻 Arquitecto Cloud AWS | Especialista en Terraform** ☁️

<p align="center">
  <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/terraform/terraform-original.svg" width="50"/>
  <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/amazonwebservices/amazonwebservices-original.svg" width="50"/>
  <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/docker/docker-original.svg" width="50"/>
  <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/kubernetes/kubernetes-plain.svg" width="50"/>
  <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/ansible/ansible-original.svg" width="50"/>
</p>

---

## **🚀 Sobre Mí**  
Soy **Arquitecto de Soluciones AWS** con más de **5 años de experiencia** diseñando e implementando infraestructuras escalables, altamente disponibles y seguras en la nube.

🔹 **Especializado en:**  
✔ **Infraestructura como Código (IaC)** con Terraform y AWS CDK  
✔ **Automatización** con AWS Lambda, Step Functions y EventBridge  
✔ **Arquitecturas Serverless** (API Gateway, DynamoDB, Lambda)  
✔ **Contenedores** (ECS, EKS, Fargate)  
✔ **DevOps** (CI/CD con CodePipeline, GitHub Actions, Jenkins)  
✔ **Seguridad en la nube** (IAM, KMS, Security Hub, GuardDuty)  

---

## **🛠️ Tecnologías Principales**  

### **🔧 Servicios AWS**  
| **Categoría**       | **Tecnologías**                                                                 |
|---------------------|---------------------------------------------------------------------------------|
| **Compute**         | EC2, Lambda, ECS, EKS, Fargate, Batch                                          |
| **Almacenamiento**  | S3, EBS, EFS, Glacier, FSx                                                     |
| **Bases de Datos**  | RDS (PostgreSQL/MySQL), DynamoDB, Aurora, ElastiCache, Redshift                |
| **Redes**           | VPC, ALB/NLB, Route 53, CloudFront, Direct Connect, Transit Gateway            |
| **Seguridad**       | IAM, KMS, Secrets Manager, WAF, Shield, GuardDuty                              |
| **DevOps**          | CodePipeline, CodeBuild, CodeDeploy, CloudFormation, Systems Manager           |

### **⚙️ Herramientas de Automatización**  
- **Terraform** (Gestión de infraestructura multicloud)  
- **Ansible** (Gestión de configuración de servidores)  
- **Docker & Kubernetes** (Orquestación de contenedores)  
- **Prometheus & Grafana** (Monitoreo y alertas)  

---

## **📌 Ejemplos de Infraestructura como Código**  

### **🌐 VPC + EC2 + RDS (Alta Disponibilidad)**  
```hcl
module "vpc" {
  source  = "terraform-aws-modules/vpc/aws"
  version = "3.14.0"

  name = "prod-vpc"
  cidr = "10.0.0.0/16"

  azs             = ["us-east-1a", "us-east-1b"]
  private_subnets = ["10.0.1.0/24", "10.0.2.0/24"]
  public_subnets  = ["10.0.101.0/24", "10.0.102.0/24"]

  enable_nat_gateway = true
  single_nat_gateway = true
}

resource "aws_instance" "servidor_web" {
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = "t2.micro"
  subnet_id     = module.vpc.public_subnets[0]

  vpc_security_group_ids = [aws_security_group.web_sg.id]

  tags = {
    Name = "ServidorWeb-Prod"
  }
}

resource "aws_db_instance" "postgres" {
  allocated_storage    = 20
  engine               = "postgres"
  instance_class       = "db.t3.micro"
  db_name              = "bd_prod"
  username             = "admin"
  password             = var.db_password
  multi_az             = true
  vpc_security_group_ids = [aws_security_group.db_sg.id]
  db_subnet_group_name = aws_db_subnet_group.default.name
  skip_final_snapshot  = true
}
