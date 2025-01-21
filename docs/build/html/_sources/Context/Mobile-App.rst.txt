Mobile-App
++++++++++

Overview
--------
The mobile client is built using **React Native** and serves
as a user interface for interacting with the automation platform. It communicates with
the **application server** via a REST API and supports Android and Windows
Mobile. Key responsibilities include:

- Rendering UI screens (registration, AREA creation, service subscriptions).
- Forwarding user requests to the server (e.g., authentication, trigger activation).
- Managing OAuth2 flows for third-party service integrations (Google etc.).


**Tech Stack**:

- **Framework**: React Native
- **HTTP Client**: Axios
- **OAuth2**: ``react-native-app-auth``

Dependencies
------------
Add to ``package.json``:

.. code-block:: json

   {
     "dependencies": {
       "react": "18.2.0",
       "react-native": "0.70.5",
       "axios": "^1.4.0",
       "react-native-app-auth": "^6.4.2"
     }
   }



Authentication
~~~~~~~~~~~~~~
- **Email/Password**:
  - UI Form → Axios POST to ``/api/auth/login``.
  - JWT token stored in AsyncStorage.
- **OAuth2** (Google/Facebook):
  - Uses ``react-native-app-auth`` to handle redirects.
  - Token sent to server via ``/api/auth/oauth-callback``.

Service Integration
~~~~~~~~~~~~~~~~~~~
- Users subscribe to services (e.g., Gmail, OneDrive):
  - Tap "Add Service" → Select from server-provided list.
  - OAuth2 flow initiated for authorization.
  - Success: Server stores credentials, client updates UI.


Docker Integration
------------------
Defined in ``docker-compose.yml``:

.. code-block:: yaml

    app_mobile:
    build:
      context: ./MonApp
      dockerfile: Dockerfile
    ports:
      - "8081:8081"
    command: ["npm", "run", "android"]
    depends_on:
      - backend_service
    volumes:
      - area_volume:/app

.. APK Generation
.. ~~~~~~~~~~~~~~
.. - Built via:

..   .. code-block:: bash

..      cd android && ./gradlew assembleRelease

.. - Output: ``android/app/build/outputs/apk/release/app-release.apk``.
.. - Accessible at ``http://localhost:8081/client.apk``.

Troubleshooting
---------------
- **OAuth2 Failures**:
  - Ensure redirect URIs match server configuration.
- **Network Errors**:
  - Verify ``API_BASE_URL`` points to the running server.
- **Docker Issues**:
  - Run ``docker-compose build --no-cache`` to rebuild images.
