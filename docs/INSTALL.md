# Install Flick by Transformaiton.com

This page is the install guide for [Flick by Transformaiton.com](../README.md). It covers the computer requirements, the accounts that can cost money, and the commands that install this repository.

Flick is a skill for a coding assistant. The assistant then builds animations with Remotion on your computer. Install the assistant first, then install Flick from `Transformaiton/flick`.

## 1. Check the computer

Flick’s setup script requires:

- **Node.js 20 or newer.** Check with `node -v`. Downloads: [https://nodejs.org/](https://nodejs.org/).
- **Python 3.9 or newer.** Check with `python3 --version` on macOS and Linux, or `python --version` on Windows. Downloads: [https://www.python.org/downloads/](https://www.python.org/downloads/).
- **A network connection** for the first setup. Setup downloads Remotion, FFmpeg, Whisper, and yt-dlp.
- **Enough disk space** for those tools, a Whisper model on the first transcription, and the rendered videos. A first Remotion render can also download a headless Chrome build of about 100 MB.

On Windows, the project’s own messages suggest:

```text
winget install OpenJS.NodeJS.LTS
winget install Python.Python.3.12
```

On macOS and Linux, install Node.js 20+ and Python 3.9+ with the platform package manager, or from the download pages above. Close and reopen the terminal after either install so the new commands are on your PATH.

## 2. Choose a coding assistant

The assistant is the part that can require an API key or a paid plan. Flick itself has no API key. Current prices, checked on 16 November 2026, are summarized in the [README](../README.md#what-you-need-and-what-it-costs). Confirm them on the vendor’s site before you pay.

| If you want to use | Sign in with | Install Flick with |
| --- | --- | --- |
| Claude Code | A paid Claude plan (Pro, Max, Ultra, Team, or Enterprise), or an Anthropic API key | The Claude Code commands below |
| Codex | A ChatGPT plan, or an OpenAI API key | The Codex command below |
| Cursor, Gemini CLI, OpenCode, Cline, or Grok Build | That product’s account | The general command below |

A pasted script skips speech-to-text. A video file or a public link is transcribed locally with Whisper. That step does not use an OpenAI API key.

## 3. Install this copy

These commands install **this** repository, `Transformaiton/flick`. The original project remains at [github.com/Creatorberry/flick](https://github.com/Creatorberry/flick).

### Claude Code

In Claude Code, run:

```text
/plugin marketplace add Transformaiton/flick
/plugin install flick@flick
```

Start a job with `/flick`.

To receive updates automatically, open `/plugin`, choose **Marketplaces**, select **flick**, and enable auto-update. Claude Code checks when it starts. When it asks you to activate an update, run:

```text
/reload-plugins
```

To update by hand:

```text
/plugin update flick@flick
/reload-plugins
```

### Codex

```text
npx skills add Transformaiton/flick --skill flick --agent codex --global --yes
```

Start a job by asking: `Use $flick to animate this.`

Update later with:

```text
npx skills update
```

### Cursor and other agents

For Cursor, Gemini CLI, OpenCode, Cline, Grok Build, and other compatible agents:

```text
npx skills add Transformaiton/flick --skill flick --global
```

Follow the prompts. Update later with:

```text
npx skills update -g
```

In Cursor you can also open a clone of this repository and ask the agent to follow `skills/flick/SKILL.md`.

### Work from a clone

```text
git clone https://github.com/Transformaiton/flick.git
cd flick
```

The skill the assistant reads is `skills/flick/SKILL.md`. Cloning the repository gives you the guide and the skill files. The assistant still has to be installed and signed in.

## 4. Let the first run finish its setup

The first Flick job creates a workspace and installs the local tools. You can also run that setup yourself from a clone:

```text
node skills/flick/scripts/bootstrap.mjs --project flick-output
```

Bootstrap does four things:

1. It copies the Remotion starter into `flick-output/remotion` when that project is not there yet.
2. It runs `npm install` in that Remotion project. The package manager is npm unless you set `FLICK_PACKAGE_MANAGER`.
3. It upgrades pip for the current user and installs `openai-whisper` and `yt-dlp`.
4. It prints `Flick is ready: <folder>` when those steps succeed.

npm is the default. On Windows the script looks for `npm.cmd`. A pnpm path is supported: the script installs with `--ignore-scripts`, approves the `esbuild` and `ffmpeg-static` builds, then rebuilds those two packages.

When setup succeeds you should have:

```text
flick-output/remotion/package.json
flick-output/brand-assets/
flick-output/scenes/
flick-output/flick.config.json
```

The next video transcription downloads the Whisper `base` model. That download happens once, on the first transcription, and it stays on the machine.

## 5. Confirm it is ready

1. `node -v` prints v20 or higher.
2. `python3 --version` (or `python --version` on Windows) prints 3.9 or higher.
3. The assistant accepts `/flick` or `$flick`, depending on the product you installed.
4. After bootstrap, `flick-output/remotion/node_modules` exists.
5. Ask the assistant to animate a short pasted script, using the [usage guide](USAGE.md). A script is the smallest test, because it skips the video download and the Whisper model.

## Updates

Use the update command for the assistant you installed, in [section 3](#3-install-this-copy). Those commands refresh the skill from the source you added. Install from `Transformaiton/flick` when you want this copy.

## Install troubleshooting

**`Flick needs Node.js 20+.`** The running Node is older than 20. Install Node.js 20 LTS or newer and open a new terminal. On Windows: `winget install OpenJS.NodeJS.LTS`.

**`Flick needs Python 3.9+.`** Python is missing, or the command on your PATH is older than 3.9. On Windows: `winget install Python.Python.3.12`. On macOS and Linux, install Python 3.9+ with the platform package manager. Open a new terminal and run the version check again.

**`Python 3 was not found. Run Flick bootstrap after installing Python 3.`** Same fix as the line above. Bootstrap has to succeed before transcription.

**`FFmpeg was not found. Run Flick bootstrap first.`** The Remotion project and its `ffmpeg-static` package are not installed yet. Run the bootstrap command in section 4 from the clone, or start a Flick job and let the assistant run it.

**`Whisper is not installed. Run Flick bootstrap first.`** The `openai-whisper` pip package is missing. Run bootstrap. The package name does not mean you need an OpenAI API key.

**`Refusing to overwrite non-empty directory.`** Setup will not replace a folder that already has files. Pick a new folder name, or remove the old `flick-output` folder after you have copied out anything you want to keep.

**npm or pip fails halfway through.** Check the network connection and run bootstrap again. npm reuses packages it already downloaded.

**`/plugin marketplace add` cannot find the repository.** Confirm the GitHub path is `Transformaiton/flick`, with that spelling, and that you are online.

**Claude Code is charging the API while you have a subscription.** If `ANTHROPIC_API_KEY` is set in the environment, Claude Code uses that key. Unset it for the session when you want the subscription’s included usage, then sign in again with `/login`.

**The skill replies with a link to the original project.** Two prompts in the upstream skill text still name the original website. This copy leaves those prompts in place. The customer guide you are reading is the Transformaiton.com documentation.
