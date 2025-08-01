<script lang="ts">
  import { onMount } from 'svelte';
  import QRCode from 'qrcode';

  interface Props {
    params: Record<string, string>;
    navigate: (route: string, params?: Record<string, string>) => void;
  }

  let { params, navigate }: Props = $props();
  let qrCodeDataUrl = $state('');
  let isLoading = $state(false);
  let error = $state('');
  let foregroundColor = $state('#000000');
  let backgroundColor = $state('#ffffff');
  let margin = $state(2);

  async function generateQRCode(text: string) {
    if (!text) {
      error = 'No text provided';
      return;
    }

    isLoading = true;
    error = '';

    try {
      const dataUrl = await QRCode.toDataURL(text, {
        width: 500,
        margin: margin,
        color: {
          dark: foregroundColor,
          light: backgroundColor
        }
      });
      qrCodeDataUrl = dataUrl;
    } catch (err) {
      error = 'Failed to generate QR code';
      console.error(err);
    } finally {
      isLoading = false;
    }
  }

  function handleBackToHome() {
    navigate('home');
  }

  function handleDownload() {
    if (!qrCodeDataUrl) return;

    const link = document.createElement('a');
    link.download = 'qrcode.png';
    link.href = qrCodeDataUrl;
    link.click();
  }

  $effect(() => {
    const text = params.text;
    if (text) {
      generateQRCode(text);
    }
  });

  $effect(() => {
    const text = params.text;
    if (text && qrCodeDataUrl) {
      generateQRCode(text);
    }
  });
</script>

<div class="container">
  <main class="main" role="main">
    {#if isLoading}
      <div class="loading" aria-live="polite">
        <div class="spinner"></div>
        <p>Generating QR code...</p>
      </div>
    {:else if error}
      <div class="error" role="alert" aria-live="assertive">
        <p>{error}</p>
        <button onclick={handleBackToHome} class="retry-button">
          Try Again
        </button>
      </div>
    {:else if qrCodeDataUrl}
      <section class="qr-container" aria-labelledby="qr-result-heading">
        <h1 id="qr-result-heading" class="sr-only">Generated QR Code Result</h1>
        <div class="qr-wrapper">
          <img src={qrCodeDataUrl} alt="QR Code for: {params.text}" class="qr-image" />
        </div>
      </section>


      <div class="text-display">
        <strong>Text:</strong> <span style="user-select: all;">{params.text}</span>
      </div>
      
      <div class="customization-panel">
        <h3>Customize QR Code</h3>
        <div class="controls">
          <div class="control-group">
            <label for="foreground-color">Foreground Color:</label>
            <input 
              type="color" 
              id="foreground-color"
              bind:value={foregroundColor}
            />
          </div>
          <div class="control-group">
            <label for="background-color">Background Color:</label>
            <input 
              type="color" 
              id="background-color"
              bind:value={backgroundColor}
            />
          </div>
          <div class="control-group">
            <label for="margin">Margin:</label>
            <input 
              type="range" 
              id="margin"
              min="0" 
              max="10" 
              bind:value={margin}
            />
            <span class="margin-value">{margin}</span>
          </div>
        </div>
      </div>
      
      <div class="actions" role="group" aria-label="QR Code actions">
         <button onclick={handleBackToHome} class="back-button">
      ← Back to Home
    </button>
        <button onclick={handleDownload} class="download-button" aria-label="Download QR code as PNG image">
          Download QR Code
        </button>
      </div>
    {:else}
      <div class="error" role="alert">
        <p>No text provided for QR code generation</p>
        <button onclick={handleBackToHome} class="retry-button">
          Go to Home
        </button>
      </div>
    {/if}
  </main>
</div>

<style>
  .container {
    max-width: 800px;
    margin: 0 auto;
    padding: 0;
    min-height: 100vh;
    display: flex;
    flex-direction: column;
    background: white;
  }

  .back-button {
    background: white;
    border: 1px solid #e2e8f0;
    padding: 0.5rem 1rem;
    border-radius: 6px;
    cursor: pointer;
    font-size: 0.875rem;
    color: #475569;
    transition: all 0.2s ease;
  }

  .back-button:hover {
    background: #f8fafc;
    transform: translateY(-1px);
  }

  h1 {
    font-size: 2rem;
    font-weight: 600;
    color: #1e293b;
    margin: 0;
    text-align: center;
  }

  .main {
    flex: 1;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
  }

  .loading {
    text-align: center;
    color: #64748b;
  }

  .spinner {
    width: 40px;
    height: 40px;
    border: 4px solid #e2e8f0;
    border-top: 4px solid #3b82f6;
    border-radius: 50%;
    animation: spin 1s linear infinite;
    margin: 0 auto 1rem;
  }

  @keyframes spin {
    0% { transform: rotate(0deg); }
    100% { transform: rotate(360deg); }
  }

  .error {
    text-align: center;
    color: #dc2626;
  }

  .retry-button {
    background: #3b82f6;
    color: white;
    border: none;
    padding: 0.75rem 1.5rem;
    border-radius: 8px;
    cursor: pointer;
    font-size: 1rem;
    font-weight: 500;
    transition: all 0.2s ease;
    margin-top: 1rem;
  }

  .retry-button:hover {
    background: #2563eb;
    transform: translateY(-1px);
  }

  .qr-container {
    text-align: center;
    width: 100%;
    max-width: 500px;
  }

  .qr-wrapper {
    background: white;
    padding: 0;
    margin-bottom: 1.5rem;
  }

  .qr-image {
    width: 100%;
    max-width: 500px;
    height: auto;
    display: block;
  }

  .sr-only {
    position: absolute;
    width: 1px;
    height: 1px;
    padding: 0;
    margin: -1px;
    overflow: hidden;
    clip: rect(0, 0, 0, 0);
    white-space: nowrap;
    border: 0;
  }

  .text-display {
    background: white;
    padding: 1rem;
    border-radius: 8px;
    margin: 1.5rem auto;
    word-break: break-all;
    color: #475569;
    border: 1px solid #e2e8f0;
  }

  .customization-panel {
    background: white;
    padding: 1.5rem;
    border-radius: 8px;
    margin: 1.5rem auto;
    border: 1px solid #e2e8f0;
    max-width: 500px;
    width: 100%;
  }

  .customization-panel h3 {
    margin: 0 0 1rem 0;
    font-size: 1.125rem;
    font-weight: 600;
    color: #1e293b;
  }

  .controls {
    display: flex;
    flex-direction: column;
    gap: 1rem;
  }

  .control-group {
    display: flex;
    align-items: center;
    gap: 1rem;
  }

  .control-group label {
    font-weight: 500;
    color: #374151;
    min-width: 120px;
  }

  .control-group input[type="color"] {
    width: 50px;
    height: 35px;
    border: 1px solid #e2e8f0;
    border-radius: 6px;
    cursor: pointer;
  }

  .control-group input[type="range"] {
    flex: 1;
    max-width: 150px;
  }

  .margin-value {
    font-weight: 500;
    color: #374151;
    min-width: 20px;
  }

  .actions {
    display: flex;
    justify-content: center;
    gap: 1rem;
  }

  .download-button {
    background: #10b981;
    color: white;
    border: none;
    padding: 0.5rem 1rem;
    border-radius: 8px;
    cursor: pointer;
    font-size: 1rem;
    font-weight: 500;
    transition: all 0.2s ease;
  }

  .download-button:hover {
    background: #059669;
    transform: translateY(-1px);
    box-shadow: 0 4px 12px rgba(16, 185, 129, 0.3);
  }

  @media (max-width: 640px) {
    .container {
      padding: 0rem;
    }

    h1 {
      font-size: 1.5rem;
    }

    .qr-wrapper {
      padding: 1rem;
    }

    .text-display {
      font-size: 0.875rem;
    }

    .customization-panel {
      padding: 1rem;
    }

    .control-group {
      flex-direction: column;
      align-items: flex-start;
      gap: 0.5rem;
    }

    .control-group label {
      min-width: auto;
    }

    .control-group input[type="range"] {
      max-width: 100%;
    }

    .actions {
      flex-direction: column;
    }

    .download-button {
      width: 100%;
    }
  }
</style>