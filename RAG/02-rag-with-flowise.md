# Build RAG completely on cloud with flowise
## Steps
### Credentials
- Create openai key: https://platform.openai.com/settings/organization/api-keys
- Create credential with the openai key: https://cloud.flowiseai.com/credentials

### Chatflow
- Create new chatflow in flowise
- Add node -> Agent -> Tool Agent
- Add node -> Chat models -> ChatOpenAI
  - Select credential and model
  - link to Tool Calling Chat Model
- Add node -> Memory -> Add Buffer Window Memory
  - Set size to 30
  - Link to the Memory
- Add node -> Tool -> RetrieverTool
  - Input name and descriptions
  - Link to Tools
- Add node -> Vector Stores -> Document Store(Vector)
  - Select Document Store
  - Link to Retriever

### Document Store
- Add a document store
- In the document store, add Document Loader. Select Microsoft Excel
  - No need to set Text Splitter for excel
- Also add word loader
  - use the Recursive Character Text Splitter
- For pdf loader
  - use One document per page and no splitter
- After uploading, select more actions -> Upsert All Chunks -> Set up Embeddings, Vector store and Record Manager
- After upsert, check record count in pinecorn

#### Embeddings
- Select Credential, and model(the small one)
- Set dimensions to 512

#### Vector store
- Add pinecorn api key to flowise credentials
- Create a new index in pinecorn: https://app.pinecone.io/organizations/-Oi09TXeF3OC8sBjCRX6/projects/d026b3f6-87ec-4ea5-b678-3241d88274e5/create-index/serverless
  - Select text-embedding-3-small
- In flowise
  - Select credential and input pinecorn index
  - Set Top K to 10

### Record Manager
- Select Postgres
- In supabase, create new project
- Click Connect on the top
- Method: Session Pooler, click view parameters
- In flowise, Create new credential with the info above
- Set Cleanup to full