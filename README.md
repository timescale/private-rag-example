# Local RAG Example

[Watch the video tutorial here](https://youtu.be/-ikCYKcPoqU)
[Read the blog post using Mistral here](https://www.timescale.com/blog/build-a-fully-local-rag-app-with-postgresql-mistral-and-ollama/)

This repository contains an example project for building a private Retrieval-Augmented Generation (RAG) application using Llama3.2, Ollama, and PostgreSQL. It demonstrates how to set up a RAG pipeline that does not rely on external API calls, ensuring that sensitive data remains within your infrastructure.

## Prerequisites

 * Docker
 * Python, [psycopg](https://www.psycopg.org/)
 * [Ollama](https://github.com/ollama/ollama)
 * [PostgreSQL](https://docs.timescale.com/self-hosted/latest/install/installation-docker/), [pgai](https://github.com/timescale/pgai)

## Docker Setup

* Create a network through which the Ollama and PostgreSQL containers will interact:

  `docker network create local-rag`

* [Ollama](https://hub.docker.com/r/ollama/ollama) docker container: (Note: [`--network`](https://docs.docker.com/engine/network/) tag to make sure that the container runs on the network defined)

  `docker run -d --network local-rag -v ollama:/root/.ollama -p 11434:11434 --name ollama ollama/ollama` 

     * [Llama3.2](https://www.llama.com/docs/model-cards-and-prompt-formats/llama3_2): `docker exec -it ollama ollama pull llama3.2`
     * [Mistral](https://mistral.ai/news/announcing-mistral-7b/?ref=timescale.com): `docker exec -it ollama ollama pull mistral`
     * [Nomic Embed v1.5](https://www.nomic.ai/blog/posts/nomic-embed-matryoshka?ref=timescale.com): `docker exec -it ollama ollama pull nomic-embed-text`

* [TimescaleDB](https://docs.timescale.com/self-hosted/latest/install/installation-docker/): `docker run -d --network local-rag --name timescaledb -p 5432:5432 -e POSTGRES_PASSWORD=password timescale/timescaledb-ha:pg16`

     * [pgai](https://github.com/timescale/pgai?tab=readme-ov-file#use-a-timescale-cloud-service): `CREATE EXTENSION IF NOT EXISTS "ai" VERSION '0.4.0' CASCADE;` (also installs [`pgvector`](https://github.com/pgvector/pgvector) and `plpython3`)


 
