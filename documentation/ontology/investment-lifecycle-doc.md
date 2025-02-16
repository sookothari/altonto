# Investment Lifecycle Ontology Documentation

## Overview
The Investment Lifecycle ontology models the complete lifecycle of private market investments, from sourcing through exit.

## Class Hierarchy

### InvestmentState
Represents stages in the investment lifecycle.
- **Subclasses**:
  - Sourcing
  - DueDiligence
  - Execution
  - ValueCreation
  - Exit

### InvestmentActivity
Defines activities within each state.
- **Subclasses**:
  - DealScreening
  - ValuationAnalysis
  - TermNegotiation

### InvestmentDocument
Documents required throughout the lifecycle.
- **Subclasses**:
  - LOI (Letter of Intent)
  - PurchaseAgreement

## Key Relationships

### State Management
- hasCurrentState: Links investment to current state
- involvestActivity: Links state to required activities
- requiresDocument: Links activities to required documents

### Temporal Properties
- startDate: Beginning of state/activity
- completionDate: End of state/activity

## Integration Points
- Links to Fund Structure ontology through Investment class
- Supports workflow management
- Enables status tracking

## Usage Examples

### Tracking Investment Progress
```owl
Individual: abc:ExampleInvestment
    Types: Investment
    Facts: 
     hasCurrentState abc:DueDiligence
     startDate "2024-01-15"^^xsd:date
```

## Best Practices
1. Maintain state transition history
2. Document all required activities
3. Track temporal aspects
4. Link to relevant documents

## Extensions
The ontology supports extensions for:
- Custom investment stages
- Industry-specific processes
- Regulatory requirements
- ESG considerations
