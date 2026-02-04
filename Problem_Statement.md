Here is a case study designed to test senior-level competencies, followed by a submission guide that mirrors a professional technical take-home.

---

### Part 1: The Case Study

**Title:** Scaling the Passenger Experience Feedback Loop

**Business Context:**
The airline’s leadership team has noticed a dip in satisfaction. Currently, survey data is processed in monthly batches using manual Excel files. This prevents the operational teams (Gate, In-flight, Ground Staff) from reacting to issues in real-time.

**The Mission:**
Design and implement an **ELT (Extract, Load, Transform)** pipeline that ingests raw passenger survey data, applies a robust data model for downstream analytics, and exposes "Satisfaction Alert" views for operational stakeholders.

**The Dataset:** [Maven Airline Satisfaction](https://mavenanalytics.io/data-playground/airline-passenger-satisfaction)

* **Scale:** ~130k records (Assume this will grow to 10M+ in your design).
* **Key Attributes:** ID, Demographics, Flight Distance, Service Ratings (1-5), Delays, and Overall Satisfaction.

---

### Part 2: Senior DE Requirements

Your solution must address these advanced engineering concerns:

1. **Architecture:** Choose a modern stack (e.g., S3/Azure Blob → Snowflake/BigQuery/Databricks → dbt).
2. **Schema Evolution:** How will you handle the addition of new survey questions (e.g., "Food Quality" being split into "Meal Quality" and "Snack Quality")?
3. **Data Modeling:** Move away from the flat CSV. Implement a **Star Schema** (Fact and Dimension tables) to optimize for BI tools.
4. **Data Quality (DQ):** Implement automated checks (e.g., ratings must be 0–5, ID must be unique, Arrival Delay cannot be null).

---

### Part 3: Submission Guide (The "Portfolio" Structure)

A senior candidate doesn't just submit code; they submit a **System Design Document**. Structure your GitHub README or presentation as follows:

#### 1. Architecture Diagram

* **Ingestion:** Source (S3) → Ingestion Tool (Airflow/Fivetran/Copy Into).
* **Storage:** The Raw/Bronze layer (unmodified CSV data).
* **Transformation:** The Silver/Gold layers (Cleaned and Star Schema).
* **Orchestration:** How is the pipeline triggered? (e.g., "An Airflow DAG running daily").

#### 2. Data Model Design

Explain your Star Schema. For this dataset, a senior approach would be:

* **`fact_survey_responses`**: Contains `id`, `flight_distance`, `arrival_delay`, `departure_delay`, and FKs to dimensions.
* **`dim_passengers`**: `id`, `gender`, `customer_type`, `age`.
* **`dim_flight_details`**: `id`, `travel_type`, `class`.
* **`dim_service_ratings`**: (Optional: pivot the ratings into a long format for easier trend analysis).

#### 3. dbt (or SQL) Transformation Logic

Showcase "Senior" SQL. Don't just `SELECT *`.

* **Handling Nulls:** How did you handle the 300+ missing values in `Arrival Delay`? (e.g., imputing with 0 or using the median).
* **Business Logic:** Create a "Weighted Satisfaction Score" or a "Service Index" (averaging the 14 service columns).

#### 4. Data Quality & Testing

List your test cases:

* **Uniqueness:** `id` is primary.
* **Accepted Values:** `Class` must be in ('Business', 'Eco', 'Eco Plus').
* **Relationship:** Every `fact` record must have a corresponding `dim_passenger` record.

#### 5. "The Senior Edge" (Discussion Points)

Add a section titled **"Production Considerations"**:

* **Partitioning:** "I would partition the data by `Survey_Date` to optimize query performance as the dataset scales."
* **CI/CD:** "I would use GitHub Actions to run dbt tests before any code is merged to production."
* **Monitoring:** "I would set up an alert if the percentage of 'Satisfied' passengers drops below a specific threshold (Data Observability)."

---

### Interview Tip: The "Why"

In a senior interview, the **"Why"** is more important than the **"How."**

* **Why ELT over ETL?** (Scalability of modern cloud warehouses).
* **Why a Star Schema?** (Performance for BI tools like Tableau/PowerBI).
* **Why dbt?** (Version control, documentation, and testing for SQL logic).