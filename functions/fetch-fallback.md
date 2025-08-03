## **Fallback Logic for API (For When Data Isn’t in Cosmos DB)**

## **1. Check if Record Exists in Cosmos DB**
- When an API request is made to retrieve a billing record:
  - First, check if the record is present in **Cosmos DB**.

## **2. Check Blob Storage if Not Found**
- If the record is not found in Cosmos DB :
  - Check **Azure Blob Storage** for the archived data.
  - If the data is found in Blob Storage, return it in the API response.
  - If the data isn’t found, return **404 Not Found** response.
