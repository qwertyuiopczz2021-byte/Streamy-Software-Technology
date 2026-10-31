# Streamy Backend Dynamic Behavioral Modeling

## System Architecture Interaction Streams

### 1. Architectural Core Mechanics
This tracking document details how application payloads pass between data layers before structural code layers are compiled. By designing how inputs move through software boundaries beforehand, we ensure development velocity remains high.

### 2. Media Playback Transaction Loop
The sequence model below outlines how the system validates viewer access permissions before initiating video file playback:

```mermaid
---
title: Streamy Playback Initialization Sequence Analysis
---
sequenceDiagram
    %% Define System Lifeline Entities
    actor Viewer as 👤 Platform Viewer
    participant Auth as 🔒 Auth Token Validator
    participant Catalog as 🎬 Media Catalog Engine

    %% Chronological Message Stream
    Viewer->>Auth: 1. Request Playback Route (stream_id, token)
    Auth->>Auth: 2. Check token expiration state
    alt token is expired / invalid
        Auth-->>Viewer: 3a. Return 401 Unauthorized Payload
    else token is active
        Auth->>Catalog: 3b. Request Streaming Meta-payload (stream_id)
        Catalog->>Catalog: 4. Fetch content encryption descriptors
        Catalog-->>Viewer: 5. Handshake OK (Return 200 OK + HLS Stream URL)
    end
```

### 3. Signed-Up Viewer Authentication Pipeline (Alternate Error Branches)
The second stream models the registration path, including the unhappy paths mandated by FR-4:

```mermaid
---
title: Streamy Viewer Sign-Up Sequence Analysis
---
sequenceDiagram
    actor Viewer as 👤 Platform Viewer
    participant Reg as 📝 Registration Controller
    participant Guard as 🔐 Credential Guard
    participant Store as 🗃️ Account Repository

    Viewer->>Reg: 1. Submit credentials (email, password)
    Reg->>Guard: 2. Validate password policy and email format
    alt password policy violated / email malformed
        Guard-->>Reg: 3a. Validation failure payload
        Reg-->>Viewer: 4a. Return 422 Unprocessable Entity (field errors)
    else credentials well-formed
        Guard->>Store: 3b. Check for an existing account (email)
        alt email already registered
            Store-->>Reg: 4b. Duplicate account record found
            Reg-->>Viewer: 5b. Return 409 Conflict (email in use)
        else email is available
            Store->>Store: 4c. Persist account with salted hash
            Store-->>Reg: 5c. Account identifier
            Reg-->>Viewer: 6c. Return 201 Created + session token
        end
    end
```

### 4. Component Structure of the Listing Container (C4 Component View)
Zooms inside the Streamy web application container to expose its controllers and namespaces. Callers sit above the boundary, internal components are rendered in pale blue, and downstream services and datastores are ranked below it.

```mermaid
---
title: "Listing Service C4 Model: Component Diagram"
---
flowchart TD
    classDef container fill:#1168bd,stroke:#0b4884,color:#ffffff
    classDef externalSystem fill:#666666,stroke:#0b4884,color:#ffffff
    classDef component fill:#85bbf0,stroke:#5d82a8,color:#000000

    Browser["Browser\n[Web Browser]\n\nUsed by a user to browse\nthe website"]
    MA["Application\n[Xamarin Application]\n\nAllows members to view and review\ntitles from their mobile devices"]
    R[("In-Memory Cache\n[Redis]\n\nTitles and their reviews\nare cached")]
    K["Message Broker\n[Kafka]\n\nImportant domain events\nare published to Kafka"]
    TS["Title Service\n[Software System]\n\nProvides an API to retrieve\ntitle information"]
    RS["Review Service\n[Software System]\n\nProvides an API to retrieve\nand submit reviews"]
    SS["Search Service\n[Software System]\n\nProvides an API to search\nfor titles"]

    subgraph listing-service [Listing Service Web Application]
        TCont["Title Controller\n[ASP.NET MVC Controller]\n\nAllows users to view details\nabout titles"]
        SCont["Search Controller\n[ASP.NET MVC Controller]\n\nAllows users to search\nfor titles"]
        RCont["Review Controller\n[ASP.NET MVC Controller]\n\nAllows users to read and\nwrite reviews"]
        TComp["Title Component\n[ASP.NET Namespace]\n\nProvides information on titles,\nretrieves info from Title Service\nand caches titles"]
        SComp["Search Component\n[ASP.NET Namespace]\n\nSearches titles using the\nSearch Service"]
        RComp["Review Component\n[ASP.NET Namespace]\n\nProvides review info,\nsubmits new reviews\nand publishes domain events"]

        TCont ---> TComp
        RCont ---> TComp
        RCont ---> RComp
        SCont ---> SComp
    end

    Browser-- "Submits requests to\n[HTTPS]" -->TCont
    Browser-- "Submits requests to\n[HTTPS]" --->SCont
    Browser-- "Submits requests to\n[HTTPS]" --->RCont
    MA-- "Submits requests to\n[HTTPS]" --->TCont
    MA-- "Submits requests to\n[HTTPS]" --->SCont
    MA-- "Submits requests to\n[HTTPS]" --->RCont
    TComp ---> TS
    TComp ---> R
    RComp ---> R
    RComp ---> K
    RComp ---> RS
    SComp ---> SS

    class MA,R container
    class SS,RS,TS,K,Browser externalSystem
    class RComp,SComp,TComp,RCont,SCont,TCont component
    style listing-service fill:none,stroke:#CCC,stroke-width:2px,stroke-dasharray:5 5
```

### 5. Complex Branched Decision Logic
Where a scenario degenerates into nested decision engines, a sequence diagram stops being readable and a standard flowchart is the correct tool. The model below exercises the diamond decision node syntax:

```mermaid
---
title: "Loan Eligibility Decision Flow"
---
flowchart TD
    Start([Applicant Applies]) --> CheckCredit{Credit Score above 700?}
    CheckCredit -- Yes --> CheckIncome{Debt-to-Income below 40%?}
    CheckCredit -- No --> ManualReview[Flag for Manual Underwriting]
    CheckIncome -- Yes --> AutoApprove[Automatically Approve Loan]
    CheckIncome -- No --> ManualReview
    ManualReview --> SeniorReview{Senior Underwriter Decision}
    SeniorReview -- Approved --> SendOffer[Issue Loan Offer]
    SeniorReview -- Denied --> SendRejection[Send Adverse Action Notice]
    AutoApprove --> SendOffer
```
