FROM librdkafka:latest as librdkafka
FROM python:3

ENV SERVICE_NAME=product_service

WORKDIR /usr/local/lib
COPY --from=librdkafka /usr/local/lib/librdkafka* ./
WORKDIR /usr/local/include/librdkafka
COPY --from=librdkafka /usr/local/include/librdkafka/ ./
WORKDIR /

COPY ./requirements.txt ./requirements.txt
RUN pip install -r ./requirements.txt

WORKDIR /app
COPY /${SERVICE_NAME}/*.py ${SERVICE_NAME}/
COPY /lib/*.py lib/

ENV KAFKA_CONFIG "{\"bootstrap.servers\": \"kafka:29092\"}"

ENV FLASK_ENV development
ENV FLASK_APP /app/${SERVICE_NAME}/${SERVICE_NAME}.py

ENV LD_LIBRARY_PATH /usr/local/lib
ENV PYTHONPATH /app
EXPOSE 5000
CMD [ "flask", "run", "--host=0.0.0.0" ]
