#BUILD: docker build -t envconsul
#Edit config.hcl
#RUN: docker run -it --rm -v `pwd`/config.hcl:/app/config.hcl -e VAULT_TOKEN=${VAULT_TOKEN} -e VAULT_ADDR=${VAULT_ADDR} envconsul -config=/app/config.hcl -exec env

FROM debian:stretch AS base
LABEL maintainer="ecliptik@gmail.com"

ENV VERSION="0.7.3" \
    VAULT_TOKEN= \
    VAULT_ADDR=

WORKDIR /app

#Install required packages for build
RUN apt update && \
    apt install -y ca-certificates && \
    apt clean

#Build image
FROM base AS build

#Install required packages for build
RUN apt update && \
    apt install -y curl

#Download sourcecode
RUN curl -sSL https://releases.hashicorp.com/envconsul/${VERSION}/envconsul_${VERSION}_linux_amd64.tgz > ./envconsul.tgz && \
    tar -xvzf envconsul.tgz

#Run image
FROM base AS run

#Copy compiled binary to WORKDIR for runtime
COPY --from=build /app/envconsul /app/envconsul
RUN chmod +x /app/envconsul

ENTRYPOINT ["/app/envconsul"]
CMD ["-h"]
