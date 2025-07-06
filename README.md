# Easyeffects-IRS-profiles-database
## A community maintained database of IRS+easyeffects profiles files, to be used with the convolver

On this repository, you'll find a database with IRS (Impulse Response Sound) files, and easyeffects profiles.

This project is based heavily on the work of https://github.com/shuhaowu/linux-thinkpad-speaker-improvements

I thank them a lot, as it fixed my audio, and now I want to expand on it.

The IRS files were made from Windows/macOS, with the goal in mind of capturing the way those OS do the audio. by using the convolver, with an IRS file, you can replicate the sound that you had with Windows/macOS, on your Linux system.

This project is meant to be a collab with the Linux community, so you don't have a RAW, flat audio output, by having users uploading their own IRS + Easyeffects profiles.

Just create a new folder, and upload your configuration files. the more, the merrier.

## How to use:

Download the files matching your laptop/PC model, and place the json on `~/.config/easyeffects/output`, and the `.irs` on `~/.config/easyeffects/irs`

Open EasyEffects, and load the profile for your laptop/PC.
Notice the audio quality improving.

For fine tuning, check that the convolver is using the right `.irs` file.

## Contributing:

I would love anyone on the Linux community, to contribute, and help me grow this repo, into a full database. That way, new Linux users won't need to profile their Windows audio, and just enjoy. 

Of course, if the .irs file doesn't exist, you can create a new one.

# Creating a new .irs to contribute to this database:

## How to profile Windows audio and get the .irs based on your device:
We need to figure out how the filtering is done with Dolby on Windows. To do this, we can simply play an impulse audio file, and measure the output audio, which is the impulse response and we can use it in a convolution filter. Step by step:

1. Install `Audacity` and `VLC` on Windows. If you don't have Windows, you can
   try installing [Windows to go](https://www.pcmag.com/how-to/how-to-run-windows-10-from-a-usb-drive).
2. Enable all Dolby effects using the Dolby app (`Dolby Audio Premium` at the time of writing this). Convince yourself that it is working by playing some music and turning the Dolby effects on and off.
3. Open `Audacity`, select `Edit` -> `Preferences` and then select the `Audio Settings` on the left tree.
4. Under `Interface` -> `Host`, select `Windows WASAPI`
5. Under `Playback` -> `Device`, select the speakers (`Speaker (Realtek(R) Audio)` for me).
6. Under `Recording` -> `Device`, select the speakers loopback (`Speaker (Realtek(R) Audio) (loopback)` for me).
7. Under `Quality`, make sure the `Project Sample Rate` and `Default Sample Rate` is `48000Hz`.
8. I selected 24-bit for the `Default Sample Format`. 32 bit float might be fine as well.
9. Click `OK` to save the settings.
10. Download the impulse WAV file from either [the original source](https://freesound.org/people/unfa/sounds/205620/) or [a mirror of it in this repo](impulse48khz-2sec.wav).
11. Open the WAV file with VLC. Pause the playback.
12. Go to `Tools` -> `Preferences` in VLC. On the bottom left, it says `Show settings` and there are two radio boxes, `Simple` and `All`. For me, `Simple` is selected, click `All` to get a more detailed preferences menu.
13. On the left tree, go to `Audio` -> `Output modules`.
14. On the right side, the `Audio output module` should be `Windows Multimedia Device output` and the `Media role` should be set to `Video`. I also tried a `Media role` of `Music`, but it made no difference in the impulse response profile on my machine.
15. Click `Save` to save the settings.
16. Go back to Audacity, click the Record button (big circle at the top). The recording might be stuck (no waveform shows on screen), which is OK.
17. Click play in VLC. This should cause the recording to show a waveform in audacity.
18. Stop the recording in Audacity and stop playback in VLC (if necessary).
19. Zoom into the peak of the wavform and see something like the following. You have to zoom in quite far. In the screenshot below, that's a range of abouy 4ms.

<img src="./ImpulseResponseScreenshot.PNG" />

20. Select the audio clip centered around the maximum of the waveform, as shown in the above screenshot. Then go to `File` -> `Export Selected Audio`. Save the file as a WAV file.
21. Rename the resulting file's extension from `.wav` to `.irs`. Keep this file around.

## Importing the .irs file into Easyeffects:

0. Ensure no external speakers are plugged in and this is for the internal
   speakers only. Start playing some music.
1. `sudo apt intall pulseeffects`
2. On the left hand side, activate `Convolver`, on the right panels. There's a
   button that looks like a wave form above `Stereo Width`. Click it.
3. `Import Pulse` and select the desired `.irs` file. Click `Apply`.
4. On the left panel, use the arrow buttons of the `Limiter` row and move it
   below the `Convolver`. Activate that as well.
   - Without doing this, the convolution may cause the audio to clip and you
     may here artifacts due to that.
5. We need to setup a preset so this setup can be turned off when headphones
   are plugged in. On the top right corner, there's a button to set a preset.
   Click on it and a small popup will show with a textbox. Type `Laptop
   Speaker` into it. Press the `+` button. Then press the save button (should
   be the left-most icon button in the row of 3 buttons).
6. Press the middle icon button that looks like an refresh button. This will
   cause this preset to be loaded when the speakers are used.
7. Plug headphones in. Create a new preset called `Headphones` with the top
   right buttons. Switch to the new preset by clicking `Load` next to it.
8. Disable both the `Convolver` and `Limiter` filter.
9. Press save (left icon button) to save to the filter pipeline (which should
   have no filters) to the pipeline.
10. Press the middle icon button to instruct PulseEffects to automatically load
    this preset when headphones are plugged in.
11. Click the hamburger menu at the top right of the screen. Go to `General`
    and click `Start Service at Login`. This makes PulseEffects run on boot.
    - I have seen that I have to reload the filters in PulseEffects after a
      reboot for them to take effect.
12. Optionally could set the `Priority Type` to be `Real Time`. 

Your screen should look something like this (note I edited the Output gain of
the Convolver filter, but that's up to you):

<img src="./PulseEffects.png" />

You can test the headphone/laptop speaker switching. My observation is that it
takes about 1-3 seconds for PulseEffects to switch between the presets.
I recommend to add a limiter after the convolver or other effects, so it doesn't sound saturated/distorted. of course, adjust the gain of the effects, and ensure it doesn't peak beyond 0 dB. 
It is recommended to do this on a silent room, with speakers at 70% volume, so you can notice the small differences. 
