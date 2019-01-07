FROM ubuntu:16.04

WORKDIR /
RUN apt-get update && apt-get install -y curl
COPY curler.sh .
RUN chmod u+x curler.sh
CMD ["./curler.sh"]

