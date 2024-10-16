# Cloud Run application utilizing Streamlit Framework that demonstrates working with Vertex AI Gemini API | made with the help of google cloud skillboost tasks

|           |                                                |
| --------- | ---------------------------------------------- |
| Author(s) | [Avilash Bhowmick](https://github.com/AvilashBhowmick12) |

This application demonstrates a Cloud Run application that uses the [Streamlit](https://streamlit.io/) framework.

Sample screenshots and video demos of the application are shown below:

## Application screenshots

<img src="https://storage.googleapis.com/github-repo/img/gemini/sample-apps/gemini-streamlit-cloudrun/assets/gemini_pro_text.png" width="50%" alt="Vertex AI Gemini App"/>

## Run the Application locally (on Cloud Shell)

> NOTE: **Before you move forward, ensure that you have followed the instructions in [SETUP.md](../SETUP.md).**
> Additionally, ensure that you have cloned this repository and you are currently in the `gemini-streamlit-cloudrun` folder. This should be your active working directory for the rest of the commands.

To run the Streamlit Application locally (on Cloud Shell), we need to perform the following steps:

1. Setup the Python virtual environment and install the dependencies:

   In Cloud Shell, execute the following commands:

   ```bash
   python3 -m venv gemini-streamlit
   source gemini-streamlit/bin/activate
   pip install -r requirements.txt
   ```

   For performing in local machine windows use this instead if faced issue !

   ```bash
   python.exe -m venv gemini-streamlit
   .\gemini-streamlit\Scripts\Activate
   pip install -r requirements.txt
   ```

2. Your application requires access to two environment variables:

   - `GCP_PROJECT` : This the Google Cloud project ID.
   - `GCP_REGION` : This is the region in which you are deploying your Cloud Run app. For example, `us-central1` [if found busy netowrk use : `europe-west1` ].

   These variables are needed since the Vertex AI initialization needs the Google Cloud project ID and the region. The specific code line from the `app.py`
   function is shown here:
   `vertexai.init(project=PROJECT_ID, location=LOCATION)`

   In Cloud Shell, execute the following commands:

   ```bash
   export GCP_PROJECT='<Your Google Cloud Project ID>'  # Change this
   export GCP_REGION='us-central1'             # If you change this, make sure the region is supported.
   ```

3. To run the application locally, execute the following command:

   In Cloud Shell, execute the following command:

   ```bash
   streamlit run app.py \
     --browser.serverAddress=localhost \
     --server.enableCORS=false \
     --server.enableXsrfProtection=false \
     --server.port 8080
   ```

The application will startup and you will be provided a URL to the application. Use Cloud Shell's [web preview](https://cloud.google.com/shell/docs/using-web-preview) function to launch the preview page. You may also visit that in the browser to view the application. Choose the functionality that you would like to check out and the application will prompt the Vertex AI Gemini API and display the responses.

## Build and Deploy the Application to Cloud Run

> NOTE: **Before you move forward, ensure that you have followed the instructions in [SETUP.md](../SETUP.md).**
> Additionally, ensure that you have cloned this repository and you are currently in the `gemini-streamlit-cloudrun` folder. This should be your active working directory for the rest of the commands.

To deploy the Streamlit Application in [Cloud Run](https://cloud.google.com/run/docs/quickstarts/deploy-container), we need to perform the following steps:

1. Your Cloud Run app requires access to two environment variables:

   - `GCP_PROJECT` : This the Google Cloud project ID.
   - `GCP_REGION` : This is the region in which you are deploying your Cloud Run app. For e.g. us-central1.

   These variables are needed since the Vertex AI initialization needs the Google Cloud project ID and the region. The specific code line from the `app.py`
   function is shown here:
   `vertexai.init(project=PROJECT_ID, location=LOCATION)`

   In Cloud Shell, execute the following commands:

   ```bash
   export GCP_PROJECT='<Your Google Cloud Project ID>'  # Change this
   export GCP_REGION='us-central1'             # If you change this, make sure the region is supported.
   ```

2. Build and deploy the service to Cloud Run:

   In Cloud Shell, execute the following command to name the Cloud Run service:

   ```bash
   export SERVICE_NAME='gemini-streamlit-app' # This is the name of our Application and Cloud Run service. Change it if you'd like.
   ```

   If wanted to deploy it on google cloud run, then can perform this commands to deploy it successfully on cloudrun !

   ```bash
   AR_REPO='gemini-repo'
   SERVICE_NAME='gemini-streamlit-app' 
   gcloud artifacts repositories create "$AR_REPO" --location="$GCP_REGION" --repository-format=Docker
   gcloud builds submit --tag "$GCP_REGION-docker.pkg.dev/$GCP_PROJECT/$AR_REPO/$SERVICE_NAME"
   ```

   In Cloud Shell, execute the following command:

   ```bash
   gcloud run deploy "$SERVICE_NAME" \
  --port=8080 \
  --image="$GCP_REGION-docker.pkg.dev/$GCP_PROJECT/$AR_REPO/$SERVICE_NAME" \
  --allow-unauthenticated \
  --region=$GCP_REGION \
  --platform=managed  \
  --project=$GCP_PROJECT \
  --set-env-vars=GCP_PROJECT=$GCP_PROJECT,GCP_REGION=$GCP_REGION
   ```
```

## The Page after successfully running will look like this !

<img src="https://github.com/AvilashBhowmick12/Gemini-Streamlit-Model-2/blob/main/images/Screenshot%202024-10-15%20224030.png" width="50%" alt="Vertex AI Gemini App"/>
<img src="https://github.com/AvilashBhowmick12/Gemini-Streamlit-Model-2/blob/main/images/Screenshot%202024-10-15%20224054.png" width="40%" alt="Successful Deployment on Google Cloud run"/>

### On successful deployment, you will be provided a URL to the Cloud Run service. You can visit that in the browser to view the Cloud Run application that you just deployed. Choose the functionality that you would like to check out and the application will prompt the Vertex AI Gemini API and display the responses.

## Visit this URL to test my application !

[AvilashB - Gemini - Streamlit App](https://gemini-streamlit-app-1019799482474.europe-west1.run.app)
