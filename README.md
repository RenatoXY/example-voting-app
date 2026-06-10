# Example Voting App

A simple distributed application running across multiple Docker containers.

## Getting started

Download [Docker Desktop](https://www.docker.com/products/docker-desktop) for Mac or Windows. [Docker Compose](https://docs.docker.com/compose) will be automatically installed. On Linux, make sure you have the latest version of [Compose](https://docs.docker.com/compose/install/).

This solution uses Python, Node.js, .NET, with Redis for messaging and Postgres for storage.

Run in this directory to build and run the app:

```shell
docker compose up
```

The `vote` app will be running at [http://localhost:8080](http://localhost:8080), and the `results` will be at [http://localhost:8081](http://localhost:8081).

Alternately, if you want to run it on a [Docker Swarm](https://docs.docker.com/engine/swarm/), first make sure you have a swarm. If you don't, run:

```shell
docker swarm init
```

Once you have your swarm, in this directory run:

```shell
docker stack deploy --compose-file docker-stack.yml vote
```

## Run the app in Kubernetes

The folder k8s-specifications contains the YAML specifications of the Voting App's services.

Run the following command to create the deployments and services. Note it will create these resources in your current namespace (`default` if you haven't changed it.)

```shell
kubectl create -f k8s-specifications/
```

The `vote` web app is then available on port 31000 on each host of the cluster, the `result` web app is available on port 31001.

To remove them, run:

```shell
kubectl delete -f k8s-specifications/
```

## Architecture

![Architecture diagram](architecture.excalidraw.png)

* A front-end web app in [Python](/vote) which lets you vote between two options
* A [Redis](https://hub.docker.com/_/redis/) which collects new votes
* A [.NET](/worker/) worker which consumes votes and stores them in…
* A [Postgres](https://hub.docker.com/_/postgres/) database backed by a Docker volume
* A [Node.js](/result) web app which shows the results of the voting in real time

## Notes

The voting application only accepts one vote per client browser. It does not register additional votes if a vote has already been submitted from a client.

This isn't an example of a properly architected perfectly designed distributed app... it's just a simple
example of the various types of pieces and languages you might see (queues, persistent data, etc), and how to
deal with them in Docker at a basic level.
---







## 🚀 Documentación del Pipeline CI/CD y Calidad (IE4 & IE5)

Este repositorio implementa un ciclo de vida automatizado mediante **GitHub Actions** para garantizar la trazabilidad, seguridad y gobernanza de la solución de microservicios.

### 1. Garantía de Calidad y Estabilidad (IE2)
* **Verificación de Entorno:** Cada `git push` o `Pull Request` activa un job aislado en un contenedor Linux limpio (`ubuntu-latest`) donde se configura el entorno de ejecución oficial.
* **Validación de Código:** Se realiza una compilación automatizada del microservicio (`python -m compileall`) antes de generar cualquier artefacto, aislando errores sintácticos de forma temprana para asegurar la estabilidad operativa antes de pasar a la etapa de empaquetado.

### 2. Parámetros de Seguridad y Gobernanza (IE3)
* **Análisis de Vulnerabilidades Activo:** Se integró la herramienta de seguridad **Trivy** directamente en el workflow de integración continua para realizar un escaneo profundo del sistema de archivos y las dependencias declaradas.
* **Política de Bloqueo Coherente:** El pipeline está configurado con gobernanza estricta mediante la bandera `exit-code: 1`. Esto significa que si el análisis automatizado detecta vulnerabilidades de severidad **CRITICAL** o **HIGH**, el pipeline fallará explícitamente y **bloqueará de manera automática** el avance hacia la fase de despliegue.
* **Gobernanza en Contenedores (IE1):** El `Dockerfile` fue optimizado reemplazando la base pesada por una imagen ligera de `alpine`, y se implementó un control de accesos restringido mediante la creación de un usuario sin privilegios del sistema (`USER appuser`), mitigando riesgos de seguridad de ejecución.

### 3. Trazabilidad, Orquestación y Despliegue Simulado (IE4 & IE5)
* **Trazabilidad de Extremo a Extremo:** El pipeline de GitHub Actions unifica el flujo completo: desde el commit del desarrollador, pasando por validación sintáctica y escaneo de vulnerabilidades, hasta la entrega final.
* **Estrategia de Orquestación:** El despliegue automatizado simula un entorno cloud utilizando **Docker Compose**. La herramienta orquesta de forma integrada las capas multicapa de la arquitectura (el servicio de votación, almacenamiento en caché con Redis, procesamiento con Worker, base de datos Postgres y el panel de resultados).
* **Verificación de Despliegue:** Al finalizar el levantamiento de los contenedores, se ejecuta un diagnóstico en caliente (`docker ps`) que reporta en los logs del pipeline el estado operativo real y la trazabilidad de cada puerto expuesto.

---

## 🤖 Declaración del Uso de Inteligencia Artificial
En cumplimiento estricto con las políticas de uso ético institucional de Duoc UC (https://bibliotecas.duoc.cl/ia):
* **Herramientas utilizadas:** Gemini (AI Collaborator).
* **Ámbito de aplicación:** Soporte y consultoría técnica para la reestructuración declarativa del archivo `.github/workflows/pipeline.yml` conforme a los requerimientos de la pauta y optimización de las directivas de seguridad en el archivo `vote/Dockerfile`.
* **Validación Humana:** Todo el contenido de infraestructura, código y configuraciones lógicas fue revisado, corregido y validado manualmente por los integrantes del equipo para asegurar la consistencia técnica con el proyecto.

---

## 📝 Conclusiones y Reflexiones Individuales (Obligatorias - Redactadas a Mano sin IA)

### Reflexión Personal - Integrante 1: [Tu Nombre Aquí]
*Escribe aquí con tus propias palabras qué aprendiste en este encargo sobre pipelines, integración continua, Docker y seguridad.*

### Reflexión Personal - Integrante 2: [Nombre de tu Compañero Aquí]
*Tu compañero debe redactar en este espacio su propia reflexión sobre el aprendizaje del proyecto y su contribución.*
