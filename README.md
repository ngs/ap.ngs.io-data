# ap.ngs.io-data

Data repository for the [ap.ngs.io](https://github.com/ngs/ap.ngs.io) ActivityPub server.

## Overview

This repository serves as the source of truth for the ActivityPub server, managing:

- Account profile information
- Posts (Markdown format)
- Media files (images, etc.)
- Follower/following data

## Directory Structure

```
accounts/
└── {handle}/
    ├── profile.json      # Profile settings
    ├── public_key.pem    # Public key
    ├── posts/            # Posts (Markdown)
    │   └── {ulid}.md
    ├── media/            # Media files
    │   ├── avatar.jpg
    │   ├── header.jpg
    │   └── ...
    └── data/
        ├── followers.json
        ├── following.json
        └── received/
            ├── replies.json
            ├── likes.json
            └── boosts.json
```

## Creating Posts

Create a Markdown file in `accounts/{handle}/posts/` directory.

ULID is recommended for filenames (e.g., `01JJDNH8G000000000000.md`).

```markdown
---
published: "2025-01-22T10:00:00Z"
visibility: "public"
sensitive: false
---

Post content goes here.

Use #hashtag for hashtags, ![alt](image.jpg) for images.
```

### Front Matter

| Field | Description | Default |
|---|---|---|
| `published` | Publication date (ISO 8601) | Required |
| `visibility` | `public`, `unlisted`, `followers`, `direct` | `public` |
| `sensitive` | Sensitive content flag | `false` |
| `spoiler_text` | Content warning text | - |

## Profile Settings

`accounts/{handle}/profile.json`:

```json
{
  "name": "Display Name",
  "summary": "Bio (HTML supported)",
  "icon": "avatar.jpg",
  "image": "header.jpg",
  "fields": [
    { "name": "Website", "value": "https://example.com" },
    { "name": "GitHub", "value": "https://github.com/username" }
  ],
  "manuallyApprovesFollowers": false,
  "discoverable": true
}
```

### Verified Links

Links in `fields` will be displayed as verified on Mastodon if the linked page contains a `rel="me"` link back to this profile.

## Automatic Deployment

On push to `master` branch, GitHub Actions automatically:

1. Syncs with the server's D1 database
2. Publishes new posts to followers

## Related Repository

- [ap.ngs.io](https://github.com/ngs/ap.ngs.io) - ActivityPub server
