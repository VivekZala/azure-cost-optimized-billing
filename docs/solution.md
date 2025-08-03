# **Solution: Combining Solution 1 and Solution 2 for Cost Optimization**

## **Overview**
The solution combines **Solution 1** (Data Tiering via Cold Storage) and **Solution 2** (Cosmos DB TTL + Change Feed Archival) to achieve efficient cost optimization for billing records in a serverless Azure architecture. This approach will move cold data (older than 90 days) from **Cosmos DB** to **Blob Storage** and reduce storage costs while maintaining seamless access to both hot and cold data.

## **Solution Approach**

### **How It Works**
1. **TTL in Cosmos DB**:
   - The **TTL (Time-to-Live)** feature in **Cosmos DB** is enabled, which automatically deletes records older than 90 days.
   
2. **Change Feed for Archival**:
   - **Cosmos DB Change Feed** is used to capture records that are about to be deleted due to TTL expiration.
   - An **Azure Function** is triggered by the Change Feed to move these records to **Azure Blob Storage** before deletion.

3. **Blob Storage for Cold Data**:
   - After the TTL expiry, the old records are stored in **Azure Blob Storage**, in an appropriate storage tier (Hot, Cool, or Archive), ensuring low-cost storage for cold data.

4. **Retrieving Cold Data**:
   - The system continues to check **Cosmos DB** first for records.
   - If a record is not found in **Cosmos DB** (because it was archived), the **metadata stored in Cosmos DB** (like the **Blob URL**) is used to retrieve the data from **Blob Storage**.
   - No changes are required to the existing system or API layer; the system will simply fall back to **Blob Storage** when needed, using the stored metadata.

### **Why This Approach Works**

#### **Cost Efficiency**:
- **Cosmos DB** is optimized for hot data (frequently accessed data). By archiving old, infrequently accessed records to **Blob Storage**, the overall storage costs are drastically reduced.
- **Blob Storage** is much cheaper compared to Cosmos DB, especially for cold and rarely accessed data.

#### **Simplicity & Maintenance**:
- The solution requires minimal changes to the existing system.
- **TTL** and **Change Feed** features are native to Cosmos DB, and **Azure Functions** provide an easy way to handle the archival process with minimal manual intervention.

#### **No Downtime or Data Loss**:
- The solution ensures no downtime, as the **TTL** feature handles data expiry automatically, and the **Change Feed** captures data before it’s deleted.
- There is no data loss as all records are archived to Blob Storage before deletion, and the system is always able to retrieve records if requested.

#### **No API Contract Changes**:
- The existing **read/write system** does not need to be altered. The system continues to check **Cosmos DB** first and use **metadata** stored in Cosmos DB (like the **Blob URL**) to fetch data from **Blob Storage** if needed. This ensures no changes to the **API contract** or business logic.

### **Detailed Workflow**
1. **Cosmos DB** stores the billing records.
2. **TTL** is configured on the container to delete records older than 90 days.
3. The **Change Feed** is used to capture records before they are deleted by TTL.
4. An **Azure Function** triggered by the Change Feed moves the data to **Blob Storage**.
5. When a record is requested, the system first checks **Cosmos DB**.
6. If the record is not found in **Cosmos DB**, the metadata stored in Cosmos DB (such as the **Blob URL**) is used to retrieve the data from **Blob Storage**.

### **Implementation Steps**
1. **Enable TTL in Cosmos DB**:
   - Configure TTL on the **Cosmos DB** container for 90 days.
   - This ensures automatic deletion of old records.

2. **Create Azure Function**:
   - Set up an **Azure Function** triggered by the **Cosmos DB Change Feed**.
   - This function will be responsible for archiving the records into **Blob Storage** before TTL deletion.

3. **Configure Blob Storage**:
   - Create a **Blob Storage Account**.
   - Archive the old records in **Blob Storage** under an appropriate tier (Hot, Cool, or Archive).

4. **Use Metadata for Cold Data Retrieval**:
   - No changes to the existing system or API. The system will use metadata (such as Blob URLs) stored in **Cosmos DB** to retrieve cold data from **Blob Storage** when needed.

---

### **Next Steps**
1. Configure **TTL** in **Cosmos DB**.
2. Implement the **Azure Function** to handle archival to **Blob Storage**.
3. Use metadata stored in **Cosmos DB** (e.g., **Blob URL**) to fetch cold data from **Blob Storage** without changes to the API layer.
4. Monitor the solution for efficiency and any potential performance issues as the system scales.
