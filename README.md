# Chatbot Webhook Integration (Botpress + n8n)

![Botpress](https://img.shields.io/badge/Botpress-Chatbot-blue)
![n8n](https://img.shields.io/badge/n8n-Automation-orange)
![Docker](https://img.shields.io/badge/Docker-Container-blue)
![Webhooks](https://img.shields.io/badge/Webhooks-HTTP-green)

## Descrição Sobre o Projeto

Projeto de integração entre um chatbot no Botpress e um workflow no n8n, com comunicação via Webhook HTTP para roteamento de atendimentos por departamento, processamento no backend e retorno de resposta em JSON.

## Proposta de Valor

O projeto demonstra uma arquitetura desacoplada em que o Botpress atua como camada de interface conversacional e o n8n como camada de automação. Essa separação facilita manutenção, evolução de fluxos e reutilização da lógica de atendimento sem acoplamento com a interface de chat.

## Arquitetura

Componentes principais:

- Botpress: captura a intenção do usuário e envia a solicitação para o backend.
- n8n: recebe o payload no endpoint de Webhook, aplica roteamento por regra e retorna a resposta.
- Webhook HTTP: canal de comunicação entre chatbot e automação.
- Docker: execução do n8n em ambiente isolado e reproduzível.

Fluxo arquitetural:

```text
Usuário -> Botpress -> POST /webhook/chatbot -> n8n (Switch) -> Respond to Webhook -> Botpress -> Usuário
```

## Funcionalidades

### Botpress

- Interface conversacional com menu de atendimento.
- Coleta da opção de departamento selecionada pelo usuário.
- Chamada HTTP para o endpoint do n8n com payload JSON.

### n8n

- Recebimento de requisições POST no endpoint `chatbot`.
- Roteamento com Switch Node com base no campo `body.message`.
- Retorno de respostas JSON específicas para Suporte Técnico, Financeiro e Vendas.

### Integracao

- Comunicação sincronizada via Webhook HTTP.
- Contrato simples de dados com payload de entrada e resposta em JSON.
- Separação de responsabilidades entre experiência conversacional e processamento de regra.

## Fluxo da Aplicação

1. O usuário inicia a conversa no Botpress.
2. O chatbot apresenta as opções de atendimento.
3. O usuário escolhe um departamento.
4. O Botpress envia um POST para o Webhook do n8n com o valor escolhido.
5. O n8n avalia o campo `message` no Switch Node.
6. O fluxo direciona para o ramo correspondente (Suporte Técnico, Financeiro ou Vendas).
7. O n8n retorna uma resposta JSON ao Botpress.
8. O chatbot exibe a mensagem retornada ao usuário.

## Exemplo de Payload (JSON)

```json
{
  "message": "Suporte Técnico"
}
```

## Exemplo de Resposta (JSON)

```json
{
  "response": "Vou te encaminhar para o suporte técnico. Pode descrever o problema?"
}
```

## Stack Tecnológica

- Botpress Studio
- n8n
- Webhooks HTTP
- Docker
- JSON

## Estrutura do Projeto

```text
.
|-- botpress/
|   `-- conecta-sul-chatbot.bpz
|-- docs/
|   |-- arquiteturabotpress.png
|   |-- arquiteturabotpress2.png
|   |-- automacaon8n.png
|   |-- n8n-workflow.png
|   `-- testechatbot.png
|-- n8n/
|   `-- workflow.json
|-- docker-compose.yml
|-- LICENSE
`-- README.md
```

## Instalação

Pré-requisitos:

- Docker
- Docker Compose
- Botpress Studio
- Git

Passo a passo:

1. Clonar o repositório:

```bash
git clone https://github.com/NatanLuz/chatbot-webhook-integration-botpress-n8n.git
cd chatbot-webhook-integration-botpress-n8n
```

2. Subir o n8n com Docker Compose:

```bash
docker compose up -d
```

3. Acessar o n8n:

```text
http://localhost:5678
```

4. Importar o workflow no n8n:

- Menu de importação do n8n.
- Selecionar o arquivo `n8n/workflow.json`.

5. Importar o bot no Botpress:

- Opção de importação de bot no Botpress Studio.
- Selecionar o arquivo `botpress/conecta-sul-chatbot.bpz`.

6. Configurar no Botpress a URL do Webhook para o endpoint do n8n ativo.

## Testes

Checklist de validação manual:

1. Iniciar conversa no Botpress.
2. Selecionar cada opção de departamento (Suporte Técnico, Financeiro e Vendas).
3. Verificar no n8n se o Webhook recebe o payload com `message`.
4. Confirmar que o Switch Node direciona para o ramo correto.
5. Validar o retorno JSON e a exibição da resposta no chatbot.

## Conceitos Demonstrados

- Integração entre sistemas via Webhooks HTTP.
- Arquitetura desacoplada entre interface e backend de automação.
- Roteamento de fluxo por regra de negócio no n8n.
- Contrato de comunicação simples baseado em JSON.
- Execução de serviço em container Docker.

## Autor

**Natan Da Luz**

Desenvolvedor Backend

[LinkedIn](https://www.linkedin.com/in/natandaluzdesenvolvedor/)
