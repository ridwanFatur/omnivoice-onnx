# OmniVoice ONNX

This repository contains experiments and implementation notebooks for converting [OmniVoice](https://github.com/k2-fsa/OmniVoice) into ONNX format.

The main goal is to simplify the OmniVoice inference pipeline, separate its components into individual models, and export them to ONNX so they can potentially run in different environments, including web browsers using [ONNX Runtime Web](https://onnxruntime.ai/docs/get-started/with-javascript.html).

This work is part of an ongoing effort to bring OmniVoice inference to the browser without relying on a Python backend for model inference.

## Project Overview

The original OmniVoice inference pipeline consists of multiple components, including audio tokenization, LLM processing, audio embeddings, and audio generation heads.

Instead of exporting the entire pipeline as a single ONNX model, this project explores separating the pipeline into smaller, independent ONNX models.

This approach makes it easier to:

- Understand and inspect each component of the original inference pipeline.
- Export individual model components into ONNX format.
- Validate input and output tensor shapes.
- Test ONNX inference independently against the original PyTorch implementation.
- Integrate the exported models into JavaScript environments using ONNX Runtime Web.
- Build a browser-based inference pipeline with more flexibility.

## Notebooks

The notebooks are organized sequentially, following the exploration, separation, conversion, and testing process.

| Notebook | Description |
|---|---|
| [1. OmniVoice Simple Inference](https://github.com/ridwanFatur/omnivoice-onnx/blob/master/1.%20Omnivoice%20Simple%20Inference.ipynb) | Run the original OmniVoice inference pipeline to understand its overall workflow. |
| [2. Separate LLM Parts](https://github.com/ridwanFatur/omnivoice-onnx/blob/master/2.%20Separate%20LLM%20Parts.ipynb) | Explore and separate the LLM components into individual parts. |
| [3. Simple Inference with Separated Models](https://github.com/ridwanFatur/omnivoice-onnx/blob/master/3.%20Simple%20Inference%20with%20Separated%20Models.ipynb) | Reconstruct a simplified inference pipeline using the separated components. |
| [4. Check Input Output Shape](https://github.com/ridwanFatur/omnivoice-onnx/blob/master/4.%20Check%20Input%20Output%20Shape.ipynb) | Inspect and verify input and output tensor shapes for model conversion. |
| [5. Audio Tokenizer - Convert ONNX](https://github.com/ridwanFatur/omnivoice-onnx/blob/master/5.%20Audio%20Tokenizer%20-%20Convert%20ONNX.ipynb) | Export the Higgs Audio V2 tokenizer encoder and decoder to ONNX. |
| [6. Audio Tokenizer - Test ONNX](https://github.com/ridwanFatur/omnivoice-onnx/blob/master/6.%20Audio%20Tokenizer%20-%20Test%20ONNX.ipynb) | Test the exported audio tokenizer ONNX models. |
| [7. LLM Parts - Convert ONNX](https://github.com/ridwanFatur/omnivoice-onnx/blob/master/7.%20LLM%20Parts%20-%20Convert%20ONNX.ipynb) | Export the separated OmniVoice LLM components to ONNX. |
| [8. LLM Qwen3 - Convert ONNX](https://github.com/ridwanFatur/omnivoice-onnx/blob/master/8.%20LLM%20Qwen3%20-%20Convert%20ONNX.ipynb) | Export the Qwen3 model to ONNX format. |
| [9. LLM Input Embeddings - Convert ONNX](https://github.com/ridwanFatur/omnivoice-onnx/blob/master/9.%20LLM%20Input%20Embeddings%20-%20Convert%20ONNX.ipynb) | Export the Qwen3 input embedding component to ONNX. |
| [10. ONNX - Simple Inference](https://github.com/ridwanFatur/omnivoice-onnx/blob/master/10.%20ONNX%20-%20Simple%20Inference.ipynb) | Test and run a simplified inference pipeline using the exported ONNX models. |

## Exported ONNX Models

The exported models are available on Hugging Face:

**[ridwanFatur98/omnivoice-onnx](https://huggingface.co/ridwanFatur98/omnivoice-onnx)**

Each component is stored in its own directory, with the exported model saved as `model.onnx`.

### Model Components

| Directory | Description |
|---|---|
| [onnx_higgs_audio_v2_tokenizer_encoder](https://huggingface.co/ridwanFatur98/omnivoice-onnx/tree/main/onnx_higgs_audio_v2_tokenizer_encoder) | Higgs Audio V2 tokenizer encoder. |
| [onnx_higgs_audio_v2_tokenizer_decoder](https://huggingface.co/ridwanFatur98/omnivoice-onnx/tree/main/onnx_higgs_audio_v2_tokenizer_decoder) | Higgs Audio V2 tokenizer decoder. |
| [onnx_omnivoice_audio_embeddings](https://huggingface.co/ridwanFatur98/omnivoice-onnx/tree/main/onnx_omnivoice_audio_embeddings) | OmniVoice audio embedding component. |
| [onnx_omnivoice_audio_heads](https://huggingface.co/ridwanFatur98/omnivoice-onnx/tree/main/onnx_omnivoice_audio_heads) | OmniVoice audio generation heads. |
| [onnx_qwen3](https://huggingface.co/ridwanFatur98/omnivoice-onnx/tree/main/onnx_qwen3) | Qwen3 language model. |
| [onnx_qwen3_input_embeddings](https://huggingface.co/ridwanFatur98/omnivoice-onnx/tree/main/onnx_qwen3_input_embeddings) | Qwen3 input embedding component. |

The repository will be updated as additional components and improvements become available.

## Inference Pipeline

The exported models are intended to work together as a modular inference pipeline.

The general workflow is:

1. Process the input text and prepare the required model inputs.
2. Generate text and audio representations using the Qwen3 and OmniVoice components.
3. Perform iterative audio token generation.
4. Decode the generated audio tokens using the Higgs Audio V2 tokenizer decoder.
5. Produce the final waveform.

The audio tokenizer encoder is also available for processing reference audio, which is useful for voice cloning.

## Browser Inference

One of the main motivations behind this project is to enable OmniVoice inference directly in the browser.

With the models exported to ONNX, individual components can be loaded and executed using ONNX Runtime Web.

This opens up possibilities such as:

- Running model inference directly in the browser.
- Using WebAssembly or WebGPU execution providers where supported.
- Loading individual ONNX models instead of depending on a Python-based inference server.
- Integrating the inference pipeline into a JavaScript or TypeScript application.

The browser implementation is currently under development.

### Related Project

**[OmniVoice Web UI](https://github.com/ridwanFatur/omnivoice-web-ui)**

This is the ongoing project that aims to integrate the exported ONNX models into a browser-based OmniVoice application.

## Status

<details>
<summary>Current progress</summary>

- [x] Explore the original OmniVoice inference pipeline.
- [x] Separate the LLM components.
- [x] Implement a simplified inference pipeline using separated components.
- [x] Inspect model input and output tensor shapes.
- [x] Export Higgs Audio V2 tokenizer components to ONNX.
- [x] Test audio tokenizer ONNX inference.
- [x] Export OmniVoice LLM components to ONNX.
- [x] Export Qwen3 to ONNX.
- [x] Export Qwen3 input embeddings to ONNX.
- [x] Test a simplified ONNX inference pipeline.
- [ ] Integrate all components into the browser inference pipeline.
- [ ] Optimize browser inference performance.

</details>

## References

- [OmniVoice](https://github.com/k2-fsa/OmniVoice)
- [ONNX Runtime](https://onnxruntime.ai/)
- [ONNX Runtime Web](https://onnxruntime.ai/docs/get-started/with-javascript.html)
- [Exported ONNX Models on Hugging Face](https://huggingface.co/ridwanFatur98/omnivoice-onnx)
- [OmniVoice Web UI](https://github.com/ridwanFatur/omnivoice-web-ui)

---

**Note:** This repository is an ongoing experimental project. The notebooks document the process of separating, exporting, and validating OmniVoice components in ONNX format. Browser compatibility and inference performance may vary depending on the execution provider, model operators, and available hardware.
