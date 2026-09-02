# Audio Steganography System

A Python/Jupyter implementation of **Least Significant Bit (LSB) audio steganography** — hiding a secret text message inside a WAV audio file so it's imperceptible to the ear, then extracting it back out. Includes a waveform comparison to confirm audio quality is preserved, plus a written investigation of **Echo Hiding** as an alternative technique.

## Contents

| File | Description |
|---|---|
| `exercise 3.2.ipynb` | Main implementation: embeds a secret message into `Ex3_sound5.wav` using LSB encoding, extracts it back, and plots the original vs. stego waveforms. |
| `exercise 3.3.ipynb` | Written research task comparing LSB with **Echo Hiding**, a DSP-based steganography method not covered in the main exercise. |
| `Ex3_sound5.wav` | Original "carrier" audio file. |
| `stego_Ex3_sound5.wav` | Output file — the carrier audio with the secret message embedded. |

## How It Works (exercise 3.2)

1. **Text ↔ bits** — the secret message is converted to a binary string (8 bits per character) and back again.
2. **Embedding** — the carrier WAV is loaded and converted to 16-bit PCM integers. A fixed random seed picks a pseudo-random set of sample positions, and the message bits are written into the least significant bit(s) of those samples (`LSB_COUNT` controls how many bits per sample are used).
3. **Extraction** — using the same seed (so the same sample positions are regenerated), the LSBs are read back and reassembled into the original text.
4. **Verification** — the recovered message is printed and checked against the original, and the stego audio is played back in-notebook.
5. **Visual comparison** — a short window (10 ms) of the original and stego waveforms is plotted side by side to show that the embedding introduces no perceptible change to the signal.

### Key parameters

```python
AUDIO_FILE = 'Ex3_sound5.wav'
STEGO_FILE = 'stego_Ex3_sound5.wav'
SECRET_MESSAGE = 'An eye for an eye makes the whole world blind'
SEED = 42          # reproducible random embedding positions
LSB_COUNT = 2       # bits per sample used for embedding
```

## Echo Hiding Investigation (exercise 3.3)

`exercise 3.3.ipynb` researches **Echo Hiding**, a DSP-based alternative to LSB:

- Instead of modifying bits directly, a faint echo (delayed by ~1–2 ms) is mixed into the signal. A short delay encodes a "0", a longer delay encodes a "1".
- Decoding uses signal analysis (autocorrelation / cepstrum) to detect the echo delay.
- **Strengths:** doesn't alter samples directly, so audio quality is very well preserved; more robust than LSB against resampling/filtering.
- **Weaknesses:** harder to implement, low data rate (only suited to short messages), and can be masked by audio that already has natural reverb.
- **Compared to LSB:** LSB is simple and fast but fragile (easily destroyed by re-encoding); Echo Hiding is more robust and stealthier but slower and more complex to implement correctly.

## Usage

Open `exercise 3.2.ipynb` in Jupyter and run all cells top to bottom. It will:

1. Load `Ex3_sound5.wav`
2. Embed the secret message and write `stego_Ex3_sound5.wav`
3. Extract the message back out and print it for verification
4. Play the stego audio in the notebook
5. Plot the original vs. stego waveforms

`exercise 3.3.ipynb` is a markdown-only research write-up and doesn't need to be executed.

## Skills Demonstrated

- Digital signal processing (DSP) fundamentals
- Binary/text encoding and bit manipulation
- Audio I/O and waveform analysis in Python
- Information hiding / steganography concepts
- Technical research and written comparison of techniques
