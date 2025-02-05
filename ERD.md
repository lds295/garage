# Mr. Hook’s Academic Library — Chen‐Style ERD (Approx.)

```mermaid
flowchart TB
    %% === STYLING (optional) =======================================
    classDef entityColor fill:#ffffcc,stroke:#333,stroke-width:1px,color:#000
    classDef relColor fill:#ccffff,stroke:#333,stroke-width:1px,color:#000
    classDef attrColor fill:#ffffff,stroke:#666,stroke-width:1px,color:#000,stroke-dasharray:2 2
    classDef isaColor fill:#ffe0e0,stroke:#333,stroke-width:1px,color:#000

    %% ===============================================================
    %% SUPERCLASS: USER
    %% ISA: STUDENT, TEACHER, GUEST
    %% ===============================================================
    subgraph userEntity[" "]
        direction TB
        USER( "USER" ):::entityColor

        U_userNo(("userNo PK")):::attrColor
        U_name(("firstName, lastName")):::attrColor
        U_age(("age")):::attrColor
        U_edu(("educationLevel")):::attrColor

        USER --- U_userNo
        USER --- U_name
        USER --- U_age
        USER --- U_edu
    end

    %% ISA Relationship diamond
    ISA_USER{{ISA}}:::isaColor
    USER -- ISA_USER
    %% We'll branch out to Student, Teacher, Guest.

    %% =============== STUDENT SUBCLASS =================
    subgraph studentEntity[" "]
        direction TB
        STUDENT("STUDENT"):::entityColor

        ST_stID(("studentID")):::attrColor
        ST_course(("course")):::attrColor
        ST_year(("yearOfEntry")):::attrColor

        STUDENT --- ST_stID
        STUDENT --- ST_course
        STUDENT --- ST_year
    end
    ISA_USER -- STUDENT

    %% =============== TEACHER SUBCLASS =================
    subgraph teacherEntity[" "]
        direction TB
        TEACHER("TEACHER"):::entityColor

        T_dept(("department")):::attrColor
        T_degree(("degree")):::attrColor
        T_courses(("coursesTaught (multi-valued)")):::attrColor

        TEACHER --- T_dept
        TEACHER --- T_degree
        TEACHER --- T_courses
    end
    ISA_USER -- TEACHER

    %% =============== GUEST SUBCLASS =================
    subgraph guestEntity[" "]
        direction TB
        GUEST("GUEST"):::entityColor
        %% Guest has no extra attributes

    end
    ISA_USER -- GUEST

    %% ===============================================================
    %% SUPERCLASS: PUBLICATION
    %% ISA: BOOK, PERIODICAL
    %% ===============================================================
    subgraph publicationEntity[" "]
        direction TB
        PUBLICATION("PUBLICATION"):::entityColor

        PUB_isbn(("ISBN PK")):::attrColor
        PUB_title(("title")):::attrColor
        PUB_issue(("issueNumber")):::attrColor
        PUB_year(("year")):::attrColor
        PUB_copies(("availableCopies")):::attrColor

        PUBLICATION --- PUB_isbn
        PUBLICATION --- PUB_title
        PUBLICATION --- PUB_issue
        PUBLICATION --- PUB_year
        PUBLICATION --- PUB_copies
    end

    %% ISA diamond for Book & Periodical
    ISA_PUB{{ISA}}:::isaColor
    PUBLICATION -- ISA_PUB

    %% ================= BOOK SUBCLASS ==================
    subgraph bookEntity[" "]
        direction TB
        BOOK("BOOK"):::entityColor

        B_authors(("authors (multi-valued)")):::attrColor
        B_pub(("publisher")):::attrColor
        B_city(("city")):::attrColor

        BOOK --- B_authors
        BOOK --- B_pub
        BOOK --- B_city
    end
    ISA_PUB -- BOOK

    %% ================= PERIODICAL SUBCLASS ==================
    subgraph periodicalEntity[" "]
        direction TB
        PERIODICAL("PERIODICAL"):::entityColor

        P_org(("organizers")):::attrColor
        P_subtitle(("subtitle")):::attrColor
        P_vol(("volume")):::attrColor
        P_num(("number")):::attrColor

        PERIODICAL --- P_org
        PERIODICAL --- P_subtitle
        PERIODICAL --- P_vol
        PERIODICAL --- P_num
    end
    ISA_PUB -- PERIODICAL

    %% ===============================================================
    %% RELATIONSHIPS
    %%  1) BORROWS (User <-> Book)
    %%  2) READS (User <-> Publication)
    %%  3) REQUESTS (Teacher <-> Book)
    %% ===============================================================

    %% ---------------- BORROWS ---------------- 
    %% A User can borrow multiple Books; 
    %% A Book can be borrowed by many Users (but subject to copy limits).
    %% We store checkoutDate, expectedReturnDate, actualReturnDate, fine (derived).
    BORROWS{{BORROWS}}:::relColor
    USER -- "N" --- BORROWS
    BORROWS -- "N" --- BOOK

    %% Relationship attributes 
    %% (in Chen, we can attach them to the diamond)
    %% We'll simulate them as "oval" nodes hanging off the diamond.
    BR_chOut(("checkoutDate")):::attrColor
    BR_exp(("expectedReturnDate")):::attrColor
    BR_act(("actualReturnDate")):::attrColor
    BR_fine(("fine derived")):::attrColor

    BORROWS --- BR_chOut
    BORROWS --- BR_exp
    BORROWS --- BR_act
    BORROWS --- BR_fine

    %% ---------------- READS ----------------
    %% A User can read many Publications; 
    %% A Publication can be read by many Users (M:N).
    %% We store the date/time the user checked out & returned it to tray.
    READS{{READS}}:::relColor
    USER -- "M" --- READS
    READS -- "N" --- PUBLICATION

    R_out(("dateTimeOut")):::attrColor
    R_in(("dateTimeIn")):::attrColor
    READS --- R_out
    READS --- R_in

    %% ---------------- REQUESTS (Teacher -> Book) ----------------
    %% A Teacher can request many Books; 
    %% A Book can be requested by many Teachers (M:N).
    %% We store dateRequested, expirationDate.
    REQUESTS{{REQUESTS}}:::relColor
    TEACHER -- "M" --- REQUESTS
    REQUESTS -- "N" --- BOOK

    Req_date(("dateRequested")):::attrColor
    Req_exp(("expirationDate")):::attrColor
    REQUESTS --- Req_date
    REQUESTS --- Req_exp
