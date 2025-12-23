# Google Docs Skill for Claude Code

A Claude Code skill for managing Google Docs and Google Drive with comprehensive document and file operations.

## Features

### Google Docs Operations
- Read document content and structure
- Create new documents
- Insert and append text
- Find and replace text
- Text formatting (bold, italic, underline)
- Insert page breaks and images
- Delete content ranges

### Google Drive Operations
- Upload and download files
- Search across Drive
- Create and list folders
- Share files and folders
- Move and organize files
- Export files to different formats (PDF, PNG, etc.)

## Prerequisites

- **Ruby** (2.7 or later recommended)
- **Google Cloud Project** with Docs and Drive APIs enabled

### Install Ruby Dependencies

```bash
gem install google-apis-docs_v1 \
            google-apis-drive_v3 \
            google-apis-sheets_v4 \
            google-apis-calendar_v3 \
            google-apis-people_v1 \
            googleauth
```

If you don't have root access, use the `--user-install` flag:

```bash
gem install --user-install google-apis-docs_v1 \
                           google-apis-drive_v3 \
                           google-apis-sheets_v4 \
                           google-apis-calendar_v3 \
                           google-apis-people_v1 \
                           googleauth
```

## Installation

Add this skill to your Claude Code configuration:

```bash
# Clone to your skills directory
git clone https://github.com/robtaylor/google-docs-skill.git ~/.claude/skills/google-docs

# Or add as submodule to your claude-config
cd ~/.claude
git submodule add https://github.com/robtaylor/google-docs-skill.git skills/google-docs
```

## Setup

1. **Create Google Cloud Project** and enable the Docs and Drive APIs
2. **Create OAuth 2.0 credentials** (Desktop application type)
3. **Download credentials** and save as `~/.claude/.google/client_secret.json`
4. **Run any command** - the script will prompt for authorization

The OAuth token is shared with other Google skills (Sheets, Calendar, Gmail, etc.).

## Usage

See [SKILL.md](SKILL.md) for complete documentation and examples.

### Quick Examples

```bash
# Read a document
scripts/docs_manager.rb read <document_id>

# Create a document
echo '{"title": "My Doc", "content": "Hello World"}' | scripts/docs_manager.rb create

# Upload a file to Drive
scripts/drive_manager.rb upload --file ./myfile.pdf --name "My PDF"

# Search Drive
scripts/drive_manager.rb search --query "name contains 'Report'"
```

## License

MIT License - see [LICENSE](LICENSE) for details.
