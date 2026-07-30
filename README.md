
# Automação de Infraestrutura com AWS CloudFormation

Projeto prático desenvolvido durante o programa **AWS re:Start** (Escola da Nuvem), com o objetivo de aplicar conceitos de **Infraestrutura como Código (IaC)** utilizando o AWS CloudFormation para implantar, atualizar e remover recursos de forma automatizada e reprodutível.

## Contexto

Implantar infraestrutura manualmente é um processo sujeito a erro humano, difícil de padronizar e de repetir com segurança — principalmente fora do horário comercial ou em ambientes com múltiplas equipes. O CloudFormation resolve isso permitindo definir toda a infraestrutura em um modelo declarativo (YAML ou JSON), que pode ser versionado, revisado e implantado de forma automática e consistente.

## Objetivo

Ganhar experiência prática na criação e edição de *stacks* do CloudFormation, evoluindo um modelo do zero até uma arquitetura funcional com rede, armazenamento e computação.

## Arquitetura implantada

- **VPC** dedicada com bloco CIDR customizável via parâmetro
- **Security Group** de aplicação, controlando o tráfego permitido
- **Subnet pública**
- **Bucket Amazon S3**
- **Instância Amazon EC2**, provisionada com a AMI mais recente do Amazon Linux 2, associada ao Security Group e à subnet pública

## O que foi feito

### 1. Implantação da stack inicial (rede)
- Deploy de um modelo CloudFormation (`task1.yaml`) definindo a VPC e o Security Group da aplicação.
- Uso da seção `Parameters` para tornar os blocos CIDR configuráveis no momento da implantação, evitando valores fixos ("hardcoded") no modelo.
- Análise da seção `Outputs`, usada para expor de forma seletiva informações da stack (neste caso, o Security Group padrão da VPC).

### 2. Adição de um bucket S3 via atualização de stack
- Edição do modelo para incluir um recurso `AWS::S3::Bucket`.
- Atualização da stack existente (*Update Stack*) com o modelo revisado, sem necessidade de recriar os recursos já existentes.
- Validação do *changeset* (prévia de alterações) antes de aplicar — prática recomendada para evitar mudanças destrutivas não intencionais em produção.

### 3. Adição de uma instância EC2 com dependências entre recursos
- Uso do **AWS Systems Manager Parameter Store** para resolver dinamicamente o ID da AMI mais recente do Amazon Linux 2 (`AWS::SSM::Parameter::Value<AWS::EC2::Image::Id>`), eliminando a necessidade de atualizar manualmente o ID da AMI a cada nova versão ou região.
- Uso de referências intrínsecas (`!Ref`) para conectar a instância EC2 ao Security Group e à Subnet definidos em outras partes do mesmo modelo — demonstrando como o CloudFormation resolve dependências entre recursos automaticamente, determinando a ordem correta de criação.
- Definição de tags para identificação do recurso (`Name: App Server`).

### 4. Remoção controlada da infraestrutura
- Exclusão da stack via CloudFormation, validando que todos os recursos associados (VPC, Security Group, bucket S3, instância EC2) foram removidos automaticamente e na ordem correta — sem necessidade de exclusão manual recurso por recurso.

## Trecho de modelo — referência entre recursos
<img width="1850" height="942" alt="Captura de tela 2026-07-29 111556" src="https://github.com/user-attachments/assets/de0fe3c2-6da0-4a66-a96b-3e3d6a119ca8" />
<img width="1258" height="667" alt="Captura de tela 2026-07-29 112146" src="https://github.com/user-attachments/assets/1d195b5b-ff41-4695-9bc9-f17a19fc46ac" />
<img width="1502" height="312" alt="Captura de tela 2026-07-29 113345" src="https://github.com/user-attachments/assets/5932f4a3-97dd-4681-af41-0b2ac2440b15" />
<img width="1478" height="247" alt="Captura de tela 2026-07-29 125645" src="https://github.com/user-attachments/assets/ddcee172-07f8-48ed-8356-7c62bd7cadfc" />
<img width="1437" height="441" alt="Captura de tela 2026-07-29 130149" src="https://github.com/user-attachments/assets/6d5d1afb-d1e9-4a05-8e0d-0cf0ac253619" />






## Conceitos aplicados
- Infraestrutura como Código (IaC)
- Modelos declarativos em YAML
- Parametrização de modelos (`Parameters`)
- Resolução de dependências entre recursos (`!Ref`)
- Integração com AWS Systems Manager Parameter Store
- Atualização incremental de stacks (*Update Stack* / *Change Sets*)
- Ciclo de vida completo de infraestrutura: criação, atualização e remoção
## Tecnologias

`AWS CloudFormation` · `YAML` · `Amazon EC2` · `Amazon S3` · `Amazon VPC` · `AWS Systems Manager Parameter Store`
