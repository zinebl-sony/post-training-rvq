---
title: "QinCodec: Neural Audio Compression with Implicit Neural Codebooks"
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

The tables below provide audio clips for evaluating the reconstruction quality of our model in comparison to the baselines presented in the paper. Some differences between audio samples may be subtle, so we recommend using headphones for an accurate assessment.

{% assign audio_ids = "0050, 0209, 0526, 0530, 0626, 0808" | split: ", " %}
{% assign formats = "wav" | split: ", " %}
{% assign image_audio_ids = "0050, 0209" | split: ", " %}

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
<th> Qincodec </th>
<th> DAC </th>
<th> Encodec </th>

</tr>
</thead>
<tbody>
{% assign characters = "src, qincodec16kbps, descript16kbps, encodec16kbps" | split: ", " %}
{% for audio_id in audio_ids %}
    <tr>
        {% for char in characters %}
        <td>
            <audio controls preload='metadata'>
                {% for format in formats %}
                    <source src="{{ site.baseurl }}/assets/audio/audio{{ audio_id }}-{{ char }}.{{ format }}" 
                            alt="audio{{ audio_id }}_{{ char }}" 
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

## Comparison with baselines, at 8kbps

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
<!-- <th> Reconstruction (Continuous) </th> -->
<th> Qincodec </th>
<th> DAC </th>
<th> Encodec </th>

</tr>
</thead>
<tbody>
{% assign characters = "src, qincodec8kbps, descript8kbps, encodec8kbps" | split: ", " %}
{% for audio_id in audio_ids %}
    <tr>
        {% for char in characters %}
        <td>
            <audio controls preload='metadata'>
                {% for format in formats %}
                    <source src="{{ site.baseurl }}/assets/audio/audio{{ audio_id }}-{{ char }}.{{ format }}" 
                            alt="audio{{ audio_id }}_{{ char }}" 
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
<th> Original </th>
<th> QinCodec </th>
<th> QinCodec (w/o finetuning) </th>
</tr>
</thead>
<tbody>
{% assign characters = "src, qincodec16kbps, qincodec16kbps_nofinetune" | split: ", " %}
{% for audio_id in audio_ids %}
    <tr>
        {% for char in characters %}
        <td>
            <audio controls preload='metadata'>
                {% for format in formats %}
                    <!-- <source src="{{ site.baseurl }}/assets/audio/{{ audio_id }}_{{ char }}.{{ format }}"  -->
                    <source src="{{ site.baseurl }}/assets/audio/audio{{ audio_id }}-{{ char }}.{{ format }}" 
                            alt="audio{{ audio_id }}_{{ char }}" 
                            type="audio/{{ format }}">
                {% endfor %}
            </audio>
            {% if image_audio_ids contains audio_id %}
                <img src="{{ site.baseurl }}/assets/images/spectrograms/audio{{ audio_id }}/{{ char }}.PNG" alt="spec_{ audio_id }}_{{ char }}"  width="300" height="150"/>
            {% endif %}
        </td>
        {% endfor %}
    </tr>
{% endfor %}
</tbody>
</table>
</div>
