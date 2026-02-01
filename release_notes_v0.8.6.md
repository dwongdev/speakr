# Release Notes - v0.8.6

**Folders & Automation**

## New Features

### Folders Organization System

- **Folder Management** - Organize recordings into folders with a dedicated sidebar selector
- **Per-Folder Settings** - Each folder can have its own custom AI prompts and ASR settings
- **Folder Pills** - Visual indicators showing which folder a recording belongs to
- **Title Bar Integration** - Folder icon in the title bar for quick access

### Auto Speaker Labeling

- **Voice Embedding Matching** - Automatic speaker identification using voice embeddings from speaker profiles
- **Seamless Integration** - Works with existing speaker profiles created via WhisperX ASR service

### Per-User Auto-Summarization

- **User-Configurable** - Each user can enable/disable automatic summary generation in their account settings
- **Flexible Control** - Override the system default on a per-user basis

### Azure OpenAI Connector (Experimental)

- **New Transcription Provider** - Connect to Azure OpenAI for transcription
- **Community Testing** - Experimental feature - feedback welcome!

### HTTPS Validation

- **Clear Error Messages** - Recording on non-HTTPS connections now provides helpful error messages explaining the requirement
- **Pre-Recording Check** - Validates HTTPS before starting a recording session

## Improvements

### Architecture Cleanup

- **Legacy ASR Removal** - Fully migrated to connector architecture, legacy ASR transcription code removed
- **Cleaner Codebase** - Reduced complexity and improved maintainability

### Audio Handling

- **Codec Fallback** - Automatic fallback to MP3 when configured codec conflicts with connector requirements

### Share Page Enhancements

- **Server-Side Rendering** - Improved share page with SSR for better performance
- **Click-to-Seek** - Click on transcript segments to jump to that position in shared recordings
- **Playback Speed Menu Fix** - Fixed positioning of playback speed menu on share page

### Configuration Options

- **Pre-rendered Share Pages** - New `READABLE_PUBLIC_LINKS` option to server-render transcripts in share pages, making them accessible to LLMs and scrapers
- **Skip Email Domain Check** - New `SKIP_EMAIL_DOMAIN_CHECK` env var for admin user creation flexibility

## Bug Fixes

- **PostgreSQL Boolean Defaults** - Fixed migration compatibility for PostgreSQL boolean default values
- **Folders Feature Detection** - Fixed feature detection in account settings for folders functionality
- **Audio Player Visibility** - Fixed audio player visibility for incognito recordings (prevents 404 errors)

## Configuration

### New Environment Variables

| Variable | Description | Default |
|----------|-------------|---------|
| `READABLE_PUBLIC_LINKS` | Server-render transcripts in share pages (accessible to LLMs/scrapers) | `false` |
| `SKIP_EMAIL_DOMAIN_CHECK` | Skip email domain validation for admin creation | `false` |

### Azure OpenAI Configuration

For Azure OpenAI connector (experimental):
```bash
TRANSCRIPTION_CONNECTOR=azure_openai
AZURE_OPENAI_API_KEY=your-api-key
AZURE_OPENAI_ENDPOINT=https://your-resource.openai.azure.com
AZURE_OPENAI_DEPLOYMENT=your-deployment-name
```

## Compatibility

Compatible with v0.8.x. Existing recordings and workflows continue to work unchanged.

### Database Migrations

**Required:** Automatic migrations run on startup and will:
- Create new `folder` table
- Add `folder_id` column to `recording` table
- Add user preference columns for auto speaker labelling and auto-summarization
- Create indexes for performance

PostgreSQL users will see proper boolean defaults applied (fixes from this release).

## Upgrade Notes

1. **Folders Disabled by Default** - The folders feature is **disabled by default**. Enable it in Admin Settings by setting `enable_folders` to `true`, or set `ENABLE_FOLDERS=true` in environment
2. **Pre-rendered Share Pages** - Set `READABLE_PUBLIC_LINKS=true` if you want transcripts on share pages to be server-rendered (useful for LLMs, scrapers, or accessibility tools)
3. **Auto-Summarization** - New per-user setting defaults to enabled (matching previous system behavior)
