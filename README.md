# 🎬 MiniMax H3 no Google Colab (Tesla T4) & Local ComfyUI

Repositório contendo o fluxo otimizado do **MiniMax H3** (modelo de difusão multimodal de vídeo + áudio sincronizado) com **Turbo LoRA**, adaptado para execução em hardware com recursos limitados de memória (como a GPU Tesla T4 gratuita do Google Colab e placas como a RTX 2060).

---

## 📚 Documentação e Guias

* 📖 **[Guia Completo de Modos de Geração e Links de Download (T2V, I2V e R2V)](./MODOS_DE_GERACAO_E_MODELOS.md)**
* 📋 **[Instruções de Execução Rápida no Google Colab](./LEIAME.md)**

---

## 🚀 Destaques do Projeto

* **Suporte Completo a GPU Tesla T4:** Pipeline dividido em 3 estágios independentes (Encode $\rightarrow$ Sample $\rightarrow$ Decode) para contornar o limite de 12,7 GB de RAM do Colab sem sofrer com *Out of Memory (OOM)*.
* **Integração Nativa com Google Drive:**
  * Escaneia e detecta automaticamente a pasta `Meu Drive/ComfyUI/models/`.
  * Se os modelos já existirem no Drive, reutiliza imediatamente sem gastar banda ou tempo.
  * Se faltarem modelos, baixa direto para as subpastas corretas (`diffusion_models`, `loras`, `vae`, `text_encoders`).
* **Suporte a Consistência de Personagens (R2V / Ref2VA):** Suporta até 9 imagens de referência para manter o mesmo ator/personagem ao longo de diferentes tomadas e ângulos.
* **Turbo LoRA (4 a 6 Passos):** Aceleração do sampling de ~20 passos para apenas 4 a 6 passos mantendo sincronia labial/áudio.
* **Stage 7 Video Upscaler (1080p / 4K + Áudio Preservado):** Super-resolução neural frame a frame na GPU com `4x-UltraSharp` ou `RealESRGAN`, preservando o áudio original sincronizado e salvando direto no Drive (`video_upscaled/`).
* **Prevenção de Tela Preta / NaNs:** Configurado com `--bf16-unet` e `fp32 RMSNorm` nos tensores residuais para estabilidade matemática total.
* **Pronto para 1 Clique (*Run All*):** O notebook possui guarda de GPU e roda do início ao fim sem requerer ajustes manuais.

---

## 📁 Estrutura do Repositório

```text
├── MiniMax_H3_on_Colab_T4.ipynb       # Notebook principal: Geração de Vídeo + Áudio no Colab T4
├── Video_Upscaler_Pro_Colab.ipynb     # Notebook dedicado: Upscaler Pro (1080p/4K) individual e em lote
├── README.md                          # Visão geral do repositório
├── MODOS_DE_GERACAO_E_MODELOS.md      # Guia detalhado dos 3 modos e links oficiais de download
├── LEIAME.md                          # Guia passo a passo de execução
└── .gitignore                         # Filtro para ignorar modelos pesados e saídas temporárias
```

---

## 📦 Modelos e Links Oficiais de Download

Todos os arquivos devem ser salvos nas suas pastas correspondentes dentro de `ComfyUI/models/`:

| Componente | Arquivo | Modo | Tamanho | Download Oficial |
| :--- | :--- | :--- | :--- | :--- |
| **DiT FL2VA** | `MiniMax-H3-FL2VA-Pruned-Q4_K_M.gguf` | T2V / I2V | ~10,77 GB | [Baixar via Hugging Face](https://huggingface.co/Abiray/MiniMax-H3-Pruned-GGUF/resolve/main/MiniMax-H3-FL2VA-Pruned-Q4_K_M.gguf) |
| **DiT Ref2VA** | `MiniMax-H3-Ref2VA-Pruned-Q4_K_M.gguf` | R2V (Personagem) | ~10,77 GB | [Baixar via Hugging Face](https://huggingface.co/Abiray/MiniMax-H3-Pruned-GGUF/resolve/main/MiniMax-H3-Ref2VA-Pruned-Q4_K_M.gguf) |
| **Turbo LoRA** | `minimax_h3_turbo_v4_step600_ema.safetensors` | Todos | ~0,73 GB | [Baixar via Hugging Face](https://huggingface.co/larryvrh/MiniMax-H3-Turbo-Lora/resolve/main/minimax_h3_turbo_v4_step600_ema.safetensors) |
| **Text Encoder** | `qwen3vl_32b_minimax_h3_nvfp4_awq.safetensors` | Todos | ~14,61 GB | [Baixar via Hugging Face](https://huggingface.co/Comfy-Org/MiniMax-H3/resolve/main/text_encoders/qwen3vl_32b_minimax_h3_nvfp4_awq.safetensors) |
| **Video VAE** | `minimax_h3_video_vae_fp16.safetensors` | Todos | ~4,85 GB | [Baixar via Hugging Face](https://huggingface.co/Comfy-Org/MiniMax-H3/resolve/main/vae/minimax_h3_video_vae_fp16.safetensors) |
| **Audio VAE** | `minimax_h3_audio_vae_fp32.safetensors` | Todos | ~0,56 GB | [Baixar via Hugging Face](https://huggingface.co/Comfy-Org/MiniMax-H3/resolve/main/vae/minimax_h3_audio_vae_fp32.safetensors) |
| **Upscale Nomos8kDAT** | `4xNomos8kDAT.pth` | Upscale (DAT) | ~295 MB | [Baixar via Hugging Face](https://huggingface.co/Maxivi/SDXLModels/resolve/main/4xNomos8kDAT.pth) |
| **Upscale NMKD** | `4x_NMKD-Superscale-SP_178000_G.pth` | Upscale | ~64 MB | [Baixar via Hugging Face](https://huggingface.co/gemasai/4x_NMKD-Superscale-SP_178000_G/resolve/main/4x_NMKD-Superscale-SP_178000_G.pth) |
| **Upscale UltraSharp** | `4x-UltraSharp.pth` | Upscale | ~64 MB | [Baixar via Hugging Face](https://huggingface.co/lokcx/4x-Ultrasharp/resolve/main/4x-UltraSharp.pth) |
| **Upscale RealESRGAN** | `RealESRGAN_x4plus.pth` | Upscale | ~64 MB | [Baixar via Hugging Face](https://huggingface.co/lllyasviel/Annotators/resolve/main/RealESRGAN_x4plus.pth) |

---

## 🎨 Templates de Workflow ComfyUI (.JSON)

* 📄 **Texto para Vídeo (T2V):** [`video_minimax_h3_t2v.json`](https://raw.githubusercontent.com/Comfy-Org/workflow_templates/main/templates/video_minimax_h3_t2v.json)
* 🖼️ **Imagem para Vídeo (I2V):** [`video_minimax_h3_i2v.json`](https://raw.githubusercontent.com/Comfy-Org/workflow_templates/main/templates/video_minimax_h3_i2v.json)
* 🎭 **Referência de Personagem para Vídeo (R2V):** [`video_minimax_h3_r2v.json`](https://raw.githubusercontent.com/Comfy-Org/workflow_templates/main/templates/video_minimax_h3_r2v.json)

---

## ⚡ Como Rodar no Google Colab

1. Acesse o [Google Colab via GitHub](https://colab.research.google.com/github/gefexfarias/ComfyUI_Colab/blob/main/MiniMax_H3_on_Colab_T4.ipynb).
2. No menu **Ambiente de execução** $\rightarrow$ **Alterar tipo de ambiente de execução**, selecione **GPU T4**.
3. (Opcional) Edite o `PROMPT` desejado na célula do **Stage 0**.
4. Clique em **Ambiente de execução** $\rightarrow$ **Executar tudo** (*Run all*).
5. Autorize a conexão com o Google Drive para reutilizar e salvar os modelos/vídeos.

---

## 💻 Execução Local (RTX 2060 6GB + 32GB RAM)

No PC com 32 GB de RAM, execute o ComfyUI com a interface gráfica visual completa utilizando o argumento:
```bat
python main.py --lowvram
```
Arraste qualquer um dos arquivos `.json` acima para a tela do ComfyUI e aproveite a aceleração nativa de `fp16` dos Tensor Cores da placa, gerando vídeos em cerca de **8 a 10 minutos**!
