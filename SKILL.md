---
name: google-docs
description: Manage Google Docs and Google Drive with full document operations and file management. Includes document reading, editing, formatting, and Drive operations (upload, download, share, search). Use for document content operations, Drive file management, and integration with other Google services.
category: productivity
version: 1.1.0
key_capabilities: read content, insert/append/replace text, format text, Drive upload/download/share/search
when_to_use: Document content operations, text insertion/replacement, formatting, Drive file management, sharing files
---

# Google Docs & Drive Management Skill

## Purpose

Manage Google Docs documents and Google Drive files:

**Google Docs:** Read, create, insert/append text, find-replace, format (bold/italic/underline), page breaks, delete content, insert images

**Google Drive:** Upload, download, search, list, share, create folders, move, copy, delete files

**Integration**: Both scripts share OAuth credentials at `~/.claude/.google/`

## When to Use

- User requests to read, create, or edit a Google Doc
- User wants to upload, download, search, or share Drive files
- Keywords: "Google Doc", "document", "Drive", "upload", "share"

## Google Docs Operations

### Read & Structure
```bash
scripts/docs_manager.rb read <document_id>
scripts/docs_manager.rb structure <document_id>
```

### Create Document
```bash
echo '{"title": "My Doc", "content": "Initial text"}' | scripts/docs_manager.rb create
```

### Insert & Append Text
```bash
echo '{"document_id": "ID", "text": "New text", "index": 1}' | scripts/docs_manager.rb insert
echo '{"document_id": "ID", "text": "Appended text"}' | scripts/docs_manager.rb append
```

### Find and Replace
```bash
echo '{"document_id": "ID", "find": "old", "replace": "new"}' | scripts/docs_manager.rb replace
```

### Format Text
```bash
echo '{"document_id": "ID", "start_index": 1, "end_index": 50, "bold": true}' | scripts/docs_manager.rb format
```
Options: `bold`, `italic`, `underline` (all true/false, can combine)

### Delete Content
```bash
echo '{"document_id": "ID", "start_index": 100, "end_index": 200}' | scripts/docs_manager.rb delete
```

### Insert Image
```bash
echo '{"document_id": "ID", "image_url": "https://..."}' | scripts/docs_manager.rb insert-image
```
Options: `width`, `height` (in points), `index` (position). URL must be publicly accessible.

### Page Break
```bash
echo '{"document_id": "ID", "index": 500}' | scripts/docs_manager.rb page-break
```

## Google Drive Operations

### Upload & Download
```bash
scripts/drive_manager.rb upload --file ./doc.pdf [--folder-id ID] [--name "Name"]
scripts/drive_manager.rb download --file-id ID --output ./local.pdf [--export-as pdf|csv|txt]
```

### Search & List
```bash
scripts/drive_manager.rb list [--max-results 20]
scripts/drive_manager.rb search --query "name contains 'Report'"
scripts/drive_manager.rb search --query "mimeType='application/vnd.google-apps.document'"
```

### Share Files
```bash
scripts/drive_manager.rb share --file-id ID --email user@example.com --role reader|writer
scripts/drive_manager.rb share --file-id ID --type anyone --role reader  # Public link
```

### Folder Management
```bash
scripts/drive_manager.rb create-folder --name "Folder" [--parent-id ID]
scripts/drive_manager.rb move --file-id ID --folder-id FOLDER_ID
```

### Other Operations
```bash
scripts/drive_manager.rb get-metadata --file-id ID
scripts/drive_manager.rb copy --file-id ID --name "Copy Name"
scripts/drive_manager.rb delete --file-id ID    # Moves to trash (recoverable)
```

## Safety Guidelines

**Destructive operations** - Always confirm with user before:
- Deleting documents or files (even to trash)
- Using `replace` which affects ALL occurrences
- Deleting content ranges from documents

**Best practices:**
- Read document/file first before modifying
- Show user what will change before executing
- Prefer `append` over `insert` when adding to end
- Delete moves to trash by default (recoverable from Drive)

## Authentication

**Credentials Location**: `~/.claude/.google/client_secret.json` and `token.json`

**First Time Setup**:
1. Run any command - browser opens automatically for Google sign-in
2. Grant access to requested services
3. Token saved automatically (shared with other Google skills)

**Re-auth**: Run `scripts/docs_manager.rb auth` or `scripts/docs_manager.rb auth --force`

## Output Format

All commands return JSON:
```json
{"status": "success", "operation": "...", ...}
{"status": "error", "error_code": "...", "message": "..."}
```

## Additional Resources

- `references/docs_operations.md` - Complete parameter reference and examples
- `references/formatting_guide.md` - Text formatting and document structure
- `references/troubleshooting.md` - Error handling and debugging
- `references/integration-patterns.md` - Workflow examples
- Run `scripts/docs_manager.rb --help` or `scripts/drive_manager.rb --help`

---

**Dependencies**: Ruby (2.7+) with gems:
```bash
gem install google-apis-docs_v1 google-apis-drive_v3 google-apis-sheets_v4 \
            google-apis-calendar_v3 google-apis-people_v1 googleauth
```
