FROM python:3.11.4-slim as builder

WORKDIR /app


ENV PYTHONDONTWRITEBYTECODE 1
ENV PYTHONUNBUFFERED 1
ENV HNSWLIB_NO_NATIVE=1 


# Update apt and install gcc
RUN apt-get update && apt-get install -y --no-install-recommends gcc g++

COPY requirements.txt .

RUN pip wheel --no-cache-dir --no-deps --wheel-dir /app/wheels -r requirements.txt

FROM python:3.11.4-slim

WORKDIR /app

RUN addgroup --gid 1001 --system chatbot && \
    adduser --no-create-home --shell /bin/false --disabled-password --uid 1001 --system --group chatbot


COPY --from=builder /app/wheels /wheels

RUN pip install --no-cache-dir $(find /wheels -name "*.whl")

COPY ./app /app

RUN chown chatbot:chatbot /app -R

USER chatbot

CMD ["python", "botscript.py"]


