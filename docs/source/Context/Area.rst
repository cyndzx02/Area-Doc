.. image:: images/autoreactlogo.png
   :width: 1000px
   :align: center



AREA
++++

Area is a "action-reaction" project.
The goal of this project is to discover, as a whole, the software platform that you have chosen
through the creation of a business application.
To do this, you must implement a software suite that functions similar to that of **IFTTT and/or
Zapier**.
This software suite will be broken into three parts:

- ✓ An application server to implement all the features
- ✓ A web client to use the application from your browser by querying the application server
- ✓ A mobile client to use the application from your phone by querying the application server

Functions
=========
The application will offer the following funtionalities:

- ✓ The user registers on the application in order to obtain an account
- ✓ The registered user then confirms their enrollment on the application before being able to use it
- ✓ The application then asks the authenticated user to subscribe to Services
- ✓ Each service offers the following components:

Type Action
===========
Each service may offer Action components. These components enable the activation of a trigger when the condition is detected.

Type REAction
=============
Each service may offer REAction components. These components perform a specific task by
activating a trigger.

- ✓ The authenticated user composes AREA by interconnecting an Action to a REAction previously configured
- ✓ The application triggers AREA automatically thanks to triggers.


1. Project Overview
===================

This automation platform enables users to define workflows where **Actions** trigger corresponding **ReActions**. The platform comprises:

- **Backend**: A Django-based REST API handling business logic and database interactions.
- **Frontend**: A React-based web client for user interaction.
- **Mobile Client**: A React Native app for mobile users.

Core Features
-------------
- User Registration and Authentication (username/password and OAuth2).
- Subscription to third-party services.
- Trigger-based workflows connecting **Actions** to **ReActions**.
- REST API for seamless client-server interaction.


2. Architecture Overview
========================

High-Level Diagram
------------------
::

    

- **Frontend** and **Mobile App**: Send requests to the backend via REST API.
- **Backend**: Handles business logic and triggers workflows.
- **External Services**: Integrated via OAuth2 for Actions and ReActions.


3. Backend (Django)
===================

Responsibilities
----------------
- Manage user authentication (username/password, OAuth2).
- Provide APIs for managing Actions, ReActions, and AREA workflows.
- Store user data, workflows, and logs in the database.

Key Endpoints
-------------
.. list-table:: 
   :header-rows: 1

   * - Method
     - Endpoint
     - Description
   * - POST
     - `/api/register/`
     - Register a new user
   * - POST
     - `/api/login/`
     - Authenticate a user
   * - GET
     - `/api/services/`
     - List available services


File Structure
--------------
::

    server/
    ├── manage.py     # Django project manager
    ├── requirements.txt  # Python dependencies
    ├── settings.py   # Project settings
    ├── urls.py    # API endpoints
    ├── models.py     # Database models
    └── views.py   # Business logic

Tools and Libraries
-------------------
- **Django**: Web framework.
- **Django REST Framework (DRF)**: API development.


4. Frontend (React)
===================

Responsibilities
----------------
- Provide an intuitive user interface.
- Allow users to interact with the backend (e.g., login, manage workflows).

Key Pages
---------
- **Home Page**: Overview of the application.
- **Login/Register**: User authentication.

File Structure
--------------
::

    client_web/
    ├── src/
    │   ├── components/ # components files
    │   ├── pages/ # Page-specific components
    │   ├── Components/src  # Main application files
    │   └── index.js # Entry point
    ├── package.json  # Node.js dependencies
    └── Dockerfile # Docker setup

Tools and Libraries
-------------------
- **React**: Frontend library.
- **Axios**: For HTTP requests.
- **CSS**: Styling.


5. Mobile Client (React Native)
===============================

Responsibilities
----------------
- Provide a mobile-first interface for managing workflows.
- Leverage device capabilities (e.g., notifications).

Key Features
------------
- Authentication (OAuth2 support).
- Manage AREA workflows.

File Structure
--------------
::

    client_mobile/
    ├── src/
    │   ├── components/        # Reusable components
    │   ├── screens/           # Screen-specific components
    │   ├── App.js             # Main application file
    │   └── index.js           # Entry point
    ├── package.json           # Node.js dependencies
    └── Dockerfile             # Docker setup

Tools and Libraries
-------------------
- **React Native**: Framework for mobile development.
- **Expo**: Build and run React Native apps.
- **React Navigation**: Navigation between screens.



6. Deployment with Docker Compose
=================================

Docker Services
---------------
- **Backend**: Runs on port `8080`.
- **Frontend**: Runs on port `8081`.
- **Mobile Client**: Shares a common volume with the frontend.

``docker-compose.yml``
----------------------
.. code-block:: yaml

   version: '3.9'

   services:
     server:
       build:
         context: ./server
         dockerfile: Dockerfile
       ports:
         - "8080:8080"
       volumes:
         - ./shared:/shared
       environment:
         - DJANGO_SETTINGS_MODULE=your_project.settings
       command: ["python", "manage.py", "runserver", "0.0.0.0:8080"]

     client_web:
       build:
         context: ./client_web
         dockerfile: Dockerfile
       ports:
         - "8081:8081"
       volumes:
         - ./shared:/shared
       command: ["npm", "start"]

     client_mobile:
       build:
         context: ./client_mobile
         dockerfile: Dockerfile
       volumes:
         - ./shared:/shared
       command: ["npm", "run", "android"]

Shared Volume
-------------
The `shared` volume is used to exchange files (e.g., APKs) between the web and mobile clients.



7. Security and Best Practices
==============================
- **Authentication**: Use HTTP and secure OAuth2 flows.
- **Data Validation**: Validate all inputs to prevent SQL injection and XSS.
- **Secrets Management**: Store API keys and sensitive data in environment variables.



8. Future Enhancements
======================
- Add support for additional services.
- Implement push notifications for mobile users.
- Improve accessibility (WCAG compliance).
