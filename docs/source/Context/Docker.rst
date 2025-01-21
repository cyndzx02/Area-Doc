Docker Documentation
=====================

Overview
--------
This project is an automation platform inspired by **IFTTT/Zapier**, allowing users to create
workflows (called **AREA**) that link **Actions** (events) to **REActions** (responses). 
The platform consists of three main components:

1. **Backend Service**: Django application serving the REST API.
2. **Frontend Service**: React Vite application for the web interface.
3. **Mobile Service**: React Native application for the mobile interface (APK generation).

The project is containerized using Docker to ensure consistency across development and production environments.


Docker Setup
------------

1. **Docker Compose File**
The `docker-compose.yml` file defines the services, their configurations, and dependencies.

.. code-block:: yaml

    services:
    backend_service:
        build:
        context: ./backend/area
        dockerfile: Dockerfile
        command:
        - python
        - manage.py
        - runserver
        - 0.0.0.0:8080
        ports:
        - "8080:8080"
        env_file: "./backend/area/.env"
        # environment:
        #   - KEY={SECRET_KEY}
        #   - CLIENT_ID={GOOGLE_CLIENT_ID}
        #   - CLIENT_SECRET={GOOGLE_CLIENT_SECRET}
    
    frontend_service:
        build:
        context: ./frontend/
        dockerfile: Dockerfile
        ports:
        - "8081:8081"
        command: ["npm", "run", "dev"]
        depends_on:
        - backend_service
        volumes:
        - area_volume:/app

    # app_mobile:
    #   build:
    #     context: ./MonApp
    #     dockerfile: Dockerfile
    #   ports:
    #     - "8081:8081"
    #   command: ["npm", "run", "android"]
    #   depends_on:
    #     - backend_service
    #   volumes:
    #     - area_volume:/app

    volumes:
    area_volume: {}
