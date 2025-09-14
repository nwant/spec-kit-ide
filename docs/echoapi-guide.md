# Using EchoAPI with Spec Kit

EchoAPI is a powerful API testing tool integrated directly into VSCode, providing a superior alternative to Thunder Client for spec-driven API development.

## Why EchoAPI?

### Advantages over Thunder Client
- **Better OpenAPI Integration**: Native support for importing/exporting OpenAPI specifications
- **Advanced Testing**: Built-in test scripts, assertions, and response validation
- **Environment Management**: Sophisticated variable management across environments
- **Mock Server**: Built-in mock server generation from specifications
- **Collaboration**: Better team sharing capabilities for API collections
- **Performance**: Lighter weight and faster response times
- **AI Features**: AI-powered test generation and request building

## Installation

1. **Install the Extension**:
   - Open VSCode
   - Go to Extensions (`Cmd+Shift+X`)
   - Search for "EchoAPI"
   - Install the extension by `haok`

2. **Access EchoAPI**:
   - Click the EchoAPI icon in the Activity Bar (left sidebar)
   - Or use `Cmd+Shift+P` → "EchoAPI: Open Panel"

## Configuration for Spec Kit

The `.vscode/settings.json` includes optimized EchoAPI settings:

```json
{
  "echoapi.defaultWorkspace": "${workspaceFolder}/tests/api",
  "echoapi.autoSave": true,
  "echoapi.enableSync": false,
  "echoapi.defaultEnvironment": "development"
}
```

## Workflow Integration

### 1. Import Specification
```yaml
# In your spec file (specs/001/spec.yml)
api:
  openapi: 3.0.0
  info:
    title: Feature API
    version: 1.0.0
  paths:
    /api/users:
      post:
        summary: Create user
        requestBody:
          required: true
          content:
            application/json:
              schema:
                type: object
                properties:
                  email:
                    type: string
                  password:
                    type: string
```

### 2. Generate Tests in EchoAPI
1. Open EchoAPI panel
2. Click "Import" → "OpenAPI"
3. Select your specification file
4. EchoAPI generates a complete collection

### 3. Environment Variables
Create environments for different stages:

```json
{
  "development": {
    "baseUrl": "http://localhost:3000",
    "apiKey": "dev-key-123"
  },
  "staging": {
    "baseUrl": "https://staging.example.com",
    "apiKey": "{{STAGING_API_KEY}}"
  },
  "production": {
    "baseUrl": "https://api.example.com",
    "apiKey": "{{PROD_API_KEY}}"
  }
}
```

### 4. Test Automation
Add test scripts to validate responses:

```javascript
// In EchoAPI test scripts
pm.test("Status code is 201", function () {
    pm.response.to.have.status(201);
});

pm.test("Response matches specification", function () {
    const schema = pm.globals.get("userSchema");
    pm.response.to.have.jsonSchema(schema);
});

pm.test("User ID is returned", function () {
    const jsonData = pm.response.json();
    pm.expect(jsonData).to.have.property('id');
    pm.environment.set("userId", jsonData.id);
});
```

### 5. Mock Server
Generate mock responses from specifications:

1. In EchoAPI, select your collection
2. Click "Mock Server" → "Create"
3. EchoAPI generates endpoints matching your spec
4. Use mock URLs during development

## Best Practices

### Organizing Collections
```
tests/api/
├── collections/
│   ├── authentication.json
│   ├── users.json
│   └── products.json
├── environments/
│   ├── development.json
│   ├── staging.json
│   └── production.json
└── mocks/
    └── responses.json
```

### Naming Conventions
- Collections: Match specification numbers (`001-authentication`, `002-users`)
- Requests: Use descriptive names (`POST Create User`, `GET User by ID`)
- Variables: Use SCREAMING_SNAKE_CASE (`BASE_URL`, `API_KEY`)

### Testing Strategy
1. **Unit Tests**: Individual endpoint validation
2. **Integration Tests**: Multi-endpoint workflows
3. **Contract Tests**: Specification compliance
4. **Performance Tests**: Response time validation

## Integration with Spec Kit Workflow

### During Specification Phase
```yaml
# specs/001/spec.yml
api_tests:
  - name: "User Creation"
    endpoint: "POST /api/users"
    scenarios:
      - valid_user_creation
      - duplicate_email_rejection
      - invalid_data_validation
```

### During Planning Phase
```yaml
# specs/001/plan.yml
testing_strategy:
  tool: echoapi
  collections:
    - authentication_flow
    - user_management
  environments:
    - development
    - staging
```

### During Implementation
1. Generate code from specifications
2. Run EchoAPI tests against implementation
3. Validate responses match specification
4. Update tests as implementation evolves

## Advanced Features

### Dynamic Variables
```javascript
// Pre-request script
const timestamp = Date.now();
pm.environment.set("timestamp", timestamp);

const hash = CryptoJS.MD5(timestamp + pm.environment.get("secret")).toString();
pm.environment.set("signature", hash);
```

### Chained Requests
```javascript
// Save response data for next request
const response = pm.response.json();
pm.collectionVariables.set("authToken", response.token);
```

### Response Validation
```javascript
// Validate against JSON Schema
const schema = {
  type: "object",
  required: ["id", "email", "created_at"],
  properties: {
    id: { type: "string", format: "uuid" },
    email: { type: "string", format: "email" },
    created_at: { type: "string", format: "date-time" }
  }
};

pm.test("Response matches schema", () => {
  pm.response.to.have.jsonSchema(schema);
});
```

### Performance Testing
```javascript
pm.test("Response time is less than 200ms", () => {
  pm.expect(pm.response.responseTime).to.be.below(200);
});
```

## Troubleshooting

### Common Issues

**Collections not saving**:
- Check workspace settings for `echoapi.defaultWorkspace`
- Ensure directory exists and has write permissions

**Import failing**:
- Validate OpenAPI specification format
- Check for unsupported OpenAPI extensions
- Ensure YAML/JSON is properly formatted

**Environment variables not working**:
- Select correct environment in EchoAPI
- Check variable syntax: `{{VARIABLE_NAME}}`
- Ensure variables are defined in environment

**Mock server not responding**:
- Verify mock server is running
- Check port availability
- Validate mock response configuration

## Tips and Tricks

1. **Use Collection Variables** for shared data across requests
2. **Create Test Suites** for different specification scenarios
3. **Export Collections** as part of specification documentation
4. **Generate Code** from EchoAPI for client SDKs
5. **Use Pre-request Scripts** for dynamic data generation
6. **Implement Response Caching** for faster test execution
7. **Set up CI/CD Integration** with EchoAPI CLI

## Resources

- [EchoAPI Documentation](https://www.echoapi.com/docs)
- [EchoAPI VSCode Extension](https://marketplace.visualstudio.com/items?itemName=haok.echo-api)
- [OpenAPI Specification](https://swagger.io/specification/)
- [Spec Kit Documentation](https://github.com/github/spec-kit)

## Command Reference

### VSCode Commands
- `EchoAPI: Open Panel` - Open EchoAPI interface
- `EchoAPI: Import OpenAPI` - Import specification
- `EchoAPI: Export Collection` - Export to file
- `EchoAPI: Run Collection` - Execute all requests
- `EchoAPI: Generate Mock` - Create mock server

### Keyboard Shortcuts
- `Cmd+Enter` - Send request
- `Cmd+S` - Save collection
- `Cmd+E` - Switch environment
- `Cmd+T` - New request tab
- `Cmd+D` - Duplicate request
