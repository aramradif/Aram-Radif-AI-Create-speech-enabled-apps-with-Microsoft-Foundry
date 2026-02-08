# Aram Radif AI-Create-speech-enabled-apps-with-Microsoft-Foundry

A speech-enabled Python application using Azure AI Speech (Microsoft Foundry)

Azure Speech SDK – Speaking Clock App

A speech-enabled Python application using Azure AI Speech (Microsoft Foundry) that demonstrates:

Speech to Text (audio → text)

Text to Speech (text → audio)

SSML-based voice control

Azure Speech SDK best practices

📁 Project Structure
azure-speech-speaking-clock/
│
├── audio/
│   └── time.wav                  # Sample audio input
│
├── src/
│   └── speaking_clock.py         # Main application logic
│
├── .env.example                  # Environment variable template
├── requirements.txt              # Python dependencies
├── README.md                     # Project documentation
├── .gitignore                    # Ignore secrets & virtual env
└── LICENSE                       # Optional (MIT recommended)

📄 README.md
# Azure Speech SDK – Speaking Clock App

This project demonstrates how to build a speech-enabled application using **Azure AI Speech (Microsoft Foundry)**.

The application:
- Transcribes speech from an audio file (Speech-to-Text)
- Generates spoken responses (Text-to-Speech)
- Uses SSML for voice customization
- Saves synthesized speech to an audio file

---

## 🚀 Features

- Azure Speech SDK integration (Python)
- Speech recognition from audio files
- Speech synthesis with neural voices
- SSML-based speech control
- Cloud-shell compatible (no mic/speaker required)

---

## 🧰 Tech Stack

- Python 3.9+
- Azure AI Speech SDK
- Azure Speech Resource
- SSML (Speech Synthesis Markup Language)

---

## 🔐 Prerequisites

- Azure subscription
- Azure AI Speech resource
- Python installed locally or Azure Cloud Shell

---

## 📦 Installation

### 1. Clone the repository

```bash
git clone https://github.com/your-username/azure-speech-speaking-clock.git
cd azure-speech-speaking-clock

2. Create virtual environment
python -m venv venv
source venv/bin/activate   # Linux/Mac
venv\\Scripts\\activate    # Windows

3. Install dependencies
pip install -r requirements.txt

⚙️ Configuration

Create a .env file using the template:

cp .env.example .env


Update with your Azure Speech credentials:

SPEECH_KEY=your_azure_speech_key
SPEECH_REGION=eastus

▶️ Run the App
python src/speaking_clock.py


Output:

Recognized speech printed to console

Synthesized audio saved as output.wav

🗣️ SSML Example

This project uses SSML to control voice tone and pronunciation:

<speak version="1.0" xmlns="http://www.w3.org/2001/10/synthesis">
  <voice name="en-GB-LibbyNeural">
    <mstts:express-as style="cheerful">
      Time to end this lab!
    </mstts:express-as>
  </voice>
</speak>

🎧 Using Microphone & Speaker (Optional)

If running locally with audio hardware, update AudioConfig:

speech_sdk.AudioConfig(use_default_microphone=True)
speech_sdk.audio.AudioOutputConfig(use_default_speaker=True)

🧹 Cleanup

After testing, delete your Azure Speech resource to avoid costs.

📚 References

Azure Speech SDK Documentation

Speech-to-Text API

Text-to-Speech API

SSML Reference

📄 License

MIT License


---

## 📄 speaking_clock.py (Core App)

```python
import os
import datetime
from dotenv import load_dotenv
import azure.cognitiveservices.speech as speech_sdk

load_dotenv()

speech_key = os.getenv("SPEECH_KEY")
speech_region = os.getenv("SPEECH_REGION")

def main():
    speech_config = speech_sdk.SpeechConfig(speech_key, speech_region)
    print("Using speech service in:", speech_config.region)

    command = transcribe_command(speech_config)
    if "time" in command.lower():
        tell_time(speech_config)

def transcribe_command(speech_config):
    audio_config = speech_sdk.AudioConfig(filename="audio/time.wav")
    recognizer = speech_sdk.SpeechRecognizer(speech_config, audio_config)

    print("Listening...")
    result = recognizer.recognize_once_async().get()

    if result.reason == speech_sdk.ResultReason.RecognizedSpeech:
        print("Recognized:", result.text)
        return result.text
    else:
        print("Recognition failed:", result.reason)
        return ""

def tell_time(speech_config):
    now = datetime.datetime.now()
    response_text = f"The time is {now.hour}:{now.minute:02d}"

    output_file = "output.wav"
    speech_config.speech_synthesis_voice_name = "en-GB-RyanNeural"
    audio_config = speech_sdk.audio.AudioConfig(filename=output_file)
    synthesizer = speech_sdk.SpeechSynthesizer(speech_config, audio_config)

    ssml = f"""
    <speak version='1.0' xmlns='http://www.w3.org/2001/10/synthesis'>
        <voice name='en-GB-LibbyNeural'>
            {response_text}
            <break strength='weak'/>
            Time to end this lab!
        </voice>
    </speak>
    """

    result = synthesizer.speak_ssml_async(ssml).get()
    if result.reason == speech_sdk.ResultReason.SynthesizingAudioCompleted:
        print("Audio saved as", output_file)

if __name__ == "__main__":
    main()

📄 requirements.txt
azure-cognitiveservices-speech==1.42.0
python-dotenv

📄 .env.example
SPEECH_KEY=your_key_here
SPEECH_REGION=eastus

📄 .gitignore
.env
venv/
__pycache__/
output.wav

--

Aram Radif
