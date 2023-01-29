ARG PYTHON_VERSION=3.11.1
ARG PYTHON_BUILD_VERSION=$PYTHON_VERSION-buster

FROM python:${PYTHON_VERSION} AS build

ENV PYTHONDONTWRITEBYTECODE 1
ENV PYTHONUNBUFFERED 1

ENV VIRTUAL_ENV=/opt/venv

STOPSIGNAL SIGQUIT

RUN apt-get update -y && \
    apt-get install -y && \
    apt-get clean \
    && rm -rf /var/lib/apt/lists/*

RUN python -m venv $VIRTUAL_ENV
ENV PATH="$VIRTUAL_ENV/bin:$PATH"

COPY requirements requirements
RUN pip install --no-cache-dir -r requirements/requirements.txt

FROM python:${PYTHON_VERSION}-slim AS target

ARG USER_ID=1000
ARG GROUP_ID=1000
ENV PYTHONDONTWRITEBYTECODE 1
ENV PYTHONUNBUFFERED 1

ENV VIRTUAL_ENV=/opt/venv
ENV APP_DIR=/opt/app

RUN apt-get update -y && \
    apt-get install -y && \
    apt-get clean \
    && rm -rf /var/lib/apt/lists/*

RUN mkdir -p $APP_DIR
WORKDIR $APP_DIR

RUN groupadd -g $GROUP_ID -o nonroot   \
    && useradd -m -u $USER_ID -g nonroot nonroot \
    && chown -R nonroot $APP_DIR

COPY --chown=nonroot:nonroot --from=build $VIRTUAL_ENV $VIRTUAL_ENV
ENV PATH="$VIRTUAL_ENV/bin:$PATH"

COPY --chown=nonroot:nonroot src .

USER nonroot

EXPOSE 8000

ENTRYPOINT ["sh", "./entrypoint.sh"]
