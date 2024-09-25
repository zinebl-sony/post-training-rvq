---
title: Challenging Common Practices In Vector Quantized Neural Audio Compression
description: Zineb Lahrichi, Gaëtan Hadjeres, Gaël Richard, Geoffroy Peeters
layout: default
---

  In this paper, we introduce a straightforward alternative to end-to-end
audio codecs by separating the training of continuous
compression models from latent quantization, enabling
the use of any quantization techniques, including non-differentiable
ones. Our strategy supports a shared continuous and quantized
representation, making it adaptable for diffusion models
and large language models. We propose a fair comparison of quantization methods
using a fixed audio representation, unlike previous codecs
with varying latent dimensions and downsampling factors. Eventually, we propose an improvement of Residual Vector Quantization (RVQ), by introducing a standardization step.

<div style="text-align: center;">
  <figure>
    <img src="{{ site.baseurl }}/assets/images/posttrainingquantization.png" alt="Post-training quantization" width="70%">     
    <figcaption> <strong> Post training quantization </strong> : First, we train a continuous
compression model with spectral and adversarial losses.
Next, we quantize the bottleneck latent vectors into discrete
embeddings and reintroduce them for finetuning, allowing the
decoder to reconstruct audio from the discrete latent space.</figcaption>
  </figure>
</div>


# Experiments and results

In this work, we refer to our proposed method as PostRVQ (post-training quantization) and end-to-end models as E2E. The following audio clips were randomly sampled from the AudioCaps test set, which was used to evaluate our method in the paper.

## Audio reconstruction by bitrate (w/o finetuning)

The table below presents audio clips to evaluate the reconstruction quality after training the continuous model and after the post-training quantization stage, without any finetuning.

{% assign audio_ids = "PLHXGDnig4M, YQSuFyFm3Lc, DlWd7Wmdi1E, yfYNPWs7mWY, u84FiZ_omhA, sVYTOURVsQ0, GOD8Bt5LfDE" | split: ", " %}
{% assign formats = "wav" | split: ", " %}
{% assign image_audio_ids = "PLHXGDnig4M, YQSuFyFm3Lc" | split: ", " %}

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
<th> PostRVQ (w/o finetuning) </th>
<th> PostRVQ </th>
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

## Comparison to E2E models

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
<th> PostRVQ </th>
<th> E2E (1) </th>
<th> E2E (2) </th>
<th> E2E (3) </th>

</tr>
</thead>
<tbody>
{% assign characters = "continuous, 16kbps-finetuned, e2e1, e2e2, e2e3" | split: ", " %}
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
