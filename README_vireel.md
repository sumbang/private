# Vireel.app - Single README

This file centralizes the information that was previously spread across several Markdown documents in the project.

## Overview

Vireel.app is an AI-assisted video creation platform. From an uploaded video or a YouTube link, it offers the following modules:

- **Reels Generator**: turns a long-form video into short vertical clips (Reels/Shorts/TikTok), with hooks, subtitles and thumbnails; reels can also be created manually and edited.
- **AI Captions**: generates captions tailored to the target platform, with customizable styles and themes, then renders the final video.
- **Film summary**: turns a full movie into a narrated, edited summary (transcript, scene detection, AI edit plan reviewed by the user, TTS voice-over, FFmpeg assembly).
- **Anonymous stories**: turns a video testimonial into an anonymized, editable written story.
- **Social publications**: publishes the generated content to connected accounts (YouTube, TikTok, Instagram, Facebook, LinkedIn), with daily quotas and publication statistics.
- **Account and billing**: credit-based usage, Stripe subscription plans, referrals and notifications.

The project is structured around a Python/FastAPI backend, a React/Vite dashboard (French/English), a Remotion rendering service and cloud/AI integrations (Supabase, S3, AI providers).

## Project components

- `app.py`: main API and orchestration of backend jobs.
- `main.py`: video processing, splitting, analysis and clip generation.
- `job_manager.py`: job lifecycle, progress, errors, retry.
- `billing.py`: cost estimation and credit conversion.
- `subtitles.py`, `hooks.py`, `thumbnail.py`, `editor.py`: media modules (subtitles, overlays, thumbnails, editing).
- `supabase_request.py`, `supabase_media.py`: Supabase data access.
- `s3_uploader.py`: S3 upload, deletion and signed URLs.
- `dashboard/`: user UI (React/Vite/Tailwind).
- `render-service/` and `remotion/`: web/service video rendering.
- `supabase/migrations/`: database migrations.

## Main features

### Backend

- Asynchronous job management with priorities and retries.
- Clip generation with metadata and media persistence.
- Subtitle generation and burn-in.
- Management of credits and operation costs.
- Persistence of projects/reels/captions and history.
- S3 storage + Supabase database integrations.

### Dashboard

- Reel/subtitle creation flow.
- Settings (subtitle style, publishing options, social accounts).
- Display of job status and generated content.

### AI / Media

- AI analysis and suggestions (depending on the configured provider).
- Asset generation (thumbnails, hooks, descriptions).
- Video rendering tools and FFmpeg post-processing.

## Configuration

Configuration relies mainly on environment variables.

Examples of variable groups:

- Application secrets (`SECRET_KEY`, etc.)
- Supabase (`SUPABASE_URL`, `SUPABASE_SERVICE_ROLE_KEY`, tables)
- AWS (`AWS_ACCESS_KEY_ID`, `AWS_SECRET_ACCESS_KEY`, `AWS_S3_BUCKET`, region)
- AI (`GEMINI_API_KEY`, models, other providers)
- Billing/credits (unit prices, markup)
- Operational limits (max duration, timeouts)
- Media retention (see below)

Variables required at backend startup (the app refuses to start if they are missing, except when running tests):

- `SUPABASE_JWT_SECRET`: JWT secret of the Supabase project (Project Settings -> API -> JWT Secret), used to verify the session tokens sent by the frontend (`Authorization: Bearer ...`). It is the single source of truth for a user's identity on the backend side.
- `ENCRYPTION_KEY`: key (16 characters minimum) used to encrypt (AES-256-GCM) the social network OAuth tokens stored in the database.
- `RENDER_SERVICE_API_KEY`: internal key shared between the backend and the render-service (Remotion), checked on every call to `/render`. Must also be set in the environment of the `renderer` service (docker-compose).

### Media retention

Vireel no longer sells storage quota (in GB) to the user -- only credits are
billed. Internally, however, each media item (imported source or generated
output) has a physical retention period in S3, set *permanently* when it is
created, based on the user's subscription status at that exact moment (a later
change of subscription or of these variables never affects an already created
media item). See `retention_config.py` for the startup validation (negative or
inconsistent values are rejected) and `app.py` for the 3 automated jobs
(expiration, notification before expiration, cleanup of temporary files).

Variables (see `.env.example` for the default values):

- `RETENTION_FREE_SOURCE_DAYS` / `RETENTION_FREE_PRODUCED_DAYS`: retention
  period (source / output) without an active subscription.
- `RETENTION_SUBSCRIBER_SOURCE_DAYS` / `RETENTION_SUBSCRIBER_PRODUCED_DAYS`:
  retention period (source / output) with an active subscription.
- `RETENTION_TEMP_FILES_HOURS`: lifetime of local working files
  (FFmpeg frames/chunks, uploads, finished job folders) -- a separate
  category, unrelated to the durations above.
- `RETENTION_NOTIFICATION_HOURS_BEFORE`: warning delay before the
  automatic deletion of an output media item (never for a source media item).
- `S3_STORAGE_COST_PER_GB_DAY`: internal S3 cost per GB per day used to
  compute the retention cost added to credit billing (never shown
  separately to the user).

## Running locally

### Backend (root)

```zsh
pip install -r requirements.txt
python app.py
```

### Dashboard

```zsh
cd dashboard
npm install
npm run dev
```

### Docker Compose option

```zsh
docker-compose up --build
```

## Tests

### Backend (pytest)

```zsh
pytest
```

### Dashboard (vitest)

```zsh
cd dashboard
npm test
```

Unit tests have been added for the critical backend modules and for the `dashboard/src/lib` UI utilities.

## Deployment

The repo contains compose files for different environments:

- `docker-compose.yml`
- `docker-compose.staging.yml`
- `docker-compose.prod.yml`

Check the environment variables and the Supabase migrations before going to production.
