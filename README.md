# Pup Setup Instructions for AI Coding Agents

This repository contains instructions for AI coding agents to set up the [Pup CLI](https://github.com/datadog-labs/pup) and install all available skills.

## Purpose

When followed, these instructions will result in:

1. **Pup CLI installed** - A comprehensive CLI for Datadog's observability platform with 325+ commands across 57 product domains
2. **Authentication configured** - OAuth2, API key, or bearer token authentication with Datadog
3. **Skills installed** - All 7 skills and 48 domain agents installed to your AI assistant's configuration

## What's Included

- **`SETUP_PUP.md`** - Comprehensive manual instructions for setting up pup, designed for AI coding agents to follow using tool calls

## Quick Start

AI coding agents should read [`SETUP_PUP.md`](./SETUP_PUP.md) and follow the step-by-step instructions using their available tool calls. The guide includes:

- Detection of AI assistant software via environment variables
- Installation options (Homebrew, binary, source, Claude Code plugin)
- Authentication setup with user prompting
- Skills installation with location choice (global, project-specific, custom) tailored to the detected agent
- Verification steps and troubleshooting

## For AI Coding Agents

If you're an AI coding agent reading this, please:

1. Read `SETUP_PUP.md` for detailed instructions
2. Ask the user about their preferred installation location for skills
3. Detect the user's AI assistant using environment variables, directory presence, or direct prompting. Explicitly support Claude Code, Gemini Code Assist, OpenAI Codex, GitHub Copilot, and opencode.
4. Follow the step-by-step process outlined in the instructions

## Features

- **Interactive prompts** for authentication method and skills location
- **Platform detection** (macOS, Linux, Windows/WSL)
- **Multiple installation methods** to suit different environments
- **Verification steps** to ensure successful setup
- **Agent mode configuration** for optimized AI responses

## License

These instructions are provided as-is for setting up Pup CLI. Pup itself is licensed under Apache 2.0.