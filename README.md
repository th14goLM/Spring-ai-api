# 💰 Spring AI Budgeting API

> Projeto final do módulo Spring AI — DIO Spring Boot Track
> Integração de inteligência artificial em uma API de controle financeiro, respeitando os limites arquiteturais de domínio e casos de uso.

---

## 📌 Visão Geral

Esta API processa **comandos de voz** para criação e consulta de transações financeiras.
A IA atua como orquestradora do fluxo, sem violar as fronteiras entre camadas — domínio, aplicação e infraestrutura permanecem desacoplados.

---

## 🔄 Fluxo Principal

```
Cliente → [Áudio] → Transcrição (STT) → Seleção de Tool → Caso de Uso → [Resposta em Áudio (TTS)]
```

1. O cliente envia um arquivo de áudio
2. O áudio é transcrito para texto via `TranscriptionModel`
3. O modelo seleciona o tool/caso de uso adequado via function calling
4. O caso de uso persiste ou consulta dados de transações
5. A resposta textual é convertida para MP3 via `TextToSpeechModel`

---

## 🏗️ Arquitetura em Camadas

```
src/main/java/dio/budgeting/
├── domain/           # Modelo de domínio e contratos de repositório
├── application/      # Casos de uso (compartilhados entre REST e AI tools)
└── infrastructure/   # Adapters HTTP, JPA e integração com Spring AI
```

A estrutura segue os princípios de **Clean Architecture** e **DDD**, garantindo que os casos de uso permaneçam agnósticos ao mecanismo de entrega (seja REST ou IA).

---

## 🤖 Módulos Spring AI

### 🎙️ Speech-to-Text
- Utiliza `TranscriptionModel` para transcrição de áudio
- Configurado via `application.properties`

### 🔧 Tool Calling
- `ChatClient` registra os casos de uso como tools da IA
- Métodos anotados com `@Tool` expõem capacidades de negócio ao modelo

### 🔊 Text-to-Speech
- `TextToSpeechModel` gera saída em MP3 a partir do texto final
- O endpoint de IA retorna o áudio gerado diretamente ao cliente

---

## ⚙️ Como Executar

**Pré-requisito:** configure sua chave da OpenAI

```bash
export OPENAI_API_KEY="sua_chave_aqui"
```

**Executar a aplicação:**

```bash
./gradlew bootRun
```

**Executar os testes:**

```bash
./gradlew test
```

> ⚠️ Testes de integração com provedores externos podem exigir credenciais ativas.

---

## 📚 Referências — Spring AI

| Recurso | Link |
|---|---|
| Documentação Geral | [Spring AI Reference](https://docs.spring.io/spring-ai/reference/index.html) |
| ChatModel API | [ChatModel](https://docs.spring.io/spring-ai/reference/api/chatmodel.html) |
| ChatClient API | [ChatClient](https://docs.spring.io/spring-ai/reference/api/chatclient.html) |
| Tools API | [Tools](https://docs.spring.io/spring-ai/reference/api/tools.html) |
| Transcrição de Áudio | [Transcriptions](https://docs.spring.io/spring-ai/reference/api/audio/transcriptions.html) |
| Síntese de Voz | [Speech](https://docs.spring.io/spring-ai/reference/api/audio/speech.html) |

---

## 🔗 Referências de Arquitetura Compartilhada

Conceitos transversais à trilha estão documentados no README raiz:

- [Camadas DDD](../README.md#ddd-layered-architecture)
- [Class vs Record no domínio](../README.md#java-class-vs-java-record-in-domain-modeling)
- [Identificadores fortemente tipados](../README.md#strong-typed-identifiers)
- [Repository Pattern](../README.md#repository-pattern)
- [Casos de Uso e Clean Architecture](../README.md#use-cases-and-clean-architecture)
- [Suporte a Docker Compose](../README.md#docker-compose-support-in-development)

---

## 🎓 Observações

- Projeto educacional focado na disciplina arquitetural com IA
- A IA é integrada como **mecanismo de entrega**, não como substituta das regras de negócio
- O modelo não acessa o domínio diretamente — toda interação passa pelos casos de uso
