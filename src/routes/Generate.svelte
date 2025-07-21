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
        margin: 2,
        color: {
          dark: '#1e293b',
          light: '#ffffff'
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
</script>

<div class="container">
  <main class="main">
    {#if isLoading}
      <div class="loading">
        <div class="spinner"></div>
        <p>Generating QR code...</p>
      </div>
    {:else if error}
      <div class="error">
        <p>{error}</p>
        <button onclick={handleBackToHome} class="retry-button">
          Try Again
        </button>
      </div>
    {:else if qrCodeDataUrl}
      <div class="qr-container">
        <div class="qr-wrapper">
          <img src={qrCodeDataUrl} alt="Generated QR Code" class="qr-image" />
        </div>
      </div>


      <div class="text-display">
        <strong>Text:</strong> <span style="user-select: all;">{params.text}</span>
      </div>
      <div class="actions">
         <button onclick={handleBackToHome} class="back-button">
      ← Back to Home
    </button>
        <button onclick={handleDownload} class="download-button">
          Download QR Code
        </button>
      </div>
    {:else}
      <div class="error">
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

  .text-display {
    background: white;
    padding: 1rem;
    border-radius: 8px;
    margin: 1.5rem auto;
    word-break: break-all;
    color: #475569;
    border: 1px solid #e2e8f0;
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

    .actions {
      flex-direction: column;
    }

    .download-button {
      width: 100%;
    }
  }
</style>