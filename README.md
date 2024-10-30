# Leafy Restaurant

This repository equips you to build a Retrieval-Augmented Generation (RAG) model that leverages MongoDB Atlas for data storage and Vertex AI's powerful gemini-pro for large language model processing. The core functionality resides in app.py, which utilizes Streamlit for a user-friendly interface and Langchain to seamlessly integrate all components. We'll delve into a practical use case where customers can interact with a restaurant chatbot powered by this RAG model (this repository is a short version of [this guide](https://www.mongodb.com/developer/products/atlas/build-smart-applications-atlas-vector-search-google-vertex-ai/)).

## setup

### Start a codespaces environment
- [Start codespace environment](https://codespaces.new/renriquez-mongodb/MongoDB-VertexAI-Qwiklab/tree/add-fixes?quickstart=1)

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

### Install and authenticate Atlas cli

Follow the steps from [here](https://www.mongodb.com/docs/atlas/cli/current/install-atlas-cli/).

Run the following to [setup](https://www.mongodb.com/docs/atlas/cli/current/atlas-cli-getting-started/) and authenticate.

```sh
atlas setup
```

Take note from the following lines from the output:
```sh
Database User Username: <username>
Database User Password: <password>

Cluster created.
Your connection string: mongodb+srv://<cluster address>
```

Form the connection string to set it in `MONGODB_URI` like the following:
```sh
mongodb+srv://<username>:<password>@<cluster address>
```
Your connection string: mongodb+srv://Cluster05161:RWZde0J2QWtT@cluster05161.ksnda.mongodb.net

### Enable the external IP access to the cluster
- If you are on a codespace environment run `curl ifconfig.me` in the terminal.
- [Allow IP in Atlas](https://www.mongodb.com/docs/atlas/security/ip-access-list/)

### Export all the environment variable needed for the app to work
```sh
export MONGODB_URI="your-mongodb-uri-with-credentials"
export PROJECT_ID=your-project-id-with-vertexai-apis-enabled
export LOCATION=gcp-location
```

### Setup Database
- Navigate to the cluster created
- Navigate to the `Collections` tab
- Create database `vertexaiApp` and collection `chat-vec`
![create database](images/create_database.png)
- Navigate to the `Atlas Search` tab
- Click on `Create a Search Index`
- Select `Atlas Vector Search`
- Click next
- Ensure index name is `vector_index`
- On the `Database and Collection` search for `chat-vec`
- Select `chat-vec`
- Use the following definition
```json
{
    "fields": [
        {
            "type":"vector",
            "path":"vec",
            "numDimensions":768,
            "similarity": "cosine"
        }
    ]
}
```
- Click next
- Finally, click on Create Search Index

### Run the application
```sh
streamlit run app.py
```
