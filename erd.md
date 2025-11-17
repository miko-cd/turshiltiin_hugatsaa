``` mermaid
erDiagram
    UNIVERSITY {
      string id
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
      string id
      string universityId
      string name
      string type
      int amountMnt
      string description
    }

    PROGRAM {
      string id
      string name
    }

    UNIVERSITY_PROGRAM {
      string id
      string universityId
      string programId
    }

    RANKING {
      string universityId
      string name
      int qsRank
    }

    EVENT {
      int id
      string title
      date eventDate
      string time
      string location
      string type
      string icon
      string description
      int spots
      int registered
      bool online
      string hostedUniversityId
    }

    UNIVERSITY ||--o{ SCHOLARSHIP : offers
    UNIVERSITY ||--o{ UNIVERSITY_PROGRAM : offers
    PROGRAM   ||--o{ UNIVERSITY_PROGRAM : includes
    UNIVERSITY ||--|| RANKING : has
    UNIVERSITY ||--o{ EVENT : hosts
```
