FROM python:3.9

RUN pip install pandas

WORKDIR /app

COPY pipeline.py pipeline2.py

ENTRYPOINT [ "python", "pipeline2.py" ]