
# New Point System Proposal - Option B (Silver-Centric)

## Core Systems (Existing)
### `Activity Points System`
- **Role**: Manual AP tracking only (non-event/mission activities)
- **Key Changes**:
  - No longer receives automated AP
  - Converts AP in Silver

### `Silver Distribution System`
- **New Role**: Aggregates Converted Avocados + Converted Bananas + Converted AP

## New Currency Systems
### `Event-based Avocado System` *(Replaces Event-based AP Sytem)*
- **Output**: Converted Avocados in Silver
- **Variants (from Option A)**:
  - *Group Dungeons Events* → Avocados
  - *Avalonian Dungeons Events* → Avocados  
  - *Tracking Events* → Avocados
  - *Roaming Events* → Avocados

### `Mission-based Banana System` *(Replaces Mission-based AP System)*
- **Output**: Converted Bananas in Silver
- **Variants (from Option A)**:
  - *Transport Missions* → Bananas
  - *Farming Missions* → Bananas
  - *Gathering Missions* → Bananas
  - *Refining Missions* → Bananas  
  - *Crafting Missions* → Bananas

## Integration Flow
```mermaid
flowchart TD
    subgraph AvocadoSystem["Event-based Avocado Sys"]
        direction TB
        A_Base[Base]
        A_Group[Group Dungeons]
        A_Avalon[Avalonian Dungeons]
        A_Track[Tracking]
        A_Roam[Roaming]
    end

    subgraph BananaSystem["Mission-based Banana Sys"]
        direction TB
        B_Base[Base]
        B_Transport[Transport]
        B_Farm[Farming]
        B_Gather[Gathering]
        B_Refine[Refining]
        B_Craft[Crafting]
    end

    AvocadoSystem -->|Converted Avocados in Silver| Silver[Silver Distribution]
    BananaSystem -->|Converted Bananas in Silver| Silver
    AP[Activity Points System] -->|Converted AP in Silver| Silver
    Admin[One Time Operation] --> AP

    classDef avocado fill:#e3f9d2,stroke:#333
    classDef banana fill:#fde2e2,stroke:#333
```

## Key Differences from Option A
- **Currency Conversion**: 
  - Events → Avocados → Silver
  - Missions → Bananas → Silver
  - Activity Points → Silver
- **AP System**: Now handles **only** manual inputs
- **Silver**: Unified payout currency
