# Mr. Hook's Dental Clinic: Approximated Chen-Style ERD (Mermaid)

```mermaid
flowchart TB
    %% Some stylistic definitions (optional)
    classDef entityColor fill:#ffffcc,stroke:#333,stroke-width:1px,color:#000
    classDef relColor fill:#ccffff,stroke:#333,stroke-width:1px,color:#000
    classDef attrColor fill:#ffffff,stroke:#666,stroke-width:1px,color:#000,stroke-dasharray: 2 2

    %% ----------------------
    %% DENTIST (Entity)
    %% ----------------------
    subgraph dentistEntity[" "]
        direction TB
        DENTIST( DENTIST ):::entityColor

        %% Each attribute as a separate circle/oval
        D_lic((licenseNo PK)):::attrColor
        D_first((firstName)):::attrColor
        D_last((lastName)):::attrColor
        D_phone((phone)):::attrColor
        D_dob((dateOfBirth)):::attrColor
        D_spec((specialties)):::attrColor

        %% Connect "DENTIST" label to each attribute
        DENTIST --- D_lic
        DENTIST --- D_first
        DENTIST --- D_last
        DENTIST --- D_phone
        DENTIST --- D_dob
        %% "specialties" is multi-valued in Chen, but we simulate with a single circle
        DENTIST --- D_spec
    end

    %% ----------------------
    %% PATIENT (Entity)
    %% ----------------------
    subgraph patientEntity[" "]
        direction TB
        PATIENT( PATIENT ):::entityColor

        P_id((patientID PK)):::attrColor
        P_fn((firstName)):::attrColor
        P_mn((middleName)):::attrColor
        P_ln((lastName)):::attrColor
        P_ph((phone)):::attrColor
        P_dob((dateOfBirth)):::attrColor
        P_gen((gender)):::attrColor
        P_reg((dateRegistered)):::attrColor
        P_addr((address composite)):::attrColor

        PATIENT --- P_id
        PATIENT --- P_fn
        PATIENT --- P_mn
        PATIENT --- P_ln
        PATIENT --- P_ph
        PATIENT --- P_dob
        PATIENT --- P_gen
        PATIENT --- P_reg
        PATIENT --- P_addr
    end

    %% ----------------------
    %% APPOINTMENT (Entity)
    %% ----------------------
    subgraph appointmentEntity[" "]
        direction TB
        APPOINTMENT( APPOINTMENT ):::entityColor

        A_id((appointmentID PK)):::attrColor
        A_date((date)):::attrColor
        A_time((time)):::attrColor

        APPOINTMENT --- A_id
        APPOINTMENT --- A_date
        APPOINTMENT --- A_time
    end

    %% ----------------------
    %% PROCEDURE (Entity)
    %% ----------------------
    subgraph procedureEntity[" "]
        direction TB
        PROCEDURE( PROCEDURE ):::entityColor

        PR_code((procedureCode PK)):::attrColor
        PR_price((price)):::attrColor
        PR_desc((description)):::attrColor

        PROCEDURE --- PR_code
        PROCEDURE --- PR_price
        PROCEDURE --- PR_desc
    end

    %% --------------------------------
    %% RELATIONSHIPS (as diamond shapes)
    %% --------------------------------

    %% 1) DENTIST -- APPOINTMENT
    %%    A Dentist can have N appointments
    %%    An Appointment has exactly 1 Dentist ( total on appointment side )
    HAS_DENT_APPT{{HAS}}:::relColor
    DENTIST -- "1" --- HAS_DENT_APPT
    HAS_DENT_APPT -- "N" --- APPOINTMENT

    %% 2) PATIENT -- APPOINTMENT
    %%    A Patient can have N appointments
    %%    An Appointment has exactly 1 Patient ( total on appointment side )
    HAS_PAT_APPT{{HAS}}:::relColor
    PATIENT -- "1" --- HAS_PAT_APPT
    HAS_PAT_APPT -- "N" --- APPOINTMENT

    %% 3) APPOINTMENT -- PROCEDURE (M:N)
    %%    An Appointment can include multiple procedures
    %%    A Procedure can appear in multiple appointments
    %%    M:N in Chen is typical. We do one diamond node "INCLUDES".
    INCLUDES{{INCLUDES}}:::relColor
    APPOINTMENT -- "N" --- INCLUDES
    INCLUDES -- "N" --- PROCEDURE
