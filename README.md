# ItineraryChatbot

A sophisticated Telegram-based AI travel planning assistant designed specifically for Jakarta itinerary planning. This project leverages advanced AI, vector databases, and real-time weather data to provide intelligent flight, hotel, and attraction recommendations. The entire workflow orchestration is powered by **N8N**, an open-source, no-code automation tool.

## Table of Contents

- [Project Overview](#project-overview)
- [Features](#features)
- [Folder Structure](#folder-structure)
- [Project Architecture](#project-architecture)
- [Tech Stack](#tech-stack)
- [Setup Instructions](#setup-instructions)
- [Usage Guide](#usage-guide)
- [Workflows](#workflows)
- [Data Models](#data-models)
- [Contributing](#contributing)

## Project Overview

ItineraryChatbot is an intelligent travel assistant that helps users plan their trips to Jakarta with personalized recommendations. Users interact with the bot through Telegram, asking questions about flights, hotels, Instagram-worthy photo spots, and personalized itineraries. The bot uses OpenAI's GPT-5 Mini model with MySQL database storage and vector embeddings to provide fast, accurate, and context-aware responses.

The project is built around **N8N**, which orchestrates all workflows, including ETL data pipelines, vector database indexing, and real-time chatbot logic. N8N's flexibility and visual interface make it easy to manage and extend the automation processes without writing code.

### Key Capabilities

- **Flight Search**: Query 150+ flights from Indonesian cities to Jakarta with pricing and schedules
- **Hotel Search**: Find 150+ accommodation options across different Jakarta areas with amenities and ratings
- **Photo Spot Discovery**: Get recommendations for 20+ Instagram-worthy locations with optimal times and vibes
- **Weather-Based Itineraries**: Generate travel itineraries based on real-time weather forecasts
- **Conversational AI**: Natural language understanding with 7-message conversation memory
- **Multi-Intent Handling**: Process multiple queries (flights + hotels + photo spots) in a single request
- **Workflow Automation**: Seamless integration of data pipelines and chatbot logic using N8N

## Features

✨ **AI-Powered Recommendations**: Uses OpenAI GPT-5 Mini for intelligent travel suggestions  
📍 **Multi-Tool Semantic Search**: Vector embeddings and MySQL for fast searches across flights, hotels, and photo spots  
💬 **Telegram Integration**: Easy-to-use Telegram bot interface with instant responses  
☁️ **Cloud-Native Data**: Google Drive integration for scalable data management  
🗺️ **Photo Spot Database**: 20+ curated Instagram-worthy locations with optimal times and vibes  
💾 **MySQL + Vector Embeddings**: Hybrid storage for both structured queries and semantic search  
🔄 **Data Pipelines with N8N**: Fully automated ETL workflows for continuous data ingestion and indexing  
🧠 **Conversation Memory**: Maintains 7-message conversation history for context-aware responses  
🎯 **Multi-Intent Processing**: Handle multiple queries simultaneously (e.g., "flights AND hotels AND photo spots")


## Folder Structure

```
ItineraryChatbot/
├── data/
│   ├── updated_flights_with_departure_date.json
│   │   └── Flight ticket data with departure dates (150 flights from 16+ Indonesian cities)
│   ├── dummy_hotels_jakarta.json
│   │   └── Hotel accommodation data (150 hotels across Jakarta areas)
│   └── spot_photo.json
│       └── Instagram-worthy photo locations with vibes and optimal times (20 spots)
│
├── workflows/
│   ├── ETL.json
│   │   └── Extract, Transform, Load workflow for initial data processing
│   ├── Indexing Vector DB.json
│   │   └── Workflow to ingest flight/hotel data into vector database
│   └── ItineraryAgent.json
│       └── Main chatbot with Telegram, LangChain, and multi-tool routing
│
├── screenshots/
│   ├── ETL Data.png
│   │   └── Visual representation of the ETL workflow
│   ├── IndexingVectorDB.png
│   │   └── Illustration of the vector database indexing process
│   └── ItineraryChatbot.png
│       └── Overview of the chatbot's workflow architecture
│
└── README.md (this file)
```

## Project Architecture

```
User Message (Telegram)
         ↓
   [N8N Chatbot Workflow]
         ↓
  [Main Agent (LangChain)]
    ↙    ↓    ↘
   /     |     \
Travel   Spot   Itinerary
Agent   Photo   Agent
Tool    Agent    Tool
|       Tool     |
↓       ↓        ↓
MySQL  Vector   Weather
DB    Search    API
|       ↓        |
└───────┴────────┘
    ↓
Response to User
```

### How It Works

1. **User Input**: User sends a message via Telegram to the bot
2. **Message Parsing**: N8N extracts the message text and chat ID
3. **Intent Recognition**: LangChain agent analyzes the query and determines which tool(s) to use:
   - **Travel Agent Tool**: For flights & hotels → queries MySQL database
   - **Spot Photo Agent Tool**: For photo locations → semantic search in vector embeddings
   - **Itinerary Agent Tool**: For weather-based itineraries → queries Open-Meteo API
4. **Tool Execution**: Tools return relevant data
5. **Response Generation**: Agent combines results and formats response
6. **Telegram Output**: Response sent back to user via Telegram

### Workflow Files Details

#### `ETL.json` - Data Pipeline
- **Purpose**: Extract, Transform, and Load data from Google Drive to MySQL
- **Steps**:
  1. Fetch data files from Google Drive
  2. Download JSON files (flights, hotels)
  3. Split data by type (conditional routing)
  4. Create MySQL tables if not exist:
     - `flights` table: Stores ticket_id, airline, origin, destination, departure_time, arrival_time, price, class, departure_date
     - `hotels` table: Stores hotel_id, name, city, area, price_per_night, rating, stars, amenities
  5. Insert data into respective tables
- **Triggers**: Manual execution or scheduled intervals
- **Screenshot**: `ETL Data.png` shows the visual workflow

#### `Indexing Vector DB.json` - Vector Embedding Pipeline
- **Purpose**: Create embeddings and index data for semantic search
- **Steps**:
  1. Fetch processed data from MySQL or Google Drive
  2. Generate OpenAI embeddings for flight and hotel descriptions
  3. Store embeddings in vector collections
  4. Index data into collections:
     - `flight_collection`: Flight data with embeddings
     - `hotel_collection`: Hotel data with embeddings
- **Used by**: Semantic search queries in the chatbot
- **Screenshot**: `IndexingVectorDB.png` illustrates this workflow

#### `ItineraryAgent.json` - Main Chatbot Workflow
- **Purpose**: Real-time chatbot logic with multi-tool routing
- **Key Components**:
  - **Telegram Trigger**: Listens for incoming messages
  - **Message Parser**: Extracts text and chat ID using Python code
  - **LangChain Main Agent**: Orchestrates tool selection:
    - **Travel Agent Tool**: Queries MySQL for flights/hotels
    - **Spot Photo Agent Tool**: Semantic search for photo locations
    - **Itinerary Agent Tool**: Generates itineraries with weather
  - **GPT-5 Mini**: Model for understanding queries and generating responses
  - **Conversation Memory**: Keeps 7-message history for context
  - **Telegram Output**: Sends formatted responses back to users
- **Special Features**:
  - Multi-intent handling (process multiple queries at once)
  - Conditional routing based on query content
  - Error handling and fallback responses
- **Screenshot**: `ItineraryChatbot.png` shows the complete workflow

## Tech Stack

| Layer | Technology | Purpose |
|-------|-----------|---------|
| **Orchestration** | N8N | Workflow automation and visual pipeline management |
| **AI & LLM** | OpenAI GPT-5 Mini | Query understanding and response generation |
| **AI Framework** | LangChain | Agent orchestration and tool routing |
| **Chat Platform** | Telegram Bot API | User interaction interface |
| **Database** | MySQL | Structured data storage for flights and hotels |
| **Vector Search** | Vector Embeddings | Semantic search for photo spots |
| **Weather Data** | Open-Meteo API | Real-time weather forecasts |
| **Cloud Storage** | Google Drive | Source of truth for data files |
| **Development** | Node.js (N8N) | Workflow execution environment |

### Architecture Details

- **Data Flow**: Google Drive → N8N ETL → MySQL & Vector Store → Chatbot Agent → Telegram Response
- **Memory**: 7-message conversation buffer for context retention
- **Model**: GPT-5 Mini (faster and more efficient than previous versions)
- **Search**: Hybrid approach using MySQL for structured queries + vector embeddings for semantic search

## Setup Instructions

### Prerequisites

Before setting up the project, ensure you have:

- **N8N Instance**: Self-hosted or cloud instance (https://n8n.io/)
- **MySQL Database**: Local or cloud instance (MySQL 5.7+)
- **OpenAI API Key**: For GPT-5 Mini (https://platform.openai.com/)
- **Google Cloud Project**: With Google Drive API enabled
- **Telegram Bot Token**: From @BotFather on Telegram
- **Open-Meteo API**: Free, no key required (https://open-meteo.com/)
- **Node.js**: v18+ (for N8N)

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

### Step 2: Set Up MySQL Database

1. **Install MySQL**:
   ```bash
   # macOS
   brew install mysql
   
   # Windows (via Chocolatey)
   choco install mysql
   
   # Or use Docker
   docker run -d --name mysql -e MYSQL_ROOT_PASSWORD=root -p 3306:3306 mysql:latest
   ```

2. **Create Database**:
   ```sql
   CREATE DATABASE itinerary_chatbot;
   USE itinerary_chatbot;
   ```

3. **Create Tables** (or let ETL workflow do it):
   ```sql
   CREATE TABLE flights (
     ticket_id VARCHAR(20) PRIMARY KEY,
     airline VARCHAR(100),
     origin VARCHAR(100),
     destination VARCHAR(100),
     departure_time DATETIME,
     arrival_time DATETIME,
     price INT,
     class VARCHAR(50),
     departure_date DATE
   );
   
   CREATE TABLE hotels (
     hotel_id VARCHAR(20) PRIMARY KEY,
     name VARCHAR(255),
     city VARCHAR(100),
     area VARCHAR(100),
     price_per_night INT,
     rating DECIMAL(2,1),
     stars INT,
     amenities JSON
   );
   ```

### Step 3: Configure External Services

#### OpenAI Setup
1. Create an account at https://platform.openai.com/
2. Generate an API key from the dashboard
3. In N8N, create credentials for OpenAI with your API key

#### Google Drive Setup
1. Create a Google Cloud project at https://console.cloud.google.com/
2. Enable the Google Drive API
3. Create OAuth 2.0 credentials (Desktop application)
4. In N8N, authorize with Google Drive credentials
5. Share your data folder and copy the folder ID to ETL workflow

#### Telegram Bot Setup
1. Open Telegram and find @BotFather
2. Send `/newbot` and follow instructions to create your bot
3. Save the bot token
4. In N8N, create credentials with the bot token
5. Configure webhook URL in N8N for Telegram trigger

### Step 4: Import Workflows into N8N

1. Open N8N dashboard
2. Create new workflows for:
   - ETL.json (data pipeline)
   - Indexing Vector DB.json (embedding pipeline)
   - ItineraryAgent.json (chatbot)
3. Configure credentials for each workflow
4. Test each workflow before activating

## Data Models

### Flights Table
```json
{
  "ticket_id": "TKT001",
  "airline": "AirAsia",
  "origin": "Manado",
  "destination": "Jakarta",
  "departure_time": "2026-04-13 15:45",
  "arrival_time": "2026-04-13 16:45",
  "price": 1361047,
  "class": "Economy",
  "departure_date": "2026-04-13"
}
```

**Statistics**: 150 flights from 16+ Indonesian cities

### Hotels Table
```json
{
  "hotel_id": "HTL001",
  "name": "Royal Jakarta Hotel 1",
  "city": "Jakarta",
  "area": "Kemang",
  "price_per_night": 1169346,
  "rating": 4.4,
  "stars": 3,
  "amenities": ["Pool", "Bar", "Gym", "Spa", "Restaurant", "Airport Shuttle"]
}
```

**Statistics**: 150 hotels across Jakarta areas (Kemang, Thamrin, Blok M, PIK, etc.)

### Photo Spots Data
```json
{
  "text": "Monas adalah landmark ikonik Jakarta dengan latar kota. Waktu terbaik foto adalah pagi atau sunset untuk cahaya yang lembut.",
  "metadata": {
    "place": "Monas",
    "vibe": ["iconic", "city"],
    "best_time": ["morning", "sunset"],
    "crowd": "high"
  }
}
```

**Statistics**: 20 photo locations with vibes (aesthetic, nature, cultural, etc.)

### Conversation Memory

The chatbot maintains a **7-message sliding window** of conversation history, allowing it to:
- Remember previous context
- Handle follow-up questions naturally
- Maintain conversation continuity
- Clear memory after window exceeds limit

## Contributing

Contributions are welcome! Here's how you can contribute:

1. **Report Issues**: Found a bug? Open an issue with details
2. **Suggest Features**: Have ideas? Create a feature request
3. **Submit PRs**: Improvements are appreciated
4. **Improve Documentation**: Help us make docs better

### Development Workflow

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit changes (`git commit -m 'Add AmazingFeature'`)
4. Push to branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

## License

This project is licensed under the MIT License - see the LICENSE file for details.

## Support

For questions or support:
- Open an issue on GitHub
- Documentation: See README.md for detailed setup guide

---

**Last Updated**: May 5, 2026  
