<script lang="ts">
  interface Props {
    navigate: (route: string, params?: Record<string, string>) => void;
  }

  let { navigate }: Props = $props();
  let text = $state('');

  function handleSubmit() {
    if (text.trim()) {
      navigate('generate', { text: text.trim() });
    }
  }

  function handleKeydown(event: KeyboardEvent) {
    if (event.key === 'Enter') {
      handleSubmit();
    }
  }
</script>

<div class="container">
  <header class="header">
    <h1>QR Code Generator</h1>
    <p class="subtitle">Create QR codes from any text instantly</p>
  </header>

  <section class="content">
    <div class="generator-card">
      <h2>Generate QR Code</h2>
      <div class="input-group">
        <input
          type="text"
          bind:value={text}
          placeholder="Enter text, URL, or message..."
          onkeydown={handleKeydown}
          class="text-input"
        />
        <button
          onclick={handleSubmit}
          disabled={!text.trim()}
          class="generate-button"
        >
          Generate QR Code
        </button>
      </div>
    </div>

    <div class="info-card">
      <h2>How it works</h2>
      <ol>
        <li>Enter any text, URL, or message in the input field above</li>
        <li>Click "Generate QR Code" or press Enter</li>
        <li>Your QR code will be displayed on a new page</li>
        <li>Share the link or save the QR code image</li>
      </ol>
    </div>

    <div class="url-info-card">
      <h2>Direct URL Generation</h2>
      <p>You can also generate QR codes directly using URLs:</p>
      <div class="url-example">
        <code>/#generate?text=Your%20text%20here</code>
      </div>
      <p class="url-note">Replace "Your%20text%20here" with your URL-encoded text</p>
    </div>

    <div class="tech-card">
      <h2>Built with Modern Technology</h2>
      <div class="tech-inline">
        <a href="https://svelte.dev/" target="_blank" rel="noopener noreferrer" class="tech-link">
          <span class="tech-icon">⚡</span>
          Svelte 5
        </a>
        <span class="tech-separator">•</span>
        <a href="https://www.npmjs.com/package/qrcode" target="_blank" rel="noopener noreferrer" class="tech-link">
          <span class="tech-icon">📱</span>
          QRCode
        </a>
        <span class="tech-separator">•</span>
        <a href="https://vitejs.dev/" target="_blank" rel="noopener noreferrer" class="tech-link">
          <span class="tech-icon">🚀</span>
          Vite
        </a>
      </div>
      <p class="vibe-text">Vibe Coding with <a href="https://bolt.new/" target="_blank" rel="noopener noreferrer" class="bolt-link">Bolt</a></p>
    </div>
  </section>

  <footer class="footer">
    <p>Simply enter your text and generate a QR code instantly. No registration required.</p>
    <p class="footer-tech">
      Open source • Built with 
      <a href="https://svelte.dev/" target="_blank" rel="noopener noreferrer">Svelte</a>
    </p>
  </footer>
</div>

<style>
  .container {
    max-width: 800px;
    margin: 0 auto;
    padding: 2rem;
    display: flex;
    flex-direction: column;
    min-height: 100vh;
    background: white;
  }

  .header {
    text-align: center;
    margin-bottom: 3rem;
  }

  h1 {
    font-size: 3rem;
    font-weight: 700;
    color: #1e293b;
    margin: 0 0 1rem 0;
    line-height: 1.2;
  }

  .subtitle {
    font-size: 1.25rem;
    color: #64748b;
    margin: 0;
    font-weight: 400;
  }

  .content {
    flex: 1;
    display: flex;
    flex-direction: column;
    gap: 2rem;
  }

  .info-card,
  .generator-card,
  .url-info-card,
  .tech-card {
    background: white;
    padding: 2rem;
    border-radius: 12px;
    box-shadow: 0 1px 3px rgba(0, 0, 0, 0.1);
    border: 1px solid #e2e8f0;
  }

  .info-card h2,
  .generator-card h2,
  .url-info-card h2,
  .tech-card h2 {
    font-size: 1.5rem;
    font-weight: 600;
    color: #1e293b;
    margin: 0 0 1.5rem 0;
  }

  .info-card ol {
    padding-left: 1.5rem;
    margin: 0;
  }

  .info-card li {
    margin-bottom: 0.5rem;
    color: #475569;
  }

  .input-group {
    display: flex;
    gap: 1rem;
    flex-wrap: wrap;
  }

  .text-input {
    flex: 1;
    min-width: 300px;
    padding: 0.75rem 1rem;
    border: 2px solid #e2e8f0;
    border-radius: 8px;
    font-size: 1rem;
    transition: border-color 0.2s ease;
  }

  .text-input:focus {
    outline: none;
    border-color: #3b82f6;
    box-shadow: 0 0 0 3px rgba(59, 130, 246, 0.1);
  }

  .generate-button {
    padding: 0.75rem 1.5rem;
    background: #3b82f6;
    color: white;
    border: none;
    border-radius: 8px;
    font-size: 1rem;
    font-weight: 500;
    cursor: pointer;
    transition: all 0.2s ease;
    white-space: nowrap;
  }

  .generate-button:hover:not(:disabled) {
    background: #2563eb;
    transform: translateY(-1px);
    box-shadow: 0 4px 12px rgba(59, 130, 246, 0.3);
  }

  .generate-button:disabled {
    background: #cbd5e1;
    cursor: not-allowed;
    transform: none;
    box-shadow: none;
  }

  .url-example {
    background: #f8fafc;
    padding: 1rem;
    border-radius: 6px;
    border: 1px solid #e2e8f0;
    margin: 1rem 0;
    font-family: 'Monaco', 'Menlo', 'Ubuntu Mono', monospace;
    font-size: 0.875rem;
    word-break: break-all;
  }

  .url-note {
    color: #64748b;
    font-size: 0.875rem;
    margin: 0;
  }

  .tech-inline {
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 1rem;
    flex-wrap: wrap;
    margin-bottom: 1rem;
  }

  .tech-link {
    display: flex;
    align-items: center;
    gap: 0.5rem;
    color: #3b82f6;
    text-decoration: none;
    font-weight: 500;
    padding: 0.5rem 1rem;
    border-radius: 6px;
    transition: all 0.2s ease;
  }

  .tech-link:hover {
    background: #f1f5f9;
    text-decoration: none;
    transform: translateY(-1px);
  }

  .tech-icon {
    font-size: 1.2rem;
  }

  .tech-separator {
    color: #cbd5e1;
    font-weight: bold;
  }

  .vibe-text {
    text-align: center;
    color: #64748b;
    font-size: 0.875rem;
    margin: 0;
    font-style: italic;
  }

  .bolt-link {
    color: #3b82f6;
    text-decoration: none;
    font-weight: 500;
  }

  .bolt-link:hover {
    text-decoration: underline;
  }

  .footer {
    text-align: center;
    margin-top: 3rem;
    padding-top: 2rem;
    border-top: 1px solid #e2e8f0;
  }

  .footer p {
    color: #64748b;
    margin: 0 0 0.5rem 0;
  }

  .footer-tech {
    font-size: 0.875rem;
  }

  .footer-tech a {
    color: #3b82f6;
    text-decoration: none;
  }

  .footer-tech a:hover {
    text-decoration: underline;
  }

  @media (max-width: 640px) {
    .container {
      padding: 1rem;
    }

    h1 {
      font-size: 2rem;
    }

    .subtitle {
      font-size: 1.125rem;
    }

    .info-card,
    .generator-card,
    .url-info-card,
    .tech-card {
      padding: 1.5rem;
    }

    .input-group {
      flex-direction: column;
    }

    .text-input {
      min-width: auto;
    }

    .generate-button {
      width: 100%;
    }

    .url-example {
      font-size: 0.75rem;
    }

    .tech-inline {
      gap: 0.5rem;
    }

    .tech-link {
      padding: 0.25rem 0.5rem;
      font-size: 0.875rem;
    }
  }
</style>