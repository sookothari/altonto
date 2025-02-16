# Fund Structure Ontology Documentation

## Overview
The Fund Structure ontology defines the fundamental concepts and relationships in private market fund structures, building upon FIBO's foundation.

## Class Hierarchy

### PrivateMarketFund
Base class for all private market investment vehicles.
- **Properties**:
  - vintageYear: Year the fund began investing
  - fundSize: Total committed capital
  - investmentPeriod: Duration of investment phase
- **Subclasses**:
  - BuyoutFund
  - VentureCapitalFund
  - GrowthEquityFund

### FundStructureComponent
Base class for fund structure participants.
- **Subclasses**:
  - GeneralPartner
  - LimitedPartner
  - ManagementCompany

## Key Relationships

### Fund-Participant Relationships
- hasGeneralPartner: Links fund to GP
- hasLimitedPartner: Links fund to LPs
- managedBy: Links fund to management company

### Fund Characteristics
- vintageYear: gYear
- fundSize: decimal
- investmentPeriod: duration

## Integration with FIBO
- Extends FIBO LegalEntity concepts
- Maintains compatibility with FIBO financial instrument definitions
- Aligns with FIBO business entity structures

## Usage Examples

### Creating a New Buyout Fund Instance
```owl
Individual: abc:ExampleBuyoutFund
    Types: BuyoutFund
    Facts: 
     vintageYear "2024"^^xsd:gYear
     fundSize "1000000000"^^xsd:decimal
     investmentPeriod "P5Y"^^xsd:duration
```

## Best Practices
1. Always specify vintage year
2. Include fund size in base currency
3. Define complete GP/LP relationships
4. Link to management company

## Extensions
The ontology can be extended for:
- Specific fund structures (e.g., master-feeder)
- Regional variations
- Regulatory requirements
