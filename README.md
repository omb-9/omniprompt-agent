# Omni-Prompt Engineer Bot

## Overview

This n8n workflow powers a Telegram bot designed for prompt engineering. By passing user messages through OpenRouter to the Gemini 3.1 Pro Preview model, it transforms rough ideas into highly optimized, production-ready system prompts. The AI is constrained by a strict system prompt to ensure it only returns its reasoning and the final prompt, completely eliminating conversational filler.

## Features

- **Seamless Telegram Integration:** Listens for incoming messages and replies directly in the chat.
- **Non-Blocking Architecture:** Immediately acknowledges the Telegram webhook to prevent timeout errors.
- **UX Enhancements:** Sends a "typing" action to Telegram while the AI generates the response.
- **Premium AI Processing:** Connects to OpenRouter to utilize the `google/gemini-3.1-pro-preview` model.
- **Strict Output Formatting:** Forces the AI to reply with bulleted reasoning and the finalized prompt inside a Markdown code block.
- **Error Handling:** Automatically catches API failures and notifies the user directly in Telegram.

## Prerequisites

Before importing this workflow, you will need the following:

- An active n8n instance (self-hosted or cloud).
- A Telegram Bot Token (obtained via BotFather).
- An OpenRouter API Key.

## Installation

1. Download the workflow file named **Omni-Prompt Engineer Bot(1).json** to your local machine.
2. Open your n8n workspace.
3. Navigate to **Workflows** and click **Add Workflow**.
4. Click the options menu in the top right corner and select **Import from File**.
5. Upload the **Omni-Prompt Engineer Bot(1).json** file.
6. Open the **Telegram Trigger**, **Show Typing Status**, **Push Result**, and **Notify Error** nodes to connect your Telegram API credentials.
7. Open the **OpenRouter API** node and update the Bearer token in the Headers section with your actual OpenRouter API key.
8. Save and toggle the workflow to **Active**.

## Workflow Architecture

This workflow executes in six distinct steps:

1. **Telegram Trigger:** Receives the text message from the user.
2. **Ack (Release Connection):** Returns a quick JSON status payload to release the webhook connection immediately.
3. **Show Typing Status:** Triggers a chat action in Telegram so the user knows the bot is processing the request.
4. **OpenRouter API:** Sends the user's text alongside a rigid system prompt to the Gemini model.
5. **Push Result:** Parses the successful Markdown response and sends it back as a reply to the original Telegram message.
6. **Notify Error:** If the OpenRouter API fails, this fallback node catches the error and sends a formatted warning to the user.
