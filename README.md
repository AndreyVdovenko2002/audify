# Audify.js
Audify.js - Play/Stream/Record PCM audio data &amp; Encode/Decode Opus to PCM audio data

## Features
* Encode 16-bit integer PCM or floating point PCM to Opus packet using C++ Opus library.
* Decode Opus packets to 16-bit integer PCM or floating point PCM using C++ Opus library.
* Complete API for realtime audio input/output across Linux (native ALSA, JACK, PulseAudio and OSS), Macintosh OS X (CoreAudio and JACK), and Windows (DirectSound, ASIO and WASAPI) operating systems using C++ RtAudio library.

## Example
#### Opus Encode & Decode
```javascript
const { OpusEncoder, OpusDecoder, OpusApplication } = require("audify");

// Init encoder and decoder
// Sample rate is 48kHz and the amount of channels is 2
// The opus coding mode is optimized for audio
const encoder = new OpusEncoder(48000, 2, OpusApplication.OPUS_APPLICATION_AUDIO);
const decoder = new OpusDecoder(48000, 2);

const frameSize = 1920; // 40ms
const buffer = ...

// Encode and then decode
var encoded = encoder.encode(buffer, frameSize);
var decoded = decoder.decode(encoded, frameSize);
```

#### Record audio and play it back realtime
```javascript
const { RtAudio, RtAudioFormat } = require("audify");

// Init RtAudio instance using default sound API
const rtAudio = new RtAudio(/* Insert here specific API if needed */);

// Open the input/output stream
rtAudio.openStream(
	{ deviceId: 0, // Output device id (Get all devices using `getDevices`)
	  nChannels: 1, // Number of channels
	  firstChannel: 0 // First channel index on device (default = 0).
	},
	{ deviceId: 0, // Input device id (Get all devices using `getDevices`)
	  nChannels: 1, // Number of channels
	  firstChannel: 0 // First channel index on device (default = 0).
	},
	RtAudioFormat.RTAUDIO_SINT16, // PCM Format - Signed 16-bit integer
	48000, // Sampling rate is 48kHz
	1920, // Frame size is 1920 (40ms)
	"MyStream", // The name of the stream (used for JACK Api)
	pcm => rtAudio.write(pcm) // Input callback function, write every input pcm data to the output buffer
);

// Start the stream
rtAudio.start();
```

## Documentation
Full documentation available [here](audify.github.io).

## Legal
This project is licensed under the MIT license.