# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

OnlineWardleyMaps is a React/Next.js web application for creating Wardley Maps - strategic visualization tools. Maps are created using a text-based DSL (Domain Specific Language) that is parsed and rendered as interactive SVG visualizations.

## Commands

All commands run from the `frontend/` directory:

```bash
yarn install          # Install dependencies
yarn dev              # Start dev server at http://localhost:3000
yarn build            # Production build
yarn test             # Run all tests
yarn test <path>      # Run specific test (e.g., yarn test src/__tests__/GivenConverter.js)
yarn lint             # Run ESLint with Prettier formatting
yarn lint:check       # Check linting without fixing
```

## Architecture

### Data Flow Pipeline

1. **Text Input** - User enters mapText in the DSL format via ACE Editor
2. **Parsing** - `Converter.ts` orchestrates multiple extraction strategies to parse the DSL
3. **WardleyMap Object** - Structured data object containing all map elements
4. **Rendering** - React components render the map as interactive SVG

### Key Directories

- `frontend/src/conversion/` - DSL parser with strategy pattern. Each `*ExtractionStrategy.ts` handles a specific element type (components, links, pipelines, etc.)
- `frontend/src/components/map/` - React components for rendering map elements
- `frontend/src/components/symbols/` - SVG symbol components (circles, squares, markers)
- `frontend/src/linkStrategies/` - Strategy pattern for computing component connections
- `frontend/src/types/` - TypeScript type definitions

### Core Files

- `Converter.ts` - Main parser entry point, runs all extraction strategies
- `MapEnvironment.tsx` - Top-level component, invokes Converter and manages state
- `MapView.tsx` - Renders title and passes data to canvas
- `MapCanvas.js` - Main SVG container using MapElements class

### Coordinate System

- SVG coordinates: (0,0) top-left to (1,1) bottom-right
- X-axis = Evolution (Genesis → Custom → Product → Commodity)
- Y-axis = Visibility/Value Chain (invisible to visible)
- `PositionCalculator` converts between logical and screen coordinates

## DSL Syntax Examples

```
title My Wardley Map
component Customer [0.9, 0.2]        # [visibility, maturity]
component Service [0.7, 0.55]
Customer->Service                     # Link components
evolve Service 0.9                    # Show evolution movement
pipeline Kettle {
  component Electric Kettle [0.63]
}
```

## Type Guidelines

- Types stored in `frontend/src/types/`
- Prefer extending existing types over creating new ones
- Set default values upstream in `Converter.ts` rather than making types optional
- Use the original WardleyMap types from `types/base.ts` for consistency
