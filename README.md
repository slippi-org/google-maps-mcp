# Google Maps MCP Server

MCP Server for the Google Maps API.

## Tools

1. `maps_geocode`

   - Convert address to coordinates
   - Input: `address` (string)
   - Returns: location, formatted_address, place_id

2. `maps_reverse_geocode`

   - Convert coordinates to address
   - Inputs:
     - `latitude` (number)
     - `longitude` (number)
   - Returns: formatted_address, place_id, address_components

3. `maps_search_places`

   - Search for places using text query
   - Inputs:
     - `query` (string)
     - `location` (optional): { latitude: number, longitude: number }
     - `radius` (optional): number (meters, max 50000)
   - Returns: array of places with names, addresses, locations

4. `maps_place_details`

   - Get detailed information about a place
   - Input: `place_id` (string)
   - Returns: comprehensive place details including:
     - name, address, location coordinates
     - contact information (phone, website)
     - ratings and reviews
     - opening hours and time zone
     - photos
     - price level and business status
     - accessibility options
     - payment options
     - parking options
     - attributes (takeout, delivery, etc.)
     - and more data provided by the Places API (New)

5. `maps_distance_matrix`

   - Calculate distances and times between points
   - Inputs:
     - `origins` (string[])
     - `destinations` (string[])
     - `mode` (optional): "driving" | "walking" | "bicycling" | "transit" | "two_wheeler"
   - Returns: distances and durations matrix

6. `maps_elevation`

   - Get elevation data for locations
   - Input: `locations` (array of {latitude, longitude})
   - Returns: elevation data for each point

7. `maps_directions`
   - Get directions between points
   - Inputs:
     - `origin` (string)
     - `destination` (string)
     - `mode` (optional): "driving" | "walking" | "bicycling" | "transit" | "two_wheeler"
   - Returns: route details with steps, distance, duration

## Setup

### Installation

This is a standalone fork of the Google Maps MCP server with enhanced features. To install:

```bash
git clone https://github.com/slippi-org/google-maps-mcp.git
cd google-maps-mcp
npm install
npm run build
```

### API Key

Get a Google Maps API key by following the instructions [here](https://developers.google.com/maps/documentation/javascript/get-api-key#create-api-keys).

### Usage with Claude Desktop

Add the following to your `claude_desktop_config.json`:

#### Local Installation

```json
{
  "mcpServers": {
    "google-maps": {
      "command": "node",
      "args": ["/full/path/to/google-maps-mcp/dist/index.js"],
      "env": {
        "GOOGLE_MAPS_API_KEY": "<YOUR_API_KEY>"
      }
    }
  }
}
```

#### Docker

```bash
# Build the Docker image
docker build -t slippi-org/google-maps-mcp .
```

```json
{
  "mcpServers": {
    "google-maps": {
      "command": "docker",
      "args": [
        "run",
        "-i",
        "--rm",
        "-e",
        "GOOGLE_MAPS_API_KEY",
        "slippi-org/google-maps-mcp"
      ],
      "env": {
        "GOOGLE_MAPS_API_KEY": "<YOUR_API_KEY>"
      }
    }
  }
}
```

### Usage with VS Code

Add the following JSON block to your User Settings (JSON) file in VS Code. You can do this by pressing `Ctrl + Shift + P` and typing `Preferences: Open User Settings (JSON)`.

Optionally, you can add it to a file called `.vscode/mcp.json` in your workspace.

> Note that the `mcp` key is not needed in the `.vscode/mcp.json` file.

```json
{
  "mcp": {
    "inputs": [
      {
        "type": "promptString",
        "id": "maps_api_key",
        "description": "Google Maps API Key",
        "password": true
      }
    ],
    "servers": {
      "google-maps": {
        "command": "node",
        "args": ["/full/path/to/google-maps-mcp/dist/index.js"],
        "env": {
          "GOOGLE_MAPS_API_KEY": "${input:maps_api_key}"
        }
      }
    }
  }
}
```

For Docker installation:

```json
{
  "mcp": {
    "inputs": [
      {
        "type": "promptString",
        "id": "maps_api_key",
        "description": "Google Maps API Key",
        "password": true
      }
    ],
    "servers": {
      "google-maps": {
        "command": "docker",
        "args": ["run", "-i", "--rm", "slippi-org/google-maps-mcp"],
        "env": {
          "GOOGLE_MAPS_API_KEY": "${input:maps_api_key}"
        }
      }
    }
  }
}
```

## Development

### Building

```bash
npm run build
```

### Watching for changes

```bash
npm run watch
```

### Testing locally

```bash
GOOGLE_MAPS_API_KEY=your_api_key_here node dist/index.js
```

### Docker build

```bash
docker build -t slippi-org/google-maps-mcp .
```

## Features

This fork includes enhanced features compared to the original:

- **Two-wheeler support**: Added `two_wheeler` travel mode for directions and distance matrix
- **Enhanced place details**: Uses the new Google Places API for comprehensive place information
- **Improved error handling**: Better error messages and debugging information
- **Updated APIs**: Uses latest Google Maps APIs including Routes API v2

## License

This MCP server is licensed under the MIT License. This means you are free to use, modify, and distribute the software, subject to the terms and conditions of the MIT License. For more details, please see the LICENSE file in the project repository.