# 🤖 N8N-CHAT-TRIGGER

<div align="center">

![N8N](https://img.shields.io/badge/n8n-FF6D5A?style=for-the-badge&logo=n8n&logoColor=white)
![Node.js](https://img.shields.io/badge/Node.js-339933?style=for-the-badge&logo=nodedotjs&logoColor=white)
![Telegram](https://img.shields.io/badge/Telegram-26A5E4?style=for-the-badge&logo=telegram&logoColor=white)
![License](https://img.shields.io/badge/License-MIT-yellow.svg?style=for-the-badge)

**An advanced n8n workflow automation system that triggers complex workflows through chat messages**

[🚀 Quick Start](#-quick-start) • [📖 Documentation](#-documentation) • [🛠️ Examples](#-workflow-examples) • [🤝 Contributing](#-contributing)

</div>

---

## 📋 Table of Contents

- [✨ Features](#-features)
- [🎯 Use Cases](#-use-cases)
- [📋 Prerequisites](#-prerequisites)
- [🚀 Quick Start](#-quick-start)
- [⚙️ Configuration](#️-configuration)
- [🛠️ Workflow Examples](#️-workflow-examples)
- [🔧 API Reference](#-api-reference)
- [📱 Supported Platforms](#-supported-platforms)
- [🐛 Troubleshooting](#-troubleshooting)
- [🤝 Contributing](#-contributing)
- [📄 License](#-license)

## ✨ Features

### 🎯 Core Functionality
- **Multi-Platform Support**: Works with Telegram, Discord, Slack, WhatsApp, and more
- **Smart Parsing**: Advanced message parsing with regex and keyword detection
- **IoT Integration**: Direct control of IoT devices through chat commands
- **Conditional Logic**: Complex branching based on user roles, time, location
- **Rate Limiting**: Built-in protection against spam and abuse
- **Error Handling**: Comprehensive error recovery and logging

### 🔄 Advanced Workflows
- **Chainable Actions**: Link multiple workflows together
- **Database Integration**: Store and retrieve data from various databases
- **API Orchestration**: Connect to external services and APIs
- **File Processing**: Handle file uploads, downloads, and transformations
- **Scheduling**: Time-based triggers and delayed executions
- **User Management**: Role-based access control and permissions

## 🎯 Use Cases

### 🏠 Home Automation
User: "Turn on living room lights"
Bot: ✅ Living room lights activated

text

### 📊 Data Retrieval
User: "/weather Chennai"
Bot: 🌤️ Chennai: 32°C, Partly Cloudy, Humidity: 78%

text

### 🔧 System Management
User: "/server status"
Bot: 🖥️ Server Status: ✅ Online, CPU: 45%, RAM: 62%

text

### 📱 Notification Hub
User: "/notify team Project deployed successfully"
Bot: 📢 Message sent to 12 team members

text

## 📋 Prerequisites

### System Requirements
- **Node.js**: 16.x or higher
- **npm**: 7.x or higher
- **Memory**: Minimum 512MB RAM
- **Storage**: 100MB available space

### Dependencies
Core Dependencies
n8n >= 1.0.0
node >= 16.0.0

Optional Dependencies
redis >= 6.0.0 # For caching and rate limiting
postgresql >= 12 # For data persistence

text

## 🚀 Quick Start

### 1. Installation

Clone the repository
```git clone https://github.com/Jernish-FDO/N8N-CHAT-TRIGGER.git```

cd N8N-CHAT-TRIGGER

Install n8n globally
npm install -g n8n

Install project dependencies
npm install

Start n8n
n8n start

text

### 2. Import Workflow

1. Open n8n web interface (default: `http://localhost:5678`)
2. Go to **Workflows** → **Import from file**
3. Select the `chat-trigger-workflow.json` file
4. Configure your credentials

### 3. Basic Setup

// Example webhook configuration
{
"httpMethod": "POST",
"path": "chat-trigger",
"responseMode": "responseNode"
}

text

## ⚙️ Configuration

### 🔑 Environment Variables

Create a `.env` file in your project root:

N8N Configuration
N8N_HOST=localhost
N8N_PORT=5678
N8N_PROTOCOL=http

Database (Optional)
DB_TYPE=sqlite
DB_SQLITE_DATABASE=database.sqlite

Security
N8N_ENCRYPTION_KEY=your_encryption_key_here

Chat Platform Tokens
TELEGRAM_BOT_TOKEN=your_telegram_bot_token
DISCORD_BOT_TOKEN=your_discord_bot_token
SLACK_BOT_TOKEN=your_slack_bot_token

text

### 📱 Platform Configuration

#### Telegram Setup
1. Create bot with @BotFather
2. Get bot token
3. Set webhook URL
curl -X POST "https://api.telegram.org/bot<TOKEN>/setWebhook"
-H "Content-Type: application/json"
-d '{"url": "https://your-domain.com/webhook/telegram"}'

text

#### Discord Setup
// Discord bot configuration
{
"token": "your_discord_bot_token",
"intents": ["GUILDS", "GUILD_MESSAGES", "MESSAGE_CONTENT"],
"prefix": "!"
}

text

## 🛠️ Workflow Examples

### 💡 Smart Home Control

{
"name": "IoT Device Control",
"trigger": {
"type": "webhook",
"platform": "telegram"
},
"conditions": [
{
"field": "message.text",
"operator": "regex",
"value": "^(turn on|turn off|dim|brighten) (.+)$"
}
],
"actions": [
{
"type": "http_request",
"url": "http://home-assistant.local/api/services/light/toggle",
"method": "POST",
"headers": {
"Authorization": "Bearer {{$env.HOME_ASSISTANT_TOKEN}}"
}
}
]
}

text

### 📊 System Monitoring

{
"name": "Server Health Check",
"trigger": {
"type": "webhook",
"command": "/health"
},
"actions": [
{
"type": "execute_command",
"command": "df -h && free -m && uptime"
},
{
"type": "format_response",
"template": "🖥️ System Status\n\n📁 Disk: {{disk_usage}}\n💾 RAM: {{memory_usage}}\n⏱️ Uptime: {{uptime}}"
}
]
}

text

### 🌐 API Integration

{
"name": "Weather Information",
"trigger": {
"type": "webhook",
"command": "/weather"
},
"actions": [
{
"type": "http_request",
"url": "https://api.openweathermap.org/data/2.5/weather",
"params": {
"q": "{{$json.location}}",
"appid": "{{$env.OPENWEATHER_API_KEY}}"
}
},
{
"type": "code",
"code": "return { weather: 🌤️ ${items.json.name}: ${items.json.main.temp}°C, ${items.json.weather.description} };"
}
]
}

text

## 🔧 API Reference

### Webhook Endpoints

| Endpoint | Method | Description |
|----------|--------|-------------|
| `/webhook/telegram` | POST | Telegram bot webhook |
| `/webhook/discord` | POST | Discord bot webhook |
| `/webhook/slack` | POST | Slack events API |
| `/webhook/whatsapp` | POST | WhatsApp Business API |

### Response Format

{
"success": true,
"data": {
"message": "Command executed successfully",
"result": "...",
"timestamp": "2025-08-23T12:59:00Z"
},
"error": null
}

text

## 📱 Supported Platforms

<div align="center">

| Platform | Status | Features |
|----------|--------|----------|
| 📱 **Telegram** | ✅ Full Support | Inline keyboards, file uploads, webhooks |
| 💬 **Discord** | ✅ Full Support | Slash commands, embeds, reactions |
| 💼 **Slack** | ✅ Full Support | Interactive messages, modals, shortcuts |
| 📞 **WhatsApp** | 🔄 Beta | Text messages, media sharing |
| 📧 **Email** | ✅ Full Support | SMTP/IMAP integration |
| 🌐 **Web Chat** | 🔄 Coming Soon | Custom web widget |

</div>

## 🐛 Troubleshooting

### Common Issues

#### ❌ Webhook Not Receiving Messages
Check n8n logs
n8n logs --level=debug

Verify webhook URL is accessible
curl -X GET https://your-domain.com/webhook/telegram

text

#### ❌ Authentication Errors
// Verify credentials in n8n
// Credentials → Add → Select Platform → Test Connection

text

#### ❌ Workflow Not Triggering
1. Check webhook configuration
2. Verify platform permissions
3. Review execution logs
4. Test with manual execution

### Debug Mode

Start n8n in debug mode
DEBUG=n8n* n8n start

View detailed logs
tail -f ~/.n8n/logs/n8n.log

text

## 🚀 Advanced Features

### 🔒 Security & Rate Limiting

// Rate limiting configuration
{
"rateLimit": {
"windowMs": 60000, // 1 minute
"maxRequests": 30, // per user
"skipSuccessfulRequests": true
},
"authentication": {
"type": "bearer",
"required": true
}
}

text

### 📊 Analytics & Monitoring

// Built-in analytics
{
"analytics": {
"trackCommands": true,
"trackUsers": true,
"trackErrors": true,
"exportFormat": "csv"
}
}

text

## 🤝 Contributing

We welcome contributions! Here's how you can help:

### 🔧 Development Setup

Fork and clone the repository
git clone https://github.com/your-username/N8N-CHAT-TRIGGER.git

Create a feature branch
git checkout -b feature/amazing-feature

Make your changes and test
npm test

Commit your changes
git commit -m "Add amazing feature"

Push and create pull request
git push origin feature/amazing-feature

text

### 📝 Contribution Guidelines

- **Code Style**: Follow ESLint configuration
- **Testing**: Add tests for new features
- **Documentation**: Update README for new features
- **Commit Messages**: Use conventional commit format

### 🐛 Bug Reports

Please use the issue template and include:
- n8n version
- Node.js version
- Platform (Telegram, Discord, etc.)
- Error logs
- Steps to reproduce

## 📄 License

This project is licensed under the **MIT License** - see the [LICENSE](LICENSE) file for details.

---

<div align="center">

**Made with ❤️ by [Jernish](https://github.com/Jernish-FDO)**

⭐ **Star this repo if you found it helpful!** ⭐

[Report Bug](https://github.com/Jernish-FDO/N8N-CHAT-TRIGGER/issues) • [Request Feature](https://github.com/Jernish-FDO/N8N-CHAT-TRIGGER/issues) • [Discussions](https://github.com/Jernish-FDO/N8N-CHAT-TRIGGER/discussions)

</div>
