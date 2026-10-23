# NIDAN offline-first runtime on Windows

NIDAN does not require Bhashini or Gemini keys for basic intake, records, Unicode typing, browser speech, browser text-to-speech, uploads, and explicit fallbacks. The server exposes optional local adapters when the corresponding runtime is installed.

## Tesseract OCR

Install Tesseract and the language packs available for your machine. The exact installer and language-pack set depends on the Windows distribution:

```cmd
winget install UB-Mannheim.TesseractOCR
where tesseract
tesseract --list-langs
```

Set an optional path if `tesseract` is not on PATH:

```cmd
set TESSERACT_BIN=C:\Program Files\Tesseract-OCR\tesseract.exe
```

NIDAN accepts PNG, JPEG, WEBP, TIFF, and BMP for local OCR. PDF files remain stored for staff review; use a PDF-to-image preprocessing step if needed.

## Whisper-compatible local ASR

Install Python 3.10+ and the Whisper CLI. The first transcription downloads the selected local model, so plan for disk space and download time:

```cmd
py -m pip install --upgrade openai-whisper
where whisper
whisper --model small --language Hindi --task transcribe sample.wav
```

Set the command path if needed:

```cmd
set WHISPER_BIN=whisper
```

NIDAN sends the selected language code's language name to Whisper. Browser speech remains available when the local model is not installed.

## Piper or browser/OS TTS

The browser UI uses `window.speechSynthesis` for instructions when the selected OS/browser voice exists. For server-side local TTS, install Piper and download a voice model that you have permission to use:

```cmd
py -m pip install piper-tts
where piper
set PIPER_BIN=piper
set PIPER_MODEL=C:\models\voice.onnx
```

If Piper or a matching locale voice is unavailable, NIDAN reports that state and does not substitute English silently.

## Local transliteration

NIDAN includes a deterministic heuristic transliteration fallback for several Devanagari-family languages and reports unsupported for scripts without a bundled rule. For production-quality transliteration, install and integrate a reviewed local Indic transliteration model/library, then keep the original patient text alongside the transliteration.

## Verify offline capability status

Start NIDAN and query the public tRPC `offline.status` procedure through the application. The status reports whether Tesseract, Whisper, or Piper is detected. Provider statuses remain separate: local success is labeled `local-tesseract`, `local-whisper`, or browser/OS TTS, never as Bhashini success.
