# asdf-whispercpp

[`whisper.cpp`](https://github.com/ggml-org/whisper.cpp) plugin for the [asdf version manager](https://asdf-vm.com) and [mise](https://mise.jdx.dev).

This plugin compiles `whisper.cpp` from source to ensure maximum performance and compatibility with your hardware (AVX, Metal, CUDA, etc.).

## Prerequisites

Since this plugin builds from source, you need a working C++ compiler and build tools installed on your system.

- **Common**: `git`, `curl`, `make`, `cmake`
- **macOS**: Xcode Command Line Tools (`xcode-select --install`)
- **Linux**: `gcc`, `g++` (or `clang`), `make`, `cmake` (e.g., `build-essential` and `cmake` on Debian/Ubuntu)
- **Optional**: `ffmpeg` (highly recommended for audio conversion)

## Install

### using asdf

```bash
asdf plugin add whispercpp https://github.com/workmangeneral/asdf-whispercpp.git
asdf install whispercpp latest
```

### using mise

```bash
mise plugin install whispercpp https://github.com/workmangeneral/asdf-whispercpp.git
mise install whispercpp@latest
```

## Configuration

You can customize the build process using environment variables.

### Custom CMake Flags
Use `WHISPER_CPP_CMAKE_FLAGS` to pass arguments to CMake.

**Example: Enable NVIDIA CUDA support**
```bash
export WHISPER_CPP_CMAKE_FLAGS="-DWHISPER_CUBLAS=1"
asdf install whispercpp latest
```

**Example: Enable CoreML (macOS)**
```bash
export WHISPER_CPP_CMAKE_FLAGS="-DWHISPER_COREML=1"
asdf install whispercpp latest
```

## Usage

This plugin installs the main `whisper-cli` executable (aliased from `main` if necessary) and several helper tools.

### Check installation

Whisper.cpp binaries do not consistently support a `--version` flag. You can verify the installation by checking the help output:

```bash
whisper-cli --help
```

### Download Models

Whisper.cpp requires model files to function. A helper script is provided:

```bash
# Download base English model
whisper-download-model base.en

# Download large model
whisper-download-model large
```

Models are downloaded to the current working directory (by default behavior of the script).

### Transcribe Audio

```bash
# Transcribe a WAV file (must be 16kHz 16-bit mono)
whisper-cli -m models/ggml-base.en.bin -f my_audio.wav
```

## Binaries Included

The plugin builds and installs the following tools by default:

- `whisper-cli` (main transcription tool)
- `whisper-quantize` (model quantization tool)
- `whisper-stream` (real-time audio transcription)
- `whisper-talk` (talk with a GPT-2 bot)
- `whisper-talk-llama` (talk with a LLaMA bot)
- `whisper-server` (HTTP inference server)
- `whisper-bench` (benchmark tool)
- `whisper-command` (voice commands example)
- `whisper-download-model` (helper script)

## License

MIT
