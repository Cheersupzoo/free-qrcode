# QR Code Generator

A modern, fast QR code generator built with Svelte 5. Generate QR codes from any text, URL, or message instantly with a clean, responsive interface.

## Features

- 🚀 **Instant Generation**: Create QR codes in real-time
- 📱 **Mobile Responsive**: Works perfectly on all devices
- 🔗 **Direct URL Support**: Generate QR codes via URL parameters
- 💾 **Download Support**: Save QR codes as PNG images
- ⚡ **Fast & Lightweight**: Built with Svelte 5 and Vite
- 🎨 **Clean Design**: Modern, intuitive user interface

## Live Demo

Visit the live application: [QR Code Generator](https://qrcode.supacheer.com)

## Technology Stack

- **Frontend**: [Svelte 5](https://svelte.dev/) with TypeScript
- **QR Generation**: [qrcode](https://www.npmjs.com/package/qrcode) library
- **Build Tool**: [Vite](https://vitejs.dev/)
- **Styling**: Custom CSS with responsive design

## Getting Started

### Prerequisites

- Node.js (version 16 or higher)
- npm or yarn package manager

### Installation

1. Clone the repository:
```bash
git clone <repository-url>
cd qr-code-generator
```

2. Install dependencies:
```bash
npm install
```

3. Start the development server:
```bash
npm run dev
```

4. Open your browser and navigate to `http://localhost:5173`

### Available Scripts

- `npm run dev` - Start development server
- `npm run build` - Build for production
- `npm run preview` - Preview production build locally
- `npm run check` - Run Svelte type checking

## Usage

### Basic Usage

1. Enter any text, URL, or message in the input field
2. Click "Generate QR Code" or press Enter
3. View and download your generated QR code

### Direct URL Generation

You can generate QR codes directly using URL parameters:

```
/#generate?text=Your%20text%20here
```

Replace "Your%20text%20here" with your URL-encoded text.

### Examples

- Generate QR for a website: `/#generate?text=https%3A//example.com`
- Generate QR for text: `/#generate?text=Hello%20World`
- Generate QR for contact info: `/#generate?text=Contact%3A%20john%40example.com`

## Project Structure

```
src/
├── routes/
│   ├── Home.svelte      # Main landing page
│   └── Generate.svelte  # QR code generation page
├── App.svelte           # Main app component with routing
├── main.ts             # Application entry point
└── app.css             # Global styles
```

## Building for Production

1. Build the application:
```bash
npm run build
```

2. The built files will be in the `dist/` directory, ready for deployment.

## Deployment

This project is configured for easy deployment to:

- **Netlify**: Drag and drop the `dist/` folder or connect your Git repository
- **Vercel**: Import your Git repository
- **GitHub Pages**: Use GitHub Actions to build and deploy
- **Any static hosting**: Upload the contents of `dist/` folder

## Contributing

1. Fork the repository
2. Create a feature branch: `git checkout -b feature-name`
3. Make your changes and commit: `git commit -m 'Add feature'`
4. Push to the branch: `git push origin feature-name`
5. Submit a pull request

## License

This project is open source and available under the [MIT License](LICENSE).

## Acknowledgments

- Built with [Svelte 5](https://svelte.dev/) - The reactive framework
- QR code generation powered by [qrcode](https://www.npmjs.com/package/qrcode) library
- Developed with [Vite](https://vitejs.dev/) for fast development experience
- Vibe Coding with [Bolt](https://bolt.new/)