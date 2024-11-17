## Streaming aggregates
### Explanation
- The **Bronze** layer ingests and stores raw data into the `invoices_bz` table with a checkpointed state store for reliability.
- The **Gold** layer performs **streaming aggregation** (e.g., sum and derived calculations) using a state store to maintain partial results grouped by `CustomerCardNo`.
- The `.outputMode("complete")` in the Gold layer writes the entire aggregated table to the `customer_rewards` table during every trigger. This mode is a **heavy operation**, as it outputs all rows (not just updates), making it **unsuitable for streams with millions of rows**. However, it works effectively for streams with **thousands of rows**.
- Checkpointing ensures that the **state store** persists across micro-batches and stream restarts, enabling fault tolerance and consistent streaming processing.