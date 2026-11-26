services:
  back:
    ports: # (строки 4-7 свернуты)
      - "8000:8000"
    depends_on:
      db:
        condition: service_healthy

  db:
    image: postgres:18
    container_name: postgres_db
    restart: always
    volumes:
      - postgres_data:/var/lib/postgresql
    ports:
      - "5432:5432"
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U ${DB_USER} -d ${DB_NAME}"]
      interval: 5s
      timeout: 5s
      retries: 5

volumes:
  postgres_data:
