# storybook
A program that uses generative models on a Raspberry Pi to create fantasy storybook pages on the Inky Impression e-ink display

![Storybook example](https://github.com/tvldz/storybook/blob/main/examples/storybook.png?raw=true)

## Hardware
- [Raspberry Pi 5 8GB](https://www.raspberrypi.com/products/raspberry-pi-5/). Certainly possible with other hardware, but may be slower and require simpler models.
- [Inky Impression 5.7"](https://shop.pimoroni.com/products/inky-impression-5-7). Code can be modified to support other resolutions.
- SD Card. 32GB is probably the minimum. Use a bigger one to support experimenting with multiple models and installing desktop components if desired.

## Setup
- Image the SD card with RPi OS, then boot and update the OS
- Enable I2C and SPI interfaces: `sudo raspi-config`
- [Install Ollama](https://ollama.com/download/linux)
- Pull and serve an Ollama model. I find that Mistral and Gemma models work well. `ollama run gemma:7b`
- [Build/install XNNPACK and Onnxstream](https://github.com/vitoplantamura/OnnxStream?tab=readme-ov-file#how-to-build-the-stable-diffusion-example-on-linuxmacwindowstermux)
- Download an SD model. I find that [Stable Diffusion XL Turbo 1.0
](https://github.com/vitoplantamura/OnnxStream?tab=readme-ov-file#stable-diffusion-xl-turbo-10) works well.
- Clone this repository. `git clone https://github.com/tvldz/storybook.git`
- Create a Python virtual environment: `cd storybook && mkdir .venv && python -m venv .venv`
- Activate the environment: `source .venv/bin/activate`
- Install the [Inky libraries](https://github.com/pimoroni/inky). Follow these instructions for RPi 5 compatibility: https://github.com/pimoroni/inky/pull/182
- Install requests and pillow: `pip install requests pillow`
- Modify the constants (paths) at the top of `main.py` to match your own environment.
- execute main.py: `python main.py`. Execution takes ~5 minutes.

## ISSUES/IDEAS/TODO
- Currently, the program just renders a single page at a set interval. It would certainly possible to ask Ollama to generate multiple pages for a complete "story", and then generate illustrations for each page. The entire "story" could be saved locally and "flipped" through more rapidly than discrete page generation.
- The output lacks some diversity, with many of the same characters and themes. This may be improved with a higher quality prompt, modifying the model temperature, or creating a prompt generator that randomly generates prompts from a set of themes, characters, creatures, artifacts, etc.
- The current font doesn't look great on the display. Finding a better font, or perhaps rendering the page horizontally instead of rotating it might have a better result.
- Fitting the text on the screen doesn't always work, since I'm requesting that the model limit itself and naively splitting the output programmatically.
- This would be easily modifiable to create other things like sci-fi stories, weird New Yorker cartoons or off-brand Pokemon.
- This may be thermally taxing on the RPi. Inferrence consumes all CPUs for many minutes, then sits idle for the set interval.
- The code isn't very reslilient but seems to work reliably.
