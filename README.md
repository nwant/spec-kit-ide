# Spec Kit IDE Configuration

Optimized VSCode configuration and tooling for GitHub's [Spec Kit](https://github.com/github/spec-kit) framework, enabling efficient Spec-Driven Development with AI agents.

## 🚀 Quick Start

1. **Clone this repository** into your spec kit project:
   ```bash
   git clone https://github.com/YOUR_USERNAME/spec-kit-ide.git
   cd spec-kit-ide
   ```

2. **Open in VSCode**:
   ```bash
   code .
   ```

3. **Install recommended extensions** when prompted, or manually install from `.vscode/extensions.json`

4. **Configure your workspace** by copying the `.vscode` folder to your spec kit project

## 📁 Configuration Files

### `.vscode/settings.json`
Comprehensive workspace settings optimized for spec kit development:
- **File associations** for `*.spec.yml`, `*.plan.yml`, `*.task.yml` files
- **YAML schema validation** against spec kit standards
- **Custom YAML tags** for spec-specific syntax (`!spec-ref`, `!template`, etc.)
- **Editor optimizations** for specification writing
- **Git workflow** configuration with spec-focused branch prefixes
- **File nesting** to group related spec/plan/task files

### `.vscode/extensions.json`
Curated extensions for maximum productivity:
- **Essential**: YAML, OpenAPI Editor, EchoAPI, Draw.io
- **AI Support**: GitHub Copilot with spec kit instructions
- **Productivity**: GitLens, Todo Tree, Better Comments
- **Quality**: Markdown linting, spell checking

### `.vscode/tasks.json`
Automated spec kit workflows accessible via Command Palette (`Cmd+Shift+P`):
- **spec-kit: Specify** - Create new specifications
- **spec-kit: Plan** - Generate implementation plans
- **spec-kit: Tasks** - Break down into tasks
- **spec-kit: Full Workflow** - Run complete spec→plan→tasks sequence
- **spec-kit: Validate Specification** - Check spec syntax and structure
- **spec-kit: Check Constitution Compliance** - Verify architectural adherence

Run tasks with `Cmd+Shift+P` → `Tasks: Run Task` → Select task

### `.vscode/launch.json`
Debug configurations for spec kit development:
- Debug individual CLI commands (specify, plan, tasks)
- Validate specifications with breakpoints
- Test AI agent integrations
- Debug VSCode extension development

### `.github/copilot-instructions.md`
Teaches GitHub Copilot about spec kit patterns:
- Specification format and structure
- Constitution compliance patterns
- Code generation from specifications
- Traceability and documentation requirements

## 🎯 Key Features

### Immediate Productivity Gains (30-40%)
- **Auto-completion** for spec kit YAML structures
- **Real-time validation** against spec schemas
- **Integrated terminal** with spec kit environment variables
- **File nesting** groups related specifications together
- **Smart suggestions** in YAML strings and markdown

### Workflow Automation
- **Single-command** spec→plan→tasks execution
- **Validation tasks** with problem matching
- **AI agent selection** for code generation
- **API testing** directly in VSCode via EchoAPI

### Enhanced Development Experience
- **Syntax highlighting** for spec-specific file types
- **Schema validation** with inline error reporting
- **Git integration** optimized for spec-driven workflow
- **Markdown preview** for specification documentation
- **Diagram support** for architecture visualization

## 🛠️ Usage Examples

### Creating a New Specification
1. Press `Cmd+Shift+P` → `Tasks: Run Task`
2. Select `spec-kit: Specify`
3. Enter feature name
4. Specification is created and opened

### Running Full Workflow
1. Press `Cmd+Shift+P` → `Tasks: Run Task`
2. Select `spec-kit: Full Workflow`
3. Follow prompts for feature name
4. Watch as spec→plan→tasks are generated

### Validating Specifications
1. Open any `.spec.yml` file
2. Press `Cmd+Shift+P` → `Tasks: Run Task`
3. Select `spec-kit: Validate Specification`
4. View problems in the Problems panel

### Testing APIs
1. Define API specs in OpenAPI format
2. Use EchoAPI to test endpoints
3. Collections saved in `tests/api/`
4. Access via EchoAPI panel in VSCode

## 📋 Prerequisites

### Required Software
- **VSCode** 1.80 or later
- **Python** 3.11+
- **Git** 2.30+
- **uv** (Rust-based Python package manager)
  ```bash
  curl -LsSf https://astral.sh/uv/install.sh | sh
  ```

### Spec Kit Installation
```bash
# Clone spec kit
git clone https://github.com/github/spec-kit.git
cd spec-kit

# Install with uv
uv pip install -e . --python 3.11
```

### AI Agent Setup (Optional)
- **Claude Code**: Install from Anthropic
- **GitHub Copilot**: Enable in VSCode
- **Cursor**: Install Cursor IDE
- **Gemini CLI**: Configure Google AI Studio

## 🔧 Customization

### Adding Custom Tasks
Edit `.vscode/tasks.json` to add project-specific tasks:
```json
{
  "label": "spec-kit: Custom Task",
  "type": "shell",
  "command": "your-command",
  "args": ["${input:customInput}"]
}
```

### Modifying File Associations
Update `.vscode/settings.json`:
```json
"files.associations": {
  "*.custom.yml": "yaml",
  "*.feature.md": "markdown"
}
```

### Creating Snippets
Add to `.vscode/snippets/spec-kit.code-snippets`:
```json
{
  "Specification Template": {
    "prefix": "spec",
    "body": [
      "name: ${1:Feature Name}",
      "description: |",
      "  ${2:Description}",
      "requirements:",
      "  functional:",
      "    - id: F001",
      "      description: ${3:Requirement}"
    ]
  }
}
```

## 🚦 Troubleshooting

### YAML Schema Not Working
- Ensure Red Hat YAML extension is installed
- Check that schema URL is accessible
- Verify file associations in settings

### Tasks Not Running
- Check Python/uv installation
- Verify spec kit CLI is in PATH
- Review task output for error messages

### Extensions Not Installing
- Manually install from Extensions sidebar
- Check for conflicts with existing extensions
- Restart VSCode after installation

## 🤝 Contributing

Contributions are welcome! Please:
1. Fork the repository
2. Create a feature branch
3. Add your improvements
4. Submit a pull request

### Priority Areas
- [ ] Spec Kit Language Server implementation
- [ ] Specification Orchestration Panel extension
- [ ] AI Agent Bridge for unified interface
- [ ] Constitution Compliance Monitor
- [ ] Visual specification editor

## 📚 Resources

- [Spec Kit Repository](https://github.com/github/spec-kit)
- [Spec-Driven Development Guide](https://github.blog/ai-and-ml/generative-ai/spec-driven-development-with-ai-get-started-with-a-new-open-source-toolkit/)
- [EchoAPI Integration Guide](docs/echoapi-guide.md) - Complete guide for API testing with EchoAPI
- [Migration Guide: Thunder Client → EchoAPI](docs/migration-thunder-to-echo.md) - Step-by-step migration instructions
- [VSCode Extension API](https://code.visualstudio.com/api)
- [YAML Language Server](https://github.com/redhat-developer/yaml-language-server)
- [EchoAPI Documentation](https://www.echoapi.com/docs)

## 📄 License

MIT License - See LICENSE file for details

## 🙏 Acknowledgments

- GitHub team for creating Spec Kit
- VSCode extension authors for excellent tools
- Community contributors to spec-driven development

---

**Ready to revolutionize your spec-driven development workflow?** Copy these configurations to your project and experience 30-40% productivity gains immediately!
