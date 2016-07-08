autoscale: true
build-lists: true

# Roadie
---

# Objetivos

- Padronizar e automatizar os procedimentos DevOps da Coderockr
- Permitir que cada projeto tenha espaço para customizar mas sem sair do padrão
- Reusar as melhores ferramentas do mercado como Docker, AWS, doctl, etc
- Permitir que um novo ambiente de desenvolvimento, homolog ou produção seja criado em minutos
- Facilitar a identificação de erros de build, release ou execução
- Permitir que qualquer desenvolvedor possa fazer deploy para qualquer ambiente
- Reduzir custos de ambientes de teste e homologação
- Facilitar a criação e gerenciamento de projetos baseados nos conceitos dos *12factors*

---

# Conceitos

---

## Estágios

## Ambientes

---


# Estágios

### Install
### Build
### Release
### Run

----

## Install

Executado logo depois do clone do projeto

---

### Pre
Script que executa como primeiro passo do estágio de instalação.   
Recebe o ambiente como parâmetro e copia as configurações para o "local". Exemplos: instalação de dependências, criação de arquivos de configuração, criação de diretórios. 

### Pos
Script que executa como último passo do estágio de instalação, caso exista. Exemplos: criação de banco, carregamento de dados, alteração de permissões de arquivos, notificação (Slack) 

---

## Build

Executado para transformar o código em uma versão a ser executada ou enviada para release. Deve executar testes e falhar caso os mesmos falhem. Deve executar compilações, compactações, geração de imagens Docker. Recebe como parâmetro obrigatório o ambiente. O resultado deve ser armazenado em um diretório criado cujo nome é o timestamp da execução

---

### Pre
Script que executa como primeiro passo do estágio do build. Exemplos: instalação de dependências, testes unitários, compilação, compactações.

### Pos
Script que executa como último passo do estágio de build, caso exista. Exemplo: cria diretório do release, apaga os build antigos, notificação (Slack)

---

## Release

Executado para enviar o último build gerado, ou o build recebido como parâmetro, para um servidor. Deve receber o ambiente como parâmetro obrigatório e usar as configurações de release deste ambiente para executar o processo

---

### Pre
Script que executa como primeiro passo do estágio do release. Exemplos: mudar configurações de roteamento para não parar o serviço, alterar load balancer, fazer lock de arquivos ou bancos de dados, mudar páginas de status (em manutenção). 

### Pos
Script que executa como último passo do estágio de release, caso exista. Exemplo: reinicialização de serviços, limpeza de cache, configuração de crontab, envio de notificação (Slack)

---

## Run

Executado para realizar a execução de um build em um ambiente, devendo receber ambos como parâmetro. 

---

### Pre

Script que executa como primeiro passo do estágio de execução. Exemplos: migrations, "esquentar" cache, executar fixtures

#### Pos

Script que executa como último passo do estágio de execução, caso exista. Exemplos: parar serviços, limpar caches e arquivos temporários, remover ambientes criados para execução (remover máquinas na Amazon por exemplo)

---

# Ambientes

---

# Dev

### Envs

Variáveis de ambiente como endereços de servidores, senhas, urls. 

### Dependencies

Dependências especiais do ambiente. Exemplo: phpunit, jscs, phpcs. 

---

## Homolog

### Envs
### Dependencies

---

## Production

### Envs
### Dependencies

---

## Local

Os arquivos "locais" vão ser uma cópia (ou link simbólico) de um dos ambientes anteriores (dev, prod, homolog) e não devem ser salvos no repositório. Os arquivos dos demais ambientes servem como templates e que devem ser copiados/linkados para os locais, assim cada ambiente vai sempre executar o local e nas fases anteriores (install, release, run) um dos ambientes vai ser copiado para o local. 

---

# Exemplos

---

```
cd project
roadie install dev
```

```
roadie build homolog
```

```
roadie release homolog
```

```
roadie run homolog
```

```
roadie run production
```

---


# Dúvidas

---

## Linguagem usada

- Shell script?
- PHP?
- Python?
- Go?

---

## Formato das configurações

- JSON?
- YML?
- Shell script?
- Arrays PHP/Python/etc?

---


## Quantia de scripts por estágio

- Apenas um script por estágio (pre/pos install/release/build/run)?
- Diretório com scripts para cada estágio

---

## Instalação

- Um repositório que seria clonado?
- Um script que seria executado e criaria as pastas?
- Pacote para distro (apt-get, yum, brew, etc)?
- Uma imagem Docker com o ambiente de automatização?
