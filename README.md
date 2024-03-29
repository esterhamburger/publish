# GPKG pipeline

The primary objective of this project is to generate GPKGs (GeoPackage) from TIFF files, facilitating convenient access without the need to store the original TIFFs. Within the scope of this endeavor, we diligently worked on the development of four end-to-end services. These services seamlessly integrate with two existing services, and the updates are efficiently managed through Redis.

## Table of Contents

1. [Services Overview](#services-overview)
    - [DivideService:](#divideservice)
    - [TiffsToVRTService](#tiffstovrtservice)
    - [FinalVRTService](#finalvrtservice)
    - [CheckStatusService](#checkstatusservice)
2. [Installation](#installation)
3. [Configuration](#configuration)

## Services Overview

### DivideService

 Thie service divides large lists of TIFFs into smaller, each containing up to 1,500 TIFFs.

### TiffsToVRTService

The service creates a VRT using the path to the received file

### FinalVRTService

If the arrival is greater than 1500,  resulting in the VRT files were created in the CreateVrtFromTiffsService service, it creates one VRT from all the VRT files and sends a message to the GRID creation service.
In the case of a smaller arrival, it creates a "VRT" file from tiffs and sends a message to the GPKG creation service.

### CheckStatusService

The service checks that all the GPKGs are ready and it is possible to inform the consumer that the process is complete

## Installation

1. **Clone the Repository:**

    ```bash
    git clone https://github.com/skyvar-newspace/GPKG-pipeline.git
    cd GPKG-pipeline
    ```

2. **Build Docker Images for Each Service:**

    Navigate to each service directory and build the Docker image using its respective Dockerfile.

    ```bash
        cd each-service
        docker build -t service-name-image .
    ```

3. **Run Docker Containers for Each Service:**

    Run each service in a separate Docker container. Adjust the ports as needed.

    ```bash
        docker run -d -p 8081:80 --name each-service-container each-service-image
    ```

## Configuration

Before running the services, make sure to set up the following environment variables.

1. **Copy Environment Variables from `env.sample`:**
    In each service directory, you'll find an `env.sample` file. Duplicate this file, rename it to `.env`, and fill in the necessary values.

2. **Define Environment Variables:**
   Open the `.env` file in each service and configure the environment variables, for example:

    ```plaintext
    RABBITMQ_HOST=rabbitmq-host
    ```

    you also can export on the terminal, for example:

    ```bash
        export RABBITMQ_HOST=<rabbitmq-host>
    ```
