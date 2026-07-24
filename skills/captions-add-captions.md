---
name: Add stylized captions to a video
description: Pick a caption template, submit a video (upload or existing video ID) for captioning, and poll the job to completion.
api: openapi/captions-mirage-openapi-original.json
base_url: https://api.mirage.app
auth: x-api-key header
operations:
  - list_caption_templates_v1_videos_captions_templates_get
  - create_captioned_video_v1_videos_captions_post
  - get_video_status_v1_videos__video_id__get
---

# Add captions to a video

Apply Captions' stylized, animated captions to a video using the Mirage Video API.

## Prerequisites
- API key sent as `x-api-key` on every request. Base URL `https://api.mirage.app`.

## Steps

1. **Choose a caption template** — `GET /v1/videos/captions/templates`
   (`list_caption_templates_v1_videos_captions_templates_get`). Each
   `MACaptionTemplate` has an `id`, `name`, and optional `preview_url`. Pick a
   `caption_template_id`. (Fetch one template with
   `get_caption_template_v1_videos_captions_templates__template_id__get` if needed.)

2. **Create the captioned video** — `POST /v1/videos/captions`
   (`create_captioned_video_v1_videos_captions_post`). Multipart body:
   - `caption_template_id` (required).
   - Exactly **one** of: `video` (a `.mp4`/`.mov` upload, 9:16 aspect ratio, max 50MB)
     **or** `video_id` (an existing video ID).
   Returns a `MAVideo` job with `status: PROCESSING` and a `source_video_id`.

3. **Poll for completion** — `GET /v1/videos/{video_id}`
   (`get_video_status_v1_videos__video_id__get`) until `status` is `COMPLETE`
   (then download via `/v1/videos/{video_id}/content`) or `FAILED` (inspect `error`).

## Notes
- Provide only one of `video` / `video_id`; supplying both (or neither) is a `422`.
- Uploads must be 9:16 and ≤ 50MB per the reference.
