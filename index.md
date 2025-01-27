---
title: "QinCodec: Neural Audio Compression with Implicit Neural Codebooks
description: Anonymous
layout: default
---

<div style="max-width: 600px; margin: 50px auto">

In this paper, we challenge the common practice of training neural audio codecs end-to-end, instead proposing a three-stages strategy that allows us to rely on an implicit neural quantization layer for neural audio coding.

<ul>
    <li>We propose QINCODEC, a 44.1 kHz audio codec based on the decoupled training of an autoencoder and a neural residual vector quantizer QINCO2, trained offline.</li>
    <li>Our model is the first auto-encoder that relies on Vocos [Siuzdak, 2024] blocks, providing a lightweight and fast way to encode/decode audio, making its integration easy into the training pipelines of generative models.</li>
    <li>QINCODEC outperforms state-of-the-art methods at 16 kbps bitrate and achieves competitive results at lower bitrates with both objective and subjective metrics.</li>
    <li>Our offline approach offers a simple yet robust framework that allows to consider any off-the-shelf quantizer with a fixed pre-trained autoencoder, paving the way for adaptable and frugal codec design</li>
    </ul>

</div>

<div style="text-align: center;">
  <figure>
    <img src="{{ site.baseurl }}/assets/images/posttrainingquantization.png" alt="Post-training quantization" width="70%">     
    <figcaption> <strong> Training procedure of QINCODEC with offline quantization: </strong> First, we train a continuous compression model with spectral

and adversarial losses. Next, we quantize the bottleneck latent vec-
tors into discrete embeddings. We then finetune the decoder on the
quantized representations</figcaption>
  </figure>
</div>


# Experiments and results

The table below presents audio clips to evaluate the reconstruction quality after training the continuous model and after the post-training quantization stage, without any finetuning.

{% assign audio_ids = "PLHXGDnig4M, YQSuFyFm3Lc, DlWd7Wmdi1E, yfYNPWs7mWY, u84FiZ_omhA, sVYTOURVsQ0, GOD8Bt5LfDE" | split: ", " %}
{% assign formats = "wav" | split: ", " %}
{% assign image_audio_ids = "PLHXGDnig4M, YQSuFyFm3Lc" | split: ", " %}

## Comparison with baselines, at 16kbps

<div markdown="0">
<table class="tableFixHead tableDoubleRows audio-table">
<colgroup>
<col/>
<col/>
<col/>
</colgroup>
<thead>
<tr class="header">
<th> Original </th>
<th> Reconstruction (Continuous) </th>
<th> Qincodec </th>
<th> DAC </th>
<th> Encodec </th>

</tr>
</thead>
<tbody>
{% assign characters = "continuous, 16kbps-finetuned, e2e1, e2e2" | split: ", " %}
{% for audio_id in audio_ids %}
    <tr>
        {% for char in characters %}
        <td>
            <audio controls preload='metadata'>
                {% for format in formats %}
                    <source src="{{ site.baseurl }}/assets/audio/{{ audio_id }}_{{ char }}.{{ format }}" 
                            alt="{{ audio_id }}_{{ char }}" 
                            type="audio/{{ format }}">
                {% endfor %}
            </audio>
        </td>
        {% endfor %}
    </tr>
{% endfor %}
</tbody>
</table>
</div>

## Dynamic bitrate

<div markdown="0">
<table class="tableFixHead tableDoubleRows audio-table">
<colgroup>
<col/>
<col/>
<col/>
</colgroup>
<thead>
<tr class="header">
<th> Original </th>
<th> Reconstruction (Continuous)</th>
<th>24 kbps</th>
<th>16 kbps</th>
<th>10 kbps</th>
<th>3 kbps</th>
</tr>
</thead>
<tbody>
{% assign characters = "original, continuous, 24kbps, 16kbps, 10kbps, 3kbps" | split: ", " %}

{% for audio_id in audio_ids %}
    <tr>
        {% for char in characters %}
        <td>
            <audio controls preload='metadata'>
                {% for format in formats %}
                    <source src="{{ site.baseurl }}/assets/audio/{{ audio_id }}_{{ char }}.{{ format }}" 
                            alt="{{ audio_id }}_{{ char }}" 
                            type="audio/{{ format }}">                    
                {% endfor %}
            </audio>
            {% if image_audio_ids contains audio_id %}
                <img src="{{ site.baseurl }}/assets/images/spectrograms/{{ audio_id }}/{{ char }}.PNG" alt="/assets/images/{{ audio_id }}/{{ char }}.PNG"  width="300" height="150"/>
            {% endif %}
        </td>
        {% endfor %}
    </tr>
{% endfor %}
</tbody>
</table>  
</div>



## Impact of finetuning

<div markdown="0">
<table class="tableFixHead tableDoubleRows audio-table">
<colgroup>
<col/>
<col/>
<col/>
</colgroup>
<thead>
<tr class="header">
<th> Reconstruction (Continuous) </th>
<th> QinCodec (w/o finetuning) </th>
<th> QinCodec </th>
</tr>
</thead>
<tbody>
{% assign characters = "continuous, 16kbps, 16kbps-finetuned" | split: ", " %}
{% for audio_id in audio_ids %}
    <tr>
        {% for char in characters %}
        <td>
            <audio controls preload='metadata'>
                {% for format in formats %}
                    <!-- <source src="{{ site.baseurl }}/assets/audio/{{ audio_id }}_{{ char }}.{{ format }}"  -->
                    <source src="{{ site.baseurl }}/assets/audio/{{ audio_id }}_{{ char }}.{{ format }}" 
                            alt="{{ audio_id }}_{{ char }}" 
                            type="audio/{{ format }}">
                {% endfor %}
            </audio>
            {% if image_audio_ids contains audio_id %}
                <img src="{{ site.baseurl }}/assets/images/spectrograms/{{ audio_id }}/{{ char }}.PNG" alt="spec_{ audio_id }}_{{ char }}"  width="300" height="150"/>
            {% endif %}
        </td>
        {% endfor %}
    </tr>
{% endfor %}
</tbody>
</table>
</div>
