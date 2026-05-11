# Data Modeling

Data model defines how the data is:

- structured: what entities exist?
- stored: how you find them?
- related: how do they connect one to another?

## System design thinking

Hello interview delivery framework

1. Requirements
   - Functional requirements
   - Non functonal requirements
2. Core entities
3. API or Interface
4. Data flow
5. High-level design
6. Deep dives

## Database model options

1. Relational db: usually the best choice
   - Postgres
2. Document db
   - Choose this only if you want to support schema flexibility
3. Key-value store: redis, dynamodb
   - Similar to a big hash table
   - Useful for cache that sits in front of the db
   - Data needs to be duplicated for many different functionalities
4. Wide-column databases
5. Graph db
