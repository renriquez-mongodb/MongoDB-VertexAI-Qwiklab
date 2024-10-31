# Leafy Restaurant

This repository equips you to build a Retrieval-Augmented Generation (RAG) model that leverages MongoDB Atlas for data storage and Vertex AI's powerful gemini-pro for large language model processing. The core functionality resides in app.py, which utilizes Streamlit for a user-friendly interface and Langchain to seamlessly integrate all components. We'll delve into a practical use case where customers can interact with a restaurant chatbot powered by this RAG model (this repository is a short version of [this guide](https://www.mongodb.com/developer/products/atlas/build-smart-applications-atlas-vector-search-google-vertex-ai/)).

## setup

### Start a codespaces environment
- [Start codespace environment](https://codespaces.new/renriquez-mongodb/MongoDB-VertexAI-Qwiklab/tree/add-fixes?quickstart=1)

### Install the pre-requisites
Install python3 with pip
```sh
sudo apt update
sudo apt install python3-pip -y
python3 -m pip install --upgrade pip
```

Install the requirements from this app.
```sh
pip3 install -r requirements.txt
```

### Have a GCP project with VertexAI enabled
- [Create project](https://developers.google.com/workspace/guides/create-project)
- [Enable the VertexAI apis](https://console.cloud.google.com/flows/enableapi?apiid=aiplatform.googleapis.com)

### Install and authenticate `gcloud` cli

Follow the steps from [here](https://cloud.google.com/sdk/docs/install)(for Debian/Ubuntu) to install the `gcloud` cli.

Run the following commands to initialize the cli.

```sh
gcloud init
```

Run the following to create default credentials for apps.
```sh
gcloud auth application-default login 
```

### Install and authenticate Atlas cli

Follow the steps from [here](https://www.mongodb.com/docs/atlas/cli/current/install-atlas-cli/)(for Apt & Ubuntu).

Run the following to [setup](https://www.mongodb.com/docs/atlas/cli/current/atlas-cli-getting-started/) and authenticate.

```sh
atlas setup --noBrowser --currentIp --skipSampleData --skipMongosh --provider GCP --region CENTRAL_US
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

### Export all the environment variable needed for the app to work

Replace the values for each of the variables with the correct values for your setup. Where the MongoDB Atlas cluster is based on the above commands as well as the GCP setup you did previously.
```sh
export MONGODB_URI="mongo-uri"
export PROJECT_ID="gcp-project-id"
export LOCATION="gcp-location"
export TEXT_MODEL="text-embedding-004"
export VECTOR_INDEX_NAME="vector_index"
export CHAT_APP_DB="rag-db"
export CHAT_APP_COL="chat-vec"
export CHAT_VERIFY_COL="chat-vec-verify"
```

When running outside of codespaces, ensure you set this environment variable:
```sh
export CLOUDENV_ENVIRONMENT_ID="random-id"
```

### Setup DB
Execute this in the terminal to have a skeleton DB and collection with index.
```sh
mongosh $MONGODB_URI --eval "use $CHAT_APP_DB" \
--eval "db.createCollection('$CHAT_APP_COL')" \
--eval "db.runCommand({'createSearchIndexes':'$CHAT_APP_COL', 'indexes':[{'name':'$VECTOR_INDEX_NAME','type':'vectorSearch', 'definition':{'fields':[{'type':'vector', 'path':'vec', 'numDimensions':768, 'similarity': 'cosine' }]}}]});"
```

### Run the application
```sh
python3 -m streamlit run app.py
```
