# Migrating from Thunder Client to EchoAPI

This guide helps you transition from Thunder Client to EchoAPI for API testing in your spec kit projects.

## Why Migrate?

### EchoAPI Advantages
- **Better OpenAPI Support**: Direct import/export of OpenAPI specifications
- **Advanced Testing**: More sophisticated test scripts and assertions
- **Mock Servers**: Built-in mock generation from specifications
- **AI Features**: AI-powered request generation and testing
- **Performance**: Faster and more lightweight
- **Team Collaboration**: Better sharing and synchronization features

## Migration Steps

### 1. Install EchoAPI

```bash
# Uninstall Thunder Client (optional)
code --uninstall-extension rangav.vscode-thunder-client

# Install EchoAPI
code --install-extension haok.echo-api
```

### 2. Export Thunder Client Collections

If you have existing Thunder Client collections:

1. Open Thunder Client
2. Click on Collections
3. Right-click each collection → Export → JSON
4. Save to a temporary folder

### 3. Import into EchoAPI

1. Open EchoAPI panel (Activity Bar → EchoAPI icon)
2. Click Import → Thunder Client Collection
3. Select your exported JSON files
4. EchoAPI will convert and import them

### 4. Update File Structure

**Old Structure (Thunder Client):**
```
thunder-tests/
├── thunderActivity.json
├── thunderCollection.json
├── thunderEnvironment.json
└── thunderclient.json
```

**New Structure (EchoAPI):**
```
tests/api/
├── collections/
│   ├── 001-authentication.json
│   ├── 002-users.json
│   └── 003-products.json
├── environments/
│   ├── development.json
│   ├── staging.json
│   └── production.json
└── mocks/
    └── responses.json
```

### 5. Convert Environment Variables

**Thunder Client Format:**
```json
{
  "client": "Thunder Client",
  "environmentName": "Development",
  "dateExported": "2024-01-01T00:00:00.000Z",
  "version": "1.0",
  "variables": [
    {
      "name": "baseUrl",
      "value": "http://localhost:3000"
    }
  ]
}
```

**EchoAPI Format:**
```json
{
  "name": "Development",
  "values": {
    "baseUrl": "http://localhost:3000",
    "apiKey": "dev-key-123",
    "timeout": 5000
  }
}
```

### 6. Update Test Scripts

**Thunder Client Tests:**
```javascript
// Basic status check
tests["Status is 200"] = responseCode.code === 200;
tests["Has user id"] = json.id !== undefined;
```

**EchoAPI Tests:**
```javascript
// More advanced testing with pm API
pm.test("Status code is 200", function () {
    pm.response.to.have.status(200);
});

pm.test("Response has required fields", function () {
    const jsonData = pm.response.json();
    pm.expect(jsonData).to.have.property('id');
    pm.expect(jsonData.id).to.be.a('string');
});

pm.test("Response time is acceptable", function () {
    pm.expect(pm.response.responseTime).to.be.below(200);
});
```

### 7. Update VSCode Settings

Remove Thunder Client settings and add EchoAPI settings:

```json
{
  // Remove these Thunder Client settings
  // "thunder-client.saveToWorkspace": true,
  // "thunder-client.workspaceRelativePath": "thunder-tests",
  
  // Add EchoAPI settings
  "echoapi.defaultWorkspace": "${workspaceFolder}/tests/api",
  "echoapi.autoSave": true,
  "echoapi.enableSync": false,
  "echoapi.defaultEnvironment": "development"
}
```

### 8. Update .gitignore

```diff
- # Thunder Client
- thunder-tests/
- thunder-client/
+ # EchoAPI
+ echoapi/
+ .echoapi/
+ *.echoapi.json
+ echoapi-workspace/
```

## Feature Comparison

| Feature | Thunder Client | EchoAPI |
|---------|---------------|----------|
| Basic Requests | ✅ | ✅ |
| Collections | ✅ | ✅ |
| Environments | ✅ | ✅ |
| Variables | ✅ | ✅ |
| Pre-request Scripts | ❌ | ✅ |
| Test Scripts | Basic | Advanced |
| OpenAPI Import | Limited | Full |
| Mock Server | ❌ | ✅ |
| GraphQL | ✅ | ✅ |
| WebSocket | ❌ | ✅ |
| gRPC | ❌ | ✅ |
| AI Features | ❌ | ✅ |
| Team Sync | Paid | ✅ |
| Response Validation | Basic | JSON Schema |
| Performance Testing | ❌ | ✅ |

## Common Migration Issues

### Issue: Collections not appearing
**Solution**: Ensure collections are saved in the correct directory structure as configured in settings.

### Issue: Environment variables not working
**Solution**: Update variable syntax from `{{variable}}` to `{{VARIABLE}}` (uppercase).

### Issue: Tests failing after migration
**Solution**: Update test syntax to use the Postman-compatible `pm` API.

### Issue: Authentication not working
**Solution**: EchoAPI handles auth differently. Set up authentication in the request's Auth tab.

## Best Practices After Migration

1. **Organize Collections**: Group by specification number (001, 002, etc.)
2. **Use Folders**: Create folders within collections for better organization
3. **Leverage Pre-request Scripts**: Generate dynamic data before requests
4. **Implement Comprehensive Tests**: Use EchoAPI's advanced testing features
5. **Set up Mock Servers**: Generate mocks from your OpenAPI specs
6. **Use Collection Variables**: Share data across requests efficiently
7. **Document Requests**: Add descriptions to requests and parameters

## Quick Reference

### EchoAPI Keyboard Shortcuts
- `Cmd+Enter` - Send request
- `Cmd+S` - Save collection
- `Cmd+E` - Switch environment
- `Cmd+T` - New request
- `Cmd+D` - Duplicate request
- `Cmd+/` - Toggle comments

### Useful EchoAPI Commands
```bash
# VSCode Command Palette (Cmd+Shift+P)
EchoAPI: Open Panel
EchoAPI: Import OpenAPI
EchoAPI: Export Collection
EchoAPI: Generate Mock Server
EchoAPI: Run Collection
```

## Support Resources

- [EchoAPI Documentation](https://www.echoapi.com/docs)
- [EchoAPI Migration Guide](https://www.echoapi.com/docs/migration)
- [EchoAPI VSCode Extension](https://marketplace.visualstudio.com/items?itemName=haok.echo-api)
- [Spec Kit IDE Issues](https://github.com/YOUR_USERNAME/spec-kit-ide/issues)

## Need Help?

If you encounter issues during migration:
1. Check the [EchoAPI Guide](echoapi-guide.md)
2. Review your collection export/import logs
3. Verify environment variable syntax
4. Test with a simple request first
5. Report issues on the spec-kit-ide repository
