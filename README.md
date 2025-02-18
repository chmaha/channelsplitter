# Batch Audio Channel Splitter

### Prerequisites

Before using this script, ensure you have SoX (Sound eXchange) installed:
  - **Linux**: Install via your package manager using `sudo apt install sox` (Debian/Ubuntu), `sudo dnf in sox` (Fedora) etc.
  - **macOS**: Install using `brew install sox` (Homebrew).
  - **Windows**: Download and install SoX from: http://sox.sourceforge.net/.

Split any number of audio files (wav, flac, aiff or wv) into smaller channel groups via a numeric pattern made up of single digits.

### Grouping Pattern

Examples:
- 1 - a series of mono files
- 2 - a series of stereo files with a remainder mono file as necessary
- 21 - a single stereo for the first two channels, followed by a series of mono files
- 231 - An initial stereo file (channels 1-2), followed by a 3-channel file (channels 3-4-5), followed by a series of mono files

Essentially, if the individual digits of the pattern add up to less than the initial channel count, it will reuse the final digit as much as it can until it needs to create mono remainders. It also checks if files already exist as well as showing messages if the summed digits of the pattern exceed the input channel count or if there is an attempt at a needless split (i.e. using a pattern of 2 for stereo file).

Users can choose between a python script (all OSes) or POSIX shell script (Linux/MacOS).

### Python Usage:
If you don't have the script set as executable or if you're on Windows, run the script using Python:
```sh
python channel-splitter.py <grouping pattern> <file_pattern>
```
On Unix-based systems (MacOS/Linux), if you have the script as executable, you can run it directly:
```sh
chmod +x sample-right.py
./channel-splitter.py <grouping pattern> <file_pattern>
```

### Example 1 (python)

```sh
python channel-splitter.py 2 input.wav
```
creates a series of stereo wav files with any remainder as mono
e.g. 
```
input[1-2].wav
input[3-4].wav
```
### Example 2 (shell script)

```sh
channel-splitter.sh 221 *.aiff
```
creates two initial stereo files followed by a series of mono files all in aiff format.
e.g.
```
mozart[1-2].aiff
mozart[3-4].aiff
mozart[5].aiff
mozart[6].aiff
```
### Example 3 (python)

```sh
python channel-splitter.py 312 *.wv
```
creates a 3-channel file, followed by a mono file, followed by a series of stereo files with mono remainders as required (all in wavpack format).
e.g.
```
bach_violin_sontata[1-2-3].wv
bach_violin_sontata[4].wv
bach_violin_sontata[5-6].wv
bach_violin_sontata[7-8].wv
bach_violin_sontata[9-10].wv
bach_violin_sontata[11].wv
```
