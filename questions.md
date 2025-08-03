1. What are the exact read/write patterns for billing records? How many reads per second does the system handle?

2. How often do old records get accessed, even if rarely? Is there a seasonal spike in access to older records?

3. Are there specific metadata fields that must be preserved, apart from the data itself?

5. What is the current cost of Cosmos DB, and what cost reduction target are we aiming for?

6. How much cold data is expected to grow over time?

7. How critical is it to ensure no data loss during migration from Cosmos DB to Blob Storage?

8. Can the API remain completely unchanged, or should we adjust it to handle cold data retrieval?

9. How often should we re-evaluate the storage strategy as data grows?

10. How should we scale the solution if the volume of records increases significantly?

11. How flexible should the solution be in adjusting the TTL or handling changes in the Cosmos DB partitioning scheme?

12. Is there any pre-configured environment or tool available to mock Cosmos DB, Blob Storage, and Azure Functions locally to simulate the full solution?