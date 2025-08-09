
# SFL Approved Builds Verification Bot - Full Context Summary

## Project Goal
Build a TypeScript Discord bot to manage guild event build approvals with:
- Dual user roles (Applicants & Approvers)
- Automated screenshot validation with item/IP verification
- In-game name matching via OCR
- Dynamic build configuration management

## Core Requirements

### User Types
**Applicants:**
- Submit builds via screenshots
- Check approval status
- Resubmit after rejection

**Approvers:**
- Manage approval records
- Update build references
- Acknowledge permission updates

### Technical Requirements
- Automated screenshot validation (ignoring border variations)
- IP value verification
- Per-server username confirmation via OCR
- Manual approver acknowledgment system
- Dynamic build configuration updates

## Workflow Summary

### Applicant Workflow
1. `/build-review list` - View approved builds
2. `/build-review submit build_id:<id> screenshot_url:<url>` - Submit build
3. Receive auto-approval or rejection DM with specific fixes
4. `/build-review resubmit` - Fix and resubmit rejected builds

### Approver Workflow
1. `/build-review pending-acks` - View completed approvals needing acknowledgment
2. Update user permissions manually
3. `/build-review acknowledge submission_id:<id>` - Confirm permissions updated
4. `/build-review update build_id:<id>` - Modify reference builds

## Workflow Visualization

```mermaid
flowchart TB
    subgraph Applicant Workflow
        A[List Builds] --> B[Submit Build]
        B --> C{Validation}
        C -->|Approved| D[Auto-approval DM]
        C -->|Rejected| E[Rejection DM with fixes]
        E --> F[Resubmit]
        F --> C
    end

    subgraph Approver Workflow
        G[Pending Acks] --> H[Manual Permission Update]
        H --> I[Acknowledge]
        I --> J[Permissions Confirmed]
        K[Update Build] --> L[Build Reference Modified]
    end

    C -->|Notification| G
    D -->|Notification| G
```

## Technical Implementation

### Key Technologies
- Discord.js: Bot framework
- SQLite: Build/submission storage
- Jimp: Image processing
- Tesseract.js: OCR for username/IP extraction
- TensorFlow.js: Image similarity comparison (optional)

### Configuration Schema
```json
{
  "builds": [
    {
      "id": 101,
      "description": "Tank Build - Frost Staff + Ice Heart",
      "screenshot": "https://imgur.com/tank_build.png",
      "items": [
        {
          "name": "Frost Staff",
          "ipRange": [1200, 1500],
          "position": {
            "x": 100,
            "y": 200,
            "width": 50,
            "height": 50
          }
        }
      ]
    }
  ]
}
```

### Critical Components
**Screenshot Validation:**
```typescript
async function validateScreenshot(userImageUrl, buildId, username) {
  // 1. Verify username match via OCR
  // 2. Extract IP value
  // 3. Validate items focusing on center area
  // 4. Return { approved, missingItems }
}
```

**Build Update Command:**
```typescript
export async function handleUpdateBuild(interaction) {
  const attachment = interaction.options.getAttachment('new_screenshot');
  const newItems = await detectItems(attachment.url);
  updateBuildConfig(buildId, {
    screenshot: attachment.url,
    items: newItems
  });
}
```

**OCR Service:**
```typescript
export async function ocrScan(image) {
  const worker = await createWorker('eng');
  const { data } = await worker.recognize(image);
  return data.text;
}
```

### Project Structure
```
/src
├── bot
│   ├── commands.ts   # Slash command handlers
│   └── events.ts     # Button/modal handlers
├── core
│   ├── validation.ts # Image/IP/username checks
│   └── ocr.ts        # Text extraction
├── config
│   ├── manager.ts    # Build configuration loader
│   └── schemas.ts    # Type definitions
├── database          # Submission storage
└── utilities         # Helper functions
```

## Next Development Steps
1. Implement OCR integration for username/IP extraction
2. Develop item position detection algorithm
3. Build border-tolerant image comparison
4. Create DM notification pipelines
5. Add role-based permission system
6. Implement audit logging

## Security Considerations
- Validate all image URLs
- Sanitize OCR input/output
- Implement size limits for uploads
- Add role-based command permissions
- Prevent permission escalation

## Dependencies Required
```bash
npm install discord.js better-sqlite3 jimp tesseract.js
npm install -D @types/jimp @tensorflow/tfjs-node
```

> This document contains all architectural decisions, technical specifications, and workflow details discussed. You can directly copy this context to continue development in another environment.
