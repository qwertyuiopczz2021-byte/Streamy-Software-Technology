# Streamy Core Domain Structural Architecture

## Foundational Object-Oriented Entity Specifications

### 1. Conceptual Domain Breakdown
This tracking document establishes the baseline static structures for the Streamy streaming platform codebase. By translating system requirements nouns straight into unified class schemas, we guarantee the underlying database and Python structures align perfectly.

The domain nouns are extracted directly from the verified requirements document [`SRS.md`](SRS.md): **Title** (FR-2, the central media entity), **Genre** (FR-1, the classification taxonomy) and **Review** (FR-3, the user metric collection).

### 2. Base Entity Model Class Diagram
The architectural class schema below plots our primary data properties, encapsulation metrics, and loose directional system associations:

```mermaid
classDiagram
class Title {
    +String name
    +int releaseYear
    +playMedia() Boolean
}

class Genre {
    +String genreName
    +getMediaCatalog() List
}

Title -- Genre : categorized_under
```

### 3. Relational Persistence Schema (Entity Relationship Diagram)
The class model above is translated into the physical storage ledger below. Entities are upper-cased, every field declares its datatype and key role, and both identifying (solid) and non-identifying (dashed) relationships are modelled with full crow's-foot cardinalities.

```mermaid
erDiagram
TITLE {
    int title_id PK
    int type_id FK
    string name
    datetime release_date
}
TITLE_TYPE {
    int type_id PK
    string type
}
ACTOR {
    int actor_id PK
    string name
    date date_of_birth
}
TITLE_ACTOR {
    int title_id PK "FK"
    int actor_id PK "FK"
}
GENRE {
    int genre_id PK
    string name
}
TITLE_GENRE {
    int title_id PK "FK"
    int genre_id PK "FK"
}
EPISODE {
    int episode_id PK
    int season_id FK
    string name
    int season_number
    int episode_number
    datetime release_date
}
SEASON {
    int season_id PK
    int title_id FK
    int season_number
    date release_year
}
REVIEW {
    int review_id PK
    int title_id FK
    int episode_id FK
    int season_id FK
    string review_by
    datetime review_date
    string review_text
}
TITLE }|..|| TITLE_TYPE: has
TITLE ||--o{ TITLE_GENRE: "belongs to"
TITLE ||--|{ TITLE_ACTOR: features
TITLE ||..|{ SEASON: contains
TITLE_GENRE }o--|| GENRE: references
TITLE_ACTOR }|--|| ACTOR: references
EPISODE }|..|| SEASON: contains
REVIEW }o..o| TITLE: "made against"
REVIEW }o..o| EPISODE: "made against"
REVIEW }o..o| SEASON: "made against"
```
