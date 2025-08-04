# Repository Tour

## 🎯 What This Repository Does

AI-New-Agent is a configuration repository for Qodo AI-powered code review agents that automatically analyze GitHub pull requests for security and performance issues.

**Key responsibilities:**
- Configure AI agents for automated code review workflows
- Define MCP (Model Context Protocol) server integrations for tool access
- Provide GitHub Actions automation for agent deployment and initialization

---

## 🏗️ Architecture Overview

### System Context
```
GitHub PR → [GitHub Actions] → [Qodo AI Agent] → [MCP Servers] → [Code Analysis]
                                      ↓
                              [Review Comments/Results]
```

### Key Components
- **Agent Configuration** - TOML-based agent definitions with commands, tools, and execution strategies
- **MCP Server Integration** - Protocol servers providing access to shell, git, filesystem, and GitHub APIs
- **GitHub Actions Workflows** - Automated CI/CD pipelines for agent execution and repository initialization
- **Multi-Model Support** - Configurable AI model selection (Claude, GPT, Gemini, etc.)

### Data Flow
1. GitHub workflow triggers on PR events or manual dispatch
2. Qodo CLI installs and configures the agent environment
3. Agent loads configuration and connects to MCP servers for tool access
4. AI model analyzes code diffs using configured tools (git, ripgrep, filesystem)
5. Results are formatted and returned as PR comments or structured output

---

## 📁 Project Structure [Partial Directory Tree]

```
AI-New-Agent/
├── agent.toml              # Main agent configuration (imports agents/agent.toml)
├── agents/                 # Agent-specific configurations
│   └── agent.toml         # Detailed code review agent definition
├── mcp.json               # MCP server configurations for tool integrations
├── .github/               # GitHub Actions workflows
│   └── workflows/
│       ├── main.yml       # Code review agent workflow
│       └── qodo-init.yml  # Repository initialization workflow
└── .git/                  # Git repository metadata
```

### Key Files to Know

| File | Purpose | When You'd Touch It |
|------|---------|---------------------|
| `agent.toml` | Main agent entry point | Changing model or adding new agent imports |
| `agents/agent.toml` | Code review agent configuration | Modifying review criteria, tools, or commands |
| `mcp.json` | MCP server definitions | Adding new tool integrations or changing server configs |
| `.github/workflows/main.yml` | Code review automation | Adjusting workflow triggers or agent parameters |
| `.github/workflows/qodo-init.yml` | Repository setup | Modifying initialization process or qodo.md generation |

---

## 🔧 Technology Stack

### Core Technologies
- **Configuration Language:** TOML - Human-readable configuration format for agent definitions
- **Protocol:** MCP (Model Context Protocol) - Standardized interface for AI tool integrations
- **AI Models:** Multi-model support (Claude-4-Sonnet, GPT-4o, Gemini-2.5-Pro, etc.)
- **Automation:** GitHub Actions - CI/CD workflows for agent execution

### Key Libraries
- **@qodo/command** - Qodo CLI for agent management and execution
- **mcp-shell-server** - Shell command execution via MCP
- **mcp-git-server** - Git operations through MCP protocol
- **mcp-fs-server** - Filesystem access via MCP
- **mcp-ripgrep-server** - Code search capabilities through ripgrep

### Development Tools
- **uvx** - Python package runner for MCP server execution
- **GitHub CLI (gh)** - GitHub API integration for PR operations
- **ripgrep** - Fast code search and pattern matching

---

## 🌐 External Dependencies

### Required Services
- **Qodo AI Platform** - Core AI agent execution and model access
- **GitHub API** - Pull request analysis and comment posting
- **GitHub Actions** - Workflow execution environment

### Optional Integrations
- **Multiple AI Models** - Fallback between different model providers based on availability
- **GitHub Copilot API** - Additional AI model access through GitHub's MCP endpoint

### Environment Variables

```bash
# Required
QODO_API_KEY=              # Authentication for Qodo AI platform
GH_TOKEN=                  # GitHub API access token
GH_MCP_TOKEN=              # GitHub MCP server authentication

# Agent Configuration (via workflow)
target_branch=             # PR target branch for comparison
severity_threshold=        # Minimum issue severity (low/medium/high)
focus_areas=               # Analysis focus (security,performance)
exclude_files=             # Files to skip during review
include_suggestions=       # Whether to include improvement suggestions
models=                    # Comma-separated list of AI models to use
```

---

## 🔄 Common Workflows

### Code Review Automation
1. Developer creates or updates a pull request
2. GitHub Actions workflow triggers the code review agent
3. Agent analyzes code diffs using configured MCP tools
4. AI model evaluates changes for security and performance issues
5. Results are posted as PR comments with severity-based filtering

**Code path:** `GitHub PR` → `.github/workflows/main.yml` → `qodo-ai/command@v1` → `agents/agent.toml`

### Repository Initialization
1. Manual workflow dispatch triggers qodo-init workflow
2. Qodo CLI is installed and configured in the GitHub Actions environment
3. `qodo init` command generates or updates the qodo.md documentation
4. Changes are automatically committed and pushed to the repository

**Code path:** `workflow_dispatch` → `.github/workflows/qodo-init.yml` → `qodo init` → `qodo.md`

---

## 📈 Performance & Scale

### Performance Considerations
- **Model Selection:** Multiple AI models available for load balancing and fallback
- **File Filtering:** Configurable exclusion patterns to skip irrelevant files (package-lock.json, *.md)
- **Severity Thresholds:** Configurable filtering to reduce noise in review comments

### Monitoring
- **Workflow Status:** GitHub Actions provides execution logs and status tracking
- **Agent Output:** Structured JSON schema for success/failure reporting with scoring

---

## 🚨 Things to Be Careful About

### 🔒 Security Considerations
- **API Keys:** Sensitive tokens stored in GitHub Secrets (QODO_API_KEY, GH_TOKEN)
- **MCP Server Access:** Shell server has broad command execution capabilities ("ALLOW_COMMANDS": "*")
- **Repository Permissions:** Workflows require write access to contents and pull requests

### Configuration Management
- **Agent Dependencies:** Changes to MCP server configurations may break tool integrations
- **Model Availability:** Ensure selected AI models are accessible and properly configured
- **Workflow Permissions:** Verify GitHub Actions have appropriate repository access levels

*Updated at: 2025-01-08 UTC*