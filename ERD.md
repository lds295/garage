# Mr. Hook’s Garage E-R Diagram

```mermaid
erDiagram
    CLIENT {
        string clientName PK
        string clientPhone PK
        string streetNo
        string streetName
        string aptNo
        string city
        string state
        string zip
    }

    AUTHORIZED_CONTACT {
        string contactName PK
        string contactPhone PK
    }

    CAR {
        string VIN PK
        string licensePlate
        string make
        string model
        string color
        int year
    }

    REPAIR {
        int repairID PK
        date dateCompleted
        string problemDescription
        decimal totalCost DERIVED
    }

    PROCEDURE {
        string procedureCode PK
        string description
        decimal cost
    }

    SUPPLY {
        string supplyID PK
        string description
        string brand
        decimal costPrice
        decimal salePrice
        int stockQuantity
    }

    USES {
        int repairID FK
        string procedureCode FK
        string supplyID FK
        int quantityUsed
    }

    CLIENT ||--|{ CAR : "owns"
    CLIENT }o--o{ AUTHORIZED_CONTACT : "authorizes"
    CAR ||--|{ REPAIR : "has"
    REPAIR }o--|{ PROCEDURE : "includes"
    REPAIR ||--|{ USES : "records usage"
    PROCEDURE ||--|{ USES : "is used in"
    SUPPLY ||--|{ USES : "is used in"
