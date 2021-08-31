FROM python:slim

# Bundle app source
COPY . /usr/src/app
WORKDIR /usr/src/app

# Install requirements
RUN pip install --no-cache-dir -r requirements.txt 

# Run flask
CMD ["python", "Binance_Detect_Moonings.py"]