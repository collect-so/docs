---
sidebar_position: 5
---
# Nesting

```mermaid
graph TB
    U0[RECORD:USER] --> B0[RECORD:BOOK]
    U0 --> B1[RECORD:BOOK]
    B1 --> V1[RECORD:VOLUME1]
    B1 --> V2[RECORD:VOLUME2]
```
