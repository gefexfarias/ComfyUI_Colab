# 🎬 MiniMax H3 no Google Colab (Tesla T4) & Local ComfyUI

Repositório contendo o fluxo otimizado do **MiniMax H3** (modelo de difusão multimodal de vídeo + áudio sincronizado) com **Turbo LoRA**, adaptado para execução em hardware com recursos limitados de memória (como a GPU Tesla T4 gratuita do Google Colab e placas como a RTX 2060).

---

## 🚀 Destaques do Projeto

* **Suporte Completo a GPU Tesla T4:** Pipeline dividido em 3 estágios independentes (Encode $\rightarrow$ Sample $\rightarrow$ Decode) para contornar o limite de 12,7 GB de RAM do Colab sem sofrer com *Out of Memory (OOM)*.
* **Integração Nativa com Google Drive:**
  * Escaneia e detecta automaticamente a pasta `Meu Drive/ComfyUI/models/`.
  * Se os modelos já existirem no Drive, reutiliza imediatamente sem gastar banda ou tempo.
  * Se faltarem modelos, baixa direto para as subpastas corretas (`diffusion_models`, `loras`, `vae`, `text_encoders`).
* **Turbo LoRA (4 a 6 Passos):** Aceleração do sampling de ~20 passos para apenas 4 a 6 passos mantendo sincronia labial/áudio.
* **Prevenção de Tela Preta / NaNs:** Configurado com `--bf16-unet` e `fp32 RMSNorm` nos tensores residuais para estabilidade matemática total.
* **Pronto para 1 Clique (*Run All*):** O notebook possui guarda de GPU e roda do início ao fim sem requerer ajustes manuais.

---

## 📁 Estrutura do Repositório

```text
├── MiniMax_H3_on_Colab_T4.ipynb   # Notebook principal otimizado para o Google Colab
├── README.md                      # Documentação principal do projeto
├── LEIAME.md                      # Guia passo a passo de execução em português
└── .gitignore                     # Filtro para ignorar modelos pesados e saídas temporárias
```

---

## ⚡ Como Rodar no Google Colab

1. Acesse o [Google Colab](https://colab.research.google.com/).
2. Faça o upload do notebook [`MiniMax_H3_on_Colab_T4.ipynb`](./MiniMax_H3_on_Colab_T4.ipynb).
3. No menu **Ambiente de execução** $\rightarrow$ **Alterar tipo de ambiente de execução**, selecione **GPU T4**.
4. (Opcional) Edite o `PROMPT` desejado na célula do **Stage 0**.
5. Clique em **Ambiente de execução** $\rightarrow$ **Executar tudo** (*Run all*).
6. Quando solicitado, autorize a conexão com o seu Google Drive.

O vídeo gerado (5,2 segundos com som) será salvo em `Meu Drive/H3/MiniMax-H3-Native-Colab/video/` e reproduzido na tela final do notebook.

---

## 📦 Modelos Utilizados

| Componente | Arquivo | Repositório Hugging Face | Tamanho |
| :--- | :--- | :--- | :--- |
| **DiT Transformer** | `MiniMax-H3-FL2VA-Pruned-Q4_K_M.gguf` | `Abiray/MiniMax-H3-Pruned-GGUF` | ~10,77 GB |
| **Turbo LoRA** | `minimax_h3_turbo_v4_step600_ema.safetensors` | `larryvrh/MiniMax-H3-Turbo-Lora` | ~0,73 GB |
| **Video VAE (fp16)** | `minimax_h3_video_vae_fp16.safetensors` | `Comfy-Org/MiniMax-H3` | ~4,85 GB |
| **Audio VAE (fp32)** | `minimax_h3_audio_vae_fp32.safetensors` | `Comfy-Org/MiniMax-H3` | ~0,56 GB |
| **Text Encoder** | `qwen3vl_32b_minimax_h3_nvfp4_awq.safetensors` | `Comfy-Org/MiniMax-H3` | ~14,61 GB |

---

## 💻 Execução Local (Ex: RTX 2060 / 3060 com 32GB RAM)

O mesmo fluxo pode ser executado localmente no ComfyUI instalando as extensões:
* [ComfyUI-GGUF](https://github.com/city96/ComfyUI-GGUF)
* [ComfyUI-MiniMax-H3-Turbo](https://github.com/Larryvrh/ComfyUI-MiniMax-H3-Turbo)

Em placas com 6 GB de VRAM e computadores com 32 GB de RAM, utilize o argumento `--lowvram` para aproveitar o *layer offloading* rápido via barramento PCIe.
