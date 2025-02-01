FROM python:3.10-slim
WORKDIR /code
ENV PORT 8000
ENV DIFFBOT_API_KEY=""
ENV GROQ_API_KEY=""
ENV EMBEDDING_MODEL="all-MiniLM-L6-v2"
ENV RAGAS_EMBEDDING_MODEL="openai"
ENV IS_EMBEDDING="true"
ENV # Enable Gemini (default is False) | Can be False or True
ENV GEMINI_ENABLED=False
ENV # Enable Google Cloud logs (default is False) | Can be False or True
ENV GCP_LOG_METRICS_ENABLED=False
ENV AWS_ACCESS_KEY_ID= ""
ENV AWS_SECRET_ACCESS_KEY=""
ENV LANGCHAIN_API_KEY=""
ENV LANGCHAIN_PROJECT=""
ENV LANGCHAIN_TRACING_V2=""
ENV LANGCHAIN_ENDPOINT=""
ENV GCS_FILE_CACHE="False" #save the file into GCS or local, SHould be True or False
ENV NEO4J_USER_AGENT=""
ENV ENABLE_USER_AGENT=""
ENV ENTITY_EMBEDDING="True" True or False
ENV YOUTUBE_TRANSCRIPT_PROXY= ""
EXPOSE 8000
# Install dependencies and clean up in one layer
RUN apt-get update && \
   apt-get install -y --no-install-recommends \
       libmagic1 \
       libgl1-mesa-glx \
       libreoffice \
       cmake \
       poppler-utils \
       tesseract-ocr && \
   apt-get clean && \
   rm -rf /var/lib/apt/lists/*
# Set LD_LIBRARY_PATH
ENV LD_LIBRARY_PATH=/usr/lib/x86_64-linux-gnu:$LD_LIBRARY_PATH
# Copy requirements file and install Python dependencies
COPY requirements.txt /code/
# --no-cache-dir --upgrade 
RUN pip install -r requirements.txt 
# Copy application code
COPY . /code
# Set command
CMD ["gunicorn", "score:app", "--workers", "8","--threads", "8", "--worker-class", "uvicorn.workers.UvicornWorker", "--bind", "0.0.0.0:8000", "--timeout", "300"]
