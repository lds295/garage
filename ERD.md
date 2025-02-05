# Mr. Hook’s Garage — Chen‐Style ERD (Approx.)

```mermaid
flowchart TB
    %% BASIC STYLING (optional) %%%%%%%%%%%%%%%%%%%%%%%%
    classDef entityColor fill:#ffffcc,stroke:#333,stroke-width:1px,color:#000
    classDef relColor fill:#ccffff,stroke:#333,stroke-width:1px,color:#000
    classDef attrColor fill:#ffffff,stroke:#666,stroke-width:1px,color:#000,stroke-dasharray:2 2

    %% ===================================================
    %% 1) CLIENT ENTITY
    %% ===================================================
    subgraph clientEntity[" "]
        direction TB
        CLIENT( CLIENT ):::entityColor

        C_name_phone(("clientName+phone PK")):::attrColor
        C_address(("address (composite)")):::attrColor
        %% For a true Chen diagram, you'd list sub-attributes (street, aptNo, etc.) 
        %% as separate ovals or treat "address" as composite.

        CLIENT --- C_name_phone
        CLIENT --- C_address
    end

    %% ===================================================
    %% 2) AUTHORIZED_CONTACT ENTITY
    %% ===================================================
    subgraph contactEntity[" "]
        direction TB
        AUTH_CONTACT( AUTHORIZED_CONTACT ):::entityColor

        AC_name_phone(("contactName+phone PK")):::attrColor

        AUTH_CONTACT --- AC_name_phone
    end

    %% ===================================================
    %% 3) CAR ENTITY
    %% ===================================================
    subgraph carEntity[" "]
        direction TB
        CAR( CAR ):::entityColor

        CAR_vin(("VIN PK")):::attrColor
        CAR_plate(("licensePlate")):::attrColor
        CAR_make(("make")):::attrColor
        CAR_model(("model")):::attrColor
        CAR_color(("color")):::attrColor
        CAR_year(("year")):::attrColor

        CAR --- CAR_vin
        CAR --- CAR_plate
        CAR --- CAR_make
        CAR --- CAR_model
        CAR --- CAR_color
        CAR --- CAR_year
    end

    %% ===================================================
    %% 4) REPAIR ENTITY
    %% ===================================================
    subgraph repairEntity[" "]
        direction TB
        REPAIR( REPAIR ):::entityColor

        REP_id(("repairID PK")):::attrColor
        REP_date(("dateCompleted")):::attrColor
        REP_problem(("problemDescription")):::attrColor
        REP_total(("totalCost derived")):::attrColor

        REPAIR --- REP_id
        REPAIR --- REP_date
        REPAIR --- REP_problem
        REPAIR --- REP_total
    end

    %% ===================================================
    %% 5) PROCEDURE ENTITY
    %% ===================================================
    subgraph procedureEntity[" "]
        direction TB
        PROCEDURE( PROCEDURE ):::entityColor

        PROC_code(("procedureCode PK")):::attrColor
        PROC_desc(("description")):::attrColor
        PROC_cost(("cost")):::attrColor

        PROCEDURE --- PROC_code
        PROCEDURE --- PROC_desc
        PROCEDURE --- PROC_cost
    end

    %% ===================================================
    %% 6) SUPPLY ENTITY
    %% ===================================================
    subgraph supplyEntity[" "]
        direction TB
        SUPPLY( SUPPLY ):::entityColor

        SUP_id(("supplyID PK")):::attrColor
        SUP_desc(("description")):::attrColor
        SUP_brand(("brand")):::attrColor
        SUP_cprice(("costPrice")):::attrColor
        SUP_sprice(("salePrice")):::attrColor
        SUP_stock(("stockQuantity")):::attrColor

        SUPPLY --- SUP_id
        SUPPLY --- SUP_desc
        SUPPLY --- SUP_brand
        SUPPLY --- SUP_cprice
        SUPPLY --- SUP_sprice
        SUPPLY --- SUP_stock
    end


    %% ===================================================
    %% RELATIONSHIPS (DIAMONDS)
    %% ===================================================

    %% 1) CLIENT -- OWNS -- CAR (1..N)
    %%    A Client can own multiple Cars; each Car must have exactly 1 Client
    OWNS{{OWNS}}:::relColor
    CLIENT -- "1" --- OWNS
    OWNS -- "N" --- CAR

    %% 2) CLIENT -- AUTHORIZES -- AUTHORIZED_CONTACT (M:N)
    %%    A Client can have multiple contacts; a contact can be tied to multiple clients
    AUTHORIZES{{AUTHORIZES}}:::relColor
    CLIENT -- "M" --- AUTHORIZES
    AUTHORIZES -- "N" --- AUTH_CONTACT

    %% 3) CAR -- HAS_REPAIR -- REPAIR (1..N)
    %%    One Car can have many Repairs; each Repair belongs to exactly 1 Car
    HAS_REPAIR{{HAS_REPAIR}}:::relColor
    CAR -- "1" --- HAS_REPAIR
    HAS_REPAIR -- "N" --- REPAIR

    %% 4) REPAIR -- INCLUDES -- PROCEDURE (M:N)
    %%    A Repair can include multiple Procedures; 
    %%    A Procedure can appear in multiple Repairs
    INCLUDES{{INCLUDES}}:::relColor
    REPAIR -- "M" --- INCLUDES
    INCLUDES -- "N" --- PROCEDURE

    %% 5) TERNARY RELATION: REPAIR + PROCEDURE + SUPPLY = USES (M:N:N)
    %%    "Which supplies were used in which procedure for which repair?"
    USES{{USES}}:::relColor
    REPAIR -- "N" --- USES
    PROCEDURE -- "N" --- USES
    SUPPLY -- "N" --- USES

