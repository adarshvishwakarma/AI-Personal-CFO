# 🚀 AI Personal CFO - Complete Microservices Project

**Your AI-Powered Personal Finance Assistant for the Middle Class**

A production-ready Spring Boot microservices architecture that automates financial management with AI/ML capabilities.

---

## 📋 Table of Contents

1. [Project Overview](#project-overview)
2. [Key Features](#key-features)
3. [Architecture](#architecture)
4. [Tech Stack](#tech-stack)
5. [Quick Start](#quick-start)
6. [Project Structure](#project-structure)
7. [API Endpoints](#api-endpoints)
8. [Development Guide](#development-guide)
9. [Deployment](#deployment)
10. [Contributing](#contributing)

---

## 🎯 Project Overview

**AI Personal CFO** is an intelligent financial assistant designed for middle-class users in India and other emerging markets. It leverages advanced AI/ML models to provide real-time financial insights, automated expense tracking, spending predictions, and personalized investment recommendations.

### Business Model
- **Free Tier**: Basic expense tracking, category-wise budgets
- **Premium Tier** ($5-10/month): Advanced analytics, forecasting, investment recommendations
- **Enterprise Tier**: B2B partnerships with fintech platforms, white-label solutions

### Market Opportunity
- **TAM**: $2B+ (Financial wellness software market)
- **SAM**: $500M (Middle-class personal finance segment)
- **SOM**: $50M (Achievable in 3-5 years)

---

## ✨ Key Features

### 1. **Intelligent Expense Tracking**
- Bank account integration (via Plaid API)
- Receipt scanning with OCR
- Auto-categorization using NLP
- Duplicate detection
- Manual transaction entry

### 2. **Spending Forecasting**
- ARIMA-based time series forecasting
- Category-wise predictions
- Seasonal trend analysis
- 30/60/90-day forecasts with confidence intervals

### 3. **Smart Budget Management**
- Multi-level budgets (monthly, quarterly, yearly)
- Budget vs actual comparison
- Real-time budget alerts (80% threshold)
- Category-wise budget optimization

### 4. **Anomaly Detection**
- Unusual spending alerts
- Isolation Forest ML model
- Context-aware anomaly scoring
- Peer comparison analytics

### 5. **AI-Powered Insights**
- Spending pattern analysis
- Monthly financial reports
- Cost reduction recommendations
- Cash flow optimization tips

### 6. **Investment Recommendations**
- Risk-adjusted portfolio suggestions
- SIP calculator
- Asset allocation recommendations
- Investment goal tracking

### 7. **Real-time Notifications**
- Multi-channel alerts (Push, Email, SMS)
- Customizable notification preferences
- Goal milestone celebrations
- Investment opportunity alerts

---

## 🏗️ Architecture

### Microservices Pattern

```
┌─────────────────────────────────────────┐
│        Client Applications               │
│  (Mobile, Web, Smart Assistants)        │
└─────────────────┬───────────────────────┘
                  │
         ┌────────▼────────┐
         │  API Gateway    │
         │ (Rate Limit,    │
         │  Auth, Routing) │
         └────────┬────────┘
                  │
    ┌─────────────┼─────────────┐
    │             │             │
    ▼             ▼             ▼
 User       Transaction      Budget
Service      Service        Service
    │             │             │
    └─────────────┼─────────────┘
                  │
         ┌────────▼────────┐
         │  Kafka Bus      │
         │ (Event Stream)  │
         └────────┬────────┘
                  │
    ┌─────────────┼──────────────────┐
    │             │                  │
    ▼             ▼                  ▼
Analytics    Investment        Notification
Service      Service           Service
    │             │                  │
    └─────────────┼──────────────────┘
                  │
    ┌─────────────▼───────────┐
    │   Redis Cache Layer     │
    │  (Predictions, Models)  │
    └─────────────┬───────────┘
                  │
    ┌─────────────▼───────────┐
    │   Data Layer            │
    │ (PostgreSQL, Elastic)   │
    └─────────────────────────┘
                  │
    ┌─────────────▼───────────┐
    │   ML Services (Python)  │
    │  • Categorization       │
    │  • Forecasting          │
    │  • Anomaly Detection    │
    └─────────────────────────┘
```

### Data Flow

```
User Transactions
    ↓
Transaction Service (Validation, Categorization)
    ↓
Kafka Event (transaction.created)
    ↓
┌─────────────────────────────────┐
│ Multiple Consumers:             │
│ • Budget Service (Alert Check)  │
│ • Analytics Service (Anomaly)   │
│ • Investment Service (Goals)    │
└─────────────────────────────────┘
    ↓
Redis Cache (Updated Predictions)
    ↓
User Dashboard + Notifications
```

---

## 💻 Tech Stack

### Backend
- **Framework**: Spring Boot 3.2.0
- **Language**: Java 21
- **Build**: Maven 3.8+
- **Reactive**: Project Reactor (Async)

### Data
- **Database**: PostgreSQL 15 (OLTP)
- **Cache**: Redis 7.0 (L2 Cache)
- **Search**: Elasticsearch 8.5
- **Time-Series** (optional): InfluxDB 2.7

### Message Queue
- **Broker**: Apache Kafka 3.5.0
- **Schema Registry**: Confluent Schema Registry

### ML/AI
- **Framework**: Python 3.10+
- **APIs**: FastAPI, Flask
- **ML Libraries**: scikit-learn, TensorFlow, Prophet, ARIMA
- **NLP**: HuggingFace Transformers

### DevOps
- **Containerization**: Docker
- **Orchestration**: Kubernetes 1.28+
- **Cloud**: AWS (EKS, RDS, ElastiCache)
- **CI/CD**: GitHub Actions

### Monitoring
- **Metrics**: Prometheus + Micrometer
- **Visualization**: Grafana
- **Tracing**: Jaeger + Spring Cloud Sleuth
- **Logs**: ELK Stack (Elasticsearch, Logstash, Kibana)

---

## 🚀 Quick Start

### Prerequisites

```bash
# System Requirements
- Java 21+
- Maven 3.8+
- Docker & Docker Compose
- Python 3.10+
- Git

# Verify
java -version    # Should be 21+
mvn -version     # Should be 3.8+
docker --version
```

### 1. Clone Repository

```bash
git clone https://github.com/yourorg/ai-cfo.git
cd ai-cfo
```

### 2. Start Infrastructure

```bash
# Create environment file
cp .env.example .env

# Edit with your credentials
nano .env

# Start all services
docker-compose up -d

# Wait for services to be healthy
docker-compose ps
sleep 30

# Verify connectivity
docker-compose logs postgres
```

### 3. Build Services

```bash
# Build all modules
mvn clean package -DskipTests

# Or build specific service
cd user-service && mvn clean package -DskipTests
```

### 4. Run Services (Development)

```bash
# Terminal 1 - User Service
cd user-service
mvn spring-boot:run

# Terminal 2 - Transaction Service
cd transaction-service
mvn spring-boot:run

# Terminal 3 - Budget Service
cd budget-service
mvn spring-boot:run

# Terminal 4 - Analytics Service
cd analytics-service
mvn spring-boot:run

# Terminal 5 - Notification Service
cd notification-service
mvn spring-boot:run
```

### 5. Access Services

```
API Gateway:        http://localhost:8080
Swagger UI:         http://localhost:8080/swagger-ui.html
PostgreSQL:         localhost:5432 (user: postgres)
Redis:              localhost:6379
Kafka:              localhost:9092
Zookeeper:          localhost:2181
Elasticsearch:      http://localhost:9200
Prometheus:         http://localhost:9090
Grafana:            http://localhost:3000
Jaeger:             http://localhost:16686
```

### 6. Test API

```bash
# Register user
curl -X POST http://localhost:8080/api/users/register \
  -H "Content-Type: application/json" \
  -d '{
    "email": "user@example.com",
    "password": "SecurePass123!",
    "fullName": "John Doe",
    "phoneNumber": "+91-9876543210"
  }'

# Create transaction
curl -X POST http://localhost:8080/api/transactions \
  -H "Authorization: Bearer YOUR_JWT_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "amount": 500.00,
    "merchantName": "Starbucks",
    "description": "Coffee",
    "transactionDate": "2024-03-29T10:30:00",
    "paymentMethod": "credit_card"
  }'
```

---

## 📁 Project Structure

```
ai-cfo/
├── api-gateway/                   # Spring Cloud Gateway
│   ├── src/main/java/...
│   ├── pom.xml
│   └── application.yml
│
├── user-service/                  # User Management & Auth
│   ├── src/main/java/...
│   │   ├── controller/
│   │   ├── service/
│   │   ├── entity/
│   │   ├── security/
│   │   └── exception/
│   ├── pom.xml
│   └── application.yml
│
├── transaction-service/           # Expense Tracking & Categorization
│   ├── src/main/java/...
│   │   ├── controller/
│   │   ├── service/
│   │   ├── entity/
│   │   ├── kafka/
│   │   ├── external/              # Bank Integration, OCR
│   │   └── ml/                    # ML Service Client
│   ├── pom.xml
│   └── application.yml
│
├── budget-service/                # Budget & Forecasting
│   ├── src/main/java/...
│   │   ├── controller/
│   │   ├── service/
│   │   ├── entity/
│   │   ├── kafka/                 # Event Listeners
│   │   └── ml/
│   ├── pom.xml
│   └── application.yml
│
├── analytics-service/             # Spending Analysis & Anomalies
│   ├── src/main/java/...
│   │   ├── controller/
│   │   ├── service/
│   │   ├── kafka/
│   │   └── ml/
│   ├── pom.xml
│   └── application.yml
│
├── investment-service/            # Portfolio & Recommendations
│   ├── src/main/java/...
│   │   ├── controller/
│   │   ├── service/
│   │   ├── entity/
│   │   └── ml/
│   ├── pom.xml
│   └── application.yml
│
├── notification-service/          # Alerts & Notifications
│   ├── src/main/java/...
│   │   ├── controller/
│   │   ├── service/
│   │   ├── kafka/                 # All event listeners
│   │   └── external/              # FCM, SendGrid, Twilio
│   ├── pom.xml
│   └── application.yml
│
├── ml-service/                    # Python ML Services
│   ├── app/
│   │   ├── main.py
│   │   ├── models/
│   │   │   ├── categorization.py  # BERT NLP
│   │   │   ├── forecasting.py     # ARIMA/Prophet
│   │   │   ├── anomaly_detection.py
│   │   │   └── recommendations.py
│   │   └── routes/
│   ├── requirements.txt
│   ├── Dockerfile
│   └── docker-compose-ml.yml
│
├── kubernetes/                    # K8s manifests
│   ├── namespace.yaml
│   ├── secrets.yaml
│   ├── postgres-deployment.yaml
│   ├── redis-deployment.yaml
│   ├── kafka-deployment.yaml
│   └── services/
│       ├── transaction-service-deployment.yaml
│       ├── budget-service-deployment.yaml
│       └── ...
│
├── docker-compose.yml             # Local dev environment
├── pom.xml                        # Parent POM
├── .gitignore
├── README.md
└── docs/
    ├── API.md
    ├── ARCHITECTURE.md
    ├── DEPLOYMENT.md
    └── CONTRIBUTING.md
```

---

## 🔌 API Endpoints

### User Service
```
POST   /api/users/register              # Register user
POST   /api/users/login                 # Login
GET    /api/users/me                    # Get profile
PUT    /api/users/{id}/profile          # Update profile
POST   /api/users/{id}/upgrade          # Upgrade subscription
```

### Transaction Service
```
POST   /api/transactions                # Create transaction
GET    /api/transactions?month=2024-03  # Get monthly transactions
GET    /api/transactions/recent         # Get recent transactions
POST   /api/transactions/bulk-import    # Upload CSV
POST   /api/bank-connections/connect    # Connect bank account
POST   /api/transactions/scan-receipt   # OCR receipt
GET    /api/transactions/categories     # Get categories
```

### Budget Service
```
POST   /api/budgets                     # Create budget
GET    /api/budgets/{month}             # Get budgets for month
PUT    /api/budgets/{id}                # Update budget
GET    /api/goals                       # Get financial goals
POST   /api/goals                       # Create goal
GET    /api/forecasts/next-month        # Get forecast
GET    /api/budgets/{id}/vs-actual      # Budget analysis
```

### Analytics Service
```
GET    /api/analytics/spending-by-category
GET    /api/analytics/trends/{category}
GET    /api/analytics/anomalies
GET    /api/analytics/peer-comparison
GET    /api/analytics/insights
GET    /api/analytics/monthly-summary
```

### Investment Service
```
GET    /api/investments/profile
POST   /api/investments/profile
GET    /api/investments/portfolio
POST   /api/investments/add-asset
GET    /api/investments/recommendations
GET    /api/investments/portfolio/performance
GET    /api/investments/asset-allocation
```

### Notification Service
```
GET    /api/alerts/preferences
PUT    /api/alerts/preferences
GET    /api/notifications
PUT    /api/notifications/{id}/read
```

---

## 💡 Development Guide

### Setting Up IDE (IntelliJ IDEA)

1. **Open Project**
   ```
   File → Open → ai-cfo/pom.xml
   ```

2. **Configure SDK**
   ```
   File → Project Structure → SDK → Java 21
   ```

3. **Enable Annotation Processing**
   ```
   Settings → Build, Execution, Deployment → Compiler → Annotation Processors
   ✓ Enable annotation processing
   ```

4. **Install Plugins**
   - Lombok
   - Kafka
   - Spring Boot
   - Docker

5. **Create Run Configurations**
   - Edit Configurations → Add Spring Boot
   - Main class: `com.aicfo.{service}.{Service}Application`
   - VM options: `-Dspring.profiles.active=dev`

### Running Tests

```bash
# Run all tests
mvn clean test

# Run specific test class
mvn test -Dtest=TransactionServiceTest

# Run with coverage
mvn clean test jacoco:report

# View coverage report
open target/site/jacoco/index.html
```

### Code Style & Conventions

```java
// Use Lombok for boilerplate
@Data
@NoArgsConstructor
@AllArgsConstructor
@Builder
public class MyEntity {
    // No getters/setters needed
}

// Use records for DTOs (Java 21)
public record TransactionResponse(
    UUID id,
    BigDecimal amount,
    String category
) {}

// Use SLF4J with Lombok
@Slf4j
public class MyService {
    log.info("Message: {}", variable);
}
```

### Creating a New Service

```bash
# 1. Create module
mkdir my-service
cd my-service
mvn archetype:generate -DgroupId=com.aicfo -DartifactId=my-service

# 2. Update parent pom.xml
# Add <module>my-service</module>

# 3. Add dependencies
# Update pom.xml with common dependencies

# 4. Create application
# com.aicfo.myservice.MyServiceApplication

# 5. Create bootstrap
# application.yml

# 6. Create Dockerfile
```

---

## 🚢 Deployment

### Docker Compose (Local)

```bash
docker-compose up -d
```

### Kubernetes (Production)

```bash
# Create namespace
kubectl apply -f kubernetes/namespace.yaml

# Deploy infrastructure
kubectl apply -f kubernetes/postgresql-deployment.yaml
kubectl apply -f kubernetes/redis-deployment.yaml

# Deploy services
kubectl apply -f kubernetes/services/

# Scale services
kubectl scale deployment transaction-service --replicas=5 -n ai-cfo
```

### AWS (EKS)

```bash
# Create EKS cluster
aws eks create-cluster --name ai-cfo-prod ...

# Deploy
kubectl apply -f kubernetes/
```

### See Full Deployment Guide

Check the **AI-CFO-Deployment-Guide.md** for comprehensive instructions.

---

## 🧪 Performance Benchmarks

### Metrics (Baseline)
- **API Response Time**: p95 < 200ms
- **Transaction Processing**: 1000+ TPS (per instance)
- **Cache Hit Ratio**: > 80%
- **ML Inference Time**: < 50ms
- **Kafka Consumer Lag**: < 1 minute

### Load Testing

```bash
# Using JMeter/Gatling
mvn gatling:execute
```

---

## 📊 Monitoring & Alerts

### Dashboards
- **Grafana**: http://localhost:3000
- **Prometheus**: http://localhost:9090
- **Jaeger**: http://localhost:16686

### Key Metrics
- Request latency (p50, p95, p99)
- Error rates by service
- Kafka consumer lag
- Cache hit ratio
- Database connection pool
- Redis memory usage

---

## 🔒 Security

### Authentication
- JWT tokens (HS256)
- OAuth2 for bank connections
- Rate limiting (100 req/min per user)

### Data Protection
- TLS/SSL for all communications
- PII encryption
- Secure credential storage
- GDPR compliance (data export, deletion)

### Best Practices
- Secrets in environment variables
- No credentials in code
- Regular security audits
- Dependency scanning (Snyk, OWASP)

---

## 📈 Roadmap

### Phase 1 (Months 1-2) ✅ MVP
- User authentication
- Transaction tracking
- Basic budgeting

### Phase 2 (Months 3-4) 🔄 Core Features
- Bank integration
- Auto-categorization
- Forecasting

### Phase 3 (Months 5-6) 📊 Analytics
- Spending insights
- Anomaly detection
- Investment recommendations

### Phase 4 (Months 7-8) 🎯 Advanced
- Goal-based planning
- Mobile app
- B2B partnerships

### Phase 5 (Months 9+) 🌍 Scale
- International expansion
- API marketplace
- White-label solution

---

## 🤝 Contributing

### Code Standards
- Java 21 features
- Spring Boot best practices
- Clean Architecture principles
- SOLID principles

### Pull Request Process
1. Fork repository
2. Create feature branch: `git checkout -b feature/amazing-feature`
3. Commit changes: `git commit -m 'Add amazing feature'`
4. Push to branch: `git push origin feature/amazing-feature`
5. Open Pull Request

### Reporting Bugs
Use GitHub Issues with:
- Clear description
- Steps to reproduce
- Expected vs actual behavior
- Environment details

---

## 📚 Documentation

1. **AI-CFO-Architecture.md** - System design, microservices, database schema
2. **AI-CFO-Implementation-Guide.md** - Code examples, implementation details
3. **AI-CFO-Deployment-Guide.md** - Deployment, operations, troubleshooting
4. **API.md** - API specifications and examples (WIP)

---

## 📞 Support & Community

- **Email**: support@aicfo.dev
- **Issues**: GitHub Issues
- **Discussions**: GitHub Discussions
- **Slack**: [Join Community](https://slack.aicfo.dev)

---

## 📜 License

This project is licensed under the MIT License - see the LICENSE file for details.

---

## 🙏 Acknowledgments

- Spring Framework Team
- Apache Kafka Team
- Redis Community
- Confluent Kafka Community
- HuggingFace for NLP models
- All open-source contributors

---

## 🎉 Let's Build the Future of Personal Finance!

This is a comprehensive, production-ready microservices project that demonstrates:
- ✅ Modern Spring Boot architecture
- ✅ Microservices design patterns
- ✅ Real-time event streaming (Kafka)
- ✅ Intelligent caching (Redis)
- ✅ AI/ML integration
- ✅ Kubernetes deployment
- ✅ Enterprise monitoring
- ✅ Scalable system design

**Ready to contribute? Start with the [Development Guide](#development-guide)!**

---

**Version**: 1.0.0  
**Last Updated**: March 2024  
**Maintained By**: AI CFO Team

---

## Quick Links

- [Spring Boot Documentation](https://spring.io/projects/spring-boot)
- [Apache Kafka Guide](https://kafka.apache.org/documentation/)
- [Redis Best Practices](https://redis.io/)
- [Kubernetes Docs](https://kubernetes.io/docs/)
- [PostgreSQL Manual](https://www.postgresql.org/docs/)
- [Docker Documentation](https://docs.docker.com/)
- [AWS EKS Guide](https://docs.aws.amazon.com/eks/)
