<script lang="ts">
  import { onMount, tick } from "svelte";

  type HistoryEntry = {
    command: string;
    output: string[];
    isHtml?: boolean;
  };

  let history: HistoryEntry[] = [
    {
      command: "",
      output: [
        "Welcome to Nitesh's Terminal.",
        "Type 'help' to see a list of available commands.",
      ],
    },
  ];
  let inputValue = "";
  let terminalRef: HTMLElement;
  let inputRef: HTMLInputElement;

  const commands: Record<string, () => string[]> = {
    help: () => [
      "Available commands:",
      "  help     - Show this help message",
      "  whoami   - Display information about me",
      "  projects - List recent projects",
      "  echo     - Print text back",
      "  clear    - Clear terminal history",
      "  sudo     - Execute a command as superuser",
    ],
    whoami: () => [
      "Nitesh Patel",
      "Software Engineer & Competitive Programmer",
      "Specialties: C++, Kotlin, Svelte, SQLite, PostgreSQL",
    ],
    projects: () => [
      "1. Tree Tracker - Application for tracking trees",
      "2. LEACH_PY - Python implementation of LEACH protocol",
      "3. PocketLog - Portable logging application",
    ],
    clear: () => {
      history = [];
      return [];
    },
  };

  async function handleKeydown(event: KeyboardEvent) {
    if (event.key === "Enter") {
      const currentCommand = inputValue.trim();
      inputValue = ""; // clear input immediately

      if (currentCommand === "") {
        history = [...history, { command: currentCommand, output: [] }];
      } else {
        const args = currentCommand.split(" ");
        const baseCmd = args[0].toLowerCase();

        let output: string[] = [];

        if (baseCmd === "clear") {
          commands.clear();
        } else if (baseCmd === "echo") {
          output = [args.slice(1).join(" ")];
          history = [...history, { command: currentCommand, output }];
        } else if (baseCmd === "sudo") {
          output = ["Nice try, but you don't have superuser privileges!"];
          history = [...history, { command: currentCommand, output }];
        } else if (commands[baseCmd]) {
          output = commands[baseCmd]();
          history = [...history, { command: currentCommand, output }];
        } else {
          output = [`Command not found: ${baseCmd}. Type 'help' for available commands.`];
          history = [...history, { command: currentCommand, output }];
        }
      }

      await tick();
      scrollToBottom();
    }
  }

  function scrollToBottom() {
    if (terminalRef) {
      terminalRef.scrollTop = terminalRef.scrollHeight;
    }
  }

  function focusInput() {
    if (inputRef) {
      inputRef.focus();
    }
  }
</script>

<!-- svelte-ignore a11y-click-events-have-key-events -->
<!-- svelte-ignore a11y-no-static-element-interactions -->
<div class="terminal-container" on:click={focusInput} bind:this={terminalRef}>
  <div class="terminal-header">
    <span class="dot red"></span>
    <span class="dot yellow"></span>
    <span class="dot green"></span>
    <span class="title">guest@portfolio:~</span>
  </div>
  <div class="terminal-body">
    {#each history as entry}
      {#if entry.command !== ""}
        <div class="prompt-line">
          <span class="prompt">guest@portfolio:~$</span>
          <span class="command">{entry.command}</span>
        </div>
      {/if}
      {#each entry.output as line}
        <div class="output-line">{line}</div>
      {/each}
    {/each}

    <div class="prompt-line active">
      <span class="prompt">guest@portfolio:~$</span>
      <input
        type="text"
        bind:value={inputValue}
        bind:this={inputRef}
        on:keydown={handleKeydown}
        autocomplete="off"
        spellcheck="false"
      />
    </div>
  </div>
</div>

<style>
  .terminal-container {
    background-color: #1e1e1e;
    border-radius: 8px;
    overflow: hidden;
    margin: 2rem 0;
    box-shadow: 0 10px 30px rgba(0, 0, 0, 0.2);
    font-family: "Fira Code", "Consolas", monospace;
    font-size: 0.9rem;
    color: #e0e0e0;
    max-height: 400px;
    overflow-y: auto;
    border: 1px solid #333;
  }

  .terminal-header {
    background-color: #2d2d2d;
    padding: 10px 15px;
    display: flex;
    align-items: center;
    position: sticky;
    top: 0;
    z-index: 10;
    border-bottom: 1px solid #111;
  }

  .dot {
    width: 12px;
    height: 12px;
    border-radius: 50%;
    margin-right: 8px;
  }

  .dot.red { background-color: #ff5f56; }
  .dot.yellow { background-color: #ffbd2e; }
  .dot.green { background-color: #27c93f; }

  .title {
    margin-left: auto;
    margin-right: auto;
    color: #888;
    font-size: 0.85rem;
  }

  .terminal-body {
    padding: 15px;
  }

  .prompt-line {
    display: flex;
    margin-bottom: 5px;
  }

  .prompt {
    color: #27c93f;
    margin-right: 10px;
    white-space: nowrap;
  }

  .command {
    color: #e0e0e0;
    word-break: break-all;
  }

  .output-line {
    color: #cccccc;
    margin-bottom: 5px;
    padding-left: 10px;
    white-space: pre-wrap;
  }

  .active input {
    background: transparent;
    border: none;
    color: #e0e0e0;
    font-family: inherit;
    font-size: inherit;
    width: 100%;
    outline: none;
    padding: 0;
    margin: 0;
  }
</style>
