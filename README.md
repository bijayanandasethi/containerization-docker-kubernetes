# Stage 1: Build environment
FROM python:3.9-slim AS builder
WORKDIR /app
COPY requirements.txt .
RUN pip install --user --no-cache-dir -r requirements.txt

# Stage 2: Production environment (minimal footprint)
FROM python:3.9-slim AS runner
WORKDIR /app
COPY --from=builder /root/.local /root/.local
COPY app.py .

ENV PATH=/root/.local/bin:$PATH
EXPOSE 8080

USER 10001
CMD ["python", "app.py"]
