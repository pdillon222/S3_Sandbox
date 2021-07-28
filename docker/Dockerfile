FROM ubuntu:20.04

LABEL maintainer="<your-email-here"

RUN apt-get update -y && apt-get install -y \
    python3.8-dev \
    python3-pip \
    python-dev

VOLUME /s3

WORKDIR /s3

COPY s3_assets/requirements.txt /s3/requirements.txt

RUN pip3 install -r requirements.txt

COPY s3_assets /s3

ENTRYPOINT [ "python3" ]

CMD [ "inf_run.py" ]
