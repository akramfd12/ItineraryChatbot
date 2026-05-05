# ItineraryChatbot

A sophisticated Telegram-based AI travel planning assistant designed specifically for Jakarta itinerary planning. This project leverages advanced AI, vector databases, and real-time weather data to provide intelligent flight, hotel, and itinerary recommendations. The entire workflow orchestration is powered by **N8N**, an open-source, no-code automation tool.

## Table of Contents

- [Project Overview](#project-overview)
- [Features](#features)
- [Folder Structure](#folder-structure)
- [Tech Stack](#tech-stack)
- [Setup Instructions](#setup-instructions)
- [Usage Guide](#usage-guide)
- [Workflows](#workflows)
- [Contributing](#contributing)
- [License](#license)

## Project Overview

ItineraryChatbot is an intelligent travel assistant that helps users plan their trips to Jakarta. Users interact with the bot through Telegram, asking questions about flights, hotels, weather, and general itinerary planning. The bot uses OpenAI's GPT-5 Mini model with a vector database (Qdrant) to provide semantic search capabilities, ensuring relevant and context-aware responses.

The project is built around **N8N**, which orchestrates all workflows, including data ingestion, vector database indexing, and chatbot logic. N8N's flexibility and visual interface make it easy to manage and extend the automation processes.

### Key Capabilities

- **Flight Search**: Query available flights from Indonesian cities to Jakarta
- **Hotel Search**: Find accommodation options across different Jakarta areas
- **Weather Information**: Get real-time weather data for trip planning
- **Conversational AI**: Natural language understanding with conversation memory
- **Intelligent Routing**: Intent-based routing to determine the best tool for each query
- **Workflow Automation**: Seamless integration of data pipelines and chatbot logic using N8N

## Features

✨ **AI-Powered Recommendations**: Uses OpenAI GPT-5 Mini for intelligent travel suggestions  
📍 **Semantic Search**: Vector embeddings enable contextual search through flights and hotels  
💬 **Telegram Integration**: Easy-to-use Telegram bot interface  
☁️ **Cloud-Native**: Google Drive integration for data management  
🗺️ **Real-Time Weather**: Open-Meteo API for accurate weather forecasts  
💾 **Qdrant Vector DB**: Fast semantic similarity search for travel options  
🔄 **Data Pipelines with N8N**: Automated ETL workflows for data ingestion and indexing

## Folder Structure

```
ItineraryChatbot/
├── data/
│   ├── updated_flights_with_departure_date.json
│   │   └── Flight ticket data with departure dates (20+ flights)
│   ├── dummy_hotels_jakarta.json
│   │   └── Hotel accommodation data (13+ hotels across Jakarta areas)
│   └── spot_photo.json
│       └── Location and photo metadata for Jakarta attractions
│
├── workflows/
│   ├── ETL.json
│   │   └── Extract, Transform, Load workflow for initial data processing
│   ├── Indexing Vector DB.json
│   │   └── Workflow to ingest flight/hotel data into Qdrant vector database
│   └── ItineraryAgent.json
│       └── Main chatbot logic with Telegram integration and AI agent
│
├── screenshots/
│   ├── FlowIndexing DB.png
│   │   └── Visualization of the vector DB indexing workflow
│   ├── FlowItineraryBot.png
│   │   └── Visualization of the itinerary chatbot workflow
│   ├── TelegramDemo.png
│   │   └── Screenshots of the chatbot in action on Telegram
│   ├── ETL Data.png
│   │   └── Visual representation of the ETL workflow
│   ├── IndexingVectorDB.png
│   │   └── Illustration of the vector database indexing process
│   └── ItineraryChatbot.png
│       └── Overview of the chatbot's workflow
│
└── README.md (this file)
```

### Workflow Files Details

#### `ETL.json`
- Extracts data from Google Drive
- Processes JSON files containing flights and hotels data
- Splits data by type (hotels vs. flights)
- Prepares data for vector embedding
- **Powered by N8N**: This workflow is fully automated using N8N's visual interface.
- **Screenshot**: `ETL Data.png` provides a visual representation of this workflow.

#### `Indexing Vector DB.json`
- Fetches processed data from Google Drive
- Creates OpenAI embeddings for all flight and hotel records
- Indexes data into Qdrant collections:
  - `flight_n8n_project`: Flight data
  - `hotel_n8n_project`: Hotel data
- **Powered by N8N**: This workflow ensures seamless integration with Qdrant.
- **Screenshot**: `IndexingVectorDB.png` illustrates the vector database indexing process.

#### `ItineraryAgent.json`
The main chatbot workflow that:
- Listens for Telegram messages
- Parses user input and extracts intent
- Routes queries to appropriate tools:
  - **Flight Vector Store**: For flight-related queries
  - **Hotel Vector Store**: For accommodation questions
  - **Weather API**: For weather information
- Uses LangChain with OpenAI for intelligent response generation
- Maintains conversation history with simple memory
- Returns formatted responses back to Telegram
- **Powered by N8N**: This workflow orchestrates the entire chatbot logic.
- **Screenshot**: `ItineraryChatbot.png` provides an overview of the chatbot's workflow.

## Tech Stack

| Component | Technology |
|-----------|-----------|
| **Workflow Orchestration** | N8N (open-source no-code automation) |
| **AI Model** | OpenAI GPT-5 Mini |
| **Embeddings** | OpenAI Embeddings API |
| **Vector Database** | Qdrant (semantic search) |
| **Chat Integration** | Telegram Bot API |
| **Weather API** | Open-Meteo |
| **Data Source** | Google Drive |
| **AI Framework** | LangChain |

## Setup Instructions

### Prerequisites

Before setting up the project, ensure you have:

- **N8N Instance**: Self-hosted or cloud instance (https://n8n.io/)
- **OpenAI API Key**: For GPT-5 Mini and embeddings (https://platform.openai.com/)
- **Google Cloud Project**: With Google Drive API enabled
- **Qdrant Instance**: Self-hosted or Qdrant Cloud (https://qdrant.tech/)
- **Telegram Bot Token**: From BotFather on Telegram
- **Open-Meteo API**: Free, no key required (https://open-meteo.com/)

### Step 1: Set Up N8N

1. **Install N8N**:
   ```bash
   npm install -g n8n
   n8n start
   ```
   Or use Docker:
   ```bash
   docker run -it --rm --name n8n -p 5678:5678 n8nio/n8n
   ```

2. **Access N8N**: Open http://localhost:5678 in your browser

### Step 2: Configure External Services

#### OpenAI Setup
1. Create an account at https://platform.openai.com/
2. Generate an API key from the dashboard
3. In n8n, create credentials for OpenAI with your API key

#### Google Drive Setup
1. Create a Google Cloud project at https://console.cloud.google.com/
2. Enable the Google Drive API
3. Create OAuth 2.0 credentials (Desktop application)
4. In N8N, authorize with Google Drive credentials

#### Qdrant Setup
1. Set up Qdrant instance:
   - **Cloud**: https://cloud.qdrant.io/
   - **Self-hosted**: `docker run -p 6333:6333 qdrant/qdrant:latest`
2. Create two collections:
   - `flight_n8n_project`
   - `hotel_n8n_project`
3. In n8n, add Qdrant connection credentials

#### Telegram Bot Setup
1. Open Telegram and find @BotFather
2. Send `/newbot` and follow instructions to create your bot
3. Save the bot token
4. In N8N, create credentials with the bot token

---