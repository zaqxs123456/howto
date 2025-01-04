# howto: Generate Terminal Commands with LLMs

`howto` is a command-line tool that uses a Large Language Model (LLM) to generate terminal commands based on natural language questions. It simplifies command-line usage by allowing you to express your intent in plain English.

## Features

*   **Natural Language Input:** Ask questions in plain English, like "create a directory named downloads" or "list all files in the current directory with size."
*   **LLM-Powered Generation:** Leverages the power of LLMs to understand your intent and generate the corresponding terminal commands.
*   **Ollama Backend:** Uses Ollama as the default LLM backend, providing a local and efficient solution.
*   **Simple Usage:** Easy to use with a straightforward command-line interface.

## Installation

1.  **Install Ollama:** Follow the installation instructions for Ollama on your operating system: [https://ollama.ai/](https://ollama.ai/)

2.  **Clone the `howto` repository:**

    ```bash
    git clone https://github.com/zaqxs123456/howto.git
    cd howto
    ```

3.  **(Optional but Recommended) Create a virtual environment:**

    ```bash
    python3 -m venv .venv  # Or python -m venv .venv
    source .venv/bin/activate # On Windows: .venv\Scripts\activate
    ```

4.  **Install project dependencies:**

    ```bash
    pip install -r requirements.txt
    ```

5.  **Add `howto` to your PATH (Important):**

    This allows you to run `howto` from any directory in your terminal.

    *   **Linux/macOS:**

        Open your `~/.bashrc`, `~/.zshrc`, or `~/.bash_profile` file (depending on your shell) in a text editor (e.g., `nano ~/.zshrc`). Add the following line at the end of the file, replacing `/path/to/howto` with the actual path to the `howto` directory you cloned:

        ```bash
        export PATH="$PATH:/path/to/howto"
        ```

        For example, if you cloned the repo to `~/projects/howto`, the line would be:

        ```bash
        export PATH="$PATH:~/projects/howto"
        ```

        Save the file and then run `source ~/.zshrc` (or the corresponding command for your shell) to apply the changes.

    *   **Windows:**

        1.  Search for "Environment Variables" in the Windows search bar and select "Edit the system environment variables."
        2.  Click on "Environment Variables..."
        3.  Under "System variables," select "Path" and click "Edit..."
        4.  Click "New" and add the full path to the `howto` directory. For example: `C:\Users\YourUser\projects\howto`.
        5.  Click "OK" on all dialogs to save the changes. You may need to restart your terminal or even your computer for the changes to take effect.

## How it Works

`howto` takes your natural language question as input and sends it to the Ollama API. Ollama processes the input and generates the most likely corresponding terminal command. The command is then printed to the console.

## Usage

```bash
howto [question]
```

**Examples:**

*   Create a directory:

    ```bash
    howto "make a new directory called my_folder"
    ```

    Output:

    ```bash
    mkdir my_folder
    ```

*   List files with sizes:

    ```bash
    howto "show file sizes in the current directory"
    ```

    Output:

    ```bash
    ls -lh
    ```

*   Convert video with specific settings (Complex Example):

    ```bash
    howto "convert this video to 1080p with h265 encoding and add subtitles from subs.srt"
    ```

    Output (Example using `ffmpeg` - the actual output will depend on the LLM):

    ```bash
    ffmpeg -i input.mp4 -vf scale=1920:-1 -c:v libx265 -crf 28 -c:a copy -vf subtitles=subs.srt output.mp4
    ```

*   Check Bluetooth connection (Example with pipe):

    ```bash
    howto "check if my Logitech mouse is connected via Bluetooth"
    ```

    Output (Example using `bluetoothctl` on Linux - will vary by OS):

    ```bash
    bluetoothctl devices | grep "Logitech"
    ```

*   Find largest files and save to file (Complex Example with pipe and redirection):

    ```bash
    howto "find the 10 largest files in this directory and list them in a file called largest_files.txt"
    ```

    Output (Example using `du` and `sort` on Linux/macOS):

    ```bash
    du -a . | sort -rh | head -n 10 > largest_files.txt
    ```
    Or (More portable version)
    ```bash
    find . -type f -print0 | xargs -0 du -h | sort -rh | head -n 10 > largest_files.txt
    ```

*   Change directory:

    ```bash
    howto "go to the documents directory"
    ```

    Output:

    ```bash
    cd Documents
    ```

*   Show current working directory:

    ```bash
    howto "print working directory"
    ```

    Output:

    ```bash
    pwd
    ```

## Configuration

`howto` uses Ollama as its LLM backend. For detailed configuration options regarding Ollama, such as managing models, setting resource limits, and more, please refer to the official Ollama documentation: [https://ollama.ai/](https://ollama.ai/)

`howto` allows you to set a default model that will be used if you don't specify one with the `--model` flag. You can set the default model using the `--default` or `-d` flag:

```bash
howto --default llama3.1  # Sets llama3.1 as the default model
howto -d llama3.1        # Equivalent using the short flag
```

The default model setting is stored in a configuration file `~/.config/howto/config.json`.

To use a specific model for a single command without changing the default:

```bash
howto --model llama2 "create a directory named downloads"
```

If you need to configure Ollama itself (e.g., to use a different model path, set environment variables for GPU usage, or configure networking), consult the Ollama documentation. These settings are external to `howto` and are handled directly by Ollama.

## Limitations

While `howto` can be a helpful tool for generating terminal commands, it's important to be aware of its limitations:

1.  **Potential for Errors:** The generated commands are based on the LLM's understanding of your question, which may not always be accurate. **Always review the generated command carefully before executing it.** Double-check syntax, file paths, and any potentially destructive operations.

2.  **Challenges with Complex Commands:** Long or multi-line commands are more complex for the LLM to generate accurately. These commands are more prone to errors and may require manual adjustments.

3.  **Terminal Limitations vs. LLM Overconfidence:** The terminal has specific capabilities and limitations. LLMs, however, may sometimes generate commands that are not valid or possible within a standard terminal environment. The LLM might hallucinate commands or assume functionalities that don't exist. Be aware that the terminal is not all-powerful, even if the LLM thinks it is.

## Contributing

Contributions are welcome! Please open an issue or submit a pull request.

## License

MIT
