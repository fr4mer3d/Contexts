# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Overview

- **MultiAI-Terminal**: Framework for AI agent interaction with configurable output styles
- **Primary Component**: output-styles directory containing response formatting templates
- **Purpose**: Standardize AI responses across different communication contexts and user preferences

## Output Styles Directory

Located at `/Users/arthurc/Contexts/MultiAI-Terminal/output-styles/`

### Available Styles

- **be-balanced.md**: Presents 3 pros and 3 cons with contrast, brief yet didactic, creates paired comparisons
- **be-concise.md**: Ultra-brief fact-focused responses, strips unnecessary elaboration, maximum information density
- **be-transparent.md**: Makes reasoning process visible, shows logical reductions, documents sources for each step
- **make-a-list.md**: Elements in list format with crucial information on same line, self-contained entries, prioritizes scanability
- **pure-execution.md**: Action-focused with zero preamble, no fluff, states action and executes immediately
- **explain.md**: Feynman technique with analogy, practical example, and alternatives list, minimalistic yet didactic

### Style File Structure

- **Frontmatter**: Contains name, description, and optional configuration flags
- **Instructions**: Defines formatting rules, structure requirements, and behavioral guidelines
- **Format Examples**: Shows expected output structure for the style

## Development Notes

- **Git Repository**: Initialized with main branch
- **Configuration**: No build tools or test frameworks currently configured
- **Style System**: Extensible by adding new .md files to output-styles directory following existing template pattern
- **Usage**: Reference styles in prompts to control response format and communication style
