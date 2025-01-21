FrontEnd
++++++++



Overview
--------
The frontend is built using **React Vite** and styled with CSS.
 It serves as the user interface for the automation platform, 
 communicating with the backend via a REST API. The frontend is
  optimized for both desktop and mobile browsers.


Setup
-----
1. **Clone the repository**:

   .. code-block:: bash

      git clone git@github.com:EpitechPromo2027/B-DEV-500-COT-5-2-area-hanniel.degbelo.git
      cd frontend

2. **Install dependencies**:

   .. code-block:: bash

      npm install

3. **Run the app**:

   .. code-block:: bash

      npm run dev

4. **Production build**:

   .. code-block:: bash

      npm run build


**Supported Services**:
-----------------------

  - **Google Tasks**: Manage and automate tasks.
  - **Google Contacts**: Sync and manage contacts.
  - **Google Drive**: Automate file uploads, downloads, and sharing.
  - **Google Calendar**: Schedule and manage events.
  - **Google Keep**: Automate note creation and updates.
  - **Google Forms**: Automate form responses and data collection.
  - **Google Photos**: Automate photo uploads and album management.
  - **YouTube**: Automate video uploads and playlist management.
  - **Gmail**: Automate email sending, labeling, and filtering.

Docker Integration
------------------
The frontend is containerized using Docker. The development server runs inside the container.

**Dockerfile**:

.. code-block:: dockerfile

   FROM node:20-alpine

   WORKDIR /app
   COPY package*.json .
   RUN npm install

   COPY . .

   EXPOSE 8081
   CMD ["npm", "run", "dev"]

**docker-compose.yml**:

.. code-block:: yaml

   frontend_service:
     build:
       context: ./frontend/
       dockerfile: Dockerfile
     ports:
       - "8081:8081"
     command: ["npm", "run", "dev"]
     depends_on:
       - backend_service

Troubleshooting
---------------

- **Docker Port Conflicts**:
  - Stop existing processes on ports 8080/8081 before running ``docker-compose up``.



.. image:: images/autoreact.jpg
   :alt: AutoReact Diagram
   :width: 500px
   :align: center
