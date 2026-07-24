---
name: Generate an AI avatar video and retrieve the result
description: Create an AI avatar/talking-head video from an audio reference and an image, poll the job to completion, and download the rendered video.
api: openapi/captions-mirage-openapi-original.json
base_url: https://api.mirage.app
auth: x-api-key header
operations:
  - create_video_generation_v1_videos_post
  - get_video_status_v1_videos__video_id__get
  - get_video_content_v1_videos__video_id__content_get
---

# Generate an AI avatar video

Use the Mirage Video API to render an AI avatar video from an audio track and an
image appearance reference.

## Prerequisites
- An API key from the Mirage platform dashboard (https://platform.mirage.app/).
- Send it on **every** request as the header `x-api-key: <YOUR_KEY>`.
- Base URL: `https://api.mirage.app`.

## Steps

1. **Create the generation job** — `POST /v1/videos`
   (`create_video_generation_v1_videos_post`). Submit a multipart body with:
   - `audio_reference` (required): a WAV or MP3 audio file.
   - `image_reference` (required): a JPEG or PNG appearance reference.
   - `model` (optional): e.g. `mirage-video-1-latest`.
   The response is a `MAVideo` job object with an `id` and `status: PROCESSING`.

2. **Poll for completion** — `GET /v1/videos/{video_id}`
   (`get_video_status_v1_videos__video_id__get`) using the returned `id`. Repeat
   until `status` is a terminal value. Watch `progress` (0–100). Terminal states:
   - `COMPLETE` → proceed to step 3.
   - `FAILED` → read the `error` object (`code`, `message`); e.g. `rate_limit_exceeded`.
   - `CANCELLED` → the job was cancelled; stop.

3. **Retrieve the video** — `GET /v1/videos/{video_id}/content`
   (`get_video_content_v1_videos__video_id__content_get`). This redirects to the
   rendered video URL for download.

## Conventions & error handling
- Timestamps (`created_at`, `completed_at`) are Unix epoch seconds.
- Request validation failures return HTTP `422` (`HTTPValidationError`).
- The API is asynchronous — never assume a video is ready at create time; always poll.
- Prefer the `id` field; `video_id` is deprecated.
