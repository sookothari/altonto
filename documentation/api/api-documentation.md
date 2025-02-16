# Private Markets Ontology API Documentation

## REST API Endpoints

### Fund Management

#### Create Fund
```http
POST /api/v1/funds
Content-Type: application/json

{
    "name": "Example Fund IV",
    "vintage": 2024,
    "strategy": "BUYOUT",
    "targetSize": 1000000000,
    "generalPartner": {
        "id": "gp123",
        "name": "Example Capital"
    }
}
```

#### Add Limited Partner
```http
POST /api/v1/funds/{fundId}/limitedPartners
Content-Type: application/json

{
    "name": "Institutional LP 1",
    "commitment": 50000000,
    "type": "PENSION_FUND",
    "jurisdiction": "US"
}
```

### Investment Management

#### Record Investment
```http
POST /api/v1/investments
Content-Type: application/json

{
    "fundId": "fund123",
    "companyName": "Target Co",
    "amount": 75000000,
    "type": "EQUITY",
    "ownership": 0.65,
    "closingDate": "2024-02-16"
}
```

#### Update Valuation
```http
PUT /api/v1/investments/{investmentId}/valuations
Content-Type: application/json

{
    "valuationDate": "2024-02-16",
    "value": 85000000,
    "methodology": "DCF",
    "currency": "USD"
}
```

## GraphQL API

### Schema
```graphql
type Fund {
    id: ID!
    name: String!
    vintage: Int!
    strategy: Strategy!
    size: Money!
    generalPartner: GeneralPartner!
    limitedPartners: [LimitedPartner!]!
    investments: [Investment!]!
    performance: Performance!
}

type Investment {
    id: ID!
    company: Company!
    amount: Money!
    ownership: Float!
    status: InvestmentStatus!
    valuations: [Valuation!]!
}

type Performance {
    irr: Float
    tvpi: Float
    dpi: Float
    rvpi: Float
    updatedAt: DateTime!
}
```

### Queries
```graphql
query GetFundDetails($fundId: ID!) {
    fund(id: $fundId) {
        name
        vintage
        investments {
            company {
                name
                sector
            }
            amount
            currentValuation
        }
        performance {
            irr
            tvpi
        }
    }
}
```

## SPARQL Endpoints

### Fund Analysis
```sparql
# Get fund performance metrics
SELECT ?fund ?irr ?tvpi
WHERE {
    ?fund a pm:PrivateMarketFund ;
        pm:hasPerformanceMetric ?perfMetric .
    ?perfMetric a pm:IRR ;
        pm:value ?irr .
    OPTIONAL {
        ?fund pm:hasPerformanceMetric ?tvpiMetric .
        ?tvpiMetric a pm:TVPI ;
            pm:value ?tvpi .
    }
}
```

### Portfolio Analysis
```sparql
# Get portfolio company concentrations
SELECT ?sector (SUM(?value) as ?exposure)
WHERE {
    ?investment a pm:Investment ;
        pm:sector ?sector ;
        pm:currentValue ?value .
}
GROUP BY ?sector
ORDER BY DESC(?exposure)
```

## WebSocket API

### Real-time Updates
```javascript
// Subscribe to fund updates
ws.subscribe(`fund/${fundId}`, (update) => {
    console.log('Fund Update:', update);
});

// Subscribe to valuation changes
ws.subscribe(`investment/${investmentId}/valuations`, (update) => {
    console.log('Valuation Update:', update);
});
```

## Integration Examples

### Node.js Client
```javascript
const PrivateMarketsClient = require('private-markets-client');

const client = new PrivateMarketsClient({
    apiKey: 'your-api-key',
    endpoint: 'https://api.example.com'
});

// Create a new fund
const fund = await client.funds.create({
    name: 'New Fund V',
    vintage: 2024,
    strategy: 'GROWTH'
});

// Add an investment
const investment = await client.investments.create({
    fundId: fund.id,
    companyName: 'Growth Co',
    amount: 50000000
});
```

### Python Client
```python
from private_markets import Client

client = Client(
    api_key='your-api-key',
    endpoint='https://api.example.com'
)

# Get fund performance
performance = client.funds.get_performance(
    fund_id='fund123',
    as_of_date='2024-02-16'
)

# Generate report
report = client.reports.generate(
    fund_id='fund123',
    template='quarterly-report',
    period='2024Q1'
)
```

## Event Webhooks

### Webhook Events
```json
// Investment Valuation Update
{
    "event": "investment.valuation.updated",
    "data": {
        "investmentId": "inv123",
        "newValue": 85000000,
        "previousValue": 75000000,
        "updateDate": "2024-02-16T10:00:00Z"
    }
}

// Fund Performance Update
{
    "event": "fund.performance.updated",
    "data": {
        "fundId": "fund123",
        "metrics": {
            "irr": 22.5,
            "tvpi": 1.8
        },
        "calculationDate": "2024-02-16T10:00:00Z"
    }
}
```

## Authentication

### API Key Authentication
```http
GET /api/v1/funds
Authorization: Bearer your-api-key
```

### OAuth 2.0
```http
POST /oauth/token
Content-Type: application/x-www-form-urlencoded

grant_type=client_credentials
&client_id=your-client-id
&client_secret=your-client-secret
```

## Rate Limits

- 1000 requests per minute for REST API
- 100 concurrent connections for WebSocket API
- 10 parallel queries for SPARQL endpoint

## Error Handling

### Error Response Format
```json
{
    "error": {
        "code": "VALIDATION_ERROR",
        "message": "Invalid fund structure",
        "details": [{
            "field": "generalPartner",
            "message": "General partner is required"
        }]
    }
}
```

## Versioning

- API versioning via URL path (/v1/, /v2/)
- GraphQL schema versioning via directives
- Webhook payload versioning via content type
