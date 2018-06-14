
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

In addition, it features elements from imperative, functional, and object-oriented programming languages, providing high-level abstractions like trait-based generics, pattern matching, type inferencing, and automatic memory management at little to no runtime cost. As the language continues to grow, the availability of a MongoDB driver written in native Rust exposes both Rust and MongoDB to new audiences.

#### Benefits

 - Fast - It's not a coincidence that a lot of Rust projects are the fastest implementation of a given thing anywhere, ever.
 - Thread-safe - Thread safety isn’t just documentation; it’s law. Even the most daring forms of sharing are guaranteed safe in Rust.
 - Modularity - promotes the design and development of modular code through Crates and Modules.

### MongoDB

MongoDB gives a NoSQL solution providing the performance and scalability required to handle high volume traffic. In addition, it can be the foundation for a distributed architecture that has no single point of failure. The result is a highly scalable sharding solution.

MongoDB alleviates the bloat typically found when dealing with ORM model classes since these simple JSON strings could be managed directly. The real architectural beauty is in the REST endpoints which represent CRUD operations on the usual ticketing entities like tickets, events, etc… This suite of endpoints is the icing on the cake because it provides the client with a robust API that is easy to integrate with, well documented and clean while, under the hood, MongoDB is providing the ultimate in performance results.  Without the bloat of model classes or data transfer objects along with a minimal web service layer, the application can easily be maintained by a small group of engineers who aren't burdened with fixing excessive bugs or trying to perpetually keep classes in sync with schemas.

#### Benefits

 - Fast - MongoDB is schemaless and as the data stored in database get bigger and bigger, MongoDB proves that it is much faster than other NoSQL databases.
 - Scalable - MongoDB supports horizontal scaling through Sharding, distributing data across several machines and facilitating high throughput operations with large sets of data.
 - Flexible - Allows us to adapt and make changes quickly.
 - Secure - Supports the following security measures:
 -- Enable Access Control and Enforce Authentication
-- Configure Role-Based Access Control
-- Encrypt Communication
-- Encrypt and Protect Data
-- Limit Network Exposure
-- Audit System Activity
-- Run MongoDB with a Dedicated User
-- Run MongoDB with Secure Configuration Options

## Architecture

All components written in Rust (including REST endpoint) and supported by a MongoDB backend should follow Clean Architecture design:

 1. **Independent of Frameworks**. The architecture does not depend on the existence of some library of feature laden software. This allows you to use such frameworks as tools, rather than having to cram your system into their limited constraints.
 2. **Testable**. The business rules can be tested without the UI, Database, Web Server, or any other external element.
 3. **Independent of UI**. The UI can change easily, without changing the rest of the system. A Web UI could be replaced with a console UI, for example, without changing the business rules.
 4. **Independent of Database**. You can swap out Oracle or SQL Server, for Mongo, BigTable, CouchDB, or something else. Your business rules are not bound to the database.
 5. **Independent of any external agency**. In fact your business rules simply don’t know anything at all about the outside world.

![enter image description here](https://lh3.googleusercontent.com/FFYMDhy1b4LwfqKtEcqPpV-lTqrcXICjoHyLnbHvZYIgf7Z8TaMXsuu4XOh590ipr7G6Vi7enA "Clean Architecture")

This will result in a logical design style, and:
- Help our software system be extensible, reusable, maintainable, and adaptable.
- Break large monolith stack into a flexible composite of collaborating modules (in monolith style).
- Help newcomers understand the business features and the functionalities of the system (because it is small enough).
- Will lend itself well to multiple contributors working on different features.

The API will be supported by a MongoDB (excluding Financial Transaction Data - blockchain) backend, exposed as a REST API (implemented with Tokenised Role Based Access and SSL) and consumed by Web and Mobile interfaces which exposes the interactive consumer functionality. The JSON payloads between the interfaces and API represent a fast and convenient medium for direct storage in MongoDB.

### Diagram

![enter image description here](https://lh3.googleusercontent.com/owiR4Trn_Kc89CYM52MzRL3sDx0u5EO5m8xRScqH4MMrIyjBnjQg5u7dvbtmK1DDJ15ROb-5kA "Design")

## Conclusion

In this RFC a high-level overview of the ticketing platform, it's components, technology stack and architecture is briefly discussed as a primer for a more detailed design.