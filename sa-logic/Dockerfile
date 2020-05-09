# Followed recommendations for a virtual environment from
# https://pythonspeed.com/articles/multi-stage-docker-python/
# To get a 2-stage build working

FROM python:3.6.6 as python-app-builder
COPY . .
WORKDIR /sa
# Use a python virtual environment for isolation
RUN python -m venv /opt/venv
ENV PATH="/opt/venv/bin:$PATH"
RUN python -m pip install -r requirements.txt


FROM python:3.6.6-alpine
# Copy and set path for virtual environment form prior stage
COPY --from=python-app-builder /opt/venv /opt/venv
ENV PATH="/opt/venv/bin:$PATH"
COPY --from=python-app-builder /sa /sa
RUN python -m textblob.download_corpora
WORKDIR /sa
EXPOSE 5000
ENTRYPOINT ["python3"]
CMD ["sentiment_analysis.py"]
