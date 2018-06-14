


# bigNEON Architecture

This RFC describes the the overall architecture for the bigNEON ticketing platform.

## Introduction

bigNEON is the first mobile-centric ticketing platform that combines the primary and secondary markets using blockchain technology.

For an in-depth overview of the platform, see [Overall Requirements Document](https://github.com/big-neon/docs/blob/master/overview.md).

## Design Considerations

**Scalability** - ticketing systems have prolonged periods of little/no demand interspersed with bursts of incredibly high demand.

**Security** - Personal information will be stored on a database (in this MVP). The database and its information needs to be as secure as possible.

**Flexibility** - bigNeon is a first of a kind, with integration into the Tari blockchain for managing ticket assets. As the Tari API settles down, we need to be able to quickly adapt our code to match.

**Blockchain friendly** - This is not a typical Web 2.0 architecture. Integrating blockchain tech into a consumer product has its own challenges. Keep this in mind.

**Modularity** - Try and keep things as modular as possible to allow small teams and contributors to work ~ independently.

## Platforms / Interfaces

### iOS Native Client / Android Native Client

Native applications for iOS and Android devices will be developed to provide consumers with the full event discovery, ticket purchasing, ticket transferring/selling and event attendance experience. There must also be a mobile experience for venue operaters to scan and verify tickets as legitimate. It is to be determined if these consumer and operator experiences will exist in the same application, or seperate applications.

### Responsive Web Client

A responsive web interface must be developed for any visitor who navigates to the platform via a desktop or mobile web browser. There must be a full organization and event management experience available on the web for Organization Owners and Members. For consumers, the web experience will ultimately drive users to install the native applications where they will be able to purchase tickets to events.

### API

An API must be developed to power these cross-platform client experiences. A read-only version of this API must be accessible to so that organizations/venues/promoters can display their events on their 3rd party websites. Integrations with popular CMS platforms will be developed.

## Technology Stack

### Rust

Officially, it is described as a language that “runs blazingly fast, prevents nearly all segfaults, and guarantees thread safety.” Its powerful type- and memory-safety features run at compile time, eliminating common missteps like dangling pointers and data races at runtime while maintaining performance typical of a low-level language.

In addition, it features elements from imperative, functional, and object-oriented programming languages, providing high-level abstractions like trait-based generics, pattern matching, type inferencing, and automatic memory management at little to no runtime cost. 

#### Benefits

 - **Fast** - It's not a coincidence that a lot of Rust projects are the fastest implementation of a given thing anywhere, ever.
 - **Thread-safe** - Thread safety isn’t just documentation; it’s law. Even the most daring forms of sharing are guaranteed safe in Rust.
 - **Modularity** - promotes the design and development of modular code through Crates and Modules.

### PostgresSQL

PostgreSQL is a powerful, open source object-relational database system with over 30 years of active development that has earned it a strong reputation for reliability, feature robustness, and performance.

PostgreSQL, often simply Postgres, places emphasis on extensibility and standards compliance. As a database server, its primary functions are to store data securely and return that data in response to requests from other software applications. It can handle workloads ranging from small single-machine applications to large Internet-facing applications (or for data warehousing) with many concurrent users; on macOS Server, PostgreSQL is the default database; and it is also available for Microsoft Windows and Linux (supplied in most distributions).

PostgreSQL is ACID-compliant and transactional. PostgreSQL has updatable views and materialised views, triggers, foreign keys; supports functions and stored procedures, and other expandability.

PostgreSQL is developed by the PostgreSQL Global Development Group, a diverse group of many companies and individual contributors. It is free and open-source, released under the terms of the PostgreSQL License, a permissive software license.

#### Benefits

 - **Fast** - PostgreSQL supports multicore queries. All header information placed in PostgreSQL tables stays in RAM. You can’t create a table that is not stored in the memory. Table entries are sorted by index, so you’re able to extract them lightning fast.
 - **Scalable** - PostgreSQL is designed to scale vertically by running on bigger faster servers when you need more performance. To scale horizontally, PostgresSQL has decent replication features so you can create multiple replicas that can be used for reading data.
 - **Flexible** - Allows us to adapt and make changes quickly, if we employ the NoSQL JSON capabilities.
 - **Secure** - Supports the following security measures:
-- Use hash-based column encryption for values that don't need to be decrypted.
-- Use physical separation to isolate datasets that need to be kept apart.
-- Lock down port-level access to the PostgreSQL database.
-- Use pg_hba.conf to specify which hosts can use SSL-encrypted and non-encrypted connections.
-- Prevent connections from networks that don’t require database access. 
-- Set appropriate monitoring and logging for database queries.
-- Disable remote access to PostgreSQL.
-- Assign a distinct role for each application.

### Tari Blockchain

The blockchain is an incorruptible digital ledger of economic transactions that can be programmed to record not just financial transactions but virtually everything of value.

Information held on a blockchain exists as a shared — and continually reconciled — database. This is a way of using the network that has obvious benefits. The blockchain database isn’t stored in any single location, meaning the records it keeps are truly public and easily verifiable. No centralized version of this information exists for a hacker to corrupt. Hosted by millions of computers simultaneously, its data is accessible to anyone on the internet.

Tari introduces a Blockchain Protocol for digital assets built on Monero. Tari a new open-source blockchain protocol, aims to redefine the digital asset experience. By leveraging blockchain technology, the Tari protocol enables consumers and business to break down walled gardens between businesses, sell and trade scarce digital assets with programmed rules, and record immutable transfer and verification of ownership.

#### Benefits

- **Secure** - Share valuable data in a secure, tamper-proof way. That’s because blockchains store data using sophisticated math and innovative software rules that are extremely difficult for attackers to manipulate.
- **Transparent** - The transparency of a blockchain stems from the fact that the holdings and transactions of each public address are open to viewing.
- **Privacy** - Tari enables users to decide the level of privacy they require for the transactions. This is accomplished using the toggle feature provided in Tari sidechain. This feature resolves several issues related to transparency.

## Architecture

All components written in Rust (including REST endpoint) and supported by a PostgresSQL and Tari Blockchain backend should follow Clean Architecture design:

 1. **Independent of Frameworks**. The architecture does not depend on the existence of some library of feature laden software. This allows you to use such frameworks as tools, rather than having to cram your system into their limited constraints.
 2. **Testable**. The business rules can be tested without the UI, Database, Web Server, or any other external element.
 3. **Independent of UI**. The UI can change easily, without changing the rest of the system. A Web UI could be replaced with a console UI, for example, without changing the business rules.
 4. **Independent of Database**. You can swap out Oracle or SQL Server, for Mongo, BigTable, CouchDB, or something else like a blockchain. Your business rules are not bound to the database.
 5. **Independent of any external agency**. In fact your business rules simply don’t know anything at all about the outside world.

![Clean Architecture](/docs/blob/feature/architecture/assets/Clean Architecture.jpg "Clean Architecture")

This will result in a logical design style, and:
- Help our software system be extensible, reusable, maintainable, and adaptable.
- Break large monolith stack into a flexible composite of collaborating modules (in monolith style).
- Help newcomers understand the business features and the functionalities of the system (because it is small enough).
- Will lend itself well to multiple contributors working on different features.

The API will be supported by a PostgresSQL and Tari Blockchain backend, exposed as a secure REST API (implemented with Tokenised Role Based Access and SSL) and consumed by Web and Mobile interfaces which exposes the interactive consumer functionality. The JSON payloads between the interfaces and API represent a fast and convenient medium for direct storage in PostgresSQL, if we employ these NoSQL features.

### Diagram

![Design](/docs/blob/feature/architecture/assets/Design.png "Design")

The Ticket Purchasing Experience and Ticket Wallet & Transfers subsystems to integrate to and utilise the Tari Blockchain. The Tari Blockchain and PostgresSQL will form the basis of the data access layer. The subsystems within the API will use the appropriate data access, based on feature requirements. In this case all digital asset management and transfers will be handled by the Tari Blockchain and everything else by the PostgresSQL database.

## Conclusion

In this RFC a high-level overview of the ticketing platform, it's components, technology stack and architecture is briefly discussed as a primer for a more detailed design.