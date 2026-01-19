# SFSwitch Cribl Documentation

This document provides a comprehensive guide to setting up, deploying, and understanding the SFSwitch Cribl application.

## 1. Setting up VS Code and Cloning the Repository

1.  **Install Visual Studio Code**: If you don't have it already, download and install VS Code from [https://code.visualstudio.com/](https://code.visualstudio.com/).
2.  **Install Git**: If you don't have Git installed, download and install it from [https://git-scm.com/](https://git-scm.com/).
3.  **Open VS Code and the Command Palette**: Open VS Code and press `Ctrl+Shift+P` (or `Cmd+Shift+P` on Mac) to open the Command Palette.
4.  **Clone the Git Repository**:
    *   Type `Git: Clone` in the Command Palette and press Enter.
    *   Paste the following URL and press Enter: `https://github.com/abdullahbutt-cribl/sfswitch-cribl.git`
    *   Select a local directory where you want to clone the repository.

## 2. Deploying to Heroku

This application is designed to be deployed to a Heroku container.

### Prerequisites

*   A Heroku account.
*   The Heroku CLI installed and authenticated.
*   The project cloned to your local machine.

### Deployment Steps

1.  **Navigate to the project directory**:
    ```bash
    cd sfswitch-cribl
    ```
2.  **Log in to Heroku**:
    ```bash
    heroku login
    ```
3.  **Create a Heroku app**:
    ```bash
    heroku create <your-app-name>
    ```
4.  **Add the Heroku remote to your Git repository**:
    ```bash
    heroku git:remote -a <your-app-name>
    ```
5.  **Push the code to Heroku**:
    ```bash
    git push heroku master
    ```

## 3. Packages and Dependencies

The application uses the following Python packages. The full list of dependencies is in the `requirements.txt` file.

*   **Django**: The web framework used to build the application.
*   **Celery**: For running background tasks, such as downloading metadata from Salesforce.
*   **requests**: For making HTTP requests to the Salesforce APIs.
*   **defusedxml**: A secure XML parser to prevent XML-related attacks.
*   **suds-jurko**: A SOAP client for interacting with the Salesforce Metadata API.
*   **gunicorn**: A Python WSGI HTTP Server for UNIX.
*   **whitenoise**: To serve static files directly from Gunicorn.
*   **psycopg2-binary**: PostgreSQL adapter for Python.
*   **dj-database-url**: A utility to help you configure your database connection from a single environment variable.

## 4. Authentication Mechanism

The application uses OAuth 2.0 to authenticate with Salesforce. The OAuth 2.0 web server flow is used to obtain an access token, which is then used to make API calls to Salesforce.

## 5. Salesforce Setup

To use this application, you need to create a Connected App in Salesforce to obtain OAuth credentials.

1.  **Create a Connected App in Salesforce**:
    *   In Salesforce Setup, search for "App Manager".
    *   Click "New Connected App".
    *   Fill in the following details:
        *   **Connected App Name**: A descriptive name for your app (e.g., "SFSwitch Cribl").
        *   **API Name**: This will be automatically populated.
        *   **Contact Email**: Your email address.
    *   **Enable OAuth Settings**: Check this box.
    *   **Callback URL**: The URL where users are redirected after a successful authentication. This should be `https://<your-heroku-app-name>.herokuapp.com/oauth_response`.
    *   **Selected OAuth Scopes**: Add the following scopes:
        *   `api`
        *   `refresh_token, offline_access`
        *   `full`
    *   Save the Connected App.
2.  **Get the Consumer Key and Consumer Secret**:
    *   After saving the Connected App, you will be taken to the app's detail page.
    *   Click "Manage Consumer Details" to view the **Consumer Key** and **Consumer Secret**.
3.  **Configure Environment Variables in Heroku**:
    *   In your Heroku app's settings, add the following config vars:
        *   `SALESFORCE_CONSUMER_KEY`: The Consumer Key from your Connected App.
        *   `SALESFORCE_CONSUMER_SECRET`: The Consumer Secret from your Connected App.
        *   `SALESFORCE_REDIRECT_URI`: The Callback URL you specified in the Connected App.

This completes the documentation.
