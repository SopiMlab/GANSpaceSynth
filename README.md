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

## Hallu

As an example application of GANSpaceSynth, we include the Hallu composition tool (`hallu.pd`). It allows composing pieces by defining paths through the latent space, interpolating between them, and stitching together audio samples synthesized at each point.

## Installation

To run GANSpaceSynth, you will need:

- [Pure Data](https://puredata.info/)
- [SopiMlab/py](https://github.com/SopiMlab/py)
- [SopiMlab/flext](https://github.com/SopiMlab/flext)
- [SopiMlab/magenta](https://github.com/SopiMlab/magenta)
- [SopiMlab/SopiLib](https://github.com/SopiMlab/SopiLib)

We have [tutorials](https://github.com/SopiMlab/DeepLearningWithAudio/tree/master/utilities/pyext-setup) for setting up a [Conda](https://conda.io/) environment and installing all the necessary bits.

## Paper (AIMC 2021)

Coming soon

```
bibtex citation goes here
```
