# SpaceX Mission Analytics

**A MongoDB & Python analysis of SpaceX launch data — exploring reliability, reusability, and landing infrastructure.**

This project was completed as a personal assignment for the **NoSQL Lab** course at HSLU.

---

## TL;DR

Used the SpaceX REST API to pull launch, rocket, launchpad, and landing pad data into MongoDB. Built a full ELT pipeline in Python, cleaned and staged the data, then ran multiple aggregation pipelines to answer questions about launch success rates, booster reuse, and landing performance. Key finding: Falcon 9 has a 98.9% launch success rate across 178 missions, and SpaceX's most-reused boosters consistently land successfully, proving that rocket reusability works at scale.

---

## Tech Stack

- **Database**: MongoDB 7 (via Docker or Atlas)
- **Language**: Python (Jupyter / Quarto Notebook)
- **Key Libraries**: pymongo, pandas, matplotlib, requests, python-dotenv
- **Data Source**: [SpaceX REST API](https://github.com/r-spacex/SpaceX-API) (v4 & v5)
- **UI**: MongoDB Compass / Mongo Express

## Project Structure

```
├── docker-compose.yml          # MongoDB + Mongo Express (Docker setup)
├── .env                        # Connection strings and credentials
├── Personal_project.qmd        # Quarto notebook (source)
├── Personal_project.pdf        # Rendered PDF report
├── requirements.txt            # Python dependencies
├── Personal_project_files/
│   └── images/                 # Charts and class diagram
└── README.md
```

## Setup

### Option A: Docker (local)

1. Create a `.env` file in the project root:
   ```
   MONGO_USERNAME=<user>
   MONGO_PASSWORD=<password>
   ```

2. Start MongoDB and Mongo Express:
   ```bash
   docker compose up -d
   ```

3. Access points:
   - **MongoDB**: `mongodb://<user>:<password>@localhost:27017/`
   - **Mongo Express**: [http://localhost:8081](http://localhost:8081)

4. In the notebook, set `USE_DOCKER = True`.

### Option B: MongoDB Atlas (cloud)

1. Create a `.env` file with your Atlas connection string:
   ```
   COMPASS_URI=mongodb+srv://<user>:<password>@<cluster>.mongodb.net/
   ```

2. In the notebook, set `USE_DOCKER = False`.

### Running the Notebook

```bash
pip install -r requirements.txt
quarto render Personal_project.qmd --to pdf
```
OR just execute the cells one after another.

## Data Pipeline

The project follows an ELT (Extract, Load, Transform) approach:

1. **Extract**: Fetch JSON data from four SpaceX API endpoints (launches, rockets, launchpads, landing pads).
2. **Load**: Insert raw data into `*_raw` MongoDB collections.
3. **Transform**: Clean and stage data using MongoDB aggregation pipelines — converting date strings to BSON dates, filtering stale records, remapping IDs, and projecting relevant fields into `*_stage` collections.

## Analysis Highlights

- **6 aggregation pipelines** using `$lookup`, `$unwind`, `$group`, `$bucket`, `$filter`, `$setUnion`, `$cond`, `$addFields`, and correlated sub-pipelines
- **7 visualizations** (matplotlib bar charts, dual-axis time series)
- **Class diagram** of the staged MongoDB data model

## Author

**Azeglio Martinelli**