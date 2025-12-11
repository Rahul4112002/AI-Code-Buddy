# AI Code Buddy 🤖

[![n8n](https://img.shields.io/badge/n8n-Workflow-FF6D5A?logo=n8n)](https://n8n.io)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Multi-Agent System](https://img.shields.io/badge/AI-Multi--Agent-blue)]()
[![LangChain](https://img.shields.io/badge/LangChain-Powered-green)](https://langchain.com)

An intelligent **multi-agent AI system** built with n8n that analyzes GitHub repositories, generates comprehensive documentation, and delivers insights through email or Google Drive. Perfect for developers who want to quickly understand codebases, generate project documentation, or get AI-powered code analysis.

## 🎯 Project Goals

AI Code Buddy helps you:

1. **Analyze GitHub Repositories** - Provide any GitHub repo URL
2. **Intelligent Code Understanding** - AI comprehends and summarizes the entire codebase
3. **Project Overview** - Get detailed information about what the project does
4. **Quick Start Guide** - Receive step-by-step instructions to get the project running
5. **Installation Instructions** - Learn how to install and set up the project on your system
6. **Module Breakdown** - Detailed explanation of what each code module does
7. **Code Reference** - Quick-glance list of important objects and functions with descriptions
8. **Interactive Q&A** - Chat with an AI assistant to ask questions about the codebase

## 🏗️ Architecture

### Multi-Agent System

AI Code Buddy uses a **coordinated multi-agent architecture** with three specialized AI agents:

```
┌─────────────────────────────────────────────────────┐
│              Orchestrator Agent                     │
│  (Coordinates GitHub & Storage agents)              │
└────────────────┬────────────────────────────────────┘
                  │
        ┌─────────┴──────────┐
        │                    │
        ▼                    ▼
┌───────────────┐    ┌──────────────────┐
│ GitHub Agent  │    │  Storage Agent   │
│ - Repo Tree   │    │  - Email         │
│ - File List   │    │  - Google Docs   │
│ - File Content│    │  - Markdown      │
│ - Metadata    │    │  - Drive Storage │
└───────────────┘    └──────────────────┘
```

#### 1. **Orchestrator Agent** 🎭
- **Role**: Main coordinator that manages workflow
- **Model**: Qwen 3-32B (via Groq)
- **Responsibilities**:
  - Parses GitHub URLs to extract owner and repo names
  - Coordinates between GitHub and Storage agents
  - Manages the overall analysis and delivery workflow
  - Maintains conversation context with memory

#### 2. **GitHub Agent** 🔍
- **Role**: Handles all GitHub repository interactions
- **Model**: Llama 3.3-70B Versatile (via Groq)
- **Tools**:
  - `getRepoTree` - Retrieves complete directory structure
  - `listFiles` - Gets flat file listings from specific paths
  - `getFile` - Fetches individual file contents
  - `getRepo` - Retrieves repository metadata
- **Features**: Window memory (last 3 interactions)

#### 3. **Storage & Delivery Agent** 📦
- **Role**: Manages content storage and delivery
- **Model**: Llama 3.1-8B Instant (via Groq)
- **Tools**:
  - `sendMail` - Sends analysis via Gmail
  - `createDocs` - Creates Google Docs documents
  - `updateDocs` - Updates existing Google Docs
  - `createMarkdown` - Saves markdown files to Google Drive
- **Features**: Window memory (last 3 interactions)

## 🛠️ Technology Stack

- **Workflow Automation**: [n8n](https://n8n.io) (AI Agent Workflow)
- **AI Framework**: [LangChain](https://langchain.com)
- **Language Models**: 
  - Qwen 3-32B (Orchestration)
  - Llama 3.3-70B (GitHub Analysis)
  - Llama 3.1-8B (Document Processing)
- **LLM Provider**: [Groq](https://groq.com) (Ultra-fast inference)
- **Integrations**:
  - GitHub API
  - Gmail API
  - Google Docs API
  - Google Drive API

## 📋 Prerequisites

Before setting up AI Code Buddy, ensure you have:

### Required Accounts
1. **n8n Instance** (Self-hosted or n8n Cloud)
2. **Groq API Key** ([Get it free](https://console.groq.com))
3. **GitHub Account** with Personal Access Token
4. **Google Account** with:
   - Gmail API enabled
   - Google Docs API enabled
   - Google Drive API enabled

### API Credentials Setup

#### 1. Groq API Key
```bash
# Visit https://console.groq.com
# Sign up and generate an API key
# Copy the key for n8n configuration
```

#### 2. GitHub Personal Access Token
```bash
# Go to GitHub Settings → Developer Settings → Personal Access Tokens
# Generate new token with 'repo' scope
# Copy token for n8n
```

#### 3. Google OAuth2 Credentials
- Visit [Google Cloud Console](https://console.cloud.google.com)
- Create a new project
- Enable Gmail, Google Docs, and Google Drive APIs
- Create OAuth2 credentials
- Add authorized redirect URIs for n8n

## 🚀 Installation

### Step 1: Clone the Repository

```bash
git clone https://github.com/Rahul4112002/AI-Code-Buddy.git
cd AI-Code-Buddy
```

### Step 2: Import Workflow to n8n

1. Open your n8n instance
2. Click **"Workflows"** → **"Import from File"**
3. Select `AI-Code-Buddy.json`
4. The workflow will be imported with all nodes

### Step 3: Configure Credentials

You need to set up the following credentials in n8n:

#### A. Groq API Credentials
1. Click on any LLM node (Github Model, Document model, Orchestrator model)
2. Add new credential → Select "Groq API"
3. Enter your Groq API key
4. Save and apply to all three model nodes

#### B. GitHub API Credentials
1. Click on any GitHub tool node (listFiles, getFile, getRepo)
2. Add new credential → Select "GitHub API"
3. Enter your Personal Access Token
4. Save and apply to all GitHub nodes

#### C. Google Gmail OAuth2
1. Click on "Send a message" node
2. Add new credential → Select "Gmail OAuth2"
3. Follow OAuth flow to authenticate
4. Grant required permissions

#### D. Google Docs OAuth2
1. Click on "Create a document in Google Docs" or "Update a document in Google Docs"
2. Add new credential → Select "Google Docs OAuth2"
3. Complete OAuth authentication
4. Grant permissions

#### E. Google Drive OAuth2
1. Click on "Create Markdown" node
2. Add new credential → Select "Google Drive OAuth2"
3. Authenticate and grant permissions

### Step 4: Configure Sub-Workflow

The workflow uses a sub-workflow for `getRepoTree` tool:

1. Import or create the "Get Repo Tree tool" sub-workflow
2. Note its workflow ID
3. Update the `getRepoTree` node with the correct workflow ID
4. Ensure the sub-workflow has GitHub credentials configured

### Step 5: Activate Workflow

1. Click the toggle at the top right to activate the workflow
2. The Chat Trigger will generate a webhook URL
3. Use this URL to interact with your AI Code Buddy

## 💡 Usage

### Basic Usage

1. **Start a Chat Session**
   - Open the workflow's chat interface (via webhook URL or n8n's test chat)

2. **Provide a GitHub Repository URL**
   ```
   User: Analyze this repo: https://github.com/owner/repository-name
   ```

3. **Ask for Specific Tasks**
   - "Summarize this project for me"
   - "Give me installation instructions"
   - "List all the main functions in the codebase"
   - "Send the analysis to my email"
   - "Create a markdown documentation file"

### Example Interactions

**Example 1: Quick Analysis**
```
User: Analyze https://github.com/langchain-ai/langchain

AI: I'll analyze the LangChain repository for you...
[Provides project summary, key features, and architecture overview]
```

**Example 2: Email Delivery**
```
User: Send me a detailed analysis of this repo via email: https://github.com/n8n-io/n8n

AI: I'll analyze the n8n repository and email you the results...
[Generates comprehensive report and sends via Gmail]
```

**Example 3: Documentation Creation**
```
User: Create documentation for https://github.com/fastapi/fastapi and save it to Google Drive

AI: Creating comprehensive documentation...
[Generates markdown file and uploads to your Google Drive]
```

## 🔧 Advanced Configuration

### Customizing Agent Behaviors

Each agent has a customizable system message. You can modify these in the workflow:

#### Orchestrator Agent
```javascript
// Modify the systemMessage in Orchestrator Agent node
// Adjust coordination logic, URL parsing, or workflow steps
```

#### GitHub Agent
```javascript
// Customize how GitHub data is fetched and formatted
// Add additional GitHub API operations
```

#### Storage Agent
```javascript
// Modify output formatting
// Add new delivery channels (Slack, Discord, etc.)
```

### Memory Configuration

All agents use **Buffer Window Memory** with a context window of 3:

```javascript
// To increase/decrease context window:
// Edit memoryBufferWindow nodes
// Change contextWindowLength parameter
```

### Switching LLM Models

You can change the models used by each agent:

1. Click on model nodes (Github Model, Document model, Orchestrator model)
2. Select a different Groq model from the dropdown
3. Available options: Llama 3.1, Llama 3.3, Mixtral, Gemma, Qwen

## 📦 Workflow Components

### Nodes Overview

| Node Name | Type | Purpose |
|-----------|------|----------|
| Chat trigger Node | Chat Trigger | Initiates conversation with natural language |
| Orchestrator Agent | AI Agent | Main coordinator for all operations |
| Github Agent | Agent Tool | Specialized in GitHub repository operations |
| Storage and Delivery Agent | Agent Tool | Handles output delivery and storage |
| Github Model | Groq LLM | Powers GitHub Agent (Llama 3.3-70B) |
| Document model | Groq LLM | Powers Storage Agent (Llama 3.1-8B) |
| Orchestrator model | Groq LLM | Powers main orchestrator (Qwen 3-32B) |
| listFiles | GitHub Tool | Lists files in repository paths |
| getFile | GitHub Tool | Retrieves file contents |
| getRepoTree | Workflow Tool | Gets complete directory structure |
| getRepo | GitHub Tool | Fetches repository metadata |
| Send a message | Gmail Tool | Sends email with analysis |
| Create a document in Google Docs | Google Docs Tool | Creates new docs |
| Update a document in Google Docs | Google Docs Tool | Updates existing docs |
| Create Markdown | Google Drive Tool | Saves markdown to Drive |

## 🎯 Use Cases

1. **Code Review Preparation**
   - Quickly understand a codebase before reviewing PRs
   - Get AI-generated summaries of changes

2. **Onboarding New Developers**
   - Generate comprehensive project documentation
   - Create quick-start guides for team members

3. **Technical Documentation**
   - Auto-generate markdown documentation
   - Create Google Docs for team wikis

4. **Code Learning**
   - Understand open-source projects
   - Get explanations of complex codebases

5. **Project Discovery**
   - Evaluate multiple repositories quickly
   - Compare different implementations

## 🔒 Security & Privacy

- **Credentials**: All API credentials are securely stored in n8n's credential system
- **Data Privacy**: No code is stored permanently; analysis happens in real-time
- **API Limits**: Respects GitHub API rate limits and Groq token limits
- **Self-Hosted**: Can be fully self-hosted for complete data control

## 🐛 Troubleshooting

### Common Issues

**Issue 1: "GitHub API Rate Limit Exceeded"**
```
Solution: Wait for rate limit reset or use authenticated requests
```

**Issue 2: "Groq API Error"**
```
Solution: Verify API key is valid and has sufficient credits
```

**Issue 3: "Google OAuth Expired"**
```
Solution: Re-authenticate Google credentials in n8n
```

**Issue 4: "Sub-workflow not found"**
```
Solution: Create/import the Get Repo Tree sub-workflow and update ID
```

### Debug Mode

1. Enable execution logging in n8n settings
2. Check node outputs in workflow execution view
3. Review agent system messages for clarity
4. Test individual tools separately

## 🤝 Contributing

Contributions are welcome! Here's how you can help:

1. **Fork the Repository**
2. **Create a Feature Branch**
   ```bash
   git checkout -b feature/AmazingFeature
   ```
3. **Commit Changes**
   ```bash
   git commit -m 'Add some AmazingFeature'
   ```
4. **Push to Branch**
   ```bash
   git push origin feature/AmazingFeature
   ```
5. **Open a Pull Request**

### Ideas for Contributions

- Add support for more LLM providers (OpenAI, Anthropic, etc.)
- Integrate additional delivery channels (Slack, Discord, Telegram)
- Add code quality analysis features
- Support for private repositories
- Create a web UI interface
- Add caching for faster repeated queries

## 📝 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 👨‍💻 Author

**Rahul Chauhan**
- GitHub: [@Rahul4112002](https://github.com/Rahul4112002)
- Twitter: [@R4hulChauhan](https://twitter.com/R4hulChauhan)
- Website: [rahul4112.me](https://rahul4112.me)

## 🙏 Acknowledgments

- [n8n](https://n8n.io) for the amazing workflow automation platform
- [LangChain](https://langchain.com) for the AI agent framework
- [Groq](https://groq.com) for blazing-fast LLM inference
- The open-source community for inspiration and tools

## 🚧 Roadmap

- [ ] Add support for GitLab and Bitbucket
- [ ] Implement code diff analysis
- [ ] Add automated testing suggestions
- [ ] Create visual code architecture diagrams
- [ ] Support for multiple programming languages analysis
- [ ] Add code quality metrics
- [ ] Implement PR review automation
- [ ] Create browser extension for quick access

## 📞 Support

If you have questions or need help:

- Open an [Issue](https://github.com/Rahul4112002/AI-Code-Buddy/issues)
- Start a [Discussion](https://github.com/Rahul4112002/AI-Code-Buddy/discussions)
- Reach out on [Twitter](https://twitter.com/R4hulChauhan)

---

⭐ **If you find this project helpful, please give it a star!** ⭐

---

**Built with ❤️ using n8n, LangChain, and Groq**