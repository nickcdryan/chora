# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Fugue Editor (also called "Chora Text Editor") is a specialized web-based text editor for creating multi-voice narratives - stories told through multiple simultaneous "voices" or perspectives, inspired by musical composition structures. The entire application is contained in a single, self-contained HTML file with embedded CSS and JavaScript.

## Development Commands

```bash
# Start development server
python -m http.server 8000
# Then open http://localhost:8000/ (serves index.html by default)

# Access deployed version
# Editor: https://nickcdryan.github.io/chora/
# About: https://nickcdryan.github.io/chora/about
```

## Architecture

### Core Structure
- **Single-file application**: The entire app (~2,463 lines, ~100KB) is in `index.html`
- **No build system**: Pure HTML5, CSS3, and vanilla JavaScript - no frameworks or dependencies
- **Static deployment**: Can be hosted on any static web server

### Key Classes
- **`MultiStreamDocument`**: Manages text data and state across multiple parallel streams
- **`MultiStreamView`**: Handles rendering and user interactions
- **Clean separation**: Document model stores continuous text strings; view model creates visual chunks

### Multi-Stream Architecture
- **Stream Groups**: Documents contain multiple stream groups (like chapters)
- **Independent Streams**: Each group has N parallel text streams that reflow independently
- **Real-time Reflow**: Automatic text wrapping and chunk creation as content changes
- **Cursor Preservation**: Maintains cursor position across automatic reflows

### Page System
- **`PAGE_CONFIG`**: Controls layout dimensions and page structure
- **Automatic Pagination**: Content flows across multiple pages automatically
- **Responsive Design**: Adapts to different screen sizes

### Persistence
- **Auto-save**: localStorage backup every 2 seconds
- **History System**: 50-step undo/redo buffer
- **File Format**: Custom YAML-based format with human-readable text chunks
- **Export Options**: HTML, PDF, and LaTeX export capabilities

## Key Technical Concepts

### Text Editing
- Uses browser's native `contentEditable` engine
- Real-time font metrics for accurate text measurement
- Smart word wrapping with proper space handling
- Keyboard navigation: Tab between streams, navigation between chunks

### Text Reflow Architecture
- **Document Model**: `MultiStreamDocument` (line ~450) stores continuous text strings per stream
- **View Model**: `MultiStreamView` (line ~873) handles visual chunking and reflow
- **Critical Process**: Text changes trigger reflow calculations that must preserve cursor position relative to logical text offset, not visual position
- **Common Bug Area**: Cursor position preservation during automatic reflows when text wraps between lines

### Core Innovation
Each voice behaves like a separate document but maintains visual synchronization through shared chunks. When you edit Voice 1, all Voice 1 chunks reflow while other voices remain unaffected. This creates parallel columns while managing independent text streams.

## File Structure
- `index.html` - Complete application (main file to edit)
- `about.html` - User-facing documentation
- `summary.txt` - Detailed technical specification
- `README.md` - Basic project info with live links
- `instructions.txt` - Server setup instructions

## Important Notes
- Always test changes by running the development server
- The app is designed to work without external dependencies
- All functionality is contained within the single HTML file
- Export features use browser APIs for PDF generation
- localStorage integration provides automatic data persistence

## Debugging Text Editing Issues
- **Cursor Position Bugs**: Focus on the relationship between logical text offset in `MultiStreamDocument` vs visual DOM position in `MultiStreamView`
- **Reflow Issues**: Check text measurement calculations and how they handle word boundaries and spaces
- **Testing Strategy**: Use specific cursor positioning scenarios (e.g., cursor before word at line boundary, type character, add space) to reproduce bugs
- **Key Functions**: Look for cursor preservation logic in reflow/chunk recreation methods