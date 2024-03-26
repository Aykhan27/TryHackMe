# c4ptur3-th3-fl4g 🚀

![Capture Flag](capture-the-flag.png)

A beginner level CTF challenge

## Overview

The "c4ptur3-th3-fl4g" project is a simulated cybersecurity challenge designed to test and improve your penetration testing skills. Participants are tasked with proving their mettle as the most elite hacker in the solar system by overcoming a series of tasks. The scenario involves a hacker boasting about their skills in a bar, leading to a challenge by a group of Bounty Hunters to prove these claims. The challenge encompasses various stages, including Translation & Shifting, Spectrograms, Steganography, and Security through Obscurity.

## Usage Guide

### Task 1: Translation & Shifting

Translate, shift, and decode the following; Answers are all case-sensitive.

1. c4n y0u c4p7u23 7h3 f149?

    ```can you capture the flag```

2. 01101100 01100101 01110100 01110011 00100000 01110100 01110010 01111001 00100000 01110011 01101111 01101101 01100101 00100000 01100010 01101001 01101110 01100001 01110010 01111001 00100000 01101111 01110101 01110100 00100001
```bash
    echo '--hash--' | xargs -n1 | while read binary; do printf "\x$(printf "%x" "$((2#$binary))")"; done 
```
3. MJQXGZJTGIQGS4ZAON2XAZLSEBRW63LNN5XCA2LOEBBVIRRHOM======
```bash
    echo "--hash--" | base32 -d 
```
4. RWFjaCBCYXNlNjQgZGlnaXQgcmVwcmVzZW50cyBleGFjdGx5IDYgYml0cyBvZiBkYXRhLg==
```bash
    echo "--bash--" | base64 -d
```
5. 68 65 78 61 64 65 63 69 6d 61 6c 20 6f 72 20 62 61 73 65 31 36 3f
```bash
    echo "--hash---" | xxd -r -p
```
6. Ebgngr zr 13 cynprf!
```bash
    echo '--hash--' | tr 'A-Za-z' 'N-ZA-Mn-za-m'
```
7. *@F DA:? >6 C:89E C@F?5 323J C:89E C@F?5 Wcf E:>6DX
```bash
    echo '--hash--' | tr 'A-Za-z' 'R-ZA-Qr-za-q'
```
8. - . .-.. . -.-. --- -- -- ..- -. .. -.-. .- - .. --- -.
. -. -.-. --- -.. .. -. --.
```bash
    sudo apt-get install morse
    echo '--hash--' | morse -d -
```
9. 85 110 112 97 99 107 32 116 104 105 115 32 66 67 68
```bash
    decimal_numbers="--hash--"
text=""
for num in $decimal_numbers; do
    text+=$(printf "\\$(printf '%o' $num)")
done
printf "$text"

```
10. LS0tLS0gLi0tLS0gLi0tLS0gLS0tLS0g...LS0gLS0tLS0gLi0tLS0gLS0tLS0gLi0tLS0=
```bash
    echo "--hash--" | base64 -d | morse -e - | xxd -b -c 1 | awk '{print $2}' | tr -d '\n' | sed 's/ //g' | tr '!-~' 'P-~!-O'
```

### Task 2: Spectrograms

A spectrogram is a visual representation of the spectrum of frequencies of a signal as it varies with time. When applied to an audio signal, spectrograms are sometimes called sonographs, voiceprints, or voicegrams. When the data is represented in a 3D plot they may be called waterfalls. 

1. Download the file

```bash
    sudo apt-get install audacity
    audacity `filename.wav`
```

Use a Spectrogram to visualize the flag.

### Task 3: Steganography

Steganography is the practice of concealing a file, message, image, or video within another file, message, image, or video.

1. Decode the image to reveal the answer. 

    You can use this [Steganographic Decoder](https://futureboy.us/stegano/decinput.html). Upload the image and submit it to reveal the answer.

### Task 4: Security through Obscurity

Security through obscurity is the reliance on the secrecy of the design or implementation as the main method of providing security for a system or component of a system.

1. Download and get 'inside' the file. What is the first filename & extension?
```bash
    binwalk -C `filename`
    cd `_filename.extracted`
```

2. Get inside the archive and inspect the file carefully. Find the hidden text.
```bash
    7z x `filename.rar`
    steghide --extract -sf `filename.png`
```

## Conclusion

This README provides a structured and comprehensive guide to the "c4ptur3-th3-fl4g" project, ensuring that participants have a clear understanding of the objectives, the steps required to complete the challenge, and how to contribute to the project.

- **Translation & Shifting:** These tasks involve converting text or binary using different methods like Base32, Base64, Morse code, etc.
- **Spectrograms:** Instructions are provided for using Audacity to analyze a sound file and find hidden information.
- **Steganography:** Participants are instructed to use a steganographic decoder to reveal hidden data within an image.
- **Security through Obscurity:** Participants are guided to extract hidden information from files using tools like Binwalk and Steghide.

Overall, these tasks cover a range of common techniques used in cybersecurity challenges, helping participants enhance their skills in various areas.