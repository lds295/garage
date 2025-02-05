# Mr. Hook’s Academic Library — Chen-Style ERD (Approx.)

```mermaid
flowchart TB
    %% ====== OPTIONAL STYLES ======
    classDef entityColor fill:#ffffcc,stroke:#333,stroke-width:1px,color:#000
    classDef relColor fill:#ccffff,stroke:#333,stroke-width:1px,color:#000
    classDef attrColor fill:#ffffff,stroke:#666,stroke-width:1px,color:#000,stroke-dasharray:2 2
    classDef isaColor fill:#ffe0e0,stroke:#333,stroke-width:1px,color:#000

    %% =========================================================
    %% SUPERCLASS: USER + ISA for STUDENT, TEACHER, GUEST
    %% =========================================================
    subgraph userEntity
        direction TB
        USER(USER):::entityColor

        U_userNo((userNoPK)):::attrColor
        U_name((firstLastName)):::attrColor
        U_age((age)):::attrColor
        U_edu((educationLevel)):::attrColor

        USER --- U_userNo
        USER --- U_name
        USER --- U_age
        USER --- U_edu
    end

    ISAUSER{{ISA}}:::isaColor
    USER -- ISAUSER

    %% === STUDENT SUBCLASS ===
    subgraph studentEntity
        direction TB
        STUDENT(STUDENT):::entityColor

        ST_id((studentID)):::attrColor
        ST_course((course)):::attrColor
        ST_year((yearOfEntry)):::attrColor

        STUDENT --- ST_id
        STUDENT --- ST_course
        STUDENT --- ST_year
    end
    ISAUSER -- STUDENT

    %% === TEACHER SUBCLASS ===
    subgraph teacherEntity
        direction TB
        TEACHER(TEACHER):::entityColor

        T_dept((department)):::attrColor
        T_degree((degree)):::attrColor
        T_courses((coursesTaughtMultiVal)):::attrColor

        TEACHER --- T_dept
        TEACHER --- T_degree
        TEACHER --- T_courses
    end
    ISAUSER -- TEACHER

    %% === GUEST SUBCLASS ===
    subgraph guestEntity
        direction TB
        GUEST(GUEST):::entityColor
        %% No extra attributes
    end
    ISAUSER -- GUEST

    %% =========================================================
    %% SUPERCLASS: PUBLICATION + ISA for BOOK, PERIODICAL
    %% =========================================================
    subgraph publicationEntity
        direction TB
        PUBLICATION(PUBLICATION):::entityColor

        PUB_isbn((isbnPK)):::attrColor
        PUB_title((title)):::attrColor
        PUB_issue((issueNumber)):::attrColor
        PUB_year((year)):::attrColor
        PUB_copies((availableCopies)):::attrColor

        PUBLICATION --- PUB_isbn
        PUBLICATION --- PUB_title
        PUBLICATION --- PUB_issue
        PUBLICATION --- PUB_year
        PUBLICATION --- PUB_copies
    end

    ISAPUB{{ISA}}:::isaColor
    PUBLICATION -- ISAPUB

    %% === BOOK SUBCLASS ===
    subgraph bookEntity
        direction TB
        BOOK(BOOK):::entityColor

        B_authors((authorsMultiVal)):::attrColor
        B_pub((publisher)):::attrColor
        B_city((city)):::attrColor

        BOOK --- B_authors
        BOOK --- B_pub
        BOOK --- B_city
    end
    ISAPUB -- BOOK

    %% === PERIODICAL SUBCLASS ===
    subgraph periodicalEntity
        direction TB
        PERIODICAL(PERIODICAL):::entityColor

        P_org((organizers)):::attrColor
        P_sub((subtitle)):::attrColor
        P_vol((volume)):::attrColor
        P_num((editionNumber)):::attrColor

        PERIODICAL --- P_org
        PERIODICAL --- P_sub
        PERIODICAL --- P_vol
        PERIODICAL --- P_num
    end
    ISAPUB -- PERIODICAL

    %% =========================================================
    %% RELATIONSHIPS
    %% 1) BORROWS (User <-> Book)
    %% 2) READS (User <-> Publication)
    %% 3) REQUESTS (Teacher <-> Book)
    %% =========================================================

    %% ---------- BORROWS ----------
    BORROWS{{BORROWS}}:::relColor
    USER -- "N" --- BORROWS
    BORROWS -- "N" --- BOOK

    BR_chout((checkoutDate)):::attrColor
    BR_expect((expectedReturnDate)):::attrColor
    BR_actual((actualReturnDate)):::attrColor
    BR_fine((fineDerived)):::attrColor

    BORROWS --- BR_chout
    BORROWS --- BR_expect
    BORROWS --- BR_actual
    BORROWS --- BR_fine

    %% ---------- READS ----------
    READS{{READS}}:::relColor
    USER -- "M" --- READS
    READS -- "N" --- PUBLICATION

    R_out((dateTimeOut)):::attrColor
    R_in((dateTimeIn)):::attrColor
    READS --- R_out
    READS --- R_in

    %% ---------- REQUESTS ----------
    REQUESTS{{REQUESTS}}:::relColor
    TEACHER -- "M" --- REQUESTS
    REQUESTS -- "N" --- BOOK

    Req_date((dateRequested)):::attrColor
    Req_exp((expirationDate)):::attrColor
    REQUESTS --- Req_date
    REQUESTS --- Req_exp

