# Private RAG Example

This repository contains an example project for building a private Retrieval-Augmented Generation (RAG) application using Llama3.2, Ollama, and PostgreSQL. It demonstrates how to set up a RAG pipeline that does not rely on external API calls, ensuring that sensitive data remains within your infrastructure.

## Prerequisites

 * Docker
 * Python, psycopg
 * [Ollama](https://github.com/ollama/ollama)
 * [PostgreSQL](https://docs.timescale.com/self-hosted/latest/install/installation-docker/), [pgai](https://github.com/timescale/pgai)

## Docker Setup

* Create a network through which the Ollama and PostgreSQL containers will interact with: `docker network docker network create rag-net`

* [Ollama](https://hub.docker.com/r/ollama/ollama) docker container: `docker run -d --network rag-net -v ollama:/root/.ollama -p 11434:11434 --name ollama ollama/ollama` (Note: `--network` tag to make sure that the container runs on the network defined)

     * Llama3.2: `docker exec -it ollama ollama pull llama3.2`
     * Nomic Embed v1.5: `docker exec -it ollama ollama pull nomic-embed-text`

* [TimescaleDB](https://docs.timescale.com/self-hosted/latest/install/installation-docker/): `docker run -d --network rag-net --name timescaledb -p 5432:5432 -e POSTGRES_PASSWORD=password timescale/timescaledb-ha:pg16`

     * [pgai](https://github.com/timescale/pgai?tab=readme-ov-file#use-a-timescale-cloud-service): `CREATE EXTENSION IF NOT EXISTS ai CASCADE;` (also installs `pgvector` and `plpython3`)


 
