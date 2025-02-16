# Validation Rules and Constraints

## Data Validation Rules

### Fund Structure Validation
1. Fund Basic Information
   - Vintage year must be specified
   - Fund size must be positive
   - Investment period must be defined
   - At least one GP must be associated

2. Participant Relationships
   - Each fund must have exactly one primary GP
   - Each LP must have defined commitment amounts
   - Management company must be specified

### Investment Lifecycle Validation
1. State Transitions
   - States must progress in allowed sequence
   - Each state change must have timestamp
   - No gaps allowed in state history

2. Document Requirements
   - Required documents per state
   - Document versioning rules
   - Approval workflow validation

### Performance Metrics Validation
1. Calculation Rules
   - IRR inputs must be properly sequenced
   - TVPI components must reconcile
   - NAV must be current within 90 days

2. Temporal Validation
   - Performance periods must be continuous
   - No future dates allowed
   - Historical data immutability rules

## Semantic Constraints

### Class Constraints
```owl
# Example: Fund must have GP
Class: PrivateMarketFund
    SubClassOf: 
        hasGeneralPartner some GeneralPartner

# Example: Investment must have current state
Class: Investment
    SubClassOf:
        hasCurrentState exactly 1 InvestmentState
```

### Property Constraints
```owl
# Example: Fund size must be positive
DatatypeProperty: fundSize
    Range: xsd:decimal[> 0]

# Example: Vintage year constraints
DatatypeProperty: vintageYear
    Range: xsd:gYear[>= 1980]
```

## Business Rules

### Risk Management Rules
1. Concentration Limits
   - Single investment exposure limits
   - Sector concentration limits
   - Geographic concentration limits

2. Performance Thresholds
   - Minimum IRR requirements
   - Maximum loss tolerance
   - Drawdown limits

### Regulatory Compliance
1. Investment Restrictions
   - Prohibited investment types
   - Leverage limitations
   - Geographic restrictions

2. Reporting Requirements
   - Minimum reporting frequency
   - Required disclosures
   - Filing deadlines

## Implementation Guidelines

### Validation Implementation
1. Technical Setup
   - OWL reasoner configuration
   - SPARQL query validation
   - External validation services

2. Error Handling
   - Validation error categorization
   - Error message templates
   - Resolution workflows

### Maintenance
1. Rule Updates
   - Version control for rules
   - Change management process
   - Impact assessment

2. Monitoring
   - Validation performance metrics
   - Error rate tracking
   - Rule effectiveness assessment
