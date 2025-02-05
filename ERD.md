# Mr. Hook's Dental Clinic ER Diagram

```mermaid
flowchart TB
    %% ENTITIES (using multiline labels with \n)
    
    DENTIST["DENTIST\n-----------\n(PK) licenseNo\nfirstName\nlastName\nphone\ndateOfBirth\nspecialties (multi-valued)"]
    PATIENT["PATIENT\n-----------\n(PK) patientID\nfirstName\nmiddleName\nlastName\nphone\ndateOfBirth\ngender\ndateRegistered\naddress (composite)"]
    APPOINTMENT["APPOINTMENT\n-----------\n(PK) appointmentID\ndate\ntime"]
    PROCEDURE["PROCEDURE\n-----------\n(PK) procedureCode\nprice\ndescription"]

    %% RELATIONSHIPS & CARDINALITIES

    %% 1) DENTIST -- APPOINTMENT
    %% One Dentist can have N Appointments. 
    %% Each Appointment is linked to exactly 1 Dentist.
    DENTIST -- "1..N" --- APPOINTMENT

    %% 2) PATIENT -- APPOINTMENT
    %% One Patient can have N Appointments.
    %% Each Appointment belongs to exactly 1 Patient.
    PATIENT -- "1..N" --- APPOINTMENT

    %% 3) APPOINTMENT -- PROCEDURE (M:N)
    %% An Appointment can include multiple Procedures,
    %% and a Procedure can appear in many different Appointments.
    APPOINTMENT -- "N..N" --- PROCEDURE
