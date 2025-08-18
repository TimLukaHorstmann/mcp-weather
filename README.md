# MCP Weather Server

[![npm version](https://img.shields.io/npm/v/@timlukahorstmann/mcp-weather)](https://www.npmjs.com/package/@timlukahorstmann/mcp-weather)
[![license](https://img.shields.io/github/license/TimLukaHorstmann/mcp-weather)](https://github.com/TimLukaHorstmann/mcp-weather/blob/main/LICENSE)
[![node version](https://img.shields.io/node/v/@timlukahorstmann/mcp-weather)](https://www.npmjs.com/package/@timlukahorstmann/mcp-weather)
[![issues](https://img.shields.io/github/issues/TimLukaHorstmann/mcp-weather)](https://github.com/TimLukaHorstmann/mcp-weather/issues)
[![weekly downloads](https://img.shields.io/npm/dm/@timlukahorstmann/mcp-weather)](https://www.npmjs.com/package/@timlukahorstmann/mcp-weather)

[![Trust Score](https://archestra.ai/mcp-catalog/api/badge/quality/TimLukaHorstmann/mcp-weather)](https://archestra.ai/mcp-catalog/timlukahorstmann__mcp-weather)

<p align="center">
  <img src="logo.png" alt="MCP Weather Server Logo" width="250"/>
  <a href="https://glama.ai/mcp/servers/@TimLukaHorstmann/mcp-weather">
    <img width="380" height="200" src="https://glama.ai/mcp/servers/@TimLukaHorstmann/mcp-weather/badge" alt="Weather MCP server" />
  </a>
</p>

A Model Context Protocol (MCP) server that provides hourly and daily weather forecasts using the AccuWeather API.

---

## Quick Start

You need an AccuWeather API key (free tier available).  
[Sign up here](https://developer.accuweather.com/) and create an app to get your key.

Export your API key as an environment variable:

```bash
export ACCUWEATHER_API_KEY=your_api_key_here
```

Then run the MCP Weather server directly with:

```bash
npx -y @timlukahorstmann/mcp-weather
```

Or, for HTTP/REST access via [supergateway](https://github.com/supercorp-ai/supergateway):

```bash
npx -y supergateway --stdio "npx -y @timlukahorstmann/mcp-weather" \
  --port 4004 \
  --baseUrl http://127.0.0.1:4004 \
  --ssePath /messages \
  --messagePath /message \
  --cors "*" \
  --env ACCUWEATHER_API_KEY="$ACCUWEATHER_API_KEY"
```

---

## MCP Server Config Example

For integration with Claude Desktop or other MCP-compatible clients, add this to your config (e.g. `claude_desktop_config.json`):

```json
{
  "mcpServers": {
    "weather": {
      "command": "npx",
      "args": ["-y", "@timlukahorstmann/mcp-weather"],
      "env": {
        "ACCUWEATHER_API_KEY": "your_api_key_here"
      }
    }
  }
}
```

---

## Overview

This MCP server allows large language models (like Claude) to access real-time weather data. When integrated with an LLM, it enables the model to:

- Fetch accurate, up-to-date weather forecasts
- Provide hourly weather data for the next 12 hours
- Access daily weather forecasts for up to 15 days
- Display data in both metric (°C) and imperial (°F) units
- View temperature, conditions, precipitation information, and other weather details

## Available Tools

### Hourly Weather Forecast
- Tool name: `weather-get_hourly`
- Provides hourly forecasts for the next 12 hours
- Parameters:
  - `location` (required): City or location name
  - `units` (optional): "metric" (Celsius, default) or "imperial" (Fahrenheit)

### Daily Weather Forecast
- Tool name: `weather-get_daily`
- Provides daily forecasts for up to 15 days
- Parameters:
  - `location` (required): City or location name
  - `days` (optional): Number of forecast days (1, 5, 10, or 15; default is 5)
  - `units` (optional): "metric" (Celsius, default) or "imperial" (Fahrenheit)

## Prerequisites

- Node.js ≥18  
- An AccuWeather API key (set via `.env` or your shell)

## Setup

1. **Clone this repository:**
   ```bash
   git clone https://github.com/TimLukaHorstmann/mcp-weather.git
   cd mcp-weather
   ```

2. **Install dependencies:**
   ```bash
   npm install
   ```

3. **Get an AccuWeather API key:**
   - Register at [AccuWeather API](https://developer.accuweather.com/)
   - Create a new app and obtain an API key

4. **Create a `.env` file with your API key:**
   ```
   ACCUWEATHER_API_KEY=your_api_key_here
   ```

5. **Build the project:**
   ```bash
   npm run build
   ```

## Usage with Claude Desktop

1. Configure Claude Desktop to use this MCP server:
   - Open Claude Desktop
   - Go to Settings > Developer > Edit Config
   - Add the following to your `claude_desktop_config.json`:

   ```json
   {
     "mcpServers": {
       "weather": {
         "command": "npx",
         "args": ["-y", "@timlukahorstmann/mcp-weather"],
         "env": {
           "ACCUWEATHER_API_KEY": "your_api_key_here"
         }
       }
     }
   }
   ```

2. Restart Claude Desktop

3. In a new conversation, enable the MCP server by clicking the plug icon and selecting "weather"

4. Now you can ask Claude for weather forecasts, such as:
   - "What's the hourly weather forecast for New York City?"
   - "Give me the 5-day forecast for London."
   - "What will the weather be like in Tokyo this week in Fahrenheit?"
   - "Will it rain in San Francisco tomorrow?"

## Development

- Install dev dependencies: `npm install`
- Lint your code:           `npm run lint`  
- Build:                    `npm run build`  
- Run tests:                `npm test`
- Start in dev mode:        `npm run dev`

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## Future Enhancements

We're always looking to improve the MCP Weather Server. Here are some features we're considering for future releases:

- **Extended Hourly Forecasts:** Beyond 12 hours, e.g., 24 or 48 hours.
- **Weather Alerts:** Integration with AccuWeather's severe weather alerts API.
- **Location Autocomplete:** Improved location searching with autocomplete suggestions.
- **Historical Weather Data:** Access to past weather conditions.

If you have ideas for other features, feel free to open an issue!

## Changelog

### 0.4.0
- Removed `sessionId` requirement from all tools as it was not used for anything internally
- This simplifies integrations and reduces confusion for LLM usage

### 0.3.0 and earlier
- Initial releases with basic functionality

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
