'''mermaid
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

    %% 
    %% "USES" is an associative entity to capture the ternary relationship:
    %% "Which supplies were used in which procedure of which repair?"
    %%
    USES {
        int repairID FK
        string procedureCode FK
        string supplyID FK
        int quantityUsed
    }

    %% RELATIONSHIPS

    %% 1) CLIENT to CAR
    CLIENT ||--|{ CAR : "owns"
    %% Interpretation: 1 Client can own 0..N Cars
    %%                 Each Car must have exactly 1 Client owner (total on Car side)

    %% 2) CLIENT to AUTHORIZED_CONTACT (M:N)
    CLIENT }o--o{ AUTHORIZED_CONTACT : "authorizes"
    %% Interpretation: A Client can have 0..N contacts; 
    %%                 A contact can be associated with 0..N clients

    %% 3) CAR to REPAIR
    CAR ||--|{ REPAIR : "has"
    %% Interpretation: 1 Car can have 0..N Repairs
    %%                 Each Repair is for exactly 1 Car (total on Repair side)

    %% 4) REPAIR to PROCEDURE (M:N)
    REPAIR }o--|{ PROCEDURE : "includes"
    %% Interpretation: A Repair can include 1..N Procedures; 
    %%                 A Procedure can appear in 0..N Repairs

    %% 5) Ternary / Associative: REPAIR + PROCEDURE + SUPPLY
    REPAIR ||--|{ USES : "records usage"
    PROCEDURE ||--|{ USES : "is used in"
    SUPPLY ||--|{ USES : "is used in"
    %% quantityUsed is stored in the USES entity.

