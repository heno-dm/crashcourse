FROM mcr.microsoft.com/devcontainers/cpp:1-debian-12

RUN apt-get update \
    && apt-get install -y --no-install-recommends clang-format valgrind \
    && rm -rf /var/lib/apt/lists/*
