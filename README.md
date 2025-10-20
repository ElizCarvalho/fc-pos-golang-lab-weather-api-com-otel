# 🌤️ Weather API com OpenTelemetry

Sistema de microserviços em Go que recebe um CEP e retorna o clima atual com observabilidade completa.

## 🏗️ Arquitetura

```mermaid
Cliente → [Service A:8080] → [Service B:8081] → [ViaCEP + WeatherAPI]
         ↓                   ↓
    [Zipkin:9411] ← [OTEL Collector:4318]
```

- **Service A**: Gateway (validação + roteamento)
- **Service B**: Processador (ViaCEP + WeatherAPI)
- **Observabilidade**: OpenTelemetry + Zipkin

## 🚀 Como Executar

### 1. Configurar

```bash
make setup
# Editar service-b/.env e adicionar WEATHER_API_KEY
```

### 2. Rodar

```bash
# Docker (recomendado)
make docker-up
```

### 3. Testar

```bash
curl -X POST http://localhost:8080/weather \
  -H "Content-Type: application/json" \
  -d '{"cep":"26140040"}'
```

## 📚 API

**POST /weather**

```json
{
  "cep": "26140040"
}
```

**Resposta:**

```json
{
  "city": "Belford Roxo",
  "temp_C": 28.5,
  "temp_F": 83.3,
  "temp_K": 301.5
}
```

## 🔍 Observabilidade

- **Zipkin**: <http://localhost:9411>
- **Traces**: Visualizar fluxo entre serviços
- **Spans**: service-a.handle-request → service-b.fetch-weather

## 🧪 Testes

```bash
make test
```

## 📁 Estrutura

```bash
├── .docker/               # Configuração OTEL
├── pkg/otel/              # OpenTelemetry compartilhado
├── service-a/             # Gateway
├── service-b/             # Processador
└── docker-compose.yml     # Stack completa
```

## 🚨 Troubleshooting

- **WEATHER_API_KEY**: Configure em `service-b/.env`
- **CEP inválido**: Use 8 dígitos (ex: 26140040)
- **Zipkin**: Verifique <http://localhost:9411>
