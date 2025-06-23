# Grammar-AI 🚀

[![npm version](https://badge.fury.io/js/grammar-ai.svg)](https://badge.fury.io/js/grammar-ai)
[![Downloads](https://img.shields.io/npm/dm/grammar-ai.svg)](https://npmjs.org/package/grammar-ai)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

> The most advanced AI-powered grammar checker built for developers. Supports 50+ languages with Groq, OpenAI, Google Gemini & DeepSeek integration.

## ✨ Why Choose Grammar-AI?

- 🧠 **AI-Powered**: Multiple AI providers (Groq, OpenAI, Google, DeepSeek)
- ⚡ **Blazing Fast**: < 100ms response time with smart caching
- 🌍 **50+ Languages**: Best-in-class multilingual support
- 🔒 **Privacy First**: Works offline, no data tracking
- 💰 **Cost Optimized**: Auto-switches to cheapest accurate provider
- 🛠️ **Developer Focused**: Code-aware, TypeScript-first, CLI included

## 🚀 Quick Start

```bash
npm install grammar-ai
```

```javascript
import { check } from 'grammar-ai';

// Zero-config setup - just works!
const result = await check('This are a test sentence.');

console.log(result.corrections);
// [{ original: 'This are', corrected: 'This is', confidence: 0.95 }]
```

## 🎯 Core Features

### 1. Zero-Config Magic ✨
```javascript
import { check } from 'grammar-ai';

// Works out of the box
const result = await check('Your text here');
```

### 2. Multiple Input Formats 📝
```javascript
// Text string
await check.text('Hello world');

// File paths
await check.file('./document.md');

// URLs
await check.url('https://blog.com/post');

// Code comments
await check.code('// Check this comment');
```

### 3. Real-time Streaming 🌊
```javascript
const stream = check.stream('Very long document text...');

for await (const chunk of stream) {
  console.log(chunk.corrections);
  // Get corrections as they're found
}
```

### 4. Smart Auto-Fix 🧠
```javascript
const result = await check(text, { 
  autoFix: 'confident', // Only fix 95%+ confidence
  preserveStyle: true   // Keep your writing voice
});

console.log(result.fixedText);
```

### 5. Domain-Specific Modes 🎭
```javascript
// Academic writing
await check(text, { mode: 'academic' });

// Marketing copy
await check(text, { mode: 'marketing' });

// Technical documentation
await check(text, { mode: 'technical' });

// Casual blog posts
await check(text, { mode: 'casual' });
```

## ⚙️ Advanced Configuration

### AI Provider Setup
```javascript
import { GrammarAI } from 'grammar-ai';

const checker = new GrammarAI({
  // Multiple AI providers
  providers: {
    groq: { apiKey: 'your-groq-key' },
    openai: { apiKey: 'your-openai-key' },
    google: { apiKey: 'your-google-key' },
    deepseek: { apiKey: 'your-deepseek-key' }
  },
  
  // Cost optimization
  budget: { maxCostPerMonth: 50 },
  provider: 'auto', // Chooses cheapest accurate option
  
  // Performance
  caching: 'aggressive',
  offline: true // Works without internet
});
```

### Express.js Integration 🚀
```javascript
import express from 'express';
import { check } from 'grammar-ai';

const app = express();
app.use(express.json());

// Grammar check endpoint
app.post('/api/grammar-check', async (req, res) => {
  try {
    const { text, options } = req.body;
    const result = await check(text, options);
    
    res.json({
      success: true,
      corrections: result.corrections,
      metrics: result.metrics,
      confidence: result.confidence
    });
  } catch (error) {
    res.status(500).json({ 
      success: false, 
      error: error.message 
    });
  }
});

// Streaming endpoint
app.post('/api/grammar-stream', async (req, res) => {
  res.setHeader('Content-Type', 'text/event-stream');
  
  const stream = check.stream(req.body.text);
  
  for await (const chunk of stream) {
    res.write(`data: ${JSON.stringify(chunk)}\n\n`);
  }
  
  res.end();
});

app.listen(3000);
```

## 🖥️ CLI Tool

```bash
# Install globally
npm install -g grammar-ai

# Check single file
grammar-ai check document.md

# Check multiple files with auto-fix
grammar-ai check *.md --fix --stats

# Watch mode for development
grammar-ai watch src/ --auto-fix

# CI/CD integration
grammar-ai check *.md --ci --fail-on-errors
```

## 🌍 Multilingual Support

```javascript
// Auto-detects language
await check('This is English text.');
await check('Ceci est un texte français.');
await check('Dies ist ein deutscher Text.');
await check('هذا نص عربي.');

// Mixed languages in same text
const mixed = "Hello! Comment ça va? 元気ですか？";
const result = await check(mixed);
// Applies appropriate rules for each language
```

## 🔥 Advanced Features

### Code-Aware Checking 💻
```javascript
const code = `
/**
 * This function calcuate the total
 * @param items - list of item to sum
 */
function sum(items) { ... }
`;

const result = await check.code(code);
// Fixes: "calcuate" → "calculate", "item" → "items"
```

### Tone & Sentiment Analysis 📊
```javascript
const result = await check(text, { 
  analyzeTone: true,
  targetTone: 'professional'
});

console.log(result.tone);
// { formality: 0.8, emotion: 'neutral', bias: 'none' }
```

### Learning & Personalization 🎯
```javascript
const checker = new GrammarAI({
  learn: true,           // Adapts to your style
  userProfile: 'dev',    // Understands technical writing
  customRules: ['no-passive-voice-in-docs']
});
```

### Export Multiple Formats 📄
```javascript
const result = await check(text);

result.export('json');     // For APIs
result.export('sarif');    // For security tools
result.export('junit');    // For CI systems
result.export('html');     // For reports
```

## 📖 TypeScript Support

```typescript
import { GrammarResult, CheckOptions } from 'grammar-ai';

interface GrammarResult {
  corrections: Correction[];
  suggestions: Suggestion[];
  confidence: number;
  metrics: ReadabilityMetrics;
  tone?: ToneAnalysis;
}

const result: GrammarResult = await check(text, {
  mode: 'technical',
  autoFix: 'confident',
  analyzeTone: true
} as CheckOptions);
```

## 🔌 Plugin Ecosystem

```javascript
// Extend functionality
import { spellCheck } from '@grammar-ai/spell';
import { plagiarism } from '@grammar-ai/plagiarism';
import { accessibility } from '@grammar-ai/a11y';

checker.use(spellCheck, plagiarism, accessibility);
```

## 💰 Pricing

### Free Tier
- ✅ 1,000 checks/month
- ✅ Basic grammar checking
- ✅ 5 languages
- ✅ Community support

### Pro Tier ($19/month)
- ✅ Unlimited checks
- ✅ All AI features
- ✅ 50+ languages
- ✅ Priority support
- ✅ Advanced analytics

### Enterprise ($99/month)
- ✅ On-premise deployment
- ✅ Custom model training
- ✅ SSO integration
- ✅ SLA guarantees

## 🤝 Contributing

We welcome contributions! Please see our [Contributing Guide](CONTRIBUTING.md).

## 📄 License

MIT License - see [LICENSE](LICENSE) file for details.

## 🆘 Support

- 📧 Email: support@grammar-ai.com
<!-- - 💬 Discord: [Join our community](https://discord.gg/grammar-ai) -->
- 📚 Docs: [Full documentation](abdelkabir.ouadoukou@gmail.com)
- 🐛 Issues: [GitHub Issues](https://github.com/khabzox/grammar-ai/issues)

---

<div align="center">
  <p>Made with ❤️ for developers who care about great writing</p>
  <!-- <p>
    <a href="https://grammar-ai.com">Website</a> •
    <a href="https://docs.grammar-ai.com">Documentation</a> •
    <a href="https://github.com/khabzox/grammar-ai">GitHub</a> •
    <a href="https://twitter.com/grammar_ai">Twitter</a>
  </p> -->
</div>