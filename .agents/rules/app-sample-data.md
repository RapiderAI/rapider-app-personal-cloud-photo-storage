# App Sample Data Rules & Zeroed-Out ID Standard

When generating a Rappider application or acting as the App Builder Harness, you must ensure that sample data is generated for the application if it is missing or incomplete.

## Requirements

1. **Check for existing sample data**: Before concluding an app generation task or QA pass, verify if a `sample-data.json` exists in the application root.
2. **Generate if missing**: If `sample-data.json` is missing or empty, you MUST generate it.
3. **Compatibility**: The sample data must exactly match the data models (`dataModels`) defined in the `app.manifest.json`.
4. **Structure**: 
   - The JSON must be an object where keys are the `name` (or `datatableName`) of the canonical data models.
   - The values must be arrays containing rich, realistic, domain-specific sample records.
   - Each record must include a valid `id` and `_id`.
   - The data must be realistic and reflect the purpose of the application.

5. **MANDATORY CONVENTION: Zeroed-Out Initial 8-Character Block with Truly Random Remaining Digits (`00000000-xxxx-xxxx-xxxx-xxxxxxxxxxxx`)**:
   - **CRITICAL PREFIX REQUIREMENT:** All sample data record IDs (`id` and `_id`) **MUST zero out the initial 8-character block** of the UUID (`00000000-`).
   - **MANDATORY RANDOMIZATION OF REMAINING 24 DIGITS:**
     - The remaining chunks (`-xxxx-xxxx-xxxx-xxxxxxxxxxxx`, matching standard UUID v4 format `-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{12}`) **MUST BE TRULY RANDOM HEXADECIMAL DIGITS**.
     - **STRICT BAN ON ALL-ZERO, SEQUENTIAL OR REPETITIVE CHUNKS:**
       - NEVER use sequential zero-padded IDs (e.g., `00000000-0000-0000-0000-000000000001`, `00000000-0000-0000-0000-000000000002`).
       - NEVER use incrementing zero patterns (e.g., `00000000-0001-4000-a000-000000000001`, `00000000-0002-4000-a000-000000000001`).
       - NEVER use repetitive digit sequences (e.g., `00000000-1111-4111-8111-111111111111`, `00000000-aaaa-4aaa-aaaa-aaaaaaaaaaaa`).
     - **Why this ban is critical:** When multiple data models, tables, or microapplications generate sample records, all-zero or sequential IDs (like `...000000000001`) inevitably collide, causing duplicate key constraint violations and cross-table reference corruption in shared databases.
   - **Valid Format Examples:**
     - `00000000-74c6-417b-b141-468b9dbf3d5e`
     - `00000000-a8f3-4e2b-9c1d-8f7a6b5c4d3e`
     - `00000000-e3b9-4f81-a62c-5d1e4b8a9f02`
     - `00000000-2d8a-4c91-b3f5-7e1a6c0d8b4e`
   - **Recommended Generator Function (JavaScript):**
     ```javascript
     const crypto = require('crypto');
     function generateSampleUuid() {
       return '00000000-' + crypto.randomUUID().slice(9);
     }
     ```
   - **Foreign Key Consistency:** Any foreign key or reference field linking to another sample entity (e.g. `accountId`, `categoryId`, `ownerId`, `customerId`, `parentId`, `sprintId`, `epicId`) **MUST** use the target sample record's matching `00000000-...` ID.
   - **Operational Purpose:** This convention allows backend jobs, migration scripts, and maintenance tasks to cleanly identify and delete all sample data using a regex filter (`/^00000000-/`) without risk of deleting real user-created or tenant data.

6. **MANDATORY LIVE SEEDER INTEGRATION (NO STATIC MOCK HTML)**:
   - Sample data in `sample-data.json` is meant to be **seeded into the database**, NOT pasted as static HTML mock rows in page templates.
   - Pages must support a 1-click "Seed Demo Records" action in their empty state to populate the Rapider project database via `window.rapiderApi.create(entityName, sampleRecord)`.
   - The CLI runner `scripts/seed-sample-data.js` must be available to programmatically seed all canonical entities into the active project database.

By enforcing this rule, the Rapider Workflow/App Builder harness guarantees that every app can be immediately previewed with real database data, while maintaining data hygiene and safe sample data purging.

