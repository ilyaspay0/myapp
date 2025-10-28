# Utilise une image Python
FROM python:3.10

WORKDIR /app

# Copie les fichiers nécessaires
COPY requirements.txt .
RUN pip install -r requirements.txt

COPY . .

CMD ["python", "src/app.py"]
