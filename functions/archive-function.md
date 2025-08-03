# **Pseudocode for Archival Process**

## **1. Check for Expired Records**
- Monitor the **Change Feed** of Cosmos DB.
- For each record in the Change Feed, check if it’s expired by looking at its **expiry date**, which we can set in TTL settings.

## **2. Archive Expired Data to Blob Storage**
- If the record is expired:
  - Get the **document ID** of the record.
  - Retrieve the **data** of the document from Cosmos DB using the document ID.
  - **Upload the data** to Azure Blob Storage as a **JSON file**. We can name the file with the document ID for easy retrieval.
  - Place this archived data in a folder like `archived/` in Blob Storage.
  - Set the Blob Storage tier based on how infrequently the data will be accessed.

## **3. Delete the Expired Record from Cosmos DB**
- Once the data is successfully archived to Blob Storage, **delete the original record** from Cosmos DB to free up storage space.

## **4. Repeat for All Expired Records**
- Continue checking the Change Feed to find any additional expired records.
- Perform the same archival and deletion process for each expired record.