# 🎬 Guia de Modos de Geração e Catálogo de Modelos (MiniMax H3)

Este guia documenta os **3 modos de geração de vídeo** suportados pelo **MiniMax H3** no ComfyUI, detalhando as diferenças entre eles, o hardware recomendado e os **links diretos oficiais para download de todos os modelos e templates**.

---

## 📌 1. Os 3 Modos de Geração do MiniMax H3

O MiniMax H3 é uma arquitetura de difusão multimodal (áudio + vídeo nativos sincronizados) dividida em duas variantes de DiT: **FL2VA** e **Ref2VA**.

### A) Text-to-Video (T2V) — Texto para Vídeo
* **Como funciona:** Você fornece apenas uma descrição em texto (prompt) com direção de cena, movimentos de câmera e efeitos sonoros desejados. O modelo sintetiza o vídeo e o áudio do zero.
* **Modelo DiT Utilizado:** `MiniMax-H3-FL2VA-Pruned-Q4_K_M.gguf`
* **Template Oficial ComfyUI:** [`video_minimax_h3_t2v.json`](https://github.com/Comfy-Org/workflow_templates/blob/main/templates/video_minimax_h3_t2v.json)
* **Melhor para:** Cenas conceituais, cenários, planos gerais, transições e tomadas onde você não precisa de um personagem pré-existente específico.

---

### B) Image-to-Video (I2V / FL2VA) — Imagem para Vídeo (Frame Inicial/Final)
* **Como funciona:** Você fornece uma imagem estática que atua **obrigatoriamente como o primeiro frame (*start frame*)** ou como primeiro e último frame do vídeo. O modelo anima a cena a partir dessa imagem estática.
* **Modelo DiT Utilizado:** `MiniMax-H3-FL2VA-Pruned-Q4_K_M.gguf`
* **Template Oficial ComfyUI:** [`video_minimax_h3_i2v.json`](https://github.com/Comfy-Org/workflow_templates/blob/main/templates/video_minimax_h3_i2v.json)
* **Limitação:** A câmera e a ação ficam "presas" ao enquadramento da foto inicial.
* **Melhor para:** Dar vida a uma foto ou imagem parada, mantendo exatamente aquele ponto de partida.

---

### C) Reference-to-Video (R2V / Ref2VA) — Referência para Vídeo (Consistência de Personagem)
* **Como funciona:** Este é o modo mais avançado para cinema e criação de histórias. A imagem **NÃO** fica travada no primeiro frame. Em vez disso, você fornece **até 9 imagens de referência** de um mesmo personagem (frente, perfil, corpo inteiro, expressões geradas no `z-image`) e até referências de áudio para clonagem de voz.
* **Vantagem revolucionária:** A cena pode começar de **qualquer ângulo, distância ou ação** (ex: correndo de costas, close no rosto, dirigindo um carro), e o modelo "veste" e projeta o rosto, cabelo e figurino do seu personagem ao longo de todo o vídeo com fidelidade extrema.
* **Modelo DiT Utilizado:** `MiniMax-H3-Ref2VA-Pruned-Q4_K_M.gguf`
* **Template Oficial ComfyUI:** [`video_minimax_h3_r2v.json`](https://github.com/Comfy-Org/workflow_templates/blob/main/templates/video_minimax_h3_r2v.json)
* **Melhor para:** Séries, filmes com atores virtuais consistentes, clipes musicais e qualquer produção onde o mesmo personagem precise aparecer em múltiplas cenas diferentes.

---

## 📥 2. Catálogo Oficial de Modelos e Links de Download

Para rodar qualquer um dos modos acima no ComfyUI, os arquivos devem ser colocados nas subpastas correspondentes dentro de `ComfyUI/models/`.

### 1. Modelos Principais de Difusão (DiT GGUF)
> Salvar em: `ComfyUI/models/diffusion_models/` (ou `models/unet/`)

| Modo de Uso | Arquivo | Repositório Hugging Face | Tamanho | Link Direto |
| :--- | :--- | :--- | :--- | :--- |
| **T2V & I2V** (Texto e Frame Inicial) | `MiniMax-H3-FL2VA-Pruned-Q4_K_M.gguf` | `Abiray/MiniMax-H3-Pruned-GGUF` | ~10,77 GB | [Baixar FL2VA Q4_K_M](https://huggingface.co/Abiray/MiniMax-H3-Pruned-GGUF/resolve/main/MiniMax-H3-FL2VA-Pruned-Q4_K_M.gguf) |
| **R2V** (Consistência de Personagem) | `MiniMax-H3-Ref2VA-Pruned-Q4_K_M.gguf` | `Abiray/MiniMax-H3-Pruned-GGUF` | ~10,77 GB | [Baixar Ref2VA Q4_K_M](https://huggingface.co/Abiray/MiniMax-H3-Pruned-GGUF/resolve/main/MiniMax-H3-Ref2VA-Pruned-Q4_K_M.gguf) |
| *(Opcional - Mais Leve)* | `MiniMax-H3-FL2VA-Pruned-Q3_K_M.gguf` | `Abiray/MiniMax-H3-Pruned-GGUF` | ~8,90 GB | [Baixar FL2VA Q3_K_M](https://huggingface.co/Abiray/MiniMax-H3-Pruned-GGUF/resolve/main/MiniMax-H3-FL2VA-Pruned-Q3_K_M.gguf) |

---

### 2. Turbo LoRA (Acelera de 20 para 4 a 6 Passos)
> Salvar em: `ComfyUI/models/loras/`

| Arquivo | Repositório Hugging Face | Tamanho | Link Direto |
| :--- | :--- | :--- | :--- |
| `minimax_h3_turbo_v4_step600_ema.safetensors` *(Recomendado)* | `larryvrh/MiniMax-H3-Turbo-Lora` | ~0,73 GB | [Baixar Turbo LoRA v4](https://huggingface.co/larryvrh/MiniMax-H3-Turbo-Lora/resolve/main/minimax_h3_turbo_v4_step600_ema.safetensors) |

---

### 3. Codificador de Texto (Text Encoder)
> Salvar em: `ComfyUI/models/text_encoders/` (ou `models/clip/`)

*Necessário para codificar novos prompts textuais. É compartilhado por todos os modos (T2V, I2V e R2V).*

| Arquivo | Repositório Hugging Face | Tamanho | Link Direto |
| :--- | :--- | :--- | :--- |
| `qwen3vl_32b_minimax_h3_nvfp4_awq.safetensors` | `Comfy-Org/MiniMax-H3` | ~14,61 GB | [Baixar Text Encoder AWQ](https://huggingface.co/Comfy-Org/MiniMax-H3/resolve/main/text_encoders/qwen3vl_32b_minimax_h3_nvfp4_awq.safetensors) |

---

### 4. VAEs (Vídeo e Áudio Sincronizado)
> Salvar em: `ComfyUI/models/vae/`

*Ambos são obrigatórios para decodificar o vídeo final em MP4 com áudio estéreo nativo.*

| Tipo | Arquivo | Repositório Hugging Face | Tamanho | Link Direto |
| :--- | :--- | :--- | :--- | :--- |
| **Video VAE** | `minimax_h3_video_vae_fp16.safetensors` | `Comfy-Org/MiniMax-H3` | ~4,85 GB | [Baixar Video VAE](https://huggingface.co/Comfy-Org/MiniMax-H3/resolve/main/vae/minimax_h3_video_vae_fp16.safetensors) |
| **Audio VAE** | `minimax_h3_audio_vae_fp32.safetensors` | `Comfy-Org/MiniMax-H3` | ~0,56 GB | [Baixar Audio VAE](https://huggingface.co/Comfy-Org/MiniMax-H3/resolve/main/vae/minimax_h3_audio_vae_fp32.safetensors) |

---

## 🎨 3. Templates Oficiais ComfyUI (.JSON)

Você pode baixar os templates oficiais e arrastá-los diretamente para a tela do ComfyUI:

1. **Text to Video:** [`video_minimax_h3_t2v.json`](https://raw.githubusercontent.com/Comfy-Org/workflow_templates/main/templates/video_minimax_h3_t2v.json)
2. **Image to Video:** [`video_minimax_h3_i2v.json`](https://raw.githubusercontent.com/Comfy-Org/workflow_templates/main/templates/video_minimax_h3_i2v.json)
3. **Reference to Video:** [`video_minimax_h3_r2v.json`](https://raw.githubusercontent.com/Comfy-Org/workflow_templates/main/templates/video_minimax_h3_r2v.json)

---

## 🖥️ 4. Guia de Hardware e Otimização

### A) No Google Colab (Tesla T4 - Gratuito)
* Utilize o notebook [`MiniMax_H3_on_Colab_T4.ipynb`](./MiniMax_H3_on_Colab_T4.ipynb) deste repositório.
* Ele divide a execução em 3 estágios (Encode $\rightarrow$ Sample $\rightarrow$ Decode) para não estourar os 12,7 GB de RAM do Colab gratuito.
* Tempo médio por vídeo de 5s: **~35 a 40 minutos**.

### B) No PC Local (RTX 2060 6GB VRAM + 32GB RAM)
* Execute o ComfyUI localmente com a interface visual completa no navegador.
* Adicione o argumento de inicialização no `.bat`:
  ```bat
  python main.py --lowvram
  ```
* O ComfyUI utilizará os seus **32 GB de RAM** para armazenar os modelos e descarregar os blocos para a RTX 2060 via PCIe sem travar.
* Tempo médio por vídeo de 5s na RTX 2060: **~8 a 10 minutos** (graças à aceleração nativa de FP16 dos núcleos tensores da placa).
