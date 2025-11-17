``` mermaid
classDiagram

direction LR

abstract class Institution {
  id: string
  name: string
  city: string
  description: string
  website: string
  email: string
  phone: string
}

class University {
  qsRank: number
  tuitionMntPerYear: number
  acceptanceRatePct: number
  getScholarships(): Scholarship[]
  getPrograms(): Program[]
}
Institution <|-- University

class Scholarship {
  name: string
  type: ScholarshipType
  amountMnt: number
  description: string
}

class Program {
  name: string
}

class UniversityProgram {
  universityId: string
  programId: string
}

class Ranking {
  universityId: string
  name: string
  qsRank: number
}

class Event {
  id: number
  title: string
  date: string
  time: string
  location: string
  type: string
  icon: string
  description: string
  spots: number
  registered: number
  online: boolean
  hostedUniversityId: string
}

class ScholarshipType {
}
ScholarshipType : merit
ScholarshipType : need_based
ScholarshipType : athletic
ScholarshipType : international

University "1" o-- "*" Scholarship : offers
University "1" o-- "*" UniversityProgram
Program "1" o-- "*" UniversityProgram
University "1" -- "1" Ranking : has
University "0..1" o-- "*" Event : hosts

class UniversityService {
  list(): University[]
  getById(id: string): University
  listRankings(): Ranking[]
}

class EventService {
  list(): Event[]
}

class ScholarshipCalculator {
  calculate(gpa: number, income: number, activities: number): number
  getRecommendation(score: number): Recommendation
}

class Recommendation {
  text: string
  color: string
  emoji: string
}

class UniversitiesPage {
  render()
  search()
}
class UniversityDetailPage {
  render(id: string)
}
class EventsPage {
  render()
}
class ScholarshipCalculatorPage {
  render()
}

UniversitiesPage --> UniversityService : uses
UniversityDetailPage --> UniversityService : uses
EventsPage --> EventService : uses
ScholarshipCalculatorPage --> ScholarshipCalculator : uses
UniversityService --> University : returns
EventService --> Event : returns

UniversitiesPage <<boundary>>
UniversityDetailPage <<boundary>>
EventsPage <<boundary>>
ScholarshipCalculatorPage <<boundary>>
UniversityService <<control>>
EventService <<control>>
ScholarshipCalculator <<control>>
University <<entity>>
Scholarship <<entity>>
Program <<valueObject>>
UniversityProgram <<entity>>
Ranking <<view>>
Event <<entity>>
```
