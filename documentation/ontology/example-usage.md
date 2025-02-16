# Private Markets Ontology Examples

## Basic Usage Examples

### 1. Creating a Buyout Fund Structure

```owl
# Define the Fund
Individual: ex:ExampleBuyoutFund
    Types: 
        BuyoutFund
    Facts:  
        vintageYear "2024"^^xsd:gYear
        fundSize "1500000000.00"^^xsd:decimal
        investmentPeriod "P5Y"^^xsd:duration

# Define the GP
Individual: ex:ExampleGP
    Types:
        GeneralPartner
    Facts:
        manages ex:ExampleBuyoutFund

# Define LPs
Individual: ex:InstitutionalLP1
    Types:
        LimitedPartner
    Facts:
        hasCommitment "100000000.00"^^xsd:decimal
```

### 2. Tracking Investment Lifecycle

```owl
# Define an Investment
Individual: ex:PortfolioCompanyA
    Types:
        Investment
    Facts:
        hasCurrentState ex:DueDiligence
        startDate "2024-02-15"^^xsd:date

# Track Activities
Individual: ex:DDProcess
    Types:
        DueDiligence
    Facts:
        involvestActivity ex:FinancialDD
        involvestActivity ex:LegalDD
        startDate "2024-02-15"^^xsd:date
```

### 3. Performance Measurement

```owl
# Define Performance Metrics
Individual: ex:FundIRR
    Types:
        IRR
    Facts:
        calculationDate "2024-02-16"^^xsd:date
        metricValue "22.5"^^xsd:decimal
        measuredOver ex:Q4_2023

Individual: ex:InvestmentTVPI
    Types:
        TVPI
    Facts:
        calculationDate "2024-02-16"^^xsd:date
        metricValue "1.8"^^xsd:decimal
```

## Advanced Examples

### 1. Complex Fund Structure

```owl
# Master-Feeder Structure
Individual: ex:MasterFund
    Types:
        PrivateMarketFund
    Facts:
        hasFeederFund ex:FeederFund1
        hasFeederFund ex:FeederFund2

Individual: ex:FeederFund1
    Types:
        PrivateMarketFund
    Facts:
        feedsInto ex:MasterFund
```

### 2. ESG Integration

```owl
# ESG Metrics
Individual: ex:ESGAssessment
    Types:
        ESGMetric
    Facts:
        hasEnvironmentalScore "85"^^xsd:integer
        hasSocialScore "75"^^xsd:integer
        hasGovernanceScore "90"^^xsd:integer
        assessmentDate "2024-02-16"^^xsd:date
```

### 3. Regulatory Reporting

```owl
# AIFMD Reporting
Individual: ex:AIFMDReport
    Types:
        RegulatoryReport
    Facts:
        reportingPeriod ex:Q4_2023
        submissionDeadline "2024-01-31"^^xsd:date
        hasReportingRequirement ex:AIFMDArticle24
```

## Query Examples

### SPARQL Queries for Common Tasks

```sparql
# Find all funds with IRR > 20%
SELECT ?fund ?irr
WHERE {
    ?fund a :PrivateMarketFund .
    ?fund :hasPerformanceMetric ?metric .
    ?metric a :IRR .
    ?metric :metricValue ?irr .
    FILTER (?irr > 20.0)
}

# Track investment stages
SELECT ?investment ?state ?date
WHERE {
    ?investment a :Investment .
    ?investment :hasCurrentState ?state .
    ?investment :startDate ?date .
}
```
