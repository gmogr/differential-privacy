# Recommendation: Use Debian 12.
# FROM debian:12-slim

# FROM us-central1-docker.pkg.dev/cloud-dataproc/spark/dataproc_2.0:latest

# Suppress interactive prompts
ENV DEBIAN_FRONTEND=noninteractive

# Install utilities required by Spark scripts.
RUN apt update && apt install -y procps tini libjemalloc2

# Enable jemalloc2 as default memory allocator
ENV LD_PRELOAD=/usr/lib/x86_64-linux-gnu/libjemalloc.so.2

# Create the 'spark' group/user.
# The GID and UID must be 1099. Home directory is required.
RUN groupadd -g 1099 spark
RUN useradd -u 1099 -g 1099 -d /home/spark -m spark

ENV SPARK_EXTRA_JARS_DIR=/opt/spark/jars/
ENV SPARK_EXTRA_CLASSPATH='/opt/spark/jars/*'
RUN mkdir -p "${SPARK_EXTRA_JARS_DIR}" \
    && chown spark:spark "${SPARK_EXTRA_JARS_DIR}"

COPY --chown=spark:spark \
    ./target/spark-1.0-SNAPSHOT-jar-with-dependencies.jar "${SPARK_EXTRA_JARS_DIR}"

USER spark