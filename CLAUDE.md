# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is **Chora Text Editor** (also called Multi-Stream Text Editor) - a sophisticated web-based text editor that supports multiple concurrent text streams/voices within a single document. The application is built as a single-page HTML application with embedded CSS and JavaScript.

## Development Setup

To run the application locally:
```bash
python -m http.server 8000
# Open http://localhost:8000/index.html
```

The application runs entirely in the browser with no build process required.

## Architecture

### Core Classes

The application is built around two main JavaScript classes in `index.html`:

1. **MultiStreamDocument** (`index.html:454`) - The data model that manages:
   - Multiple text streams (voices) grouped into stream groups
   - Document state including undo/redo history
   - Text operations (insert, delete, break paragraphs)
   - Virtual chunking for efficient text processing
   - Auto-save functionality to localStorage

2. **MultiStreamView** (`index.html:1072`) - The view layer that handles:
   - Dynamic DOM creation for pages and stream columns
   - Text input handling and cursor management
   - Real-time text measurement and line wrapping
   - Visual stream indicators and formatting
   - Keyboard shortcuts and user interactions

### Key Features

- **Multi-Stream Editing**: Each document supports multiple parallel text streams (configurable count)
- **Virtual Chunking**: Text is broken into virtual chunks for efficient processing and memory management
- **Real-time Line Breaking**: Text wraps dynamically based on column width measurements
- **Undo/Redo System**: Complete history tracking with group-based operations
- **Auto-save**: Document state persists to localStorage
- **Export Options**: HTML and PDF export functionality
- **Page Management**: Dynamic page creation as content grows

### File Structure

- `index.html` - Main application (contains all HTML, CSS, and JavaScript)
- `about.html` - About page with project information
- `README.md` - Basic project links and description
- `instructions.txt` - Simple development server instructions
- `summary.txt` - Project summary file

### Page Configuration

The application uses a `PAGE_CONFIG` object (`index.html:423`) that defines:
- Page dimensions and margins
- Column widths and spacing
- Typography settings
- Line height and character measurements

### User Interface Elements

- **Toolbar**: Contains action buttons with tooltips for undo/redo, voice management, file operations, and export
- **Stream Indicators**: Visual indicators showing active stream count and current stream
- **Dynamic Pages**: Pages are created automatically as content grows
- **Footer Counters**: Real-time statistics display

## Key Functions

- `applyPageConfiguration()` - Sets up CSS custom properties from PAGE_CONFIG
- `changeStreamCount()` - Allows users to modify the number of concurrent streams
- `insertParagraphBreak()` - Creates paragraph breaks across all streams
- `saveFile()/loadFile()` - JSON-based document persistence
- `exportHtml()/exportPdf()` - Document export functionality
- `undoAction()/redoAction()` - History management

## Development Notes

- The entire application is contained in a single HTML file with no external dependencies
- All styling is embedded CSS with custom properties for theming
- JavaScript uses ES6 classes and modern browser APIs
- Text measurement relies on a hidden DOM element (`textMeasure`) for accurate character width calculations
- The application is designed to work offline once loaded