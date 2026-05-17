# Resources
Gathering useful resources for engineering practice and use in TP-Coder Innovation Hub

พื้นที่สำหรับเก็บข้อมูลที่เกี่ยวข้องกับการพัฒนาซอฟต์แวร์และใช้ใน TP-Coder Innovation Hub

## ADR (Architecture Decision Records)

Architecture Decision Records (ADRs) document important architectural decisions made in the project.

เอกสารบันทึกการตัดสินใจสำคัญด้านสถาปัตยกรรมของโปรเจค

### How to use ADR

ในทุก ๆ โปรเจค หากจะเริ่มเขียนโค้ดใด ๆ ให้ทำการบันทึกการออกแบบและความคิดของเราเองลงใน ADR เพื่อ mitigating เรื่องต่าง ๆ เช่น business impact, technical requirements, and other relevant factors.

### Template

| File | Description | Location | Reference |
|------|-------------|----------|-----------|
| 0000-template.md | ADR Template 1 | [adr/0000-template.md](adr/0000-template.md) | [decision-record-template-by-michael-nygard](https://github.com/joelparkerhenderson/architecture-decision-record/tree/main/locales/en/templates/decision-record-template-by-michael-nygard) |
| 0001-template.md | ADR Template 2 | [adr/0001-template.md](adr/0001-template.md) | - |

## Fundamentals

Quick references for building real-world projects. Keep these handy when working on any challenge.

ข้อมูลอ้างอิงฉบับย่อสำหรับการสร้างโปรเจคจริง ควรเปิดดูควบคู่ไปเมื่อทำ Challenge ต่างๆ

| Topic | File | What it covers |
|-------|------|----------------|
| System Design | [system-design-fundamentals.md](system-design-fundamentals.md) | Load balancers, caching, DB types, message queues, CDN, API styles, observability |
| Database Schema Design | [database-schema-design.md](database-schema-design.md) | Data modeling, relationships, normalization, data types, indexing |
| API Design | [api-design/](api-design/) | REST, GraphQL, gRPC, WebSocket — when to use each, with examples |
| Testing | [testing-fundamentals.md](testing-fundamentals.md) | Testing pyramid, test types, mocking, naming, coverage |
| Security | [security-basics-checklist.md](security-basics-checklist.md) | OWASP Top 10, input validation, auth, SQL injection, XSS, secrets |
| Git & PR | [git-and-pr-conventions.md](git-and-pr-conventions.md) | Branch naming, commit messages, PR template, review checklist |
| Cost Estimation | [cost-estimation-guide.md](cost-estimation-guide.md) | Cloud pricing models, back-of-envelope calculation, AWS pricing reference |

## Code Style

This section contains coding standards and style guides for different programming languages used in our projects.

ส่วนรวบรวมมาตรฐานการเขียนโค้ดและสไตล์ไกด์สำหรับภาษาต่างๆ ที่ใช้ในโปรเจค

| Language | Style Guide / Formatter | Source |
|----------|-------------------------|--------|
| **General** | Google Style Guides | https://github.com/google/styleguide |
| **Java** | Palantir Java Format (via Spotless) | https://github.com/palantir/palantir-javaformat |
| **JavaScript / TypeScript** | Airbnb Style Guide / Prettier | https://github.com/airbnb/javascript |
| **Python** | PEP 8 / Black / Ruff | https://peps.python.org/pep-0008/ |
| **C#** | Microsoft C# Conventions | https://learn.microsoft.com/en-us/dotnet/csharp/fundamentals/coding-style/coding-conventions |
| **Go** | Built-in Standard (`gofmt`) | https://go.dev/doc/effective_go |
| **Rust** | Built-in Standard (`rustfmt` / `cargo fmt`) | https://rust-lang.github.io/api-guidelines/ |

