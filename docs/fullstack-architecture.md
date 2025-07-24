# RoomEase Fullstack Architecture Document

## Introduction

### Intro Content
This document outlines the complete fullstack architecture for RoomEase, including backend systems, frontend implementation, and their integration. Its primary goal is to serve as the single source of truth for AI-driven development, ensuring consistency across the entire technology stack. This unified approach combines what would traditionally be separate backend and frontend architecture documents, streamlining the development process for modern fullstack applications where these concerns are increasingly intertwined.

### Starter Template or Existing Project
RoomEase is a greenfield project, and no existing codebase or specific starter templates were provided or selected during the PRD phase. All tooling, bundling, and configuration for both Flutter and FastAPI will therefore require manual setup. The architecture design will proceed from scratch, aligning with the chosen Monorepo structure and other technical assumptions.

### Change Log
| Date | Version | Description | Author |
|------|---------|-------------|--------|
| 2025-07-19 | 1.0 | Initial draft. | Architect |

## High Level Architecture

### Technical Summary
RoomEase will employ a full-stack, cloud-native architecture with a Monorepo structure. The system will feature a Flutter-based cross-platform frontend and a FastAPI Monolith backend. Data persistence will be handled by PostgreSQL. For future cloud deployment, Google Cloud Platform (GCP) is recommended, leveraging services like Cloud Run for scalable backend deployment, Cloud SQL for managed PostgreSQL, and Firebase for mobile-specific features like push notifications. The architecture prioritizes simplicity and rapid iteration for the MVP while ensuring foundational scalability to meet PRD goals.

### Platform and Infrastructure Choice
- **Platform**: Google Cloud Platform (GCP)
- **Key Services**: Cloud Run (for scalable FastAPI backend deployment), Cloud SQL for PostgreSQL (managed database), Cloud Storage (for images and videos), and Firebase (for mobile-specific features like push notifications).
- **Deployment Host and Regions**: To be determined based on user location and latency requirements at the time of cloud deployment, but initially focused on India.

### Repository Structure
- **Structure**: Monorepo
- **Monorepo Tool**: While specific tooling (e.g., Nx, Turborepo, npm workspaces) will be detailed in implementation, the structure supports a single repository containing both Flutter frontend and FastAPI backend code, with shared packages for common types/utilities.
- **Package Organization**: The monorepo will organize applications (frontend, backend) and shared packages (e.g., shared models/types) in a logical directory structure.

### High Level Architecture Diagram

```mermaid
graph TD
    ---
config:
  layout: dagre
---
flowchart TD
 subgraph subGraph0["Shared Features"]
        H1["Pre-defined Templates"]
        H["Chat"]
        G6["Notifications"]
        G["Owner Dashboard"]
        I6["Notifications"]
        I["Tenant Home / Room Search"]
        I7["Approx. Location Map"]
        I1["View Room Details"]
        I8["Exact Location/Contact Reveal"]
  end
    A["App Launch"] --> B{"Existing User?"}
    B -- No --> C["Signup: Email & Password"]
    C --> D["Role Selection: Owner/Tenant"]
    D --> E{"Role Assigned"}
    B -- Yes --> F["Login: Email & Password"]
    F --> E
    E -- Owner --> G
    G --> G1["Create Building"] & G3["View Booking Requests"] & G5["Owner Profile"] & G6
    G1 --> G2["Add Room: Details, Images, Videos, AI Desc"]
    G2 --> G
    G3 --> G4{"Accept/Reject Request"}
    G4 --> G3 & H
    E -- Tenant --> I
    I --> I1 & I5["Tenant Profile"] & I6
    I1 --> I2["Send Booking Request"] & I7
    I2 --> I3["Pay Platform Fee Escrow"]
    I3 --> I4["Pending Owner Confirmation"]
    I4 --> H
    H --> H1 & I8
    G6 --> G
    I6 --> I
    style H fill:#ADD8E6
    style G fill:#90EE90
    style I fill:#90EE90
    style I8 fill:#F0E68C
    style A fill:#ADD8E6
    style B fill:#FFE4B5
    style D fill:#FFE4B5
    style E fill:#ADD8E6
    style G5 fill:#ADD8E6
    style I5 fill:#ADD8E6
```

```mermaid
graph TD
    A["Tenant/Owner - Mobile App"] -->|Flutter Frontend| B(Internet/CDN)
    B --> C[FastAPI Backend (Monolith)]
    C --> D[PostgreSQL Database]
    C --> E[AI Service (for Descriptions)]
    C --> F[Payment Gateway]
    C --> G[Google Maps API]
    C --> H[Push Notification Service]

    subgraph GCP [Future Cloud Deployment]
        C -- Cloud Run --> I(Managed Compute)
        D -- Cloud SQL --> J(Managed Database)
        K[Cloud Storage] -- S3-like --> C
    end

    F -- Webhooks --> C
    H -- FCM/APNs --> A

    style A fill:#ADD8E6
    style B fill:#FFE4B5
    style C fill:#90EE90
    style D fill:#ADD8E6
    style E fill:#F0E68C
    style F fill:#ADD8E6
    style G fill:#F0E68C
    style H fill:#ADD8E6
    style I fill:#ADD8E6
    style J fill:#ADD8E6
    style K fill:#ADD8E6
```

## Architectural Patterns

### Monolith Architecture
The FastAPI backend will be implemented as a unified monolith, containing all services and logic.

**Rationale**: This approach significantly simplifies development, deployment, and debugging for an MVP built by a solo developer, enabling rapid iteration and faster time-to-market. It avoids the complexities of distributed systems in the initial phase.

### Component-Based UI
The Flutter frontend will follow a component-based architecture, utilizing reusable UI components.

**Rationale**: This promotes modularity, maintainability, and reusability of UI elements, consistent with modern frontend development best practices.

### Repository Pattern
The backend will abstract data access logic through a repository pattern.

**Rationale**: This enables easier testing of business logic independently of the database and provides flexibility for future database migrations or changes without impacting core service logic.

### API Gateway Pattern
Implicitly, the FastAPI backend will act as the single entry point for all API calls from the frontend.

**Rationale**: This centralizes authentication, routing, and potential rate limiting, simplifying frontend interaction and security management.

### Event-Driven Communication (for Status Updates)
Leveraging webhooks from the payment gateway to trigger immediate backend processing for room status updates.

**Rationale**: This ensures real-time responsiveness and accuracy for critical features like preventing double-booking, aligning with the PRD's NFR7.

## Tech Stack
This is the DEFINITIVE technology selection for the entire RoomEase project. All development must use these exact versions.

### Technology Stack Table

| Category | Technology | Version | Purpose | Rationale |
|----------|------------|---------|---------|----------|
| Frontend | Flutter | Latest Stable (e.g., 3.22.x at time of development) | Cross-platform mobile application development. | Chosen for its ability to build natively compiled applications for mobile, web, and desktop from a single codebase, accelerating development for a solo developer. Provides a rich set of pre-built widgets and excellent performance. |
| Backend Framework | FastAPI | Latest Stable (e.g., 0.111.x at time of development) | Building high-performance, asynchronous API endpoints for the backend. | Chosen for its speed, ease of use, automatic interactive API documentation (Swagger UI/ReDoc), and strong support for asynchronous operations, which is ideal for I/O-bound tasks like database interactions and external API calls. |
| Backend Language | Python | 3.10+ | Primary language for backend logic and API development. | Widely used, strong ecosystem, excellent libraries for AI/ML (for future description generation), and good compatibility with FastAPI. |
| Database | PostgreSQL | 14+ | Relational database for storing structured data (users, rooms, buildings, bookings, payments). | Robust, reliable, open-source, and highly capable relational database with strong support for complex queries and data integrity, suitable for RoomEase's core data model. |
| ORM (Object-Relational Mapper) | SQLAlchemy | Latest Stable (e.g., 2.0.x at time of development) | Facilitating interaction between the FastAPI backend and the PostgreSQL database. | Provides a powerful and flexible ORM for Python, allowing developers to work with database records as Python objects, reducing boilerplate SQL and improving code readability and maintainability. |
| Database Migrations | Alembic | Latest Stable | Managing database schema changes over time. | Integrates seamlessly with SQLAlchemy to provide version control for database schemas, enabling smooth updates and rollbacks as the application evolves. |
| Data Validation (Backend) | Pydantic | Latest Stable (e.g., 2.x at time of development) | Data validation and settings management. | FastAPI uses Pydantic extensively for request body validation, response serialization, and data modeling, ensuring type safety and robust API contracts. |
| AI Integration (for Description) | Google Gemini API (or similar LLM API) | gemini-2.0-flash (or suitable equivalent) | Auto-generating room descriptions for owners. | Provides access to powerful large language models for creative text generation, automating a tedious task for owners and improving listing quality. |
| Payment Gateway | (To be selected based on India-specific options, e.g., Razorpay, Stripe India, Paytm) | Latest API version | Processing tenant platform fees and managing escrow-like functionality. | Essential for handling financial transactions securely and reliably, with webhook support for real-time status updates. Specific choice depends on integration ease, fees, and local compliance. |
| Mapping Service | Google Maps Platform API | Latest API version | Displaying approximate and exact room locations. | Widely used, robust, and provides comprehensive mapping functionalities necessary for location-based services in RoomEase. |
| Push Notifications | Firebase Cloud Messaging (FCM) / Apple Push Notification service (APNs) | Latest SDK/API versions | Sending real-time notifications to mobile users (e.g., new booking requests, chat messages). | Standard, reliable, and scalable services for delivering push notifications to Android and iOS devices, crucial for user engagement and timely alerts. |
| Cloud Hosting (Future) | Google Cloud Platform (GCP) | N/A (platform) | Hosting and managing RoomEase application components in a scalable and reliable cloud environment. | Offers a strong ecosystem for Python and Flutter, with excellent managed services (Cloud Run, Cloud SQL, Cloud Storage) that reduce operational overhead for a solo developer and provide inherent scalability. |
| Containerization (Future) | Docker | Latest Stable | Packaging FastAPI application for consistent deployment across environments (local, cloud). | Standard for containerization, enabling reproducible builds and deployments, which is beneficial for Cloud Run and general DevOps practices. |