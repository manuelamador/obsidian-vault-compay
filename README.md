# Vault Compay

Chat with [Claude Code](https://claude.com/product/claude-code) or [Codex](https://developers.openai.com/codex) inside [Obsidian](https://obsidian.md). The plugin runs your installed command-line tools with your vault as their working folder, so they can read your notes and help you edit them.

Claude Code runs through the Claude Agent SDK and uses your existing CLI sign-in. You can use a Claude subscription without an API key. Codex uses your ChatGPT sign-in. Choose an engine for each new chat, or set one as the default for chats and other plugin features.

> [!NOTE]
> I built this plugin for my own use. Use it at your own risk.

> [!IMPORTANT]
> Vault Compay is an independent project. It is not affiliated with, endorsed by or sponsored by Anthropic or OpenAI. It runs the Claude Code and Codex installed on your computer, signed in with your own account, and never handles your credentials. Your use of Claude Code and Codex is governed by your own agreements with Anthropic and OpenAI. Claude, Claude Code and Codex are trademarks of their owners.

Built by **Claude Opus 5** (`claude-opus-5`) and **Claude Opus 5.5** (`claude-opus-5-5`) in Claude Code.

<img src="docs/chat.webp" width="800" alt="A note with equations beside the Vault Compay chat panel in Obsidian.">

## Requirements

- **Obsidian desktop** on macOS or Windows. Enable the Bases core plugin if you want to use the Memos table.
- **Claude Code, Codex, or both**, installed and signed in. Only the engine you use needs to be installed.

### Claude Code

Use the [native installer](https://docs.claude.com/en/docs/claude-code/setup), then run `claude` in a terminal to sign in.

- macOS: `curl -fsSL https://claude.ai/install.sh | bash`
- Windows PowerShell: `irm https://claude.ai/install.ps1 | iex`

Windows also requires [Git for Windows](https://git-scm.com/downloads/win). The plugin needs the native `claude.exe`; it cannot start an npm-installed `claude.cmd`.

### Codex

Install the Codex CLI with `npm install -g @openai/codex` or `brew install codex`, or use the copy installed by the ChatGPT desktop app. Run `codex` in a terminal to sign in.

## Installation

The simplest installation is to download three files and copy them into your vault. No build is needed. Vault Compay is not yet listed in Obsidian's community plugin directory.

### Download three files

> [!TIP]
> **No build required.**
>
> 1. Download **main.js**, **manifest.json** and **styles.css** from the latest [release](https://github.com/manuelamador/obsidian-vault-compay/releases).
> 2. Move all three files into `<vault>/.obsidian/plugins/vault-claude/`. Create the folder if it does not exist.
> 3. In **Settings → Community plugins**, turn off Restricted mode if needed and enable **Vault Compay**.
> 4. Open the chat from the robot icon or **Vault Compay: Open chat**.

The folder is named after the plugin's id, `vault-claude`, from before the plugin was renamed: it was called Vault Claude, then Vault Agent. An existing installation updates in place and keeps its settings and chats.

The `.obsidian` folder is hidden. In Finder, press ⌘⇧. to show it. In Windows File Explorer, enable **View → Show → Hidden items**.

To update a manual installation, replace the three files and disable and re-enable the plugin.

### With BRAT

[BRAT](https://github.com/TfTHacker/obsidian42-brat) installs plugins from GitHub releases and can keep them updated.

1. In **Settings → Community plugins**, turn off Restricted mode if needed. Choose **Browse**, find **BRAT**, and install and enable it.
2. Run **BRAT: Add a beta plugin for testing**.
3. Enter `manuelamador/obsidian-vault-compay` and choose **Add Plugin**.
4. Enable **Vault Compay** in **Settings → Community plugins** if BRAT has not already enabled it.
5. Click the robot icon in the left ribbon, or run **Vault Compay: Open chat**.

BRAT installs `main.js`, `manifest.json` and `styles.css` in `<vault>/.obsidian/plugins/vault-claude/`. Enable startup updates in BRAT's settings, or run **BRAT: Check for updates to all beta plugins and UPDATE**. Disable and re-enable Vault Compay after an update to load the new version.

### Executable paths

The plugin looks for Claude Code in `~/.local/bin`, Homebrew's folders and PATH. On Windows, it also checks `%USERPROFILE%\.local\bin\claude.exe`.

For Codex, it checks PATH, Homebrew's folders and the ChatGPT app's installation at `~/.codex/plugins/.plugin-appserver/codex-cli/bin`.

If detection fails, set **Claude Code executable** or **Codex executable** under **Settings → Vault Compay**.

## Features

### Chatting

- **Read replies in Obsidian Markdown.** Wikilinks, callouts, equations and vault images render in the panel. Click a figure to view it over the whole window: scroll or pinch to zoom, drag to move around it, and click or press Esc to close. Thinking, tool calls and intermediate notes collapse into a **Steps** line.
- **Review file changes.** Replies list changed files and their diffs. Open a file at the affected line.
- **Work with a reply.** Quote a passage, ask a side question, copy it, insert it into a note, save it as a memo, or branch into another chat.

<details>
<summary><strong>Reply tools and navigation</strong></summary>

- Click a changed-file card to expand its diff. Click its name to open the file, or a diff line to jump to that line.

  <img src="docs/files-changed.png" width="480" alt="A changed-file card expanded to show added lines and a link to the full diff.">

- File names written in bold or code with an extension, such as `Notes.md`, open the file. Wikilinks open their notes. Hold ⌘ while hovering to preview a link.
- Select text to reveal **Quote**, **Side chat** and **Memo**. Selected equations are quoted as LaTeX.

  <img src="docs/quote-button.png" width="480" alt="Selected text and an equation with Quote, Side chat and Memo controls.">

- Open a side chat from a selection, a reference card or the "Open side chat" command. It can use the conversation so far, and accepts pasted or dropped images. Claude side chats run in Plan mode; Codex side chats are read-only copies. **Minimise** leaves a bar at the bottom so you can return to the main chat. Claude side chats can be kept with **Keep as a chat**; otherwise they are deleted when closed.

  <img src="docs/side-chat.png" width="520" alt="A side chat with controls to restart, keep, minimise or close it.">

- Buttons under each reply let you reply to it, copy it, insert it into a note, save a memo, or branch from that point.

  <img src="docs/reply-buttons.png" width="420" alt="Reply controls for replying, copying, inserting into a note and branching.">

- Checkbox ticks are saved across chat reopening and Obsidian restarts. They are your own marks; the assistant sees them only if you send them in a message. Copying or saving a reply includes its current ticks. Later ticks do not update the saved note.

  <img src="docs/checkboxes.png" width="480" alt="A chat reply with saved checkbox ticks.">

- The bar above the chat identifies the prompt being answered. Use its arrows or ⌥↑/⌥↓ to move between prompts, its list to see them all, or ⌘F to search.

  <img src="docs/message-list.png" width="560" alt="The prompt navigation bar with its list of messages open.">

- Long chats initially show their latest exchanges, up to ten exchanges and roughly 250 messages. Scroll up to load earlier ones. For the two most recently read long Claude chats, the plugin can read only newly appended content when their files have not been rewritten.
- Messages sent while Claude is working are queued. Click **send now** on a queued message to deliver it at the next pause.
- The pencil beside Send opens a draft note for your message. Send it from the line above the input, or discard it with ×.

  <img src="docs/draft-button.png" width="280" alt="The notebook button for composing a message in a note.">

</details>

<details>
<summary><strong>Adjusting equation size</strong></summary>

If equations look too small, add a CSS snippet under **Settings → Appearance → CSS snippets**. This example increases the usual MathJax size by 5%; adjust it to suit your theme:

```css
mjx-container.MathJax { font-size: calc(113.1% * 1.05) !important; }
```

</details>

### Working with notes

- **Add context.** Attach the open note, include selected text, or mention notes, files and folders with `@`. Use the paperclip, paste, or drag and drop to add files and images.
- **Suggest frontmatter updates.** Review proposed property changes based on a note, nearby notes and related chats. Edit the values and apply only the ones you select. Folder-specific guidance is remembered.
- **Edit a selection.** Right-click selected note text to ask a question or request an edit. Review the word diff before accepting.

  <img src="docs/edit-selection.png" width="560" alt="A proposed selection edit shown as a word diff, with Run again, Accept and Cancel.">

- **Send a note as a prompt.** A note's file menu can attach it or use its contents as your next message.

<details>
<summary><strong>Attachments, mentions and note menus</strong></summary>

- Use **+** on the attached-note chip to attach the open note, or **×** to detach it. Attached notes are sent by path; selected lines are included as text.
- Type `@` to find a note, file, folder, chat or Zotero item. The picker has **Files**, **Chats** and **Zotero** tabs; press Tab to switch. Search chats by title and Zotero items by title, author or year. Files excluded in **Settings → Files and links → Excluded files** do not appear.
- Mentioned notes include their text and an approximate token count. Click a note's chip to send only its path; click again to include its text. Other files and folders are sent by path. Hover over a chip to see what will be included, or click an image chip to view it at full size.
- **Zotero integration** is off by default. It adds @ mentions of Zotero items, **Zotero** and **PDF** buttons and first-page thumbnails on reference cards, and cards for cited works found in Zotero. When enabled and Zotero is running, the picker lists My Library and group-library items. A selected item includes its citation, DOI, Zotero link and local PDF path in either engine's chat. The plugin reads this information through Zotero's local API; Zotero must be running. The setting is unavailable when Zotero is not found on the computer, and shows whether Zotero is connected. When Zotero cannot be reached, the panel says so once per Obsidian session.
- Claude and Codex link each citation in a reply to the work's DOI or web page and list the works in a `reference` code block at the end (title, authors, year, venue, DOI, URL). The block shows as one line per work, each opening into a card with the DOI or web link and **Copy** (citation or BibTeX); hovering a citation shows its card in a popover after a moment, or at once with ⌘ (Ctrl), taken from the block or, with **Zotero integration** on, from Zotero. A card adds **Note** when a vault note has the work's `doi` property or is named after its title, and, with **Zotero integration** on, **Zotero** and **PDF** when the work is in Zotero. When Zotero is running and does not hold the work, the card shows **+** (Add to Zotero): it saves the card's details (title, authors, year, venue as a journal article, DOI, URL) as a new item through Zotero's local connector, the endpoint the browser extension uses, into the library and collection selected in Zotero, then replaces **+** with the **Zotero** link. The details are the ones the model wrote; Zotero's **Retrieve Metadata** can correct them from the DOI, and **Find Available PDF** can fetch the PDF. When the work's PDF is on this computer (linked from its vault note, or held by Zotero), the card shows its first page, drawn with Obsidian's PDF engine as the card comes into view; a click opens the PDF. The card is drawn from the block itself; the plugin fetches nothing from the web.
- The **References** tab of the **In this chat** window (the library button beside the paperclip, or **Show references in this chat**) lists every work the chat and its notes cite: the `reference` blocks, Zotero links and DOI links in the whole chat, including turns not drawn, and the Zotero links, DOI links and `doi` property of the attached note and each note the chat changed, was sent, mentioned or saved as a memo. A work appears once, under the first place it was found, with the other places named under it; the search field narrows the list by title, author or year. ↑ and ↓ pick a work, ↵ opens its PDF (else its DOI or web page) and ⌘↵ asks about it in a side chat. Each work is drawn as a reply's card, with the same **Note**, **Zotero** and **PDF** buttons and thumbnail. With **Zotero integration** on, a work known only by its Zotero link or DOI gets its title and details from Zotero; with it off, such a work shows as its link.

  <img src="docs/mention.png" width="480" alt="The @ picker for notes, files and folders.">

- Right-click selected text for **Ask Claude about selection** or **Edit with Claude**.

  <img src="docs/editor-menu.png" width="360" alt="Selection context menu with Ask Claude and Edit with Claude actions.">

- Right-click a file in the explorer, or open a note's ⋯ menu, for **Attach to Claude**. Notes also offer **Send to Claude as a prompt**. Selection and note actions are also available in the command palette.

  <img src="docs/note-menu.png" width="240" alt="A note menu with Attach to Claude and Send to Claude as a prompt.">

</details>

### Email cards and drafts

Gmail results appear as message and draft cards in Claude and Codex chats. Open them in Gmail, quote a message, save it as a note, or make a todo. Cards do not send or edit mail; ask the assistant in the chat to do that.

<details>
<summary><strong>Using email cards and drafts</strong></summary>

**Message cards** show the sender, To, Cc, Bcc when available, subject, date and message text. Unread messages are marked. Read a full message or thread to see its body and available attachment details.

Attachment chips open files in Gmail when a link is available. Images are grouped into a count. **Download all**, when available, opens Gmail's attachment zip download. The plugin does not fetch email or attachments itself; cards display the connector's results.

Use a card to open Gmail, quote the message, start a chat about it, make a todo, or save a note. A thread can be saved as a single note. **Make a todo** uses `Todo.md`, under **Open**, by default; both are configurable, and a due date is optional.

**Draft an email from this note…** opens a chat with the note mentioned and a request for a draft. Tell the assistant whom to write to and what to say. Draft cards show recipients, subject, text and attachments when available. Ask in the chat to change or send a draft; the engine and connector handle permissions. Later successful tool calls mark cards **Sent**, **Deleted** or **Earlier version**.

Email content is treated as data. Card links are restricted to Gmail, and saved or quoted messages escape images and HTML. These display protections do not replace the engine's permission controls.

The plugin remembers which threads each chat read. Above the input, a line lists threads read by chats associated with the open note. Connections lists a chat's threads below its map.

</details>

### Memos

**Keep useful passages as notes linked to their source.** Save a selection or reply as a memo, with a title, description and tags, or make a quick bookmark. Memos are ordinary notes, stored by default in `Claude chats/Memos/` beside `Memos.base`. The Memos table lets you browse by chat, note or tag and return to the original conversation.

<img src="docs/memos-table.webp" width="800" alt="The Memos table beside its source conversation.">

<details>
<summary><strong>Saving and using memos</strong></summary>

- Choose **Memo** above selected chat text, or the sticky-note button under a reply. A selection spanning several messages keeps separate passages labelled You or Claude. A reply memo includes its prompt. Equations remain LaTeX.
- The model configured for small jobs suggests a title and description. Edit them, add tags, and optionally explain **Why I'm keeping this**. Each save creates a new memo.
- **Related memos and notes** includes the attached note and notes linked from the selected passages. Remove any you do not want. **Link a memo or note…** lets you add others; archived memos remain available and are labelled.

  <img src="docs/memo-form.png" width="420" alt="The memo form with title, description, tags, related notes and source passages.">

- For a bookmark, leave the title empty or ⌥-click (Alt-click) **Memo** to skip the form. The opening words become the title, a timestamp is added if needed, and the memo receives the `bookmark` tag.
- A memo contains its description, **Why**, **Next** and **Sources**. Fill in Next yourself. Sources records each passage's author and date, with links back to the chat. Properties link related notes and memos.
- **Continue from this memo**, in the command palette or note menu, prepares a new chat. Choose which passages, Why and Next sections, related notes, and linked memos to include. Nothing is sent until you send the prepared message.
- **Go to the passage** opens the source message, loading earlier exchanges or searching its text if necessary. **Continue in the chat** opens that chat with the passage quoted.
- The **Memos** tab of the **In this chat** window (the library button beside the paperclip, or **Notes, memos and references…** in the chat menu) lists the chat's memos. Its links **All memos in a table** and **This chat's memos in a table** open the Memos table.

- The table has views by chat, note and tag. When left open, it follows the panel's current chat. **Send to chat** adds a memo to the input; clearing the input removes the mention. Use **Status** for your own labels, such as exploring or resolved. **Archived** moves a memo to the Archived view. The Chats column opens source chats at their passages.
- Delete memos like other notes, or select their table rows and choose **Delete**.

</details>

### Controls and approvals

- **Choose the model and permissions.** Set the model, reasoning effort, permission mode and fast mode below the input. Defaults and deny rules are in settings. Claude and Codex apply permissions differently; see the Codex section below.
- **Review plans before execution.** In a Claude chat, choose Plan mode or type `/plan`. Approve the plan, edit it in a note, reject it, or tell Claude what to change.

  <img src="docs/plan-card.png" width="560" alt="A plan awaiting approval, with Approve, Edit in a note, Reject and feedback controls.">

- **Answer questions in the panel.** Question cards accept choices and written answers.
- **Receive notifications.** While Obsidian is in the background, notifications report long replies finishing and requests for approval or input.
- **Track usage.** Hover over the meter for context use, account limits and reset times. **Compact now** asks the engine to compact the conversation. Reopened chats show their last saved context reading until fresh usage arrives.

<details>
<summary><strong>Controls, plans, questions and usage</strong></summary>

The controls below the input select the model, effort, permission mode and fast mode.

<img src="docs/controls.png" width="360" alt="Model, effort, permission mode and fast-mode controls below the input.">

- **Leave plan mode** exits without approving the plan.
- **Edit in a note** opens a temporary plan note. Approval sends your revised version and removes the temporary note. If Esc withdraws the plan, your edits are retained for Claude's next plan in that chat.
- **Skip** declines a question.

  <img src="docs/question-yes-no.png" width="500" alt="A question card with choices, a written-answer field, Send and Skip.">

- Hover over the usage meter for token counts, account usage and reset times. A divider marks compaction.

  <img src="docs/usage-card.png" width="400" alt="Context and account usage with reset times.">

- Default deny rules cover `git checkout`, `git reset`, `git restore`, `git clean` and `rm -rf`. Claude applies its deny rules across permission modes. In Codex, the plugin checks commands only when Codex asks it for approval.

</details>

### Managing chats

- **Find earlier work.** Search history by title, prompt, reply or note. Pin, rename or delete chats from the list.

  <img src="docs/history.png" width="480" alt="Chat history with search, the scratch chat and background-task status.">

- **Return to recent work.** An empty chat lists recent conversations, with pinned chats first. **Pick up where you left off** suggests recent and older conversations that may be worth continuing.
- **Delegate a task.** Right-click **Send**, or run **Run this message in a new chat, report back**. Your message runs in a linked background chat using the current engine, model and attached note. A report appears here when it finishes; you decide whether to add it to your message, open the chat or dismiss it.
- **Keep several chats running.** Switching chats preserves their drafts and attached notes. A tab on the panel's right edge marks a background reply that finished or failed; an amber one, at the top, marks a background chat waiting for your answer or approval, and goes once nothing waits there. Open separate chat tabs or branch from an earlier reply.
- **Use a scratch chat.** Keep a standing conversation for quick questions. It starts over after 24 hours unused by default; useful exchanges can be copied into ordinary chats.
- **Save a conversation.** The chat menu (**⋯** beside the title) saves the chat or a summary as a note, closes the chat, or deletes it.

<details>
<summary><strong>Pick up where you left off</strong></summary>

Open this feature from the command palette, an empty chat, or the New chat button's right-click menu. The configured small-job model reviews short excerpts from recent chats and a random selection of older, potentially unfinished chats. Suggestions are kept for the day; **Suggest again** refreshes them.

Select a suggestion to see a possible next step, related notes and memos, the reasons it was suggested, and recent exchanges. Links open their notes; ⌘-hover previews them. **Use suggested step** opens the chat and appends the step to any existing draft. **Open chat** leaves the draft alone.

Each suggestion has controls to remind you on up to five days, skip it for a week, or ignore it indefinitely. Click a reminder's clock to stop it. **Clear ignored chats** makes ignored chats eligible again.

The plugin reads chats and memos locally. It sends short excerpts to the selected model when generating suggestions, without adding a message to any source chat.

</details>

<details>
<summary><strong>History, branches and background tasks</strong></summary>

- **Close this chat**, in the chat menu (**⋯** beside the title), closes it as a browser closes a tab: it comes off the phone and the ← → arrows, its process ends, and the chat before it (or a new chat) comes up. It stays in history. A chat still working or waiting on you asks first: leave it in the background, where it finishes and then closes, or stop it. **Close this chat** in the command palette does the same.
- The clock opens history. A blue phone icon marks a chat available on the phone. **✕** closes a background chat's process while keeping its conversation, or takes a foreground chat off the phone. The plugin asks first if closing would interrupt work or a pending approval. ⌘↵ or ⌘-click opens a history item in a new tab.

  <img src="docs/history-button.png" width="265" alt="The clock button for opening chat history.">

- Press **Tab** in history to browse by note. Search by note name or folder, then add words to filter chat titles, for example `with:solow diagram`. ⌘↵ opens the note.
- Press **Tab** again to browse by folder (`in:`), then by project (`project:`). Choosing a project shows its map in Connections (⌘-click or ⌘↵ opens its note instead) and lists its chats, newest first, under **New chat in this project** and **New note in** its folder; ⌫ goes back to the projects. **Open project…** opens history on the projects.
- A grey ○ means open or idle in the background. An accent ● marks activity, pending input, background tasks or an unseen reply. No mark means the chat is closed.
- History is reconstructed from Claude session files and recorded Codex threads. **Rebuild chat list from sessions** restores missing records, refreshes titles that still match an initial prompt, and rebuilds the chats' links to notes (as **Rebuild connections from chat files…** in the Connections ⋯ menu does). It also runs automatically after an update, rebuilding the links only the first time. Chat records, settings and their backups are described under **Your data**.
- **History includes all vault sessions** also lists Claude sessions started in a terminal or desktop app. They appear in italics. Continuing one creates a copy so two programs do not write the same session. History groups the copies and offers the most recent one. Codex history lists chats started by the panel.
- The New chat button starts a chat; ⌘-click creates a new tab. Hover over a panel tab's icon to see its chat title.

  <img src="docs/tab-icons.png" width="350" alt="Two chat panel tabs with a chat-title tooltip.">

- Background replies leave ✓ or ! tabs on the panel's right edge, newest first. Hover to see the title and reply preview, click to open, or use × to dismiss. Extra tabs are grouped under **+N**. Unopened tabs survive an Obsidian restart. If the panel is hidden, a notice also reports completion.

  <img src="docs/done-tabs-closed.png" width="54" alt="Completed-chat tabs on the panel’s right edge."> <img src="docs/done-tab.png" width="300" alt="An expanded completed-chat tab showing its title, outcome and reply preview.">

- Agents and commands can continue after a reply finishes. **Stop N tasks** beside Send, or ■ in history, stops them. Stop, or Esc pressed twice, asks before ending active tasks; while a reply runs, a single Esc only says to press it again. **Show background tasks** lists progress, elapsed time and command output, with a Stop button for each task. A task named by its command shows it on one line, cut with an ellipsis; point at it for the whole command. The last 20 finished tasks remain until **Clear** or an Obsidian restart.

  <img src="docs/background-tasks.png" width="480" alt="The Stop 2 tasks control beside Send.">

- With a note open, the line above the input lists chats that changed or received it. ⌥-click removes an association. Search with `with:` in history to find chats about other notes.

  <img src="docs/note-line.png" width="480" alt="A line listing chats associated with the open note, above the attached-note chip.">

- The library button beside the paperclip, with the count of the chat's notes and memos, opens the **In this chat** window, also in the chat menu (**⋯**) as **Notes, memos and references…**. Its tabs are **Notes** (notes the chat changed or mentioned, each with its folder and which), **Memos** and **References**, each with its count; it opens on the first that has any. Tab and Shift+Tab switch tabs, the search field filters the tab shown by words, ↑ and ↓ pick a row and ↵ opens it. In Notes and Memos, ⌘↵ or ⌘-click opens in a new tab and ⌥↵ or ⌥-click attaches the note as an `@` mention.

  <img src="docs/notes-menu.png" width="510" alt="The notes menu listing memos, changed notes and mentioned notes.">

- Scratch appears first in history. Change its timeout in settings or clear it with **Clear the scratch chat** in the chat menu. **Continue as a chat** copies the conversation through a selected reply. **Continue in a chat about "…"** lets you choose a starting prompt for work on a note, copies that work to an ordinary chat, and prepares a message about the note. The scratch chat retains a link to the copy. **Keep scratch chat** copies the whole conversation.
- **Move chat to the main area, or back to the sidebar** switches between the sidebar and a full-width tab. **Open chat in a new window** gives it a separate window. Running chats keep working. Notes opened from the panel go to the main window. Closing the only chat tab outside the sidebar transfers its work to a new sidebar panel.
- The arrow beside your message offers **Copy from here on to a new chat**, **Move from here on to a new chat**, **Send this message to another chat…** and **Remove from here on**. Claude moves and removals offer Undo for ten seconds. Undoing a move also deletes its new chat unless you have continued it. A chat on your phone is taken off it for a move or removal and put back afterwards with a new link (after a move, the new chat goes on the phone). These actions change conversations, not files the assistant edited. Codex has different removal limits, described below.

  <img src="docs/copy-from-here.png" width="178" alt="The arrow beside a user message for copy, move and removal actions.">

</details>

### Connections, projects and linked chats

**See how a conversation connects to your work.** Connections maps the current chat, its notes grouped by folder, and other chats linked to it or sharing those notes. For a chat in a project, the map is the project's: the chat is ringed among the project's other chats, and its own notes and lines are drawn in full while the rest of the project is faded. Chats outside the project that share its notes or are linked with it sit on an outer ring, as on a chat's own map. The map follows the panel as you switch chats.

<img src="docs/connections.webp" width="800" alt="The Connections map linking a chat, its notes and related conversations.">

<details>
<summary><strong>Using Connections and projects</strong></summary>

- Run **Open connections**, or use the project and links chips above the input; × on the project chip takes the chat out of its project. For a chat in no project, the **No project** chip opens a project picker, the most recently changed projects first, with **New project…** last, which opens **Create project…** for this chat and puts it in the new project. Connections opens as a tab beside your notes. With no chat in the panel, it lists the projects, each opening its map, so projects, notes and folders can be browsed without a chat. Codex chats have a green speech bubble, and a **Codex** label where they are named.
- Click a chat to open it in the panel. Click its project to view the project map. Use ← and → to navigate between maps. Chats started outside the panel are initially shown without editing controls.
- The bar above a chat's map reads *project › chat*: click the project's name for its map; × on the project chip above the input takes the chat out. Its ⋯ menu holds the project's actions, **Move this chat to another project…** and the list of the map; beside it are **New chat** in the project and **Fit the map**. A chat in no project shows **No project** and the offers to put it in one on the same row.
- The map is fitted to the pane when drawn. Scroll or pinch to zoom, drag to pan, or with the map focused press + and − to zoom and 0 to fit it again; your zoom stays while the map is redrawn on the same centre. Only the lines of what is in the middle are drawn; point at a chat or note to see its other lines. Each folder is named along the inside of its arc; a name that would cover another or a node is hidden until you point at the folder or one of its notes, or zoom in far enough for it to fit. The legend sits on one line under the map, with an ⓘ explaining the map and a link such as **+8 older** that shows the chats and notes left out. The chat's links are listed under the map when it has any, one row each, under a **Links** heading with an ⓘ explaining links and **+ Link a chat…** at its end; the links chip above the input opens them, or, when the chat has none, goes straight to choosing a chat to link.
- Click a note to centre the map on it: the chats linked to it surround it, with their other notes outside. Click the note in the middle, or **Open note**, to open it. Click a folder's arc to centre the map on the folder: the chats that worked on its notes, in it or its subfolders, surround it, with those notes outside, grouped by subfolder. Right-click a note to open it, in place or in a new tab, or for **Open local graph**, which opens the note beside Obsidian's local graph (the Graph view core plugin must be enabled). Right-click a folder's arc for its project's actions. A project's map, and a chat's map when the chat is in a project, has a **New chat** icon (pen on a square), which starts a chat in the panel already in the project; its ⋯ menu also has **Open local graph**. On a chat's map, click the project in the middle to show the project's own map; there, clicking it lists its chats and notes, and ⌘-click opens the project note (also **Open project note** in its ⋯ menu). Every map has a list in its bar (an icon, or on a chat's own map an item of its ⋯ menu), which opens a window with two tabs listing what the map is about whole, rather than only the first few: chats, newest first, each with its last prompt (the newest 60 at once, the rest once you type a filter), and notes by folder. On a project's map, its chats and the notes they used, then those in its folder no chat used; on a chat's map, the chat and every chat the map relates to it (linked, or sharing its notes) and their notes; on a note's map, every chat linked to the note and their other notes; on a folder's map, every chat that worked on notes in it or its subfolders, those notes, then the notes in it no chat used. Typing filters the tab shown, and Tab switches tabs.
- A **project** is a note associated with a folder. To change a project's folder, edit the `folder:` property of its note; renaming or moving the folder is followed automatically. Chats join projects through the notes they use, or through **Put in**, **Add to a project…**, and, on the map's bar, **Move this chat to another project…** in its ⋯ menu; × on the project chip above the input takes the chat out. Project context accompanies chat messages and is sent again after Claude Code compacts the chat.
- **Create project…** creates a project. Project notes are named `Agent Project — <name>`; the panel, Connections, history and the dialogs show and edit only *<name>*, and **Create project…** and **Rename…** put the prefix back on the note's name (a project note named without it keeps its name as typed). In history, `project:` finds a project by either name. **Write every project's context anew** asks before regenerating all project contexts.
- **Link a chat…** links two chats for navigation. Linking alone sends no content. Tick **Include** in a link's row to send a short digest of the linked chat with your next message; while it is ticked, its size and the digest (folded) show under the row. **Summarise first** generates a summary from more of its conversation.
- Notes in folders named under **Folders left out of Connections** in settings (`Archive` by default: every folder so named, at any depth) are left off the maps and their chats-and-notes windows. **Show archived**, under a map or beside the window's tabs, shows them again until Obsidian restarts.
- **Rebuild connections from chat files** reads saved conversations and adds their links to notes changed, included or mentioned. It does not remove existing links. Run it from the Connections ⋯ menu or settings; **Rebuild chat list from sessions** in the command palette runs it too.

</details>

### Codex

Choose **New Codex chat** from the command palette or the New chat button's right-click menu. You can also select **Claude Code | Codex** on an empty chat before sending its first message. **New chats run on** sets the default engine.

Codex chats are labelled beside their titles, in history and in Connections. They use Codex's app-server and `~/.codex/config.toml`, with the model, effort and permissions selected in the panel.

<details>
<summary><strong>Codex features and differences</strong></summary>

- Replies, reasoning summaries, commands, changed files and diffs use the same panel as Claude chats. The model menu lists available Codex models and their supported reasoning efforts. The meter shows Codex context use and account limits.
- The permission menu applies from the next message: **Read only**, **Ask first**, **Default**, **Auto (reviewer)**, or **Full access** when bypass options are enabled. Default allows vault edits and asks for access outside the vault or online. Auto uses a Codex reviewer for those approvals. Full access permits actions without asking.
- Commands and file changes that require approval appear as cards. Plugin deny rules check commands sent to the panel for approval; they do not check commands Codex approves automatically or through its reviewer.
- Model, effort and permissions are saved for each Codex chat. Claude chats likewise remember their model, effort and permission mode, except Plan mode.
- Project context and included chats work with both engines. Codex side chats use temporary read-only copies and cannot be kept as ordinary chats.
- **Small jobs run on** selects the engine for selection edits, summaries, memo suggestions, frontmatter suggestions, project context and **Pick up where you left off**. **Scratch chat model** selects the scratch engine and model. Set all three engine choices to Codex to use the plugin without Claude Code.
- Removing a Codex chat from panel history leaves its conversation in Codex's own store. An expired or cleared Codex scratch chat is archived there.
- Branching copies the conversation from its beginning through the selected reply's turn, or copies the whole chat. The copy retains the model, effort and permissions.
- Codex supports **Send this message to another chat…** and **Remove from here on**. Removal has no Undo. It cannot copy or move only the exchanges after a selected message into another chat.
- **Compact now** requests compaction and adds a divider when it occurs. **Fast mode** (⚡) requests the fast tier for supported models and uses more of your Codex limits.
- Chats appear in Codex's own app and phone lists under their panel titles. Context added by the panel is sent separately, so your messages remain readable as typed. Messages from before 0.35.0 retain the older combined format.
- Codex chats run through Codex's shared app-server, which the panel starts when a chat needs it and stops when Obsidian quits if the panel started it. See **Phone access** for remote control and the ChatGPT app.
- Claude Code's Plan mode, `/plan` and slash commands are unavailable in Codex chats.

</details>

### Phone access

The phone button controls Claude Code and Codex separately. Its settings are remembered across Obsidian restarts.

- **Claude Code on the phone** makes the current Claude chat and background Claude chats available through the Claude app or claude.ai/code. New chats follow the same setting. You can also start new vault sessions from the phone.
- **Codex on the phone** turns remote control of Codex's shared app-server on or off, without interrupting the chats running through it. The panel's Codex chats use that server either way: the panel starts it when needed and stops it when Obsidian quits if the panel started it. A server started elsewhere is left running.

The button uses the accent colour for Claude, green for Codex, and an accent colour with a green dot when both are enabled.

Codex permits only one app-server to write a chat at a time. While the shared app-server runs, it holds the panel's Codex chats, and the ChatGPT desktop app shows them read-only. If the shared app-server cannot start, each chat runs in its own process, released after three idle minutes, and the ChatGPT app can take a chat by writing in it; after that, only quitting ChatGPT releases it. The panel follows conversation updates and preserves an unsent message when ownership prevents a send. When the ChatGPT app is already running before the panel starts the shared app-server, Codex's phone app may reach this Mac through ChatGPT instead and refuse the panel's chats ("Another Codex session is using this task"); quitting ChatGPT, or starting it after Obsidian, lets the phone reach them through the shared app-server.

### Safety

- Replies load images only from vault files. Other images and embedded notes become links. Most raw HTML is shown as text; a small set of formatting tags, including `<br>` and `<sup>`, is allowed. Rendered content is checked against an allowlist of local resources, including content added after rendering.
- Email cards display text without executing HTML. Saved and quoted email content escapes active image and HTML syntax. Gmail links are restricted to `https://mail.google.com/`. These protections do not prevent an email's words from influencing an assistant; email content is explicitly described to it as data.
- Sending and editing mail use the engine's permission flow. Display cards do not authorize those actions. Connector confirmations forwarded to the panel appear as approval cards.
- Bypass mode must be enabled in settings and is not used for Claude phone sessions. Review the Codex permission differences above before choosing Full access.
- Closing a panel transfers its running chats to another panel when possible. Closing the only main-area or pop-out panel can create a sidebar panel to keep its work running. When the last panel closes, chats still running (a reply under way, a question, plan or approval waiting, background tasks, or on the phone) keep running without a panel, and a notice that stays says how many: **Reopen the panel** opens one in the sidebar with them in its background, **Stop them** stops them as closing used to (waiting questions, plans and approvals declined). Opening a panel any other way also takes them back; the notice goes once none is running. Idle chats close as before. Quitting Obsidian ends every chat process the panel started (Claude and Codex, including background and side chats), the phone server, and the Codex server it started.
- Conversations are saved as they run and can be reopened from history.

## Keyboard shortcuts

| Keys | Action in the panel |
| --- | --- |
| ⌘↩ / Ctrl+Enter | Send; Enter sends instead if this setting is off |
| ↑ in an empty input | Recall sent messages, starting with this chat and then other chats; ↓ moves forward, Esc clears |
| Esc | Stop the reply, or close a side chat; task shutdown may require confirmation |
| ⌥↑ / ⌥↓ | Previous / next prompt |
| ⌘[ / ⌘] (Windows: Alt+← / Alt+→) | Back / forward through visited chats |
| ⌘F | Find; ↩ and ⇧↩ move between matches, Esc closes |
| ⌘-click New chat | Open a new chat tab |
| ⌘-click / ⌥-click a note in the In this chat window (or ⌘↵ / ⌥↵) | Open in a new tab / attach as an `@` mention |
| ⌘-hover a link or file name | Preview the note |
| ⌘-click a diff line | Open the note at that line in a new tab |
| ⌥-click Memo or a reply's sticky-note button | Save a bookmark without opening the form |

On Windows, use Ctrl for ⌘ and Alt for ⌥ unless a different shortcut is listed.

## Your data

- **Claude conversations** are stored outside the vault in Claude Code's session files under `~/.claude/projects/`. Deleting a Claude chat from history deletes its session file.
- **Codex conversations** remain in Codex's store under `~/.codex/`. The plugin reads them through the app-server. Removing a chat from panel history does not delete its Codex conversation.
- **Chat records** are stored in `<vault>/.obsidian/plugins/vault-claude/chats.json`. They include titles, engine choices, pins, drafts, attached notes, links, projects, summaries, checkbox ticks and other chat-related state.
- **Settings and input history** are stored in `data.json` in the same folder. Input history keeps up to 100 sent messages of up to 1,000 characters each. Deleting a chat does not clear that history. Turn off **Remember sent messages for ↑**, or use its **Clear** button, to control it.
- Both data files use temporary-file replacement and a backup. The plugin also keeps daily copies for the last seven days in its `backups/` folder. Obsidian Sync may sync these files with plugin settings.
- The **diagnostic log** contains process activity, errors, session IDs and paths. It does not deliberately log conversations, but errors can contain command text or process output. Read it before sharing it. Locations: `~/Library/Logs/vault-claude.log` on macOS, `%LOCALAPPDATA%\vault-claude\vault-claude.log` on Windows, and `~/.local/state/vault-claude/vault-claude.log` on Linux.

Messages and context are sent through Claude Code or Codex. If Zotero integration is enabled, the plugin also reads Zotero's local API on your computer, and **Add to Zotero** on a reference card writes one item through Zotero's local connector. It does not contact Gmail directly.

## Settings

| Setting | Purpose |
| --- | --- |
| New chats run on | Default chat engine: Claude Code or Codex |
| Small jobs run on | Engine for selection edits, summaries, memo and frontmatter suggestions, project context and chat suggestions |
| **Claude Code** | |
| Claude Code executable | Path to `claude`; detected when empty |
| Default model, effort | Starting model and effort for Claude chats; use the engine's default or its configured model |
| Model for small jobs | Claude model for helper features; default Sonnet |
| **Codex** | |
| Codex executable | Path to `codex`; detected when empty |
| Default Codex model, effort | Starting values; empty uses Codex's defaults |
| Default Codex permissions | Starting permission level; default Default |
| Codex model for small jobs | Helper model; defaults to the chat model |
| **Chats** | |
| Attach the open note to new chats | Attach automatically; off by default |
| Files left out of a chat's notes | Exclusion patterns; default `*attachments/*` |
| Side chat knows | Whole conversation or only the question; default whole conversation |
| Remember sent messages for ↑ | Keep input history; enabled by default, with Clear to erase it |
| History includes all vault sessions | Include Claude sessions started outside the panel |
| Rebuild connections | Recover chat-to-note links from saved conversations |
| Notify when Claude finishes, Notify after | Enable notifications and set the duration threshold; defaults on and 30 seconds |
| **Scratch chat** | |
| Scratch chat | Enable the standing scratch conversation; on by default |
| Scratch chat starts over after | Idle timeout from one hour to one week; default 24 hours |
| Scratch chat model | Engine and model; defaults to the small-job choice |
| **Permissions** | |
| Default permissions | Starting permission mode for Claude chats (Claude Code section) |
| Offer bypass permissions | Show Claude Bypass and Codex Full access options |
| Deny rules | Claude tool restrictions; in Codex, restrictions on commands sent to the panel for approval |
| **Zotero** | |
| Zotero integration | Zotero items in the @ picker, Zotero and PDF buttons and thumbnails on reference cards, and cards for cited works in Zotero; needs Zotero installed and running; off by default |
| **Gmail** | |
| Gmail features | Enable message/draft cards, email todos, note-to-draft requests and thread lists; on by default |
| Todo note | Destination note and section; defaults `Todo.md` and Open |
| **Saved notes** | |
| Folder for saved chats | Destination for saved chats and summaries; default `Agent chats` |
| Folder for memos | Destination for memos and their table; default `Agent chats/Memos` |
| **Appearance and input** | |
| Tool calls | Show a summary, individual lines, or hide them |
| Panel side margins | Horizontal margins from 8 to 96 px |
| Open one Vault Compay panel at start | When Obsidian starts, close the Vault Compay tabs left from the last session but one (the sidebar panel, else the first); on by default |
| Send with ⌘+Enter (Ctrl+Enter on Windows) | Use the modifier shortcut instead of Enter |
| **Phone access** | |
| Claude Code on the phone | Enable Remote Control for Claude chats and new vault sessions; off by default |
| Name in the Claude app | Vault name shown in the Claude app |
| Codex on the phone | Remote control of Codex's shared app-server; off by default |
| Keep Codex chats out of Codex's own lists | Archive chats while not open in the panel; off by default |
| **Advanced** | |
| Extra PATH entries | Additional directories for commands run by the engine |

## Commands

Use Obsidian's command palette and search for **Vault Compay**. Commands include:

- **Chats:** Open chat; New chat; New chat in a new tab; New Codex chat; Chat history; Rename chat; Branch this chat into a new tab; Save chat as note; Save chat summary as note; Run this message in a new chat, report back.
- **Navigation:** Find in chat; Show references in this chat; Go to previous / next message you sent; List messages you sent; Focus chat input; Move chat to the main area, or back to the sidebar; Open chat in a new window.
- **Notes and messages:** Ask Claude about selection; Edit selection with Claude; Send this note to Claude as a prompt; Suggest frontmatter updates for this note; Draft an email from this note…; Quote the selected chat text in your next message; Write this message in a note; Send the draft note.
- **Memos and projects:** Continue from this memo; Open connections; Open project…; Create project…; Write every project's context anew; Rebuild connections from chat files.
- **Scratch and suggestions:** Open scratch chat; Clear scratch chat; Pick up where you left off; Pick up where you left off: show ignored chats again.
- **Runtime and recovery:** Open side chat; Stop Claude; Show background tasks; Toggle fast mode; Toggle Claude Code on the phone; Toggle Codex on the phone; Rebuild chat list from sessions.

## Limitations

- Claude chats use separate local processes. Codex chats use individual processes or a shared app-server, depending on phone access. Keeping many chats active can use substantial memory.
- Codex cannot copy or move a conversation starting from a selected message, and removing Codex turns has no Undo.
- Background tasks end when Obsidian quits, or when you stop the chats the last panel left running.
- Claude chats enabled for phone access appear as separate sessions in the Claude app.
- Tested on macOS. Windows support is implemented but has not been tested on a Windows machine; Linux is untested.

## Troubleshooting

- **Blank panel after an update:** close the panel tab and reopen it from the ribbon.
- **Codex not found:** install and sign in to the CLI, or set **Codex executable** in plugin settings.
- **Claude Code executable not found:** use the native installer, or set **Claude Code executable** in plugin settings.
- **Claude Code / SDK version notice:** update Claude Code with `claude update`, or update the plugin if Claude Code is newer than its bundled SDK expects.
- **Windows npm installation:** replace `claude.cmd` with the native installation that provides `claude.exe`.
- **Failed to authenticate:** for Claude, run `claude` in a terminal and use `/login`. For Codex, run `codex` and sign in. Retry after signing in.
- **Codex chat held by another app:** quit the ChatGPT desktop app (closing the chat there is not enough), then try again. Your unsent message stays in the panel.
- **Phone says another Codex session is using the chat:** the ChatGPT desktop app was started before the panel's shared app-server. Quit ChatGPT, or start it after Obsidian.
- **Other errors:** include the diagnostic log in a bug report after checking it for private paths and error details.

## Alternatives

Other Obsidian chat plugins include [Claudian](https://github.com/YishenTu/claudian), [Copilot for Obsidian](https://github.com/logancyang/obsidian-copilot) and [Agent Client](https://github.com/RAIT-09/obsidian-agent-client).

## License

[MIT](LICENSE), for this plugin's own code. The released `main.js` also bundles Anthropic's [Claude Agent SDK](https://github.com/anthropics/claude-agent-sdk-typescript), which is not covered by the MIT licence: it remains © Anthropic PBC, under [Anthropic's terms](https://code.claude.com/docs/en/legal-and-compliance).

Some workarounds for running the Agent SDK inside Obsidian follow [Claudian](https://github.com/YishenTu/claudian) (MIT).
