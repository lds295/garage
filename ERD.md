# Mr. Hook's Dental Clinic (Chen-Style Approximation in Mermaid)

```mermaid
flowchart TB
    %% Define classes for some styling (optional)
    classDef entity fill:#ffffee,stroke:#333,stroke-width:1px,color:#000,border-radius:5px
    classDef relationship fill:#eeffff,stroke:#333,stroke-width:1px,color:#000
    classDef note fill:#fff,stroke:none,color:#333

    %% ============ ENTITIES ============

    DENTIST[ 
      <b>DENTIST</b><br/>
      <u>licenseNo</u> (PK)<br/>
      firstName, lastName<br/>
      phone<br/>
      dateOfBirth<br/>
      <i>specialties</i> (multi-valued)
    ]
    class DENTIST entity

    PATIENT[
      <b>PATIENT</b><br/>
      <u>patientID</u> (PK)<br/>
      firstName, middleName, lastName<br/>
      phone<br/>
      dateOfBirth, gender<br/>
      dateRegistered<br/>
      <i>address</i> (composite)
    ]
    class PATIENT entity

    APPOINTMENT[
      <b>APPOINTMENT</b><br/>
      <u>appointmentID</u> (PK)<br/>
      date, time
    ]
    class APPOINTMENT entity

    PROCEDURE[
      <b>PROCEDURE</b><br/>
      <u>procedureCode</u> (PK)<br/>
      price<br/>
      description
    ]
    class PROCEDURE entity

    %% ============ RELATIONSHIPS ============

    %% Dentist -- Appointment
    HAS_DENTIST_APPT{{ has }}:::relationship
    DENTIST --| 1 | HAS_DENTIST_APPT
    HAS_DENTIST_APPT --| N | APPOINTMENT
    %% 1 Dentist can have N Appointments
    %% An Appointment must have exactly 1 Dentist
    %% (If you want to show total vs partial participation, 
    %% we add a note or color. Mermaid doesn't do double lines.)

    %% Patient -- Appointment
    HAS_PATIENT_APPT{{ has }}:::relationship
    PATIENT --| 1 | HAS_PATIENT_APPT
    HAS_PATIENT_APPT --| N | APPOINTMENT
    %% 1 Patient can have N Appointments
    %% An Appointment belongs to exactly 1 Patient

    %% Appointment -- Procedure (M:N)
    INCLUDES{{ includes }}:::relationship
    APPOINTMENT --| N | INCLUDES
    INCLUDES --| N | PROCEDURE
    %% An Appointment can include N Procedures
    %% A Procedure can appear in N Appointments

    %% Optional note on derived or extra rules
    note1(Note: “specialties” is multi-valued. “address” is a composite attribute.):::note

    %% Position the note
    APPOINTMENT -- note1
