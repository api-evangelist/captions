---
name: Synthesize speech audio from text
description: Convert text into spoken audio for a chosen voice using the Mirage Video API text-to-speech endpoint.
api: openapi/captions-mirage-openapi-original.json
base_url: https://api.mirage.app
auth: x-api-key header
operations:
  - generate_text_to_speech_v1_audio_text_to_speech__voice_id__post
---

# Text to speech

Generate spoken audio from text for a specific voice.

## Prerequisites
- API key sent as `x-api-key` on every request. Base URL `https://api.mirage.app`.

## Steps

1. **Synthesize speech** — `POST /v1/audio/text-to-speech/{voice_id}`
   (`generate_text_to_speech_v1_audio_text_to_speech__voice_id__post`). Path
   parameter `voice_id` selects the voice. JSON body (`TTSRequest`):
   - `text` (required): the text to convert to speech.
   - `model` (required): the TTS model to use.
   A successful response returns audio (`audio/wav`) or a JSON job depending on
   the request.

## Notes
- Validation failures return HTTP `422` (`HTTPValidationError`).
- Send the `x-api-key` header on the request; keys come from the Mirage platform dashboard.
