# Leafy Restaurant

This repository equips you to build a Retrieval-Augmented Generation (RAG) model that leverages MongoDB Atlas for data storage and Vertex AI's powerful gemini-pro for large language model processing. The core functionality resides in app.py, which utilizes Streamlit for a user-friendly interface and Langchain to seamlessly integrate all components. We'll delve into a practical use case where customers can interact with a restaurant chatbot powered by this RAG model.

## setup

### Install the pre-requisites
```sh
sudo apt update
sudo apt install python3-pip
pip3 --version
pip3 install -r requirements.txt
pip3 install -qU langchain-google-vertexai
```

### Have a GCP project with VertexAI enabled
- [Create project](https://developers.google.com/workspace/guides/create-project)
- [Enable the VertexAI apis](https://console.cloud.google.com/flows/enableapi?apiid=aiplatform.googleapis.com)

### Install and authenticate `gcloud` cli

Follow the steps from [here](https://cloud.google.com/sdk/docs/install) to install the `gcloud` cli.

Run the following commands to initialize and authenticate the app.

```sh
gcloud init
gcloud auth application-default login 
```

### Export all the environment variable needed for the app to work
```sh
export MONGODB_URI=your-mongodb-uri-with-credentials
export PROJECT_ID=your-project-id-with-vertexai-apis-enabled
export LOCATION=gcp-location
```


### Run the application
```sh
streamlit run app.py
```
