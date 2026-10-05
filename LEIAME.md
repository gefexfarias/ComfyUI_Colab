# MiniMax H3 no Google Colab (Tesla T4) com Google Drive

Este diretório contém o notebook [`MiniMax_H3_on_Colab_T4.ipynb`](file:///c:/AppsIA/Comfyui/MiniMax_H3_on_Colab_T4.ipynb) configurado para **armazenar e reutilizar seus modelos diretamente na sua pasta do ComfyUI no Google Drive (`Meu Drive/ComfyUI/models/`)**.

---

## 🌟 O que foi adicionado especialmente para o seu Google Drive:

1. **Varredura Prévia Inteligente:**
   * Antes de baixar qualquer arquivo, o notebook escaneia a sua pasta `ComfyUI/models/` no Google Drive.
   * Ele confere tanto os caminhos padrões quanto pastas alternativas (ex: checa `diffusion_models` e `unet`; `text_encoders` e `clip`).
   * **Se o modelo já existir no seu Drive:** Ele exibe `[✅ JÁ EXISTE NO DRIVE]` e **NÃO baixa novamente**, economizando tempo e banda.

2. **Download Direto para as Subpastas do Drive:**
   * Se algum modelo estiver faltando, o notebook faz o download **diretamente para a pasta correta no seu Google Drive**:
     * `models/diffusion_models/` $\rightarrow$ `MiniMax-H3-FL2VA-Pruned-Q4_K_M.gguf` (10,77 GiB)
     * `models/loras/` $\rightarrow$ `minimax_h3_turbo_v4_step600_ema.safetensors` (0,73 GiB)
     * `models/vae/` $\rightarrow$ `minimax_h3_video_vae_fp16.safetensors` (4,85 GiB)
     * `models/vae/` $\rightarrow$ `minimax_h3_audio_vae_fp32.safetensors` (0,56 GiB)
     * `models/text_encoders/` $\rightarrow$ `qwen3vl_32b_minimax_h3_nvfp4_awq.safetensors` (14,61 GiB)
   * **Vantagem:** Os downloads são feitos **apenas uma vez na vida**! Nas próximas vezes que você abrir o Colab, todos os modelos já estarão no seu Drive prontos para uso imediato.

3. **Links Simbólicos Automáticos no ComfyUI:**
   * O notebook cria links simbólicos instantâneos no ComfyUI da máquina virtual apontando para os arquivos do seu Google Drive. Isso significa **zero duplicação de arquivos e zero consumo desnecessário de disco temporário**.

4. **Stage 7 -- Video Upscaler (1080p / 4K + Áudio Preservado):**
   * Super-resolução neural frame a frame na GPU com os modelos `4x-UltraSharp` ou `RealESRGAN_x4plus`.
   * Preserva a trilha de som estéreo original do MiniMax H3 com sincronia milimétrica.
   * Streaming FFmpeg direto em memória (zero arquivos temporários soltos no disco).
   * Salva os vídeos em alta resolução em `MyDrive/H3/MiniMax-H3-Native-Colab/video_upscaled/`.
   * Modelos de upscaling armazenados permanentemente em `MyDrive/ComfyUI/models/upscale_models/`.

5. **Guarda de GPU T4:**
   * Verifica se o acelerador está como GPU T4 logo no primeiro segundo de execução.

---

## 🚀 Como Executar no Google Colab (1 Clique)

1. Abra o [Google Colab](https://colab.research.google.com/).
2. Clique na aba **Fazer upload** (*Upload*) e envie o arquivo [`MiniMax_H3_on_Colab_T4.ipynb`](file:///c:/AppsIA/Comfyui/MiniMax_H3_on_Colab_T4.ipynb).
3. Verifique se o acelerador está como **GPU T4**:
   * Menu: **Ambiente de execução** $\rightarrow$ **Alterar tipo de ambiente de execução** $\rightarrow$ selecione **GPU T4**.
4. Clique em **Ambiente de execução** $\rightarrow$ **Executar tudo** (*Run all*).
5. Se aparecer o pop-up solicitando acesso ao Google Drive, clique em **Conectar ao Google Drive**.

O notebook cuidará de todo o resto sozinho, desde a geração do vídeo bruto até o upscaling em Full HD/4K!
