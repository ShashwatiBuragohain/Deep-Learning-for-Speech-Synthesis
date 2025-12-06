# Deep-Learning-for-Speech-Synthesis
Design and implementation of a neural text-to-speech system on the LibriTTS dataset by training Tacotron2, exploring model performance, speech quality, and preparing extensions with neural vocoders for naturalistic speech synthesis.
Welcome to my full speech synthesis pipeline, built completely from scratch — from raw audio all the way to cloned voices.
This project is basically my attempt to build a full Real-Time Voice Cloning stack , learning every piece along the way.

If you're into deep learning or audio stuffs, this repo shows the whole process:

1. Preprocessing everything
2. Training a Speaker Encoder (identifies the who)
3. Training a TTS model (learns the how to speak)
4. Training HiFi-GAN (turns mels → actual audio)
5. Putting it all together for voice cloning
   
Let me break down the whole journey.

## Preprocessing
Before any model learns anything, we need to prepare the data properly.
This is where the cleaning happens: text cleaning, audio trimming, mel-spectrograms, pitch extraction, the whole vibe.
This step Loads dataset (LibriTTS-style), Cleans transcripts, Generates all speech features(Mel-spectrograms, Pitch (using YIN), Energy, Builds phonemes (G2P + eSpeak),Computes normalization stats (mel mean/std), Splits speakers into train/validation and then saves everything neatly into folders

Basically:
Raw audio → structured dataset the models can actually learn from.
This step sets the foundation for the entire pipeline.

<img width="1052" height="822" alt="image" src="https://github.com/user-attachments/assets/c8691fc8-3ddc-4395-bd0c-79b0701289b0" />


 ## Speaker Encoder : “who is speaking?”

This model learns the identity of a speaker. It turns small audio clips into embedding vectors that represent the speaker’s voice.
It help achieve:
* Learns speaker characteristics
* Makes the TTS model multi-speaker
* Enables voice cloning (TTS conditioned on a target speaker embedding)
This model only cares about who, not what is being said.
<img width="973" height="676" alt="image" src="https://github.com/user-attachments/assets/7e696c6c-43c4-4511-9061-c8f605a5f894" />


## TTS Model : turning text → mel spectrograms

Now that we know who is talking, it’s time to teach the model how to talk.
The TTS model takes:
* text (converted to phonemes)
* a speaker embedding and outputs a mel-spectrogram (a visual representation of speech)

This part learns:
* Pronunciation
* Duration of each phoneme
* Pitch patterns
* Energy and speech rhythm
* How each speaker actually sounds when they speak

Depending on the architecture (Tacotron2 / FastSpeech2 style), it predicts durations, pitch, energy, and generates mel frames.
This is basically the "brain" of the TTS pipeline.
<img width="1016" height="342" alt="image" src="https://github.com/user-attachments/assets/4a82d978-185c-404a-a31d-485a199b43d6" />


## HiFi-GAN : giving the model a REAL voice

HiFi-GAN is the vocoder — the model that converts the TTS output (mel-spectrogram) into real-sounding audio.
We can think of it as the final transformation step:
       Mel spectrogram → actual waveform → speech

HiFi-GAN uses a GAN setup with:
* Multi-Period Discriminator (MPD)
* Multi-Scale Discriminator (MSD)
* Feature matching + mel loss

The result was expected to be a high-quality, natural-sounding speech, fast.

## Full Voice Cloning Pipeline
Put everything together:

Text → Phonemes
Phonemes + Speaker Embedding → Mel spectrogram
Mel → Audio waveform (HiFi-GAN)

We were supposed to get speech in that target speaker’s voice. This is how real voice cloning systems work behind the scenes.

This repo is to document the whole grind: the successes, the bugs, the surprises, everything.

Everything up to TTS is working pretty smoothly.
However… HiFi-GAN is currently getting stuck during training.
(Probably a dataloader freeze or GPU memory issue — still debugging.)

So expect updates!
I’ll keep improving this pipeline, fixing the vocoder, and eventually connecting everything into a final inference script/UI.

Stay tuned, and enjoy exploring the whole TTS journey with me:)

##  Citation

```bibtex
@article{jia2018transfer,
  title={Transfer Learning from Speaker Verification to Multispeaker Text-To-Speech Synthesis},
  author={Jia, Ye and Zhang, Yu and Weiss, Ron J and Wang, Quan and Shen, Jonathan and Ren, Fei and Chen, Zhifeng and Nguyen, Patrick and Pang, Ruoming and Moreno, Ignacio Lopez and Wu, Yonghui},
  journal={arXiv preprint arXiv:1806.04558},
  year={2018},
  doi={10.48550/arXiv.1806.04558}
}
```

##  Authors & Acknowledgments

**Built by:**  
[Shashwati Buragohain] & [Ozias Keivom]

This project was completed as part of the **Statistical Signal Processing (EE 520)** course  
at **Indian Institute of Technology Guwahati (IITG)**.

We extend our gratitude to the authors of the original research paper for their inspiring contribution to the field.
