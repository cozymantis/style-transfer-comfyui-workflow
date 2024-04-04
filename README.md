# Style Transfer with Stable Diffusion - ComfyUI Workflow To Test Style Transfer Methods

This repository contains a workflow to test different style transfer methods using Stable Diffusion. The workflow is based on ComfyUI, which is a user-friendly interface for running Stable Diffusion models. The workflow is designed to test different style transfer methods from a single reference image.

## Evaluated Methods

- **The SD 1.5 Style ControlNet Coadapter**
  - Required nodes are built into ComfyUI
  - Docs: https://github.com/TencentARC/T2I-Adapter/blob/SD/docs/coadapter.md
- **Visual Style Prompt for SD 1.5**
  - ComfyUI node: https://github.com/ExponentialML/ComfyUI_VisualStylePrompting
  - Project page: https://curryjung.github.io/VisualStylePrompt/
- **The SD XL IPAdapter** with style transfer weight types
  - ComfyUI node: https://github.com/cubiq/ComfyUI_IPAdapter_plus
  - Project page: https://ip-adapter.github.io/
- **StyleAligned for SD XL**
  - ComfyUI node: https://github.com/brianfitzgerald/style_aligned_comfy
  - Project page: https://style-aligned-gen.github.io/

![](screen.png)

## Results

**Original Image**
![](./assets/source1.png)

| Style Method               | `an elephant` | `a robot` | `a car` |
| -------------------------- | --- | --- | --- |
| SD 1.5 Style Adapter         | ![](./assets/sm.png) | ![](./assets/sm1.png) | ![](./assets/sm2.png) |
| SD 1.5 Visual Style Prompt | ![](./assets/vsp.png) | ![](./assets/vsp1.png) | ![](./assets/vsp2.png) |
| SD XL IPAdapter            | ![](./assets/ipa.png) | ![](./assets/ipa1.png) | ![](./assets/ipa2.png) |
| SD XL StyleAligned         | ![](./assets/sa.png) | ![](./assets/sa1.png) | ![](./assets/sa2.png) |



**Original Image**
![](./assets/source2.png)

| Style Method               | `an elephant` | `a robot` | `a car` |
| -------------------------- | --- | --- | --- |
| SD 1.5 Style Adapter         | ![](./assets/2sm.png) | ![](./assets/2sm1.png) | ![](./assets/2sm2.png) |
| SD 1.5 Visual Style Prompt | ![](./assets/2vsp.png) | ![](./assets/2vsp1.png) | ![](./assets/2vsp2.png) |
| SD XL IPAdapter            | ![](./assets/2ipa.png) | ![](./assets/2ipa1.png) | ![](./assets/2ipa2.png) |
| SD XL StyleAligned         | ![](./assets/2sa.png) | ![](./assets/2sa1.png) | ![](./assets/2sa2.png) |

## Installation

- Custom nodes you'll need:
  - https://github.com/ExponentialML/ComfyUI_VisualStylePrompting
  - https://github.com/cubiq/ComfyUI_IPAdapter_plus
  - https://github.com/brianfitzgerald/style_aligned_comfy

- Models you'll need:
  - SD 1.5 Style ControlNet Coadapter: https://huggingface.co/TencentARC/T2I-Adapter/blob/main/models/coadapter-style-sd15v1.pth (add to `models/controlnet`)
  - Clip Vision Large: https://huggingface.co/openai/clip-vit-large-patch14/blob/main/pytorch_model.bin (add to `models/clip_vision`)
  - OpenCLIP: https://huggingface.co/laion/CLIP-ViT-H-14-laion2B-s32B-b79K/blob/main/open_clip_pytorch_model.safetensors (add to `models/clip_vision`)
  - SD XL IPAdapter: https://huggingface.co/h94/IP-Adapter/resolve/main/sdxl_models/ip-adapter-plus_sdxl_vit-h.safetensors (add to `models/ipadapter`)
  - An SD 1.5 checkpoint
  - An SD XL checkpoint

## Notes

For the purpose of this test, we tried to keep prompts as simple as possible, and **not include style details in the generating prompt**. This way, we aim to test each style transfer method in a more isolated/prompt independent way. 

**For the best results, you should engineer the prompts to better fit your needs.**

Make sure to read the documentation of each method and each ComfyUI node to understand how to use them properly.

When loading the workflow, the ControlNet branch is disabled by default. You can enable it by selecting all the top nodes, and pressing CTRL+M. This is because the current implementation of Visual Style Prompting is incompatible with the ControlNet style model (see below). You need to use them one at a time, and restart ComfyUI if you want to switch between them.

### SD 1.5 Style ControlNet Coadapter
- tends to also capture and transfer semantic information from the reference image;
- you can use this together with other controlnets for better composition results;
- no parameters to tweak;
- we spent a lot of time fishing for good seeds to cherry pick the best results.

### Visual Style Prompt for SD 1.5
- in order to get the best results, you must engineer both the positive and reference image prompts correctly;
- focus on the details you want to derive from the image reference, and the details you wish to see in the output;
- in the current implementation, the custom node we used updates model attention in a way that is incompatible with applying controlnet style models via the "Apply Style Model" node;
- once you run the "Apply Visual Style Prompting" node, you won't be able to apply the controlnet style model anymore and need to restart ComfyUI if you plan to do so;
- this method tends to produce more abstract/creative results, which can be a good thing if you're looking for a more artistic output.

### SD XL IPAdapter
- The style transfer weight types are a feature of the IPAdapter v2 node for SD XL only;
- Outputs are consistently good and style-aligned with the original;
- Tends to focus on/add detail to faces;
- `Weight`/`start at`/`end at` are adjustable parameters;
- Maybe the best style transfer method at the time of writing this.

### StyleAligned for SD XL
- The StyleAligned method is a bit more complex to use, but it's worth the effort;
- Outputs are good but it looks like some semantics are also transfered;
- Allows customization via the `share_attn` and `share_norm` parameters.

## More Good Stuff

Made with 💚 by the [CozyMantis](https://cozymantis.gumroad.com) squad. Check out our ComfyUI nodes and workflows!

| | |
| --- | --- |
| <img src="https://github.com/cozymantis/style-transfer-comfyui-workflow/assets/5381731/124828a8-1dd2-447b-8646-ec9049f35f42" width=200 /><br />[Cozy Portrait Animator - ComfyUI Nodes & Workflow To Animate A Face From A Single Image](https://cozymantis.gumroad.com/l/cozy-animated-portraits-with-stablediffusion-comfyui-aniportrait?layout=profile) | <a href="https://cozymantis.gumroad.com/l/cozy-clothes-swap-comfyui-node-salvton?layout=profile"><img width=200 src="https://public-files.gumroad.com/x75wxfap89vzrasn4v6rniemyaj5"><br /> Cozy Clothes Swap - Customizable ComfyUI Node For Fashion Try-on</a> |
| <img src="https://public-files.gumroad.com/dtz98aq6vdup1yyo6dh1znk2sn7o" width=200 /><br />[Cozy Character Turnaround - Generate And Rotate Characters and Outfits with SD 1.5, SV3D, and IPAdapter - ComfyUI Workflow](https://cozymantis.gumroad.com/l/cozy-character-turnaround-animate-comfyui-workflow?layout=profile) | <a href="https://cozymantis.gumroad.com/l/character-face-consistency-reference-sheet-comfyui-workflow-sd15?layout=profile"><img width=200 src="https://public-files.gumroad.com/z5ngr41vztccp0isnwtarap5wy5n"><br /> Cozy Character Face Generator - ComfyUI SD 1.5 Workflow For Consistent Reference Sheets</a> |

## Licenses & Commercial Use

Please check licenses and terms of use for each of the nodes and models required by this workflow.

## Misuse, Malicious Use, and Out-of-Scope Use

The workflow should not be used to intentionally create or disseminate images that create hostile or alienating environments for people. This includes generating images that people would foreseeably find disturbing, distressing, or offensive; or content that propagates historical or current stereotypes.
