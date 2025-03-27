# Python Modules Documentation

This document outlines each Python module's purpose, functionality, and interactions within the Flask application, along with the integrated systems they form.

## Core Modules

### `app.py`

**Overview**
This file serves as the main entry point for the Flask application, initializing the app, setting up core routes, and integrating various modules for character management, chat, and AI interactions.

**Functions**
- **index()**: Serves the static `index.html` file as the application's frontend.
- **check_routes()**: Verifies that all critical API routes are properly registered.
- Routes defined within modules registered via `register_*_routes()` functions.

**Role in the Application**
- Initializes the Flask application and configures CORS.
- Sets up the primary route (`/`) to serve the static frontend.
- Acts as a central hub, importing and registering routes from other modules.
- Manages the application's startup and runtime settings via `Config`.
- Provides route verification to ensure API integrity.

### `config.py`

**Overview**
This file manages configuration settings for the application, including API keys, model settings, file paths, and default prompt templates.

**Properties**
- **API_KEYS**: OpenRouter API key for communicating with AI services.
- **API and Model Settings**: Default model configuration and local model endpoints.
- **Application Metadata**: APP_NAME and APP_REFERER for API service identification.
- **Server Settings**: Host, port, and debug flags.
- **Directory Paths**: Paths for data, static files, and various content folders.
- **Default Templates**: Predefined prompt templates for AI interaction.

**Functions**
- **ensure_directories()**: Creates necessary directories if they do not exist.

**Role in the Application**
- Centralizes configuration management, providing a single source of truth for settings.
- Loads environment variables from a `.env` file using `dotenv`.
- Defines defaults for API keys, model URLs, file paths, and prompt templates.
- Ensures the file system is prepared for character and memory storage.

## Character Management

### `character_management.py`

**Overview**
This module handles CRUD operations for characters, managing their lifecycle.

**Functions**
- **register_character_routes(app)**: Registers routes for character management.
- **get_characters()**: Retrieves a list of all saved characters.
- **get_character(character_id)**: Retrieves a specific character by ID.
- **create_character()**: Creates a new character with default or provided attributes.
- **update_character(character_id)**: Updates an existing character's attributes.
- **delete_character(character_id)**: Deletes a character and its memory file.
- **generate_field_fallback()**: Provides a fallback route for field-specific generation.

### `character_generation.py`

**Overview**
This module generates AI-powered character profiles based on user prompts.

**Functions**
- **register_character_generation_routes(app)**: Registers the character generation route.
- **generate_character()**: Creates a character profile with fields like description and personality using AI.

## Chat Management

### `chat_management.py`

**Overview**
This module manages chat interactions with characters, handling messages, player actions, and scene generation.

**Functions**
- **register_chat_routes(app)**: Registers chat-related routes.
- **get_chat_history(chat_id)**: Retrieves a chat instance's conversation history.
- **chat(chat_id)**: Processes user messages, generates AI responses, updates character state, and provides scene descriptions.

### `chat_instances.py`

**Overview**
This module manages chat instances, which represent distinct conversations with characters, and handles character selection for new chats.

**Functions**
- **register_chat_instance_routes(app)**: Registers routes for chat instance management.
- **get_chat_instances()**: Retrieves a list of all chat instances.
- **get_chat_instance(chat_id)**: Retrieves a specific chat instance by ID.
- **create_chat_instance()**: Creates a new chat instance with a character, including initial greeting.
- **update_chat_instance(chat_id)**: Updates an existing chat instance's attributes.
- **delete_chat_instance(chat_id)**: Deletes a chat instance.
- **generate_location()**: Generates a location name for a chat instance.

## AI Integration

### `ai_integration.py`

**Overview**
This module integrates with AI models, handling requests to OpenRouter and local models, and processing responses.

**Functions**
- **register_ai_routes(app)**: Registers AI-related routes including generate-field.
- **get_models()**: Retrieves available models from OpenRouter or returns defaults.
- **generate_text()**: Provides a general-purpose text generation endpoint.
- **generate_json()**: Creates structured JSON outputs from AI responses.
- **generate_field()**: Generates content for specific character fields.
- **get_openrouter_response()**: Sends a request to OpenRouter with robust error handling.
- **get_local_model_response()**: Sends a request to a local model.
- **process_llm_response()**: Extracts structured data from AI responses.
- **validate_json_response()**: Validates and extracts JSON from text.

### `scene_generation.py`

**Overview**
This module generates narrative scene descriptions and location names for character interactions.

**Functions**
- **generate_scene_description()**: Creates a vivid scene description incorporating dialogue, actions, and environment.
- **generate_location_description()**: Generates a location name appropriate for a character based on their traits.

## Supporting Systems

### `memory_management.py`

**Overview**
This module manages character memory and conversation history, creating prompts and summarizing conversations.

**Functions**
- **create_system_prompt()**: Builds a system prompt from character data, memories, and templates.
- **summarize_conversations()**: Summarizes older conversations to manage memory size, preserving recent ones.

### `player_actions.py`

**Overview**
This module handles player actions in the roleplaying context, enhancing system prompts with action details and outcomes.

**Functions**
- **handle_player_action_prompt()**: Modifies the system prompt to include player action details and instructions for AI response.

### `prompt_management.py`

**Overview**
This module manages prompt templates for AI interactions, allowing retrieval, updates, and resets.

**Functions**
- **register_prompt_routes(app)**: Registers prompt management routes.
- **get_prompts()**: Retrieves current prompt templates.
- **get_default_prompts()**: Returns default prompt templates.
- **update_prompts()**: Updates prompt templates with provided data.
- **reset_prompts()**: Resets templates to defaults.

### `system_management.py`

**Overview**
This module provides endpoints for system configuration, connection testing, and diagnostics.

**Functions**
- **register_system_routes(app)**: Registers system management routes.
- **get_public_config()**: Returns public configuration settings, including available models.
- **test_connection()**: Tests connectivity to OpenRouter or a local model.
- **get_diagnostic()**: Provides diagnostic information about the application's status.

## Integrated Systems

### Chat Instance System

The chat instance system manages multiple conversations with different characters, maintaining separate states for each chat.

**Key Components**
- Backend endpoints for managing chat instances
- Message processing and AI responses in the context of a chat instance

**Primary Features**
- Multiple concurrent chats with different characters
- Character state management within conversations
- AI-generated and custom locations for each chat
- Persistence of chat histories with timestamps
- Character selection for new chat creation

### Prompt Templates System

The prompt templates system manages text templates used throughout the application for AI interactions.

**Template Categories**
- Character Definition (basic, appearance, personality)
- Interaction & Context (context, roleplay, response)
- Scene & Environment (scene, environment, cinematic)
- Player Actions (success, failure, skills)

**Template Variables**
Templates include variables like `{name}`, `{description}`, `{personality}`, etc., which are replaced with actual values during processing.

### Model Configuration System

The model configuration system manages AI model selection, interaction parameters, and response handling.

**Default Model**
The application uses `deepseek/deepseek-llm-7b-chat` as the default model, configurable via environment variables, settings, and development options.

**Parameters**
- Temperature controls randomness (0.1-2.0)
- Response length controls verbosity

### Field Generation System

The field generation system provides AI-assisted content creation for specific character attributes.

**Field Types**
- Character names with appropriate style
- Character backgrounds and storylines
- Physical descriptions and visual details
- Psychological traits and behaviors
- Voice characteristics and speech patterns
- Initial character introductions

**Reliability Features**
- Multiple route registrations for redundancy
- Graceful error handling for configuration issues
- Fallback mechanisms when components are unavailable

### Route Verification System

The route verification system ensures API endpoints are properly registered and available.

**Verified Routes**
- Character management endpoints
- Character generation endpoint
- Field-specific generation endpoint
- Chat messaging endpoint
- Model listing endpoint
- Chat instance management endpoints

**Benefits**
- Early detection of missing routes
- Simplified troubleshooting
- Consistent API availability