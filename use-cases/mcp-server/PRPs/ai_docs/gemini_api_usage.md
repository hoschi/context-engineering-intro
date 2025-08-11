### Example usage of the Google Generative AI API for Gemini (model and API key are both environment variables)

const response = await fetch(`https://generativelanguage.googleapis.com/v1beta/models/${this.model}:generateContent?key=${this.apiKey}`, {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json',
  },
  body: JSON.stringify({
    "contents": [{
      "parts": [{
        "text": this.buildPRPParsingPrompt(prpContent, projectContext, config)
      }]
    }]
  })
});

if (!response.ok) {
  throw new Error(`Google Generative AI API error: ${response.status} ${response.statusText}`);
}

const result = await response.json();
const content = (result as any).candidates[0].content.parts[0].text;

// Parse the JSON response
const aiTasks = JSON.parse(content);