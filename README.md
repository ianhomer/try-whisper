# Try Whisper

Try voice recognition with whisper.

## [OpenAI whisper](https://github.com/openai/whisper)

### Install

    pip install -U openai-whisper
    brew install ffmpeg
    pip install setuptools-rust

### Voice to Text

    whisper ./sample/the-time-machione/the-time-machine.m4a --model small.en

Should return something like

```txt
[00:00.000 --> 00:07.200]  The time traveller, for it will be convenient to speak of him, was expounding a recondite
[00:07.200 --> 00:13.520]  matter to us. His pale grey eyes shone and twinkled, and his usually pale face was flushed
[00:13.520 --> 00:19.800]  and animated. The fire burnt brightly and the soft radiance of the incandescent lights
[00:19.800 --> 00:24.320]  in the lilies of silver caught the bubbles that flashed and passed in our glasses.
```

Run time

```txt
Executed in   15.20 secs    fish           external
   usr time   72.82 secs    0.09 millis   72.82 secs
   sys time   29.55 secs    1.64 millis   29.55 secs
```

## [whisper.cpp](https://github.com/ggerganov/whisper.cpp)

    cd ..
    git clone https://github.com/ggerganov/whisper.cpp.git
    bash ./models/download-ggml-model.sh small.en
    make
    ./main -f ../../my/try-whisper/samples/the-time-machine/the-time-machine.wav -m models/ggml-small.en.bin

Should return something like

```txt
[00:00:00.000 --> 00:00:08.500]   The time traveller, for it will be convenient to speak of him, was expounding a recondite matter to us.
[00:00:08.500 --> 00:00:14.500]   His pale grey eyes shone and twinkled and his usually pale face was flushed and animated.
[00:00:14.500 --> 00:00:24.500]   The fire burnt brightly and the soft radiance of the incandescent lights in the lilies of silver caught the bubbles that flashed and passed in our glasses.
[00:00:24.500 --> 00:00:34.500]   [BLANK_AUDIO]
```

Note this has split on sentence boundaries.

Run time 10x faster than OpenAI version

```txt
Executed in    1.99 secs      fish           external
   usr time  404.44 millis    0.08 millis  404.36 millis
   sys time  242.85 millis    1.59 millis  241.25 millis
```

## ffmpeg cheatsheet

Convert audio to 16-bit wav which whisper.cpp requires

    ffmpeg -i the-time-machine.m4a -acodec pcm_s16le -ar 16000 the-time-machine.wav
