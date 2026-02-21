# Architecture — Merve-video.online vol.2 (YouTube Merge)

## Overview

YouTube Merge v2 — микросервисная архитектура для автоматического слияния видео с YouTube. Три компонента: Telegram бот, веб-сайт и модуль обработки видео. Поддержка оплаты через QIWI, отправка через Google Drive + email.

## Tech Stack

| Layer | Technology |
|-------|-----------|
| Bot | Python (Telegram API) + Telethon client |
| Site | Python (веб-приложение) |
| Video | Python (обработка видео) |
| Payment | QIWI P2P |
| Storage | Google Drive API |
| Email | SMTP (Gmail) |

## Project Structure

```
├── bot/                  # Telegram bot
│   └── settings.ini     # BOT token, CLIENT API ID/HASH, QIWI token
├── site/                 # Web application
│   └── settings.ini     # MAIL config, QIWI token, Google OAuth
├── app_video/            # Video processing module
├── requirements.txt      # Python dependencies
├── README.md             # Setup instructions
└── LICENSE               # MIT
```

## Architecture

```
┌─────────────┐     ┌─────────────┐     ┌─────────────┐
│  Telegram   │     │    Site     │     │  app_video  │
│    Bot      │────▶│  (Web App)  │────▶│  (ffmpeg?)  │
│  + Client   │     │  + G.Drive  │     │             │
└─────────────┘     └─────────────┘     └─────────────┘
       │                    │
       ▼                    ▼
   QIWI P2P            Gmail SMTP
```

## Key Concepts

- **Microservices** — три отдельных компонента запускаются независимо
- **Telethon client** — обход ограничения Telegram на размер файлов ботом (CLIENT отправляет большие файлы)
- **Google Drive** — видео отправляются через Google Drive link на email
- **QIWI payment** — оплата через QIWI P2P API
- **Google OAuth** — `client_secrets.json` + `token.json` для авторизации Google Drive
