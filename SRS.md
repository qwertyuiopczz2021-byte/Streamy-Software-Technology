# Streamy Software Requirements Specification (SRS)

## 1. Introduction
This document outlines the functional and non-functional requirements for the Streamy video streaming backend platform.

## 2. Functional Requirements (FR)
- **FR-1**: Browse Catalog by Genre - Users must be able to filter and view video content based on specific genres.
- **FR-2**: Initialize Media Streaming - The system shall handle playback requests and stream video data to the client.
- **FR-3**: Log User Review Metrics - Users can submit ratings and comments which are stored for analytics.
- **FR-4**: User Authentication - Secure login and registration for platform access.
- **FR-5**: Search Functionality - Full-text search across video titles and descriptions.

## 3. Non-Functional Requirements (NFR)
- **NFR-1**: Performance - Video playback must start within 2 seconds under normal network conditions.
- **NFR-2**: Scalability - The backend must support up to 10,000 concurrent users.
- **NFR-3**: Security - All user data and streams must be encrypted in transit (TLS 1.3).

## 4. System Boundary Diagram
The following diagram illustrates the system context and boundaries for Streamy:

```mermaid
flowchart LR
    %% Actor Node Definition
    ViewerActor(("External Actor:<br>Platform Viewer"))
    
    %% Subgraph Perimeter Boundaries
    subgraph ExternalApp [Client Application Interface]
        UI[Web Dashboard UI]
    end

    subgraph SystemPerimeter [Streamy Core System Perimeter]
        FR1[FR-1: Browse Catalog by Genre]
        FR2[FR-2: Initialize Media Streaming]
        FR3[FR-3: Log User Review Metrics]
    end

    %% Connections
    ViewerActor --> UI
    UI -- Ingests Genre Metadata --> FR1
    UI -- Triggers Playback Route --> FR2
    UI -- Submits Rating Payload --> FR3