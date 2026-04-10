# DevOpsAT – AT DevOps

## Descrição do Projeto

Este repositório contém o desenvolvimento do Assessment Final da disciplina de DevOps. O objetivo do projeto é consolidar os conhecimentos adquiridos ao longo da disciplina por meio da implementação de uma aplicação completa utilizando práticas de integração contínua (CI), entrega contínua (CD), conteinerização e orquestração com Kubernetes.

A solução desenvolvida simula uma plataforma chamada *Ufology Investigation Unit*, responsável por gerenciar dados de avistamentos, utilizando uma arquitetura composta por aplicação, banco de dados PostgreSQL e sistema de cache Redis.

---

## Tecnologias Utilizadas

- Java (Spring Boot)
- Maven
- Docker
- Kubernetes
- Git e GitHub
- GitHub Actions
- Redis
- PostgreSQL

---

## Estrutura do Projeto

- `k8s/` – Manifestos Kubernetes (Deployments, Services, ConfigMap, Secret)
- `.github/workflows/` – Pipelines de CI/CD com GitHub Actions
- `Dockerfile` – Definição da imagem da aplicação
- `README.md` – Documentação do projeto

---

## Controle de Versão com Git

O Git foi utilizado como ferramenta de controle de versão ao longo de todo o desenvolvimento do projeto, permitindo o rastreamento das alterações realizadas no código, bem como a organização do fluxo de trabalho.

No contexto de entrega contínua, o Git desempenha um papel fundamental ao integrar-se com ferramentas como o GitHub Actions, permitindo que cada alteração enviada ao repositório seja automaticamente validada por meio de pipelines.

A utilização de branches possibilita o desenvolvimento paralelo de funcionalidades sem impactar diretamente a versão principal do projeto. Já as tags são utilizadas para marcar versões estáveis da aplicação, facilitando o versionamento e a criação de releases.

---

## Pipeline de Integração Contínua

Foram configurados diversos workflows no GitHub Actions para automatizar tarefas do projeto:

### hello.yml
Executado a cada push, exibe uma mensagem simples no log para validar a execução do pipeline.

### tests.yml
Disparado em eventos de pull request, realiza o checkout do código e executa um comando simulado de testes.

### gradle-ci.yml
Executado em pushes para a branch principal, realiza o build da aplicação utilizando Maven. Os testes foram desabilitados devido à dependência de serviços externos (PostgreSQL e Redis).

### env-demo.yml
Demonstra o uso de variáveis de ambiente dentro do pipeline.

### secret-demo.yml
Utiliza um secret configurado no repositório, garantindo que informações sensíveis não sejam expostas.

### run-monitor.yml
Workflow mais completo que inclui:
- Validação de variáveis
- Uso de secrets
- Geração de mensagens de erro e aviso
- Upload de artefatos
- Geração de resumo da execução

---

## Dockerização da Aplicação

A aplicação foi preparada para execução em container por meio da criação de um Dockerfile. A imagem gerada foi publicada em um repositório público no Docker Hub.

### Comandos utilizados:

```bash
docker build -t rodrigomorof/ufo-tracker .
docker push rodrigomorof/ufo-tracker
```
## Implantação no Kubernetes

Todos os recursos foram implantados dentro do namespace ufology, garantindo isolamento no cluster.

### Componentes implantados:
- PostgreSQL (Deployment + Service)
- Redis (Deployment + Service)
- Aplicação (Deployment com 2 réplicas + Service)
- ConfigMap (configurações não sensíveis)
- Secret (dados sensíveis)

A aplicação foi configurada para se conectar aos serviços internos do cluster por meio de variáveis de ambiente, respeitando boas práticas de segurança.

## Segurança

As informações sensíveis, como senha do banco de dados, foram armazenadas em um Secret do Kubernetes. Já as configurações não sensíveis foram armazenadas em um ConfigMap.

No GitHub Actions, foi utilizado o recurso de secrets para proteger dados críticos durante a execução dos pipelines.

## Runners do GitHub Actions

Os runners hospedados pelo GitHub foram utilizados para execução dos workflows, pois oferecem um ambiente pronto e de fácil configuração.

Já os runners auto-hospedados permitem maior controle sobre o ambiente de execução, podendo ser personalizados conforme a necessidade do projeto, porém exigem manutenção e configuração adicional.
