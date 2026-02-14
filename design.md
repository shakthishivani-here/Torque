# Torque - System Design Document
## Hyper-Contextual Autonomous Commerce Platform

**Version:** 1.0  
**Date:** February 15, 2026  
**Team:** WhiteWorld  
**Hackathon:** AI for Bharat - AI for Retail, Commerce and Market Intelligence

---

## 1. Design Overview

### 1.1 Purpose
This document provides the comprehensive technical design for Torque, a Hyper-Contextual Autonomous Commerce platform. It details the system architecture, component design, data models, API specifications, and deployment strategy using AWS cloud services.

### 1.2 Design Principles

1. **Cloud-Native Architecture**: Leverage AWS managed services for scalability and reliability
2. **Microservices Pattern**: Loosely coupled services for independent scaling and deployment
3. **Event-Driven Design**: Asynchronous processing for real-time responsiveness
4. **AI-First Approach**: ML/AI embedded in core business logic, not as an afterthought
5. **Security by Design**: End-to-end encryption, zero-trust architecture
6. **Observability**: Comprehensive logging, monitoring, and tracing
7. **Immutable Infrastructure**: Infrastructure as Code (IaC) for reproducibility
8. **API-First**: Well-defined contracts for all service interactions

### 1.3 Technology Stack

#### Core Technologies
- **Backend**: Python 3.11+ (FastAPI framework)
- **Frontend**: TypeScript, React 18+, Next.js 14
- **AI/ML**: Amazon SageMaker, AWS Bedrock (Claude 3.5 Sonnet)
- **Data Processing**: Apache Spark on AWS EMR
- **Real-time Streaming**: Amazon Kinesis Data Streams
- **API Gateway**: Amazon API Gateway
- **Container Orchestration**: Amazon ECS with Fargate

#### AWS Services
- **Compute**: AWS Lambda, Amazon ECS, AWS Fargate
- **Storage**: Amazon S3, Amazon EFS
- **Database**: Amazon Aurora PostgreSQL, Amazon DynamoDB
- **Data Warehouse**: Amazon Redshift Serverless
- **Caching**: Amazon ElastiCache (Redis)
- **Search**: Amazon OpenSearch Service
- **ML/AI**: Amazon SageMaker, AWS Bedrock
- **Messaging**: Amazon SQS, Amazon SNS, Amazon EventBridge
- **Ledger**: Amazon QLDB (Quantum Ledger Database)
- **Monitoring**: Amazon CloudWatch, AWS X-Ray
- **Security**: AWS IAM, AWS KMS, AWS Secrets Manager, AWS WAF

---

## 2. System Architecture

### 2.1 High-Level Architecture

```
┌─────────────────────────────────────────────────────────────────────────┐
│                          User Interface Layer                            │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐  ┌─────────────┐ │
│  │   Web App    │  │  Mobile Web  │  │  Admin Panel │  │  Gov Portal │ │
│  │  (Next.js)   │  │  (Responsive)│  │              │  │             │ │
│  └──────────────┘  └──────────────┘  └──────────────┘  └─────────────┘ │
└─────────────────────────────────────────────────────────────────────────┘
                                    │
                                    ▼
┌─────────────────────────────────────────────────────────────────────────┐
│                        API Gateway Layer (AWS)                           │
│  ┌──────────────────────────────────────────────────────────────────┐   │
│  │  Amazon API Gateway + AWS WAF + CloudFront CDN                   │   │
│  │  • Authentication (Cognito)  • Rate Limiting  • DDoS Protection  │   │
│  └──────────────────────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────────────────────┘
                                    │
                                    ▼
┌─────────────────────────────────────────────────────────────────────────┐
│                        Microservices Layer                               │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐  ┌─────────────┐ │
│  │   Pricing    │  │     Tax      │  │   Market     │  │     G2B     │ │
│  │   Engine     │  │   Co-Pilot   │  │ Intelligence │  │  Interface  │ │
│  │   Service    │  │   Service    │  │   Service    │  │   Service   │ │
│  └──────────────┘  └──────────────┘  └──────────────┘  └─────────────┘ │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐  ┌─────────────┐ │
│  │   Scraper    │  │     ML       │  │   Freight    │  │    User     │ │
│  │   Service    │  │   Service    │  │   Service    │  │   Service   │ │
│  └──────────────┘  └──────────────┘  └──────────────┘  └─────────────┘ │
│                    (Amazon ECS + Fargate)                                │
└─────────────────────────────────────────────────────────────────────────┘
                                    │
                                    ▼
┌─────────────────────────────────────────────────────────────────────────┐
│                        Event & Messaging Layer                           │
│  ┌──────────────────────────────────────────────────────────────────┐   │
│  │  Amazon EventBridge + SQS + SNS + Kinesis Data Streams          │   │
│  │  • Event Routing  • Async Processing  • Real-time Streaming     │   │
│  └──────────────────────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────────────────────┘
                                    │
                                    ▼
┌─────────────────────────────────────────────────────────────────────────┐
│                        AI/ML & Analytics Layer                           │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐  ┌─────────────┐ │
│  │   SageMaker  │  │  AWS Bedrock │  │  OpenSearch  │  │  Redshift   │ │
│  │   Models     │  │   (Claude)   │  │   Service    │  │  Serverless │ │
│  └──────────────┘  └──────────────┘  └──────────────┘  └─────────────┘ │
└─────────────────────────────────────────────────────────────────────────┘
                                    │
                                    ▼
┌─────────────────────────────────────────────────────────────────────────┐
│                          Data Layer                                      │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐  ┌─────────────┐ │
│  │   Aurora     │  │   DynamoDB   │  │   Amazon     │  │  Amazon S3  │ │
│  │  PostgreSQL  │  │              │  │     QLDB     │  │  Data Lake  │ │
│  │ (Transactional)│ (NoSQL/Cache)│  │   (Ledger)   │  │             │ │
│  └──────────────┘  └──────────────┘  └──────────────┘  └─────────────┘ │
└─────────────────────────────────────────────────────────────────────────┘
```

### 2.2 Architecture Patterns

#### 2.2.1 Microservices Architecture
- Each service owns its data and business logic
- Services communicate via REST APIs and event streams
- Independent deployment and scaling
- Polyglot persistence (different databases for different needs)

#### 2.2.2 Event-Driven Architecture
- Services publish events to EventBridge
- Asynchronous processing via SQS queues
- Real-time data streaming via Kinesis
- Decoupled service interactions

#### 2.2.3 CQRS (Command Query Responsibility Segregation)
- Separate read and write models
- Write operations to Aurora PostgreSQL
- Read operations from DynamoDB/ElastiCache/OpenSearch
- Event sourcing for audit trails (QLDB)


---

## 3. Component Design

### 3.1 Pricing Engine Service

#### 3.1.1 Responsibilities
- Calculate landed costs in real-time
- Compute Geopolitical Risk Factor (GRF)
- Generate pricing recommendations
- Apply price elasticity models
- Enforce business rules and constraints

#### 3.1.2 Technology Stack
- **Runtime**: Python 3.11 with FastAPI
- **Deployment**: Amazon ECS with Fargate
- **Database**: Aurora PostgreSQL (write), DynamoDB (read)
- **Cache**: ElastiCache Redis
- **ML Models**: SageMaker endpoints

#### 3.1.3 API Endpoints

```
POST /api/v1/pricing/calculate-landed-cost
POST /api/v1/pricing/calculate-grf
POST /api/v1/pricing/recommend-price
POST /api/v1/pricing/bulk-update
GET  /api/v1/pricing/sku/{sku_id}
GET  /api/v1/pricing/elasticity/{category_id}
```

#### 3.1.4 Data Model

```python
class LandedCost:
    sku_id: str
    base_cost: Decimal
    freight_cost: Decimal
    customs_duty: Decimal
    insurance: Decimal
    handling_fees: Decimal
    grf_premium: Decimal
    total_landed_cost: Decimal
    calculated_at: datetime
    valid_until: datetime

class PricingRecommendation:
    sku_id: str
    minimum_viable_price: Decimal
    recommended_price: Decimal
    competitor_prices: List[Decimal]
    margin_percentage: Decimal
    confidence_score: float
    factors: Dict[str, Any]
```


### 3.2 Tax Co-Pilot Service

#### 3.2.1 Responsibilities
- GST reconciliation (GSTR-2B matching)
- Vendor risk scoring and anomaly detection
- HSN/SAC code validation
- Self-healing compliance corrections
- Tax calculation and filing preparation

#### 3.2.2 Technology Stack
- **Runtime**: Python 3.11 with FastAPI
- **Deployment**: Amazon ECS with Fargate
- **Database**: Aurora PostgreSQL (transactional), QLDB (audit)
- **AI/ML**: AWS Bedrock (Claude for NLP), SageMaker (fraud detection)
- **Integration**: GSTN API, CBIC databases

#### 3.2.3 API Endpoints

```
POST /api/v1/tax/reconcile-gstr2b
POST /api/v1/tax/validate-hsn
POST /api/v1/tax/calculate-vendor-risk
POST /api/v1/tax/detect-anomalies
POST /api/v1/tax/generate-corrections
GET  /api/v1/tax/compliance-status/{gstin}
GET  /api/v1/tax/audit-trail/{transaction_id}
```

#### 3.2.4 Data Model

```python
class GSTReconciliation:
    gstin: str
    period: str  # MMYYYY
    gstr2b_invoices: List[Invoice]
    internal_invoices: List[Invoice]
    matched: List[InvoiceMatch]
    mismatches: List[InvoiceMismatch]
    reconciliation_status: str
    processed_at: datetime

class VendorRiskScore:
    vendor_gstin: str
    risk_score: int  # 0-100
    filing_consistency: float
    transaction_patterns: Dict
    fraud_indicators: List[str]
    recommendation: str
    calculated_at: datetime

class HSNValidation:
    sku_id: str
    hsn_code: str
    is_valid: bool
    suggested_hsn: Optional[str]
    confidence: float
    customs_duty_rate: Decimal
    validation_source: str
```


### 3.3 Market Intelligence Service

#### 3.3.1 Responsibilities
- Competitor price monitoring
- Sentiment analysis from social media
- Demand forecasting
- Digital shelf analytics
- Profitability heatmap generation

#### 3.3.2 Technology Stack
- **Runtime**: Python 3.11 with FastAPI
- **Deployment**: Amazon ECS with Fargate
- **Database**: OpenSearch (search/analytics), DynamoDB (cache)
- **ML**: SageMaker (sentiment, forecasting)
- **Data Lake**: S3 with Athena for ad-hoc queries

#### 3.3.3 API Endpoints

```
GET  /api/v1/market/competitor-prices/{sku_id}
GET  /api/v1/market/sentiment/{category}
GET  /api/v1/market/demand-forecast/{sku_id}
GET  /api/v1/market/profitability-heatmap
GET  /api/v1/market/digital-shelf/{sku_id}
POST /api/v1/market/analyze-trends
```

#### 3.3.4 Data Model

```python
class CompetitorPrice:
    sku_id: str
    competitor_id: str
    platform: str
    price: Decimal
    stock_status: str
    scraped_at: datetime
    url: str

class SentimentAnalysis:
    category: str
    sentiment_score: float  # -100 to +100
    mention_count: int
    trending_topics: List[str]
    predicted_demand_spike: Optional[datetime]
    confidence: float
    analyzed_at: datetime

class ProfitabilityMetrics:
    sku_id: str
    region: str
    revenue: Decimal
    landed_cost: Decimal
    tax_cost: Decimal
    net_profit: Decimal
    margin_percentage: Decimal
    period: str
```


### 3.4 Scraper Service

#### 3.4.1 Responsibilities
- Distributed web scraping of e-commerce platforms
- Anti-bot bypass mechanisms
- Product matching and normalization
- Data quality validation
- Rate limiting and ethical scraping

#### 3.4.2 Technology Stack
- **Runtime**: Python 3.11 with Scrapy/Playwright
- **Deployment**: AWS Lambda (scheduled) + ECS (long-running)
- **Orchestration**: AWS Step Functions
- **Storage**: S3 (raw data), DynamoDB (processed)
- **Proxy**: AWS Global Accelerator + rotating proxies

#### 3.4.3 Architecture

```
EventBridge Schedule → Lambda Trigger → Step Functions
                                              ↓
                                    ┌─────────────────┐
                                    │  Scraper Tasks  │
                                    │  (ECS Fargate)  │
                                    └─────────────────┘
                                              ↓
                                    ┌─────────────────┐
                                    │  Data Pipeline  │
                                    │  (Lambda + S3)  │
                                    └─────────────────┘
                                              ↓
                                    ┌─────────────────┐
                                    │  Product Match  │
                                    │  (SageMaker)    │
                                    └─────────────────┘
```

#### 3.4.4 Scraping Strategy

```python
class ScraperConfig:
    platform: str
    target_urls: List[str]
    scrape_frequency: str  # cron expression
    proxy_rotation: bool
    user_agent_rotation: bool
    javascript_rendering: bool
    max_retries: int
    timeout_seconds: int

class ScrapedProduct:
    platform: str
    external_id: str
    title: str
    price: Decimal
    stock_status: str
    seller: str
    rating: Optional[float]
    review_count: Optional[int]
    scraped_at: datetime
    raw_html: str  # stored in S3
```


### 3.5 G2B Interface Service

#### 3.5.1 Responsibilities
- Clean-File API for government access
- Transparency Index calculation
- Systemic risk heatmap generation
- Audit trail exposure
- Secure data sharing with regulators

#### 3.5.2 Technology Stack
- **Runtime**: Python 3.11 with FastAPI
- **Deployment**: Amazon ECS with Fargate (isolated VPC)
- **Database**: QLDB (immutable ledger), Aurora (metadata)
- **Security**: AWS PrivateLink, KMS encryption
- **Access Control**: IAM roles for government entities

#### 3.5.3 API Endpoints

```
GET  /api/v1/g2b/transparency-index/{entity_id}
GET  /api/v1/g2b/audit-trail/{transaction_id}
GET  /api/v1/g2b/systemic-risk-heatmap
GET  /api/v1/g2b/hsn-verification-history/{sku_id}
POST /api/v1/g2b/verify-compliance
POST /api/v1/g2b/query-aggregate-data
```

#### 3.5.4 Security Architecture

```
Government User → AWS PrivateLink → API Gateway (Private)
                                            ↓
                                    IAM Authentication
                                            ↓
                                    G2B Service (Isolated VPC)
                                            ↓
                                    QLDB (Immutable Records)
```

#### 3.5.5 Data Model

```python
class TransparencyIndex:
    entity_id: str
    theta_score: float  # 0-100
    filing_consistency: float
    data_integrity: float
    vendor_risk_score: float
    audit_trail_completeness: float
    calculated_at: datetime
    valid_until: datetime

class AuditTrailEntry:
    transaction_id: str
    entity_id: str
    transaction_type: str
    timestamp: datetime
    data_hash: str
    previous_hash: str
    metadata: Dict[str, Any]
    verification_status: str
```


### 3.6 ML Service

#### 3.6.1 Responsibilities
- Train and deploy ML models
- Price elasticity modeling
- Fraud detection
- Demand forecasting
- Product matching
- Sentiment analysis

#### 3.6.2 Technology Stack
- **Training**: Amazon SageMaker (AutoML, custom algorithms)
- **Inference**: SageMaker endpoints (real-time), Batch Transform (batch)
- **Feature Store**: SageMaker Feature Store
- **Model Registry**: SageMaker Model Registry
- **Monitoring**: SageMaker Model Monitor

#### 3.6.3 ML Models

| Model | Algorithm | Purpose | Update Frequency |
|-------|-----------|---------|------------------|
| Price Elasticity | XGBoost Regression | Predict volume impact of price changes | Weekly |
| Fraud Detection | Random Forest | Identify suspicious vendor patterns | Daily |
| Demand Forecasting | DeepAR | Predict demand 14 days ahead | Daily |
| Product Matching | Sentence Transformers | Match scraped products to SKUs | On-demand |
| Sentiment Analysis | BERT Fine-tuned | Analyze social media sentiment | Hourly |
| GRF Prediction | Gradient Boosting | Predict geopolitical risks | Daily |

#### 3.6.4 ML Pipeline

```
Data Collection → Feature Engineering → Model Training → Validation
                                                              ↓
                                                        Model Registry
                                                              ↓
                                                        Deployment
                                                              ↓
                                                    Inference Endpoint
                                                              ↓
                                                    Model Monitoring
```


### 3.7 Freight Service

#### 3.7.1 Responsibilities
- Integration with freight index providers (Xeneta, Drewry)
- Real-time freight cost updates
- Route optimization
- Carrier performance tracking
- Fuel surcharge calculations

#### 3.7.2 Technology Stack
- **Runtime**: Python 3.11 with FastAPI
- **Deployment**: AWS Lambda (scheduled updates)
- **Database**: DynamoDB (current rates), S3 (historical)
- **Integration**: REST APIs to freight providers

#### 3.7.3 API Endpoints

```
GET  /api/v1/freight/rates/{origin}/{destination}
GET  /api/v1/freight/routes/{origin}/{destination}
POST /api/v1/freight/calculate-cost
GET  /api/v1/freight/fuel-surcharge
GET  /api/v1/freight/carrier-performance
```

---

## 4. Data Architecture

### 4.1 Database Design

#### 4.1.1 Aurora PostgreSQL (Transactional Data)

**Schema: Products**
```sql
CREATE TABLE products (
    sku_id VARCHAR(50) PRIMARY KEY,
    name VARCHAR(500) NOT NULL,
    description TEXT,
    category_id VARCHAR(50),
    hsn_code VARCHAR(20),
    base_cost DECIMAL(15,2),
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

CREATE INDEX idx_products_category ON products(category_id);
CREATE INDEX idx_products_hsn ON products(hsn_code);
```

**Schema: Pricing**
```sql
CREATE TABLE pricing_history (
    id BIGSERIAL PRIMARY KEY,
    sku_id VARCHAR(50) REFERENCES products(sku_id),
    landed_cost DECIMAL(15,2),
    recommended_price DECIMAL(15,2),
    actual_price DECIMAL(15,2),
    margin_percentage DECIMAL(5,2),
    grf_score INT,
    valid_from TIMESTAMP,
    valid_to TIMESTAMP,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

CREATE INDEX idx_pricing_sku_date ON pricing_history(sku_id, valid_from);
```

**Schema: Tax & Compliance**
```sql
CREATE TABLE gst_reconciliation (
    id BIGSERIAL PRIMARY KEY,
    gstin VARCHAR(15) NOT NULL,
    period VARCHAR(6) NOT NULL,  -- MMYYYY
    gstr2b_data JSONB,
    internal_data JSONB,
    matched_count INT,
    mismatch_count INT,
    status VARCHAR(20),
    processed_at TIMESTAMP
);

CREATE TABLE vendor_risk (
    vendor_gstin VARCHAR(15) PRIMARY KEY,
    risk_score INT CHECK (risk_score BETWEEN 0 AND 100),
    filing_consistency DECIMAL(5,2),
    fraud_indicators JSONB,
    last_calculated TIMESTAMP
);
```


#### 4.1.2 DynamoDB (High-Speed Reads)

**Table: CurrentPricing**
```
Partition Key: sku_id
Sort Key: region
Attributes:
  - current_price (Number)
  - landed_cost (Number)
  - competitor_prices (List)
  - last_updated (Number - timestamp)
  - ttl (Number - for auto-expiry)

GSI: region-price-index
  - Partition Key: region
  - Sort Key: current_price
```

**Table: CompetitorData**
```
Partition Key: platform#sku_id
Sort Key: scraped_at
Attributes:
  - competitor_id (String)
  - price (Number)
  - stock_status (String)
  - url (String)
  - ttl (Number)

GSI: sku-latest-index
  - Partition Key: sku_id
  - Sort Key: scraped_at (descending)
```

**Table: FreightRates**
```
Partition Key: route_id (origin#destination)
Sort Key: effective_date
Attributes:
  - rate_per_kg (Number)
  - fuel_surcharge (Number)
  - carrier (String)
  - transit_days (Number)
  - ttl (Number)
```

#### 4.1.3 Amazon QLDB (Immutable Ledger)

**Ledger: TorqueAuditLedger**

**Table: PricingDecisions**
```
{
  "transaction_id": "string",
  "sku_id": "string",
  "timestamp": "datetime",
  "decision_type": "string",
  "input_data": {
    "landed_cost": "decimal",
    "grf_score": "int",
    "competitor_prices": []
  },
  "output_data": {
    "recommended_price": "decimal",
    "confidence": "float"
  },
  "metadata": {}
}
```

**Table: TaxCalculations**
```
{
  "transaction_id": "string",
  "gstin": "string",
  "timestamp": "datetime",
  "calculation_type": "string",
  "hsn_code": "string",
  "tax_amount": "decimal",
  "verification_status": "string"
}
```


#### 4.1.4 Amazon Redshift Serverless (Analytics)

**Schema: Analytics**
```sql
-- Fact Table: Sales
CREATE TABLE fact_sales (
    sale_id BIGINT IDENTITY(1,1) PRIMARY KEY,
    sku_id VARCHAR(50),
    customer_id VARCHAR(50),
    region VARCHAR(50),
    sale_date DATE,
    quantity INT,
    unit_price DECIMAL(15,2),
    landed_cost DECIMAL(15,2),
    tax_amount DECIMAL(15,2),
    net_profit DECIMAL(15,2),
    margin_percentage DECIMAL(5,2)
)
DISTKEY(sku_id)
SORTKEY(sale_date);

-- Dimension Table: Products
CREATE TABLE dim_products (
    sku_id VARCHAR(50) PRIMARY KEY,
    name VARCHAR(500),
    category VARCHAR(100),
    subcategory VARCHAR(100),
    hsn_code VARCHAR(20)
)
DISTSTYLE ALL;

-- Dimension Table: Time
CREATE TABLE dim_time (
    date_key DATE PRIMARY KEY,
    year INT,
    quarter INT,
    month INT,
    week INT,
    day_of_week INT
)
DISTSTYLE ALL;

-- Aggregate Table: Daily Profitability
CREATE TABLE agg_daily_profitability (
    date_key DATE,
    region VARCHAR(50),
    category VARCHAR(100),
    total_revenue DECIMAL(18,2),
    total_cost DECIMAL(18,2),
    total_profit DECIMAL(18,2),
    avg_margin DECIMAL(5,2),
    PRIMARY KEY (date_key, region, category)
)
DISTKEY(region)
SORTKEY(date_key);
```

#### 4.1.5 Amazon OpenSearch (Search & Analytics)

**Index: products**
```json
{
  "mappings": {
    "properties": {
      "sku_id": {"type": "keyword"},
      "name": {"type": "text", "analyzer": "standard"},
      "description": {"type": "text"},
      "category": {"type": "keyword"},
      "hsn_code": {"type": "keyword"},
      "current_price": {"type": "float"},
      "competitor_prices": {"type": "nested"},
      "sentiment_score": {"type": "float"},
      "demand_forecast": {"type": "float"},
      "last_updated": {"type": "date"}
    }
  }
}
```

**Index: market_intelligence**
```json
{
  "mappings": {
    "properties": {
      "timestamp": {"type": "date"},
      "source": {"type": "keyword"},
      "content": {"type": "text"},
      "sentiment": {"type": "float"},
      "entities": {"type": "keyword"},
      "topics": {"type": "keyword"},
      "relevance_score": {"type": "float"}
    }
  }
}
```


### 4.2 Data Flow Architecture

#### 4.2.1 Real-Time Pricing Flow

```
1. Freight Index Update (Daily)
   Freight Provider API → Lambda → DynamoDB → EventBridge Event

2. Competitor Price Change (Hourly)
   Scraper → S3 → Lambda → DynamoDB → EventBridge Event

3. Pricing Calculation (On-Demand)
   API Request → Pricing Service → Read (DynamoDB + ElastiCache)
   → Calculate → ML Model (SageMaker) → Write (Aurora + DynamoDB)
   → Publish Event → Return Response

4. Price Update Propagation
   EventBridge Event → SNS → Multiple Subscribers
   → Update Search Index (OpenSearch)
   → Update Analytics (Redshift)
   → Notify Downstream Systems
```

#### 4.2.2 Tax Reconciliation Flow

```
1. GSTR-2B Download (Monthly)
   Scheduled Lambda → GSTN API → S3 (Raw Data)

2. Reconciliation Processing
   S3 Event → Step Functions → Tax Service
   → Match Invoices → Detect Anomalies (ML)
   → Generate Report → Store (Aurora + QLDB)

3. Self-Healing
   Anomaly Detected → Bedrock (Claude) → Generate Correction
   → Human Approval (if threshold exceeded) → Apply Correction
   → Update Ledger (QLDB)
```

#### 4.2.3 Market Intelligence Flow

```
1. Data Collection
   EventBridge Schedule → Scraper Service → Raw Data (S3)
   Social Media API → Kinesis Stream → Lambda → S3

2. Processing Pipeline
   S3 Event → EMR Spark Job → Clean & Transform
   → Feature Extraction → Store (OpenSearch + Redshift)

3. ML Inference
   Scheduled Lambda → Batch Data → SageMaker Batch Transform
   → Predictions → Store (DynamoDB) → Publish Event

4. Dashboard Update
   EventBridge Event → Lambda → Update Cache (ElastiCache)
   → WebSocket Notification → Frontend Update
```


---

## 5. API Design

### 5.1 API Gateway Configuration

#### 5.1.1 API Structure
```
https://api.torque.ai/
├── v1/
│   ├── pricing/
│   ├── tax/
│   ├── market/
│   ├── freight/
│   ├── g2b/
│   └── admin/
```

#### 5.1.2 Authentication & Authorization
- **User Authentication**: Amazon Cognito User Pools
- **API Authentication**: JWT tokens (Bearer)
- **Service-to-Service**: IAM roles and policies
- **Government Access**: AWS PrivateLink + IAM roles

#### 5.1.3 Rate Limiting
```
Standard Tier:
  - 1000 requests per minute per API key
  - Burst: 2000 requests

Premium Tier:
  - 10000 requests per minute per API key
  - Burst: 20000 requests

Government Tier:
  - Unlimited (within reasonable limits)
  - Dedicated capacity
```

### 5.2 Core API Specifications

#### 5.2.1 Pricing API

**Calculate Landed Cost**
```
POST /api/v1/pricing/calculate-landed-cost
Content-Type: application/json
Authorization: Bearer {token}

Request:
{
  "sku_id": "SKU-12345",
  "origin": "Shanghai",
  "destination": "Mumbai",
  "quantity": 1000,
  "weight_kg": 500
}

Response:
{
  "sku_id": "SKU-12345",
  "base_cost": 50000.00,
  "freight_cost": 15000.00,
  "customs_duty": 7500.00,
  "insurance": 500.00,
  "handling_fees": 2000.00,
  "grf_premium": 1000.00,
  "total_landed_cost": 76000.00,
  "per_unit_cost": 76.00,
  "calculated_at": "2026-02-15T10:30:00Z",
  "valid_until": "2026-02-16T10:30:00Z"
}
```

**Get Pricing Recommendation**
```
POST /api/v1/pricing/recommend-price
Content-Type: application/json

Request:
{
  "sku_id": "SKU-12345",
  "region": "IN-MH",
  "target_margin": 25.0,
  "consider_competitors": true
}

Response:
{
  "sku_id": "SKU-12345",
  "minimum_viable_price": 95.00,
  "recommended_price": 110.00,
  "competitor_prices": [105.00, 115.00, 108.00],
  "market_position": "competitive",
  "expected_volume": 850,
  "margin_percentage": 26.5,
  "confidence_score": 0.87,
  "factors": {
    "landed_cost": 76.00,
    "grf_impact": "low",
    "demand_trend": "stable",
    "competitor_avg": 109.33
  }
}
```


#### 5.2.2 Tax API

**Reconcile GSTR-2B**
```
POST /api/v1/tax/reconcile-gstr2b
Content-Type: application/json

Request:
{
  "gstin": "27AABCU9603R1ZM",
  "period": "022026",
  "auto_download": true
}

Response:
{
  "gstin": "27AABCU9603R1ZM",
  "period": "022026",
  "total_invoices_gstr2b": 1250,
  "total_invoices_internal": 1248,
  "matched": 1230,
  "mismatches": {
    "missing_in_gstr2b": 18,
    "missing_in_internal": 20,
    "amount_differences": 12
  },
  "reconciliation_status": "completed",
  "discrepancy_amount": 45000.00,
  "report_url": "s3://torque-reports/gst/...",
  "processed_at": "2026-02-15T11:00:00Z"
}
```

**Validate HSN Code**
```
POST /api/v1/tax/validate-hsn
Content-Type: application/json

Request:
{
  "sku_id": "SKU-12345",
  "hsn_code": "8471",
  "product_description": "Laptop Computer"
}

Response:
{
  "sku_id": "SKU-12345",
  "submitted_hsn": "8471",
  "is_valid": true,
  "validated_hsn": "84713000",
  "hsn_description": "Portable automatic data processing machines",
  "confidence": 0.95,
  "customs_duty_rate": 10.0,
  "gst_rate": 18.0,
  "validation_source": "CBIC_2026",
  "last_updated": "2026-01-15"
}
```

#### 5.2.3 Market Intelligence API

**Get Competitor Prices**
```
GET /api/v1/market/competitor-prices/SKU-12345?region=IN-MH
Authorization: Bearer {token}

Response:
{
  "sku_id": "SKU-12345",
  "region": "IN-MH",
  "our_price": 110.00,
  "competitors": [
    {
      "competitor_id": "COMP-A",
      "platform": "amazon.in",
      "price": 105.00,
      "stock_status": "in_stock",
      "last_updated": "2026-02-15T09:30:00Z"
    },
    {
      "competitor_id": "COMP-B",
      "platform": "flipkart.com",
      "price": 115.00,
      "stock_status": "in_stock",
      "last_updated": "2026-02-15T09:45:00Z"
    }
  ],
  "market_position": "competitive",
  "price_rank": 2,
  "recommendation": "maintain"
}
```

**Get Demand Forecast**
```
GET /api/v1/market/demand-forecast/SKU-12345?days=14
Authorization: Bearer {token}

Response:
{
  "sku_id": "SKU-12345",
  "forecast_period": "14_days",
  "predictions": [
    {"date": "2026-02-16", "predicted_demand": 85, "confidence_interval": [75, 95]},
    {"date": "2026-02-17", "predicted_demand": 90, "confidence_interval": [80, 100]},
    ...
  ],
  "demand_spike_alert": {
    "expected_date": "2026-02-25",
    "predicted_increase": "35%",
    "reason": "social_media_trend"
  },
  "model_accuracy": 0.78
}
```


---

## 6. Security Architecture

### 6.1 Security Layers

#### 6.1.1 Network Security
```
Internet → CloudFront (CDN) → AWS WAF → API Gateway
                                  ↓
                            VPC (Private Subnets)
                                  ↓
                            ECS Services (Fargate)
                                  ↓
                            RDS/DynamoDB (Private)
```

**Security Groups Configuration:**
- API Gateway: Allow HTTPS (443) from CloudFront only
- ECS Services: Allow traffic from API Gateway security group
- RDS Aurora: Allow traffic from ECS security group only
- No direct internet access to backend services

#### 6.1.2 Identity & Access Management

**User Authentication Flow:**
```
User Login → Cognito User Pool → JWT Token
                                      ↓
API Request with Token → API Gateway Authorizer
                                      ↓
                            Validate Token → Allow/Deny
```

**Service-to-Service Authentication:**
```
Service A → Assume IAM Role → Get Temporary Credentials
                                      ↓
                            Call Service B with SigV4
                                      ↓
                            Service B validates IAM
```

#### 6.1.3 Data Encryption

**Encryption at Rest:**
- Aurora PostgreSQL: AWS KMS encryption
- DynamoDB: AWS managed encryption
- S3: SSE-KMS with customer managed keys
- QLDB: AWS managed encryption
- Redshift: KMS encryption
- EBS volumes: KMS encryption

**Encryption in Transit:**
- All API calls: TLS 1.3
- Inter-service communication: TLS 1.2+
- Database connections: SSL/TLS
- Certificate management: AWS Certificate Manager


### 6.2 Compliance & Audit

#### 6.2.1 Audit Logging
```
All API Calls → CloudTrail → S3 (Encrypted)
                                  ↓
                            Athena (Query)
                                  ↓
                            QuickSight (Visualization)

Application Logs → CloudWatch Logs → S3 Archive
                                          ↓
                                    7-year retention
```

#### 6.2.2 Compliance Controls

**SOC 2 Type II Requirements:**
- Access control: IAM with MFA
- Encryption: At rest and in transit
- Monitoring: CloudWatch + X-Ray
- Incident response: Automated alerts
- Change management: CI/CD with approvals
- Backup & recovery: Automated daily backups

**Data Residency:**
- Primary region: ap-south-1 (Mumbai)
- DR region: ap-south-2 (Hyderabad)
- All Indian customer data stays in India
- Government data: Isolated VPC with no cross-region replication

### 6.3 Threat Protection

#### 6.3.1 AWS WAF Rules
```
Rate Limiting:
  - 2000 requests per 5 minutes per IP
  - 100 requests per minute per API key

Geo Blocking:
  - Allow: India, specific countries for cross-border
  - Block: High-risk countries

SQL Injection Protection:
  - AWS Managed Rules: SQLi
  - Custom rules for specific patterns

Bot Protection:
  - AWS Managed Rules: Bot Control
  - CAPTCHA challenge for suspicious traffic
```

#### 6.3.2 DDoS Protection
- AWS Shield Standard (automatic)
- CloudFront distribution for traffic absorption
- Auto-scaling to handle legitimate traffic spikes
- Rate limiting at multiple layers

---

## 7. Deployment Architecture

### 7.1 Infrastructure as Code

#### 7.1.1 Technology Stack
- **IaC Tool**: AWS CDK (Python)
- **CI/CD**: AWS CodePipeline + CodeBuild + CodeDeploy
- **Container Registry**: Amazon ECR
- **Configuration**: AWS Systems Manager Parameter Store
- **Secrets**: AWS Secrets Manager

#### 7.1.2 Environment Strategy
```
Development (dev)
  ↓
Staging (staging)
  ↓
Production (prod)

Each environment:
  - Separate AWS accounts (AWS Organizations)
  - Separate VPCs
  - Separate databases
  - Separate KMS keys
```


### 7.2 CI/CD Pipeline

#### 7.2.1 Pipeline Stages

```
1. Source Stage
   GitHub → CodePipeline Trigger

2. Build Stage
   CodeBuild:
     - Run unit tests
     - Run linting
     - Build Docker images
     - Push to ECR
     - Run security scans (Snyk, Trivy)

3. Test Stage (Staging)
   - Deploy to staging ECS
   - Run integration tests
   - Run API tests
   - Run performance tests
   - Manual approval gate

4. Production Deployment
   - Blue/Green deployment
   - Deploy to 10% traffic (canary)
   - Monitor metrics (5 minutes)
   - Gradual rollout: 25% → 50% → 100%
   - Automatic rollback on errors

5. Post-Deployment
   - Smoke tests
   - Update documentation
   - Notify team (Slack/SNS)
```

#### 7.2.2 Deployment Strategy

**Blue/Green Deployment:**
```
Load Balancer
    ↓
    ├─→ Blue Environment (Current)
    │   - Receives 100% traffic
    │   - Stable version
    │
    └─→ Green Environment (New)
        - Deploy new version
        - Run tests
        - Switch traffic
        - Keep blue for rollback
```

### 7.3 Monitoring & Observability

#### 7.3.1 Metrics Collection

**Application Metrics (CloudWatch):**
- API latency (p50, p95, p99)
- Error rates (4xx, 5xx)
- Request throughput
- Database connection pool
- Cache hit/miss ratio
- ML model inference time

**Business Metrics:**
- Pricing recommendations generated
- GST reconciliations completed
- Competitor prices scraped
- Fraud alerts triggered
- Transparency Index calculations

**Infrastructure Metrics:**
- CPU/Memory utilization
- Network throughput
- Disk I/O
- Auto-scaling events
- Lambda cold starts


#### 7.3.2 Logging Strategy

**Log Aggregation:**
```
Application Logs → CloudWatch Logs → Log Groups
                                          ↓
                                    CloudWatch Insights
                                          ↓
                                    S3 Archive (7 years)
```

**Log Levels:**
- ERROR: System errors, exceptions
- WARN: Degraded performance, retries
- INFO: Business events, API calls
- DEBUG: Detailed troubleshooting (dev/staging only)

**Structured Logging Format:**
```json
{
  "timestamp": "2026-02-15T10:30:00Z",
  "level": "INFO",
  "service": "pricing-service",
  "trace_id": "abc123",
  "user_id": "user-456",
  "event": "pricing_recommendation_generated",
  "sku_id": "SKU-12345",
  "duration_ms": 145,
  "metadata": {}
}
```

#### 7.3.3 Distributed Tracing

**AWS X-Ray Integration:**
```
API Gateway → X-Ray Trace
    ↓
Pricing Service → X-Ray Segment
    ↓
    ├─→ DynamoDB Query → Subsegment
    ├─→ SageMaker Inference → Subsegment
    └─→ Aurora Query → Subsegment
```

**Trace Annotations:**
- user_id
- sku_id
- region
- request_type
- cache_hit

#### 7.3.4 Alerting

**Critical Alerts (PagerDuty):**
- API error rate > 1%
- Database connection failures
- ML model inference failures
- QLDB write failures
- Security incidents

**Warning Alerts (Slack):**
- API latency p95 > 500ms
- Cache hit rate < 80%
- Scraper success rate < 90%
- Disk usage > 80%

**Informational (Email):**
- Daily summary reports
- Weekly performance reports
- Monthly cost reports

---

## 8. Scalability & Performance

### 8.1 Horizontal Scaling

#### 8.1.1 Auto-Scaling Configuration

**ECS Services:**
```
Target Tracking Scaling:
  - Metric: CPU Utilization
  - Target: 70%
  - Scale out: Add 2 tasks
  - Scale in: Remove 1 task
  - Cooldown: 300 seconds

Step Scaling:
  - CPU > 85%: Add 4 tasks immediately
  - CPU < 30% for 10 min: Remove 2 tasks
```

**Lambda Functions:**
```
Concurrency:
  - Reserved: 100 (critical functions)
  - Provisioned: 50 (low-latency requirements)
  - Burst: 1000 (default)
```

**Database Scaling:**
```
Aurora Auto Scaling:
  - Min replicas: 2
  - Max replicas: 15
  - Target CPU: 70%
  - Target connections: 80% of max

DynamoDB Auto Scaling:
  - Min RCU: 5, Max RCU: 10000
  - Min WCU: 5, Max WCU: 10000
  - Target utilization: 70%
```


### 8.2 Caching Strategy

#### 8.2.1 Multi-Layer Caching

**Layer 1: CloudFront (Edge Cache)**
- Static assets: 24 hours
- API responses (GET): 5 minutes
- Invalidation: On deployment

**Layer 2: ElastiCache Redis (Application Cache)**
```
Cache Keys:
  - pricing:{sku_id}:{region} → TTL: 1 hour
  - competitor:{sku_id} → TTL: 30 minutes
  - freight:{route_id} → TTL: 24 hours
  - hsn:{code} → TTL: 7 days

Cache Patterns:
  - Cache-aside (lazy loading)
  - Write-through (critical data)
  - Cache warming (popular SKUs)
```

**Layer 3: DynamoDB (Data Cache)**
- Current pricing data
- Frequently accessed competitor data
- TTL-based auto-expiry

#### 8.2.2 Cache Invalidation

**Event-Driven Invalidation:**
```
Price Update Event → EventBridge → Lambda
                                      ↓
                            Invalidate Cache Keys
                                      ↓
                            Publish to SNS
                                      ↓
                            Update Subscribers
```

### 8.3 Performance Optimization

#### 8.3.1 Database Optimization

**Aurora PostgreSQL:**
- Connection pooling (PgBouncer)
- Read replicas for read-heavy queries
- Query optimization (EXPLAIN ANALYZE)
- Indexes on frequently queried columns
- Partitioning for large tables (by date)

**DynamoDB:**
- Efficient key design (avoid hot partitions)
- GSIs for alternate access patterns
- Batch operations for bulk reads/writes
- DynamoDB Accelerator (DAX) for ultra-low latency

**Redshift:**
- Distribution keys for join optimization
- Sort keys for range queries
- Materialized views for complex aggregations
- Workload management (WLM) queues

#### 8.3.2 API Optimization

**Response Compression:**
- Gzip compression for responses > 1KB
- Brotli for static assets

**Pagination:**
```
GET /api/v1/products?page=1&limit=100
Response:
{
  "data": [...],
  "pagination": {
    "page": 1,
    "limit": 100,
    "total": 10000,
    "next": "/api/v1/products?page=2&limit=100"
  }
}
```

**Field Selection:**
```
GET /api/v1/products/SKU-12345?fields=sku_id,name,price
```

**Batch Operations:**
```
POST /api/v1/pricing/bulk-calculate
{
  "sku_ids": ["SKU-1", "SKU-2", ..., "SKU-100"]
}
```


---

## 9. Disaster Recovery & Business Continuity

### 9.1 Backup Strategy

#### 9.1.1 Database Backups

**Aurora PostgreSQL:**
```
Automated Backups:
  - Frequency: Continuous (point-in-time recovery)
  - Retention: 35 days
  - Backup window: 02:00-04:00 IST

Manual Snapshots:
  - Before major deployments
  - Monthly archival snapshots
  - Retention: 7 years (compliance)
```

**DynamoDB:**
```
Point-in-Time Recovery (PITR):
  - Enabled on all tables
  - Recovery window: 35 days

On-Demand Backups:
  - Daily automated backups
  - Retention: 90 days
  - Cross-region backup to DR region
```

**QLDB:**
```
Journal Export:
  - Daily export to S3
  - Encrypted with KMS
  - Cross-region replication
  - Retention: 7 years
```

#### 9.1.2 Application Data Backups

**S3 Data:**
```
Versioning: Enabled
Replication: Cross-region to DR region
Lifecycle:
  - Standard: 30 days
  - Infrequent Access: 90 days
  - Glacier: 7 years
```

### 9.2 Disaster Recovery Plan

#### 9.2.1 DR Architecture

**Primary Region: ap-south-1 (Mumbai)**
**DR Region: ap-south-2 (Hyderabad)**

```
Active-Passive Configuration:

Primary (Active):
  - Full application stack
  - Read-write databases
  - 100% traffic

DR (Passive):
  - Minimal compute (warm standby)
  - Read replicas (Aurora)
  - Global tables (DynamoDB)
  - Continuous data replication
```

#### 9.2.2 Failover Procedure

**Automated Failover (Database):**
```
1. Aurora detects primary failure
2. Promotes read replica to primary (< 2 minutes)
3. Updates Route 53 DNS
4. Applications reconnect automatically
```

**Manual Failover (Region):**
```
1. Incident declared (RTO: 4 hours)
2. Activate DR compute resources (ECS tasks)
3. Promote DR databases to read-write
4. Update Route 53 to DR region
5. Verify application functionality
6. Monitor and stabilize
```


### 9.3 Recovery Objectives

| Component | RTO | RPO | Strategy |
|-----------|-----|-----|----------|
| API Gateway | < 5 min | 0 | Multi-AZ, automatic failover |
| ECS Services | < 10 min | 0 | Multi-AZ, auto-scaling |
| Aurora Database | < 2 min | < 5 min | Multi-AZ, read replicas |
| DynamoDB | < 1 min | 0 | Global tables, automatic failover |
| QLDB | < 30 min | < 15 min | Journal export, manual restore |
| S3 Data | < 5 min | 0 | Cross-region replication |
| Lambda Functions | < 5 min | 0 | Multi-AZ, automatic |
| Full Region Failover | < 4 hours | < 15 min | Manual DR activation |

---

## 10. Cost Optimization

### 10.1 Cost Management Strategy

#### 10.1.1 Compute Optimization

**ECS Fargate:**
- Right-sizing: Monitor CPU/memory usage, adjust task definitions
- Spot instances: Use for non-critical batch jobs (70% cost savings)
- Savings Plans: 1-year commitment for predictable workloads

**Lambda:**
- Memory optimization: Test different memory settings
- Provisioned concurrency: Only for latency-sensitive functions
- ARM architecture (Graviton2): 20% cost savings

#### 10.1.2 Storage Optimization

**S3:**
```
Lifecycle Policies:
  - 0-30 days: Standard
  - 31-90 days: Intelligent-Tiering
  - 91-365 days: Glacier Instant Retrieval
  - 366+ days: Glacier Deep Archive
```

**EBS:**
- Use gp3 instead of gp2 (20% cheaper)
- Delete unused volumes
- Snapshot lifecycle management

#### 10.1.3 Database Optimization

**Aurora:**
- Aurora Serverless v2 for variable workloads
- Stop dev/staging instances during off-hours
- Use T4g instances for non-production

**DynamoDB:**
- On-demand for unpredictable workloads
- Provisioned with auto-scaling for predictable
- Table class: Standard-IA for infrequently accessed data

### 10.2 Cost Monitoring

**AWS Cost Explorer:**
- Daily cost tracking
- Budget alerts (80%, 100%, 120% thresholds)
- Cost allocation tags by service, environment, team

**Estimated Monthly Costs (Production):**

| Service | Estimated Cost | Notes |
|---------|---------------|-------|
| ECS Fargate | $2,000 | 20 tasks, 2 vCPU, 4GB RAM |
| Aurora PostgreSQL | $1,500 | db.r6g.xlarge + 2 replicas |
| DynamoDB | $800 | 1M reads, 500K writes/day |
| Lambda | $300 | 10M invocations/month |
| S3 | $500 | 5TB storage, 1TB transfer |
| Redshift Serverless | $1,200 | 8 RPU hours/day |
| OpenSearch | $800 | 3 nodes, r6g.large |
| SageMaker | $1,000 | 5 endpoints, ml.m5.xlarge |
| CloudFront | $400 | 2TB data transfer |
| Other Services | $500 | WAF, KMS, CloudWatch, etc. |
| **Total** | **$9,000/month** | **~$108,000/year** |

---

## 11. Frontend Architecture

### 11.1 Technology Stack

**Framework & Libraries:**
- Next.js 14 (App Router)
- React 18
- TypeScript 5
- TailwindCSS for styling
- Shadcn/ui for components
- React Query for data fetching
- Zustand for state management
- Recharts for data visualization


### 11.2 Application Structure

```
torque-frontend/
├── app/
│   ├── (auth)/
│   │   ├── login/
│   │   └── register/
│   ├── (dashboard)/
│   │   ├── pricing/
│   │   ├── tax/
│   │   ├── market/
│   │   └── analytics/
│   ├── api/
│   └── layout.tsx
├── components/
│   ├── ui/
│   ├── pricing/
│   ├── tax/
│   └── market/
├── lib/
│   ├── api/
│   ├── hooks/
│   └── utils/
├── types/
└── public/
```

### 11.3 Key Features

#### 11.3.1 Dashboard Views

**Pricing Dashboard:**
- Real-time pricing recommendations
- Landed cost breakdown visualization
- Competitor price comparison
- Price elasticity charts
- Bulk price update interface

**Tax Compliance Dashboard:**
- GST reconciliation status
- Vendor risk scores
- HSN validation results
- Anomaly alerts
- Self-healing corrections log

**Market Intelligence Dashboard:**
- Profitability heatmaps
- Demand forecast charts
- Sentiment analysis trends
- Digital shelf analytics
- Competitor monitoring

**G2B Portal:**
- Transparency Index display
- Audit trail viewer
- Compliance reports
- Data export interface

#### 11.3.2 Real-Time Updates

**WebSocket Integration:**
```typescript
// Using AWS AppSync or API Gateway WebSocket
const useRealtimeUpdates = (sku_id: string) => {
  useEffect(() => {
    const ws = new WebSocket('wss://api.torque.ai/realtime');
    
    ws.onmessage = (event) => {
      const update = JSON.parse(event.data);
      // Update UI with new pricing/competitor data
    };
    
    return () => ws.close();
  }, [sku_id]);
};
```

### 11.4 Performance Optimization

**Code Splitting:**
- Route-based splitting (automatic with Next.js)
- Component lazy loading
- Dynamic imports for heavy libraries

**Image Optimization:**
- Next.js Image component
- WebP format with fallbacks
- Responsive images
- Lazy loading

**Caching:**
- React Query for API response caching
- Service Worker for offline support
- Static page generation where possible

---

## 12. Testing Strategy

### 12.1 Testing Pyramid

```
                    ┌─────────────┐
                    │   E2E Tests │  (10%)
                    └─────────────┘
                ┌───────────────────┐
                │ Integration Tests │  (30%)
                └───────────────────┘
            ┌───────────────────────────┐
            │      Unit Tests           │  (60%)
            └───────────────────────────┘
```

### 12.2 Testing Levels

#### 12.2.1 Unit Tests

**Framework:** pytest (Python), Jest (TypeScript)

**Coverage Target:** 80%

**Example:**
```python
def test_calculate_landed_cost():
    pricing_service = PricingService()
    result = pricing_service.calculate_landed_cost(
        base_cost=50000,
        freight_cost=15000,
        customs_duty=7500
    )
    assert result.total_landed_cost == 76000
    assert result.per_unit_cost == 76.00
```

#### 12.2.2 Integration Tests

**Framework:** pytest with moto (AWS mocking)

**Focus Areas:**
- API endpoint testing
- Database interactions
- External API integrations
- Event processing

**Example:**
```python
@pytest.mark.integration
def test_pricing_recommendation_flow():
    # Setup test data
    create_test_sku()
    mock_freight_api()
    
    # Call API
    response = client.post('/api/v1/pricing/recommend-price', json={
        'sku_id': 'TEST-SKU-001'
    })
    
    # Assertions
    assert response.status_code == 200
    assert response.json()['recommended_price'] > 0
```


#### 12.2.3 End-to-End Tests

**Framework:** Playwright

**Scenarios:**
- User login and authentication
- Complete pricing recommendation flow
- GST reconciliation workflow
- Market intelligence dashboard interaction

**Example:**
```typescript
test('pricing recommendation flow', async ({ page }) => {
  await page.goto('/login');
  await page.fill('[name="email"]', 'test@example.com');
  await page.fill('[name="password"]', 'password');
  await page.click('button[type="submit"]');
  
  await page.goto('/pricing');
  await page.fill('[name="sku_id"]', 'SKU-12345');
  await page.click('button:has-text("Get Recommendation")');
  
  await expect(page.locator('.recommended-price')).toBeVisible();
});
```

#### 12.2.4 Performance Tests

**Framework:** Locust (Python)

**Scenarios:**
- API load testing (1000 concurrent users)
- Pricing calculation throughput
- Database query performance
- Cache effectiveness

**Example:**
```python
from locust import HttpUser, task, between

class TorqueUser(HttpUser):
    wait_time = between(1, 3)
    
    @task
    def get_pricing_recommendation(self):
        self.client.post('/api/v1/pricing/recommend-price', json={
            'sku_id': f'SKU-{random.randint(1, 10000)}'
        })
```

### 12.3 Test Automation

**CI/CD Integration:**
```yaml
# .github/workflows/test.yml
name: Test Suite

on: [push, pull_request]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Run unit tests
        run: pytest tests/unit --cov
      - name: Run integration tests
        run: pytest tests/integration
      - name: Upload coverage
        uses: codecov/codecov-action@v3
```

---

## 13. Migration Strategy

### 13.1 Data Migration

#### 13.1.1 Migration Phases

**Phase 1: Historical Data (Offline)**
```
Legacy System → CSV Export → S3 → EMR Spark Job
                                        ↓
                                  Data Transformation
                                        ↓
                                  Aurora + DynamoDB
```

**Phase 2: Incremental Sync (Parallel Run)**
```
Legacy System → Change Data Capture → Kinesis
                                        ↓
                                  Lambda Processor
                                        ↓
                                  Torque Database
```

**Phase 3: Cutover (Go-Live)**
```
Stop Legacy Writes → Final Sync → Validation → Switch to Torque
```

#### 13.1.2 Data Validation

**Validation Checks:**
- Row count comparison
- Checksum validation
- Sample data verification
- Referential integrity checks
- Business logic validation

### 13.2 User Migration

**Rollout Strategy:**
```
Week 1-2: Internal team (10 users)
Week 3-4: Pilot customers (50 users)
Week 5-6: Early adopters (500 users)
Week 7-8: General availability (all users)
```

**Training & Support:**
- Video tutorials
- Documentation portal
- Live training sessions
- Dedicated support channel
- Feedback collection

---

## 14. Hackathon MVP Scope

### 14.1 Core Features (Must Have)

**For AI for Bharat Hackathon Demo:**

1. **Autonomous Pricing Engine**
   - Landed cost calculation with freight integration
   - GRF score calculation (simplified)
   - Basic pricing recommendation
   - Demo with 100 sample SKUs

2. **Tax Co-Pilot (Simplified)**
   - HSN code validation
   - Basic GST reconciliation demo
   - Vendor risk scoring (rule-based)

3. **Market Intelligence (Basic)**
   - Competitor price scraping (2-3 platforms)
   - Simple profitability heatmap
   - Basic sentiment analysis

4. **Dashboard**
   - Pricing dashboard with key metrics
   - Tax compliance overview
   - Market intelligence summary

5. **G2B Interface (Concept)**
   - Transparency Index calculation
   - Audit trail viewer (read-only)


### 14.2 MVP Architecture (Simplified)

```
Frontend (Next.js) → API Gateway → Lambda Functions
                                        ↓
                                  DynamoDB (primary)
                                        ↓
                                  SageMaker (1-2 models)
                                        ↓
                                  S3 (data storage)
```

**Simplified Stack:**
- No Aurora (use DynamoDB only)
- No Redshift (use Athena on S3)
- No OpenSearch (use DynamoDB queries)
- No QLDB (use DynamoDB with audit fields)
- Minimal ECS (use Lambda for most services)

### 14.3 Demo Scenarios

**Scenario 1: Pricing Recommendation**
```
1. User selects SKU "Laptop-XYZ"
2. System shows landed cost breakdown
3. System displays GRF score and risk factors
4. System recommends optimal price
5. User sees competitor prices comparison
6. User approves and applies price
```

**Scenario 2: Tax Compliance**
```
1. User uploads purchase register
2. System validates HSN codes
3. System flags 3 high-risk vendors
4. System shows reconciliation summary
5. System suggests corrections
6. User reviews and accepts
```

**Scenario 3: Market Intelligence**
```
1. Dashboard shows profitability heatmap
2. User drills down to "Electronics" category
3. System shows demand forecast
4. System alerts on upcoming demand spike
5. User adjusts inventory planning
```

**Scenario 4: Government Portal**
```
1. Government user logs in (demo account)
2. Views Transparency Index for entity
3. Queries audit trail for specific transaction
4. Verifies HSN code history
5. Exports compliance report
```

### 14.4 Success Criteria for Hackathon

**Technical:**
- ✅ Working end-to-end demo
- ✅ <200ms API response time
- ✅ ML model accuracy >80%
- ✅ Zero critical bugs during demo

**Business:**
- ✅ Clear demonstration of UVP (Landed-Margin Engine)
- ✅ Clear demonstration of USP (Self-Healing Compliance)
- ✅ Government value proposition articulated
- ✅ Scalability story presented

**Presentation:**
- ✅ 10-minute live demo
- ✅ Architecture diagram
- ✅ Business impact metrics
- ✅ Q&A preparation

---

## 15. Future Enhancements (Post-Hackathon)

### 15.1 Phase 2 Features

**Advanced AI/ML:**
- Deep learning for demand forecasting
- Reinforcement learning for dynamic pricing
- Computer vision for product matching
- Advanced NLP for contract analysis

**Expanded Integrations:**
- ERP systems (SAP, Oracle)
- Accounting software (Tally, QuickBooks)
- Marketplace APIs (Amazon MWS, Flipkart)
- Payment gateways

**Mobile Applications:**
- Native iOS app
- Native Android app
- Offline mode support
- Push notifications

### 15.2 Phase 3 Features

**International Expansion:**
- Multi-country support
- Currency hedging recommendations
- International tax compliance
- Cross-border logistics optimization

**Advanced Analytics:**
- Predictive maintenance
- Customer churn prediction
- Inventory optimization
- Supply chain risk modeling

**Blockchain Integration:**
- Smart contracts for vendor agreements
- Decentralized audit trails
- Cryptocurrency payment support

---

## 16. Appendix

### 16.1 AWS Service Limits

| Service | Default Limit | Required Limit | Action |
|---------|--------------|----------------|--------|
| ECS Tasks | 1000 | 2000 | Request increase |
| Lambda Concurrent Executions | 1000 | 5000 | Request increase |
| API Gateway Requests | 10000/sec | 50000/sec | Request increase |
| DynamoDB WCU | 40000 | 100000 | Request increase |
| SageMaker Endpoints | 10 | 20 | Request increase |

### 16.2 Third-Party Dependencies

| Service | Purpose | Cost | SLA |
|---------|---------|------|-----|
| Xeneta | Freight rates | $500/month | 99.5% |
| Drewry | Freight indices | $300/month | 99% |
| GSTN API | GST data | Free (govt) | 95% |
| Twilio | SMS alerts | Pay-as-you-go | 99.95% |
| SendGrid | Email | $15/month | 99.9% |

### 16.3 Glossary

| Term | Definition |
|------|------------|
| Landed Cost | Total cost including shipping, customs, insurance |
| GRF | Geopolitical Risk Factor score (0-100) |
| HSN | Harmonized System of Nomenclature code |
| GSTIN | GST Identification Number |
| GSTR-2B | Auto-drafted ITC statement |
| SKU | Stock Keeping Unit |
| RTO | Recovery Time Objective |
| RPO | Recovery Point Objective |

### 16.4 References

1. AWS Well-Architected Framework
2. GSTN Compliance Guidelines 2025-26
3. WCO Harmonized System 2022
4. SOC 2 Type II Requirements
5. WCAG 2.1 Accessibility Guidelines

---

## 17. Document Control

### 17.1 Version History

| Version | Date | Author | Changes |
|---------|------|--------|---------|
| 1.0 | 2026-02-15 | WhiteWorld Team | Initial design document for AI for Bharat Hackathon |

### 17.2 Approval

| Role | Name | Signature | Date |
|------|------|-----------|------|
| Technical Architect | | | |
| Lead Developer | | | |
| Security Lead | | | |
| Product Owner | | | |

### 17.3 Review Schedule

- **Next Review:** Post-Hackathon (February 2026)
- **Review Frequency:** Monthly during development
- **Review Participants:** Technical team, stakeholders

---

**End of Design Document**
