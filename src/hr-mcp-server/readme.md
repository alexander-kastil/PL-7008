## HR MCP Server Smoke Test

### Run the server

```powershell
dotnet run
```

### Connect with MCP Inspector

1. Start the server (see above).
2. Launch the inspector with the provided config:

```powershell
npx @modelcontextprotocol/inspector --config inspector.config.json --server hr-mcp
```

The config at `inspector.config.json` tells the inspector to use the HR MCP server's Streamable HTTP base URL `http://localhost:47002`, matching the endpoint that `app.MapMcp()` exposes. This satisfies the newer CLI requirement that `--server` reference an entry in a config file.

#### Remote (Azure) deployment

```powershell
npx @modelcontextprotocol/inspector --config inspector.config.json --server hr-mcp-azure-dev
```

This reuses the same inspector config but selects the `hr-mcp-azure-dev` entry, which points at `https://human-resource-mcp-pl7008.azurewebsites.net`. Make sure the Azure app is running and reachable before launching the inspector.

> **Heads-up:** Updating `inspector.config.json` is a local workflow change only—you don't need to republish the Azure App Service after editing this file. Republish the .NET app itself only when the server code or configuration that lives in the deployed artifact changes.

## Smoke Test

Verify the Azure deployment is operational:

1. Launch the MCP inspector connected to the remote Azure app:

   ```powershell
   npx @modelcontextprotocol/inspector --config inspector.config.json --server hr-mcp-azure-dev
   ```

2. Confirm in the browser inspector that all **4 tools** are listed and accessible:
   - `ListEmployees` – retrieves all employees
   - `AddEmployee` – adds a new employee with optional languages and skills
   - `UpdateEmployee` – updates an existing employee by email
   - `DeleteEmployee` – removes an employee by email

3. Call `ListEmployees` tool and verify the response contains the seeded employee data with proper JSON structure.

4. If all tools are present and `ListEmployees` returns data, the deployment is healthy.
