# Easyeffects-IRS-profiles-database
A community maintained database of IRS+easyeffects profiles files, to be used with the convolver

On this repository, you'll find a database with IRS (Impulse Response Sound) files, and easyeffects profiles.

The IRS files were made from Windows/macOS, with the goal in mind of capturing the way those OS do the audio. by using the convolver, with an IRS file, you can replicate the sound that you had with Windows/macOS, on your Linux system.

This project is meant to be a collab with the Linux community, so you don't have a RAW, flat audio output, by having users uploading their own IRS + Easyeffects profiles.

Just create a new folder, and upload your configuration files. the more, the merrier.

## How to use:

Download the files matching your laptop/PC model, and place the json on `~/.config/easyeffects/output`, and the `.irs` on `~/.config/easyeffects/irs`

Open EasyEffects, and load the profile for your laptop/PC.
Notice the audio quality improving.

For fine tuning, check that the convolver is using the right `.irs` file.
