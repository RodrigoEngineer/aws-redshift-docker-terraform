# Projeto: Data Warehouse na AWS Redshift com Docker e Terraform

## Visão Geral
Este projeto implementa um Data Warehouse utilizando a AWS Redshift. A infraestrutura é provisionada com Terraform, enquanto o ambiente é gerenciado com Docker. O objetivo é demonstrar como criar um pipeline básico de DW, utilizando dados fictícios armazenados localmente na pasta `dados/`.

## Estrutura do Repositório
```
.
├── Dockerfile          # Configuração do ambiente Docker
├── dados/              # Dados fictícios para cargas no DW
│   ├── dim_cliente.csv      # Dimensão Cliente
│   ├── dim_localidade.csv   # Dimensão Localidade
│   ├── dim_produto.csv      # Dimensão Produto
│   ├── dim_tempo.csv        # Dimensão Tempo
│   └── fato_vendas.csv      # Fato Vendas
├── IaC/                # Infraestrutura como Código
│   ├── etapa1/         # Configuração inicial de infraestrutura
│   │   ├── provider.tf         # Configuração do provedor Terraform (AWS)
│   │   ├── redshift.tf         # Provisionamento do cluster Redshift
│   │   └── redshift_role.tf    # Configuração de permissões IAM
│   └── etapa2/         # Configuração do banco de dados
│       └── modelo_fisico.sql  # Criação do modelo físico no Redshift
```

## Funcionalidades
- **Infraestrutura como Código (IaC):** Provisiona cluster Redshift, IAM Roles e outras dependências com Terraform.
- **Modelo de Dados Relacional:** Script SQL define tabelas e relacionamentos para um cenário de vendas.
- **Portabilidade:** Docker garante consistência no ambiente de desenvolvimento.

## Pré-requisitos
- **Docker**
- **Terraform**
- **AWS CLI** configurado com credenciais válidas

## Passo a Passo

### 1. Configurar o Ambiente Docker
1. Clone o repositório:
   ```bash
   git clone https://github.com/seu-usuario/seu-repositorio.git
   cd seu-repositorio
   ```
2. Construa o container Docker:
   ```bash
   docker build -t redshift-dw .
   ```

### 2. Provisionar Infraestrutura com Terraform
1. Navegue até o diretório `IaC/etapa1/`.
   ```bash
   cd IaC/etapa1
   ```
2. Inicialize o Terraform:
   ```bash
   terraform init
   ```
3. Provisione os recursos na AWS:
   ```bash
   terraform apply
   ```
   Confirme quando solicitado.

### 3. Configurar o Banco de Dados
1. Conecte-se ao cluster Redshift provisionado.
2. Execute o script SQL do diretório `IaC/etapa2/`:
   ```bash
   psql -h <redshift-endpoint> -U <usuario> -d <database> -f modelo_fisico.sql
   ```

### 4. Carregar Dados no Redshift
1. Utilize a ferramenta `COPY` do Redshift para importar os arquivos da pasta `dados/`.
   Exemplo:
   ```sql
   COPY dim_cliente FROM 's3://<bucket>/dim_cliente.csv' IAM_ROLE '<arn-role>' CSV;
   ```

## Melhorias Futuras
- Automação completa da carga de dados com pipelines (ex: Apache Airflow).
- Implementação de camadas adicionais no modelo de dados.
- Monitoramento e auditoria com logs detalhados.

## Contribuição
Contribuições são bem-vindas! Siga os passos abaixo:
1. Faça um fork deste repositório.
2. Crie uma branch para sua feature ou correção.
3. Submeta um pull request com a descrição detalhada.

## Licença
Este projeto está licenciado sob a [MIT License](LICENSE).
