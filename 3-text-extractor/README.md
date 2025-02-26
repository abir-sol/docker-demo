# Docker Compose Text Data Extraction Service

This project demonstrates how to deploy a multi-container application using Docker Compose. In this demo, we are building a system where users can input natural language text, and the service processes the input using the OpenAI API, extracts structured data, and stores it in a PostgreSQL database. Here's the project overview:

1. **Frontend**: A Streamlit app for users to input natural language text.
2. **Backend**: A FastAPI app that handles the API endpoints and data processing.
3. **Service**: A backend service that calls the OpenAI API to process the text.
4. **Database**: A PostgreSQL database that stores the structured data.


## Project Structure

```
docker-demo/text-extraction/
├── backend/
│   ├── main.py             # FastAPI backend and service combined
│   ├── extractor.py        # NLP extraction service code
│   ├── Dockerfile          # Dockerfile for backend
│   └── requirements.txt    # Python dependencies
├── frontend/
│   ├── app.py              # Streamlit frontend
│   ├── Dockerfile          # Dockerfile for frontend
│   └── requirements.txt    # Python dependencies
├── docker-compose.yml      # Docker Compose configuration
├── .env                    # Environment variables (OpenAI API keys, DB credentials)
└── README.md
```

## Running with Docker

1. **Build and run the Docker Containers:**

   Navigate to the project directory and run:

   ```bash
   docker compose up --build -d
   ```

   > Note: You must be in the same directory as the `docker-compose.yml` file.

   This single command will build and start up all the containers: the frontend, backend, PostgreSQL database, and the NLP service.


## Accessing the Application

1. **Frontend:**
   - Open your browser and go to `http://localhost:8501` to access the Streamlit frontend.
   - Enter natural language text and click **Process Text** to extract structured data.

2. **Backend and Service:**
   - The backend API will be running on `http://localhost:8000` and is responsible for handling requests and processing data using the NLP service.
   
3. **PostgreSQL Database:**
   - The database is running in the `postgres` container.
   - You can connect to it with your preferred Postgres client using `localhost:5432` and the credentials defined in the `.env` file.


## Checking Container Status

To check if your containers are running, use the following command:

```bash
docker ps
```

To check docker compose logs in real-time:
```bash
docker compose logs -f
```

To access the shell of a specific container, use:

```bash
docker exec -it <container_id_or_name> bash
```

For example, to enter the backend container, you can run:

```bash
docker exec -it 3-text-extraction-backend_api-1 bash
```


## Cleaning Up

To stop and remove the containers:

```bash
docker-compose down
```

To remove all images associated with this project:

```bash
docker-compose down --rmi all
```
