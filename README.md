# CAILA-BOT   [![CAILA-BOT Architecture](caila-arch.png "CAILA-BOT Architecture")](caila-arch.png)

## Overview

CAILA-BOT is a versatile bot designed to interact across multiple platforms. It leverages a robust, modular architecture for data handling and reasoning, allowing for easy extension and integration with new services. The bot processes user prompts, performs complex reasoning, and provides validated results through a multi-layered system.

![tg_image_2421543055](https://github.com/user-attachments/assets/eac4fa0a-9a89-4680-97b4-969f4b213dd5)


## Architecture

The CAILA-BOT is structured into three distinct layers:

1.  **Bot Layer:** This is the user-facing interface, responsible for receiving prompts from various platforms such as Discord, Telegram, and others. It manages the communication flow and delivers results back to the user. Think of it as the messenger for user interactions. Currently, we are internally testing the Discord bot.

    *   **Example**:
    ```python
    def handle_discord_message(message):
      user_prompt = message.content
      response = process_prompt(user_prompt)
      message.channel.send(response)
    ```

2.  **Reasoning Layer:** The core of the bot's logic. This layer takes the prompt from the Bot Layer and processes it. It uses multiple components, including a reasoning engine, a database, and external API calls. This layer is responsible for interpreting the user intent, accessing the relevant data, and generating actions, resulting in the execution of the prompt.

    The prompt is divided into two parts:

    *   **System Prompt:** This defines the bot's fundamental behavior, acting as a template for the conversation. It is set once at the beginning of a conversation and dictates the overall context (e.g., trip planning, plant harvesting, insurance company usage).

        *   **Example System Prompt:**
            ```text
            You are a helpful trip planning assistant. Your goal is to gather the user's
            travel preferences and suggest possible itineraries. Ask specific questions
            to understand their needs. Do not make assumptions.
            ```
    *   **User Prompt:** This is the ad-hoc, present information provided by the user during the conversation. It is specific to the current interaction and is processed in the context of the ongoing conversation.

        *   **Example User Prompt:**
            ```text
            I want to go to Paris next summer for a week with my family, what do you recommend?
            ```

    *   **Reasoning Engine Example**: This is a high-level example, more details would be needed for a real implementation.
        ```python
        def process_prompt(user_prompt, system_prompt, conversation_history):
            # Parse the user intent
            intent = parse_user_intent(user_prompt)
            # Build the query
            query = build_query(intent)
            # Fetch the relevant information
            data = fetch_data(query, database)
            # Generate the response
            response = generate_response(data, system_prompt, conversation_history)
            return response
        ```

3.  **Data Layer:** The foundation of the bot. It is built around a validation network that collects and validates data from various sources to ensure reliability and accuracy. This network is decentralized, allowing for diverse inputs and robust data integrity. The data is used by the Reasoning Layer to improve responses and generate context for future prompts.

    *   **Validation Process:** The validation network uses multiple validators to ensure data is accurate. This may include methods like cross-checking sources, verifying data against known parameters, and applying statistical techniques to determine validity. It may also be a distributed process.
    *    **Data Storage**: A decentralized data storage is used to keep the data.
    *   **Data Retrieval Example:** To retrieve data, the Data Layer uses a consistent interface. Here's an example of a request and response structure when retrieving weather data:

        *   **Request (Example):**
            ```
            https://<weather endpoint that will release soon>/weather/lat=37.2144&lon=-121.8574
            ```
            This request asks for weather data based on latitude and longitude parameters.
        *   **Response (Example):**
            ```json
            {
              "data": {
                "id": "0",
                "created_at": "0001-01-01T00:00:00Z",
                "updated_at": "0001-01-01T00:00:00Z",
                "latitude": 37.2144,
                "longitude": -121.8574,
                "temperature": 9.23,
                "temperature_min": 7.57,
                "temperature_max": 10.37,
                "feels_like": 7.53,
                "pressure": 1009,
                "humidity": 57,
                "wind_speed": 3.09,
                "wind_scale": 0,
                "wind_speed_list": null,
                "wind_scale_list": null,
                "wind_direction": 0,
                "condition": "Clouds",
                "condition_desc": "broken clouds",
                "condition_code": 803,
                "condition_icon": "04n",
                "uv": 0,
                "luminance": 0,
                "elevation": 1009,
                "rain": 0,
                "wet_bulb": 0,
                "timestamp": 1737859011,
                "timezone": -28800,
                "location_name": "Cambrian Park",
                "address": "",
                "source": "o",
                "tag": "",
                "is_online": false,
                "is_malfunction": false
              },
              "ok": true
            }
            ```
           This JSON response provides detailed weather information, including temperature, humidity, wind conditions, and more. The "ok" field indicates the status of the data retrieval.

## Usage

To get started with CAILA-BOT, follow these steps:

1.  **Installation:**
    *   **Description:** Detailed instructions on how to contribute to the data layer.
    *   **Nubila Node:** [Explore Marco](https://nubila.ai/device)
    *   **Nubila Network:** [Explore Nubila Network](https://nubila.network)

2.  **Configuration:**
    *   **Description:** Steps to configure the necessary API keys, database connections, and other environment variables. A detailed explanation of each configuration parameter is provided to ensure smooth setup.
    *   **Configuration Parameters:**

        *   **API Keys:**
            *   **`CAILA_API_KEY`:** This key is required to authorize requests to the CAILA-BOT API. You can obtain this key from [Link to API key acquisition].
            *   **`DISCORD_BOT_TOKEN`** (If applicable): This token is needed if you want to run and test the Discord bot locally or use it on your server. Obtain this token from the Discord Developer Portal.
            *   **Other platform-specific keys**: If integrating with Telegram or other platforms, you may need platform-specific API keys. This will need to be described in the documentation.
        *   **Database Connection:**
            *   **`DATABASE_URL` or similar:** This connection string contains information for connecting to the decentralized data storage. This might include connection strings, usernames, passwords, and so on, depending on the database used. This could also be an address or IP if it's a decentralized data storage mechanism.
            *   **Additional database parameters:** Depending on the database technology used, more parameters may be necessary, such as `DB_USERNAME`, `DB_PASSWORD`, `DB_HOST`, `DB_PORT` etc.
        *   **External API Endpoints**:
            *   `WEATHER_API_ENDPOINT`: Base URL for the weather service that returns data like in the provided example.
            *   Potentially other API endpoints for other data sources used by the bot.
        *   **Environment Variables**:
            *   **`BOT_MODE`** (Optional): Set the bot operation mode (e.g., `dev`, `test`, `production`).
            *   Other environment variables can be introduced for flexibility in configuration and testing.
        *   **Optional Configuration Parameters:**
            *  **`SYSTEM_PROMPT`** (Optional) If you want to change the default system prompt from code, you can also have this as a configurable parameter.
            *   **Data Validation Rules:** Parameters that determine how strict validation should be, potentially including tolerances.

    *   **Setup Instructions:**

        1.  **Obtain API Keys:**
            *   Get your `CAILA_API_KEY` from the [Link to API key acquisition].
            *   If you are using the discord bot, get a `DISCORD_BOT_TOKEN` from the [Discord Developer Portal].
            *   If other platforms are used, acquire the required keys from their portals.
        2.  **Set Environment Variables:**
            *   You can set environment variables in various ways, for example:
                *   Using your system's environment settings.
                *   Using a `.env` file in the project's root directory (recommended for development).
                *   For deployed environments, use your deployment platform's environment variable settings.
            *   Create a `.env` file, if applicable, and add the following:

            ```
            CAILA_API_KEY=your_caila_api_key
            DISCORD_BOT_TOKEN=your_discord_bot_token
            DATABASE_URL=your_database_connection_string
            WEATHER_API_ENDPOINT=https://<weather endpoint that will release soon>/
            BOT_MODE=dev
            ```
            *   **Note:** Do not commit your `.env` file to version control, and especially don't share your keys publicly.
        3.  **Database Configuration:**
            *   Ensure your `DATABASE_URL` is pointing to the correct database server and the credentials are correct.
        4.  **Testing the Configuration:**
            * You can test your configuration by running the bot in your preferred mode (`dev`, `test`).
            * Ensure the bot can connect to the database and the API, and respond correctly.

    *   **Best Practices:**
        *   Use `.env` files for storing sensitive configurations during development, and never commit them to the repository.
        *   Use environment variables for production and keep them separate from your source code.
        *   Follow your platform's best practices for securing keys and secrets.
        *   Ensure configuration can be adjusted as the platform and requirements evolve.
        *   Implement a way to gracefully handle configuration errors.
    *   **Documentation Location:** [Link to configuration guide]

3.  **Basic Usage Examples:**
    *   **Description:** Provides examples of how to send initial requests to the bot across different platforms. Shows how to interpret different response formats, and provides example use cases.
    *   **Documentation Location:** [Link to usage examples guide] or see the `examples` folder in the repository. 
    *   **Local examples:**  The `examples` folder in the repository contains usage examples for various platforms and use cases.
    *   **Example:**

        ```python
        # Example Python Code to call the API for processing
        import requests

        url = "https://api.cailabot.com/v1/process"

        payload = {
            "user_prompt": "Can you give me some flight recommendations?",
            "system_prompt": "You are a helpful trip planning assistant.",
        }

        headers = {
            "Content-Type": "application/json",
            "Authorization": "Bearer YOUR_API_KEY"
        }

        response = requests.post(url, json=payload, headers=headers)
        print(response.json())
        ```(response.json())
        ```

4.  **Contribution Guidelines:**
    *   **Description:** Guidelines for developers to contribute to the project.

    *   **How to Contribute:**
        *   **Data:** Follow the instructions in the Installation section. Make sure data is accurate.
        *   **Code:**
            1.  Fork the repository.
            2.  Create a branch for your changes.
            3.  Write clear code with comments.
            4.  Include tests for new features.
            5.  Submit a pull request.
        *   **Documentation:** Submit a pull request for small changes. For larger changes, open an issue first.

    *   **Code Style:**
        *   Use Python 3.
        *   Follow PEP 8 style.
        *   Keep comments clear and concise.
        *   Keep `requirements.txt` up-to-date with dependencies
    *   **Pull Requests:**
        *   Create a branch for your changes.
        *   Test your code.
        *   Submit a pull request with a clear description.
        *   Be ready to make changes based on review.

    *   **Community:**
        *   Use GitHub issues for bugs or questions.
        *   Join our Discord channel [[Nubila Discord Channel]](https://discord.com/invite/nubila) for discussions.
        *   Be respectful to other community members.

## Version Control

*   This project is versioned using [Git](https://git-scm.com/) and hosted on [GitHub](https://github.com/your-repo-link)
*   [Link to the GitHub repository](https://github.com/your-repo-link)

## License

This project is licensed under the [MIT License](https://opensource.org/licenses/MIT).

## Contact

*   If you have any questions or comments, please contact the team via:
    *   Email: [marketing@nubila.ai](mailto:marketing@nubila.ai)
    *   Twitter: [[Caila Twitter]](https://x.com/Nubi_AI)
    *   Discord: [[Nubila Discord Channel]](https://discord.com/invite/nubila)

---

**Note:** The image path `caila-arch.png` is relative to this document's location. Please ensure this path is correct or use an absolute URL if the image is hosted elsewhere.
