``` mermaid
erDiagram
    UNIVERSITY {
      string id PK
      string name
      string city
      int qsRank
      string description
      string website
      string email
      string phone
      int tuitionMntPerYear
      int acceptanceRatePct
    }

    SCHOLARSHIP {
      string id PK
      string universityId FK
      string name
      string type  // "merit" | "need-based" | "athletic" | "international"
      int amountMnt
      string description
    }

    PROGRAM {
      string id PK
      string name  // e.g., "Компьютер", "Эдийн засаг", ...
    }

    UNIVERSITY_PROGRAM {
      string id PK
      string universityId FK
      string programId FK
    }

    RANKING {
      string universityId PK FK
      string name
      int qsRank
    }

    EVENT {
      int id PK
      string title
      date date
      string time
      string location
      string type
      string icon
      string description
      int spots
      int registered
      boolean online
      string hostedUniversityId FK  // optional
    }

    UNIVERSITY ||--o{ SCHOLARSHIP : offers
    UNIVERSITY ||--o{ UNIVERSITY_PROGRAM : offers
    PROGRAM   ||--o{ UNIVERSITY_PROGRAM : included_in
    UNIVERSITY ||--|| RANKING : has
    UNIVERSITY ||--o{ EVENT : hosts (optional)
```
