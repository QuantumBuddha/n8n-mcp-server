# n8n MCP Server

An MCP server that provides access to n8n workflows, executions, credentials, and more through the Model Context Protocol. This allows Large Language Models (LLMs) to interact with n8n instances in a secure and standardized way.

## Requirements:

-node.js

-Redis

## Installation

### Get your n8n API Key

1. Log into your n8n instance
2. Click your user icon in the bottom left
3. Go to Settings
4. Select API
5. Click "Create API Key"
6. Copy your API key (you won't be able to see it again)

### Install the MCP Server

#### Option 1: Install from npm (Recommended)

```bash
npm install -g @QuantumBuddha/n8n-mcp-server
```

#### Option 2: Install from Source

1. Clone the repository:
   ```bash
   cd ~/.nvm/versions/node/v20.17.0/lib/node_modules/
   git clone https://github.com/QuantumBuddha/n8n-mcp-server.git
   cd n8n-mcp-server
   ```

2. Install dependencies and build:
   ```bash
   npm install ioredis
   npm install --save-dev @types/ioredis
   npm install
   npm run build
   ```
   IF YOU ARE USING N8N, THAT SHOULD BE IT....READY TO GO.  No need to start the server, n8n will start it when calling the tool or node.


3. Start the server in the background:
   ```bash
   nohup npm start > n8n-mcp.log 2>&1 &
   ```

   To stop the server:
   ```bash
   pkill -f "node build/index.js"
   ```

Note: When installing from npm, the server will be available as `n8n-mcp-server` in your PATH.

## Configuration

### Claude Desktop

1. Open your Claude Desktop configuration:
   ```
   ~/Library/Application Support/Claude/claude_desktop_config.json
   ```

2. Add the n8n configuration:
   ```json
   {
     "mcpServers": {
        "n8n": {
         "command": "n8n-mcp-server",
         "env": {
           "N8N_HOST": "https://your-n8n-instance.com",
           "N8N_API_KEY": "your-api-key-here"
         }
       }
     }
   }
   ```

### Cline (VS Code)

1. Install the server (follow Installation steps above)
2. Open VS Code
3. Open the Cline extension from the left sidebar
4. Click the 'MCP Servers' icon at the top of the pane
5. Scroll to bottom and click 'Configure MCP Servers'
6. Add to the opened settings file:
   ```json
   {
     "mcpServers": {
       "n8n": {
         "command": "n8n-mcp-server",
         "env": {
           "N8N_HOST": "https://your-n8n-instance.com",
           "N8N_API_KEY": "your-api-key-here"
         }
       }
     }
   }
   ```

### n8n Tool

1. Install the server (follow Installation steps above)
2. n8n
3. Add new credential
4. Select Command Line (STDIO)
5. Add the following fields:
   Command: node
   Arguments : <PATH TO index.js> "~/.nvm/versions/node/v20.17.0/lib/node_modules/@illuminaresolutions/n8n-mcp-server/build"
   Env: N8N_HOST=<FULL HOST URL> "https://n8n.example.com" N8N_API_KEY="EXAMPLE N8N API KEY"
6. It may be necessary to set the N8N_HOST, and N8N_API_KEY in a "Set" node and pass it into the agent, or hard code it into the system message for the agent.
   ```

## Validation

After configuration:

1. Restart your LLM application
2. Ask: "List my n8n workflows"
3. You should see your workflows listed

If you get an error:
- Check that your n8n instance is running
- Verify your API key has correct permissions
- Ensure N8N_HOST has no trailing slash

## Features

### Core Features
- List and manage workflows
- View workflow details
- Execute workflows
- Manage credentials
- Handle tags and executions
- Generate security audits
- Manage workflow tags

### Enterprise Features
These features require an n8n Enterprise license:
- Project management
- Variable management
- Advanced user management

## Troubleshooting

### Common Issues

1. "Client not initialized"
   - Check N8N_HOST and N8N_API_KEY are set correctly
   - Ensure n8n instance is accessible
   - Verify API key permissions

2. "License required"
   - You're trying to use an Enterprise feature
   - Either upgrade to n8n Enterprise or use core features only

3. Connection Issues
   - Verify n8n instance is running
   - Check URL protocol (http/https)
   - Remove trailing slash from N8N_HOST

## Security Best Practices

1. API Key Management
   - Use minimal permissions necessary
   - Rotate keys regularly
   - Never commit keys to version control

2. Instance Access
   - Use HTTPS for production
   - Enable n8n authentication
   - Keep n8n updated

## License

[MIT License](LICENSE)
