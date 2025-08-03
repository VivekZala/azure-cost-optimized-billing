# Azure Cost Optimized Billing System

This repository contains the design and implementation details of an optimized cost-saving solution for a serverless architecture using Azure, where billing records are stored in Cosmos DB.

## **Solution Overview**

The solution is designed to:
- **Optimize costs** by archiving old data (older than 90 days) to Blob Storage.
- Keep **recent data** in Cosmos DB for efficient access.
- Provide **seamless data access** for cold data without affecting the API contract.

## **Architecture**

### 1. **Current Architecture**
- Billing records are stored in Azure Cosmos DB.
- The system is read-heavy, but old records are rarely accessed.

### 2. **Proposed Architecture**
- Archive records older than 90 days to Blob Storage.
- Cosmos DB will hold only recent records, reducing storage costs.

## **Steps to Implement**
1. Review the **Solution Evaluation** for various approaches to data optimization.
2. Implement the **Archive Function** for moving old records.
3. Implement the **Fallback Function** to retrieve cold data from Blob Storage.

## **How to Deploy**

1. Set up the **Cosmos DB** and **Blob Storage** in your Azure environment.
2. Deploy **Azure Functions** using your preferred method.
3. Apply the **Blob Lifecycle Policy** using the provided script.

For full implementation details, see the individual files and pseudocode.
