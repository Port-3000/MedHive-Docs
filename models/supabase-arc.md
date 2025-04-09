```
flowchart TB
    subgraph Users
        EU[End Users]
        AD[Admin/Developers]
        DP[Data Providers]
        CO[Contributors]
    end
    
    subgraph Frontend
        UI[User Interface]
        Auth[Authentication]
        ModelHub[Model Hub Interface]
        Chat[Groq-powered Chatbot]
        FL_UI[FL Contribution UI]
    end
    
    subgraph Backend
        API[FastAPI Endpoints]
        Auth_S[Auth Service]
        ModelService[Model Service]
        FL_Coordinator[FL Coordinator]
    end
    
    subgraph Databases
        SupaDB[(Supabase DB)]
        AstraDB[(AstraDB Vector Store)]
    end
    
    subgraph ML_Infrastructure
        MLFlow[MLFlow Server]
        GroqAPI[Groq API]
        HF[HuggingFace Models]
    end
    
    subgraph FL_System
        FL_Server[Flower Server]
        FL_Clients[Flower Clients]
    end
    
    %% User interactions
    EU --> Auth
    EU --> UI
    EU --> Chat
    EU --> ModelHub
    
    AD --> Auth
    AD --> UI
    AD --> MLFlow
    AD --> FL_Coordinator
    
    DP --> Auth
    DP --> FL_UI
    
    CO --> Auth
    CO --> FL_UI
    
    %% Frontend to Backend flows
    Auth --> Auth_S
    UI --> API
    ModelHub --> ModelService
    Chat --> API
    FL_UI --> FL_Coordinator
    
    %% Backend services
    Auth_S <--> SupaDB
    API <--> SupaDB
    API <--> AstraDB
    ModelService <--> HF
    ModelService <--> SupaDB
    FL_Coordinator <--> FL_Server
    
    %% ML Flows
    Chat <--> GroqAPI
    API <--> GroqAPI
    ModelService --> MLFlow
    
    %% FL System
    FL_Server <--> FL_Clients
    FL_Clients <-- Local Training --> DP
    FL_Clients <-- Computation --> CO
    FL_Server --> ModelService
    
    %% Data flows
    SupaDB <-- Performance Metrics --> MLFlow
    MLFlow --> ModelService
    ModelService --> HF
    
```