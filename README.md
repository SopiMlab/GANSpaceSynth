# GANSpaceSynth

GANSpaceSynth is a hybrid architecture for sound synthesis with deep neural networks that applies the [GANSpace](https://arxiv.org/abs/2004.02546) method to the [GANSynth](https://openreview.net/forum?id=H1xQVn09FX) model. Using dimensionality reduction (PCA), we organize the latent space of trained GANSynth models, obtaining controls for exploring the range of sounds that can be synthesized.

To perform GANSpace on GANSynth, we feed a large number of random latent vectors to the model and record the activations on an early convolutional layer of the network. We then use incremental PCA to compute the most significant directions in the activation space, as well as the global mean and standard deviation along each direction. When synthesizing, we give a coefficient for each direction, denoting how far to move along that direction (scaled by the standard deviation) starting from the global mean.

## Examples

The diagram below shows spectrograms for sounds synthesized by a GANSpaceSynth model trained on recordings of "[KET conversations](https://vimeo.com/176701167)" by Thomas Bjelkeborn and Koray Tahiroğlu. GANSpace was applied on the first convolutional layer `conv0` with 4,194,304 random input samples. The coordinates indicate the position along the first two normalized GANSpace coordinates at which synthesis was performed.

![Spectrograms](media/ct-conversations-model_spectrograms/plot.svg)

Below are links to the corresponding audio files and some descriptions of our interpretation of perceived audio characteristics.

<table>
    <tr>
        <td>
            <a href="media/ct-conversations-model_spectrograms/wave_-1.0,-1.0.wav">(-1, -1)</a><br>
            muffled texture
        </td>
        <td>
            <a href="media/ct-conversations-model_spectrograms/wave_-0.5,-1.0.wav">(-0.5, -1)</a><br>
            &nbsp;
        </td>
        <td>
            <a href="media/ct-conversations-model_spectrograms/wave_0.0,-1.0.wav">(0, -1)</a><br>
            bright texture
        </td>
        <td>
            <a href="media/ct-conversations-model_spectrograms/wave_0.5,-1.0.wav">(0.5, -1)</a><br>
            &nbsp;
        </td>
        <td>
            <a href="media/ct-conversations-model_spectrograms/wave_1.0,-1.0.wav">(1, -1)</a><br>
            airy texture
        </td>
    </tr>
    <tr>
        <td>
            <a href="media/ct-conversations-model_spectrograms/wave_-1.0,-0.5.wav">(-1, -0.5)</a><br>
            &nbsp;
        </td>
        <td>
            <a href="media/ct-conversations-model_spectrograms/wave_-0.5,-0.5.wav">(-0.5, -0.5)</a><br>
            &nbsp;
        </td>
        <td>
            <a href="media/ct-conversations-model_spectrograms/wave_0.0,-0.5.wav">(0, -0.5)</a><br>
            &nbsp;
        </td>
        <td>
            <a href="media/ct-conversations-model_spectrograms/wave_0.5,-0.5.wav">(0.5, -0.5)</a><br>
            &nbsp;
        </td>
        <td>
            <a href="media/ct-conversations-model_spectrograms/wave_1.0,-0.5.wav">(1, -0.5)</a><br>
            &nbsp;
        </td>
    </tr>
    <tr>
        <td>
            <a href="media/ct-conversations-model_spectrograms/wave_-1.0,0.0.wav">(-1, 0)</a><br>
            quiet muffled texture
        </td>
        <td>
            <a href="media/ct-conversations-model_spectrograms/wave_-0.5,0.0.wav">(-0.5, 0)</a><br>
            &nbsp;
        </td>
        <td>
            <a href="media/ct-conversations-model_spectrograms/wave_0.0,0.0.wav">(0, 0)</a><br>
            quiet bright texture
        </td>
        <td>
            <a href="media/ct-conversations-model_spectrograms/wave_0.5,0.0.wav">(0.5, 0)</a><br>
            &nbsp;
        </td>
        <td>
            <a href="media/ct-conversations-model_spectrograms/wave_1.0,0.0.wav">(1, 0)</a><br>
            quiet airy texture with whistling
        </td>
    </tr>
    <tr>
        <td>
            <a href="media/ct-conversations-model_spectrograms/wave_-1.0,0.5.wav">(-1, 0.5)</a><br>
            &nbsp;
        </td>
        <td>
            <a href="media/ct-conversations-model_spectrograms/wave_-0.5,0.5.wav">(-0.5, 0.5)</a><br>
            &nbsp;
        </td>
        <td>
            <a href="media/ct-conversations-model_spectrograms/wave_0.0,0.5.wav">(0, 0.5)</a><br>
            &nbsp;
        </td>
        <td>
            <a href="media/ct-conversations-model_spectrograms/wave_0.5,0.5.wav">(0.5, 0.5)</a><br>
            &nbsp;
        </td>
        <td>
            <a href="media/ct-conversations-model_spectrograms/wave_1.0,0.5.wav">(1, 0.5)</a><br>
            &nbsp;
        </td>
    </tr>
    <tr>
        <td>
            <a href="media/ct-conversations-model_spectrograms/wave_-1.0,1.0.wav">(-1, 1)</a><br>
            very quiet muffled texture
        </td>
        <td>
            <a href="media/ct-conversations-model_spectrograms/wave_-0.5,1.0.wav">(-0.5, 1)</a><br>
            &nbsp;
        </td>
        <td>
            <a href="media/ct-conversations-model_spectrograms/wave_0.0,1.0.wav">(0, 1)</a><br>
            very quiet bright texture
        </td>
        <td>
            <a href="media/ct-conversations-model_spectrograms/wave_0.5,1.0.wav">(0.5, 1)</a><br>
            &nbsp;
        </td>
        <td>
            <a href="media/ct-conversations-model_spectrograms/wave_1.0,1.0.wav">(1, 1)</a><br>
            near silence with quiet whistling
        </td>
    </tr>
</table>

## Performance

Generation performance is more or less identical to GANSynth. Computing the linear combination of PCA coefficients adds some overhead, but it appears to be marginal compared to generation itself.

Using our test PC with

- Intel Core i7-10170U CPU,
- NVIDIA GTX 1080 Ti GPU (connected via Thunderbolt 3),

we are able to generate a batch of eight 4-second, 16000 Hz samples in

- 1.2 seconds on CPU (generation rate 107 kHz),
- 65 milliseconds on CPU (generation rate 2000 kHz).

## Hallu

As an example application of GANSpaceSynth, we include the Hallu composition tool (`hallu.pd`). It allows composing pieces by defining paths through the latent space, interpolating between them, and stitching together audio samples synthesized at each point.

The below image shows how Hallu provides a controlled structure to interpolate between points in latent space.

![Hallu](media/HALLU22.png)

An example Hallu composition, created using a model trained by Oskar Koli on a dataset of ambient music, can be found [here](https://drive.google.com/file/d/1grkiBEehfNOD_2wj-EG0SO0CBK4_hyPz/view?usp=sharing).

## Installation

We have detailed [tutorials](https://github.com/SopiMlab/DeepLearningWithAudio/tree/master/utilities/pyext-setup) for setting up GANSpaceSynth in a Conda environment. After finishing the tutorial, you can simply open the `.pd` files in this repository.

To run GANSpaceSynth, you will need:

- [Pure Data](https://puredata.info/)
- [SopiMlab/py](https://github.com/SopiMlab/py)
- [SopiMlab/flext](https://github.com/SopiMlab/flext)
- [SopiMlab/magenta](https://github.com/SopiMlab/magenta)
- [SopiMlab/SopiLib](https://github.com/SopiMlab/SopiLib)

## Paper (AIMC 2021)

Coming soon

```
bibtex citation goes here
```

## License

GANSpaceSynth, a hybrid GAN architecture using dimensionality reduction (PCA) to organise the latent space of trained GANSynth models, is provided by [SOPI research group](https://sopi.aalto.fi) Aalto University, School of Arts, Design and Architecture 2021, under the GPL license [GNU General Public License version 2 | Open Source Initiative](https://opensource.org/licenses/gpl-2.0.php) We make use of the following open source projects:
 
- pyext/flext (GPL v2)
- Magenta (Apache 2.0)
 
Copyright (C) 2021 SOPI Research Group, Aalto University

--------------------------------------------------------------------

This program is free software; you can redistribute it and/or modify it under the terms of the GNU General Public License as published by the Free Software Foundation; either version 2 of the License, or (at your option) any later version.
 

This program is distributed in the hope that it will be useful, but WITHOUT ANY WARRANTY; without even the implied warranty of MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the GNU General Public License for more details.

You should have received a copy of the GNU General Public License along with this program; if not, write to the Free Software Foundation, Inc., 51 Franklin Street, Fifth Floor, Boston, MA  02110-1301, USA. For further information please contact to Koray Tahiroglu; email: koray.tahiroglu@aalto.fi, mail: Aalto University School of Arts, Design and Architecture, Department of Media, room J101, Väre Otaniementie 14, 02150 Espoo, Finland

------------------------
