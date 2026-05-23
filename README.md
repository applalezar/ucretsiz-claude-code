<div align="center">

# 🤖 Free Claude Code

Claude-code'u terminalde, VSCode uzantısında veya OpenClaw gibi Discord'da ücretsiz olarak kullanın (ses desteği mevcuttur).

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg?style=for-the-badge)](https://opensource.org/licenses/MIT)
[![Python 3.14](https://img.shields.io/badge/python-3.14-3776ab.svg?style=for-the-badge&logo=python&logoColor=white)](https://www.python.org/downloads/)
[![uv](https://img.shields.io/endpoint?url=https://raw.githubusercontent.com/astral-sh/uv/main/assets/badge/v0.json&style=for-the-badge)](https://github.com/astral-sh/uv)
[![Tested with Pytest](https://img.shields.io/badge/testing-Pytest-00c0ff.svg?style=for-the-badge)](https://github.com/Alishahryar1/free-claude-code/actions/workflows/tests.yml)
[![Type checking: Ty](https://img.shields.io/badge/type%20checking-ty-ffcc00.svg?style=for-the-badge)](https://pypi.org/project/ty/)
[![Code style: Ruff](https://img.shields.io/badge/code%20formatting-ruff-f5a623.svg?style=for-the-badge)](https://github.com/astral-sh/ruff)
[![Logging: Loguru](https://img.shields.io/badge/logging-loguru-4ecdc4.svg?style=for-the-badge)](https://github.com/Delgan/loguru)

Ücretsiz Claude Code, Anthropic Messages API trafiğini Claude Code'dan herhangi bir sağlayıcıya yönlendirir. Claude Code'un istemci tarafı protokolünün istikrarlı kalmasını sağlarken, ücretsiz, ücretli veya yerel modeller arasından seçim yapmanıza olanak tanır.

[Quick Start](#quick-start) · [Providers](#choose-a-provider) · [Clients](#connect-claude-code) · [Integrations](#optional-integrations) · [Development](#development)

</div>

<div align="center">
  <img src="assets/pic.png" alt="Free Claude Code in action" width="700">
</div>

## Star History

<div align="center">
  <a href="https://star-history.com/#Alishahryar1/free-claude-code&Date">
    <picture>
      <source media="(prefers-color-scheme: dark)" srcset="https://api.star-history.com/svg?repos=Alishahryar1/free-claude-code&type=Date&theme=dark">
      <source media="(prefers-color-scheme: light)" srcset="https://api.star-history.com/svg?repos=Alishahryar1/free-claude-code&type=Date">
      <img alt="Star History Chart" src="https://api.star-history.com/svg?repos=Alishahryar1/free-claude-code&type=Date" width="700">
    </picture>
  </a>
</div>

## Neler Elde Edersiniz

- Claude Code'un Anthropic API çağrıları için hazır proxy.

- On bir sağlayıcı arka ucu: NVIDIA NIM, Kimi, Wafer, OpenRouter, DeepSeek, LM Studio, llama.cpp, Ollama, OpenCode Zen, OpenCode Go ve Z.ai.

- Modele özel yönlendirme: Opus, Sonnet, Haiku ve yedek trafiği farklı sağlayıcılara yönlendirin.

- Proxy'nin `/v1/models` uç noktası aracılığıyla yerel Claude Code `/model` seçici desteği (Claude Code'un Gateway model keşfine katılması gerekir; bkz. [Model Seçici](#model-picker)).

- Akış, araç kullanımı, mantık/düşünme bloğu işleme ve yerel istek optimizasyonları.

- Uzaktan kodlama oturumları için isteğe bağlı Discord veya Telegram bot sarmalayıcısı.

- VSCode uzantısı aracılığıyla isteğe bağlı kullanım.

- Yerel Whisper veya NVIDIA NIM aracılığıyla isteğe bağlı sesli not transkripsiyonu.

- Desteklenen proxy ayarlarını düzenlemek, değişiklikleri doğrulamak ve sağlayıcıları kontrol etmek için `/admin` adresindeki yerel **Yönetici Arayüzü** (yalnızca yerel ağ erişimi).

## Hızlı Başlangıç

### 1. Hızlı Kurulum

Claude Code, uv, Python 3.14.0 ve Free Claude Code'u kurun veya güncelleyin:

macOS/Linux:

```bash
curl -fsSL "https://github.com/Alishahryar1/free-claude-code/blob/main/scripts/install.sh?raw=1" | sh
```

Windows PowerShell:

```powershell
irm "https://github.com/Alishahryar1/free-claude-code/blob/main/scripts/install.ps1?raw=1" | iex
```

Review the installers at [scripts/install.sh](https://github.com/Alishahryar1/free-claude-code/blob/main/scripts/install.sh) and [scripts/install.ps1](https://github.com/Alishahryar1/free-claude-code/blob/main/scripts/install.ps1).

### 2. Start The Proxy

```bash
fcc-server
```

Uygulama başlatıldıktan sonra, Uvicorn proxy bağlantı adresini yazdırır ve uygulama yönetici URL'sini kaydeder:

```text
INFO:     Admin UI: http://127.0.0.1:8082/admin (local-only)
```
Birçok terminal bunları tıklanabilir hale getirir. `8082` değilse, yapılandırılmış `PORT` değerinizi kullanın.

### 3. Yönetici Arayüzünü Açın ve NVIDIA NIM'i Yapılandırın

Terminal çıktısından **Yönetici Arayüzü** URL'sini açın.

Bir NVIDIA NIM API anahtarına mı ihtiyacınız var? Aşağıdaki **[NVIDIA NIM sağlayıcısı](#nvidia-nim-provider)** bölümünü kullanın, ardından buraya geri dönün.
<div align="center">
  <img src="assets/admin-page.png" alt="Local admin UI for proxy settings" width="700">
</div>

NVIDIA NIM API anahtarınızı `NVIDIA_NIM_API_KEY` alanına yapıştırın, ardından **Doğrula** ve **Uygula** düğmelerine tıklayın.

Varsayılan model zaten `nvidia_nim/nvidia/nemotron-3-super-120b-a12b` olarak ayarlanmıştır. Bunu daha sonra aynı Yönetici Arayüzünden değiştirebilirsiniz.

### 4. Run Claude Code

```bash
fcc-claude
```

`fcc-claude`, her başlatıldığında mevcut yapılandırılmış portu ve kimlik doğrulama belirtecini okur, Claude Code ortam değişkenlerini (otomatik sıkıştırma için 190k belirteçli `CLAUDE_CODE_AUTO_COMPACT_WINDOW` dahil) ayarlar ve ardından gerçek `claude` komutunu başlatır.

## Bir Sağlayıcı Seçin

Bir sağlayıcı seçin, anahtarını veya yerel URL'sini Yönetici Arayüzüne girin ve `MODEL`'i sağlayıcı önekli bir model slug'ına ayarlayın. `MODEL` yedek sağlayıcıdır. `MODEL_OPUS`, `MODEL_SONNET` ve `MODEL_HAIKU`, Claude Code'un model katmanları için yönlendirmeyi geçersiz kılabilir.
<a id="nvidia-nim-provider"></a>

### 1. [NVIDIA NIM](https://build.nvidia.com/)

Get a key at [build.nvidia.com/settings/api-keys](https://build.nvidia.com/settings/api-keys).

In the Admin UI, paste it into `NVIDIA_NIM_API_KEY`. The default `MODEL` is `nvidia_nim/nvidia/nemotron-3-super-120b-a12b`.

Popular examples:

- `nvidia_nim/nvidia/nemotron-3-super-120b-a12b`
- `nvidia_nim/z-ai/glm5.1`
- `nvidia_nim/moonshotai/kimi-k2.5`
- `nvidia_nim/minimaxai/minimax-m2.5`

Browse models at [build.nvidia.com](https://build.nvidia.com/explore/discover).

### 2. [Kimi](https://platform.moonshot.ai/)

Get a key at [platform.moonshot.ai/console/api-keys](https://platform.moonshot.ai/console/api-keys).

In the Admin UI, paste it into `KIMI_API_KEY`, then set `MODEL` to a Kimi slug such as `kimi/kimi-k2.5`.

Browse models at [platform.moonshot.ai](https://platform.moonshot.ai).

### 3. [Wafer](https://wafer.ai/)

Get a key from [wafer.ai](https://wafer.ai). In the Admin UI, paste it into `WAFER_API_KEY`, then set `MODEL` to a Wafer Pass model such as `wafer/DeepSeek-V4-Pro`.

Popular examples:

- `wafer/DeepSeek-V4-Pro`
- `wafer/MiniMax-M2.7`
- `wafer/Qwen3.5-397B-A17B`
- `wafer/GLM-5.1`

This provider uses Wafer's Anthropic-compatible endpoint at `https://pass.wafer.ai/v1/messages`.

### 4. [OpenRouter](https://openrouter.ai/)

Get a key at [openrouter.ai/keys](https://openrouter.ai/keys).

In the Admin UI, paste it into `OPENROUTER_API_KEY`, then set `MODEL` to an OpenRouter slug such as `open_router/stepfun/step-3.5-flash:free`.

Browse [all models](https://openrouter.ai/models) or [free models](https://openrouter.ai/collections/free-models).

### 5. [DeepSeek](https://platform.deepseek.com/)

Get a key at [platform.deepseek.com/api_keys](https://platform.deepseek.com/api_keys).

In the Admin UI, paste it into `DEEPSEEK_API_KEY`, then set `MODEL` to a DeepSeek slug such as `deepseek/deepseek-chat`.

This provider uses DeepSeek's Anthropic-compatible endpoint, not the OpenAI chat-completions endpoint.

### 6. [LM Studio](https://lmstudio.ai/)

Start LM Studio's local server and load a model. In the Admin UI, keep or update `LM_STUDIO_BASE_URL`, then set `MODEL` to the model identifier shown by LM Studio, prefixed with `lmstudio/`.

Prefer models with tool-use support for Claude Code workflows.

### 7. [llama.cpp](https://github.com/ggml-org/llama.cpp)

Start `llama-server` with an Anthropic-compatible `/v1/messages` endpoint and enough context for Claude Code requests.

In the Admin UI, keep or update `LLAMACPP_BASE_URL`, then set `MODEL` to the local model slug, prefixed with `llamacpp/`.

For local coding models, context size matters. If llama.cpp returns HTTP 400 for normal Claude Code requests, increase `--ctx-size` and verify the model/server build supports the requested features.

### 8. [Ollama](https://ollama.com/)

Run Ollama and pull a model:

```bash
ollama pull llama3.1
ollama serve
```

In the Admin UI, keep or update `OLLAMA_BASE_URL`, then set `MODEL` to the same tag shown by `ollama list`, prefixed with `ollama/`.

`OLLAMA_BASE_URL` is the Ollama server root; do not append `/v1`. Example model slugs include `ollama/llama3.1` and `ollama/llama3.1:8b`.

### 9. [OpenCode Zen](https://opencode.ai/)

Get an API key at [opencode.ai/auth](https://opencode.ai/auth).

In the Admin UI, paste it into `OPENCODE_API_KEY`, then set `MODEL` to an OpenCode Zen model slug such as `opencode/gpt-5.3-codex`. The same `OPENCODE_API_KEY` powers **OpenCode Go** (below); use `opencode_go/` slugs there.

OpenCode Zen is a curated model gateway that provides access to models from Anthropic, OpenAI, Google, DeepSeek, and more through a single API key and OpenAI-compatible endpoint at `https://opencode.ai/zen/v1`.

Popular examples:

- `opencode/gpt-5.3-codex`
- `opencode/claude-sonnet-4`
- `opencode/deepseek-v4-flash-free` (free)
- `opencode/gemini-3-flash`
- `opencode/big-pickle` (free)
- `opencode/glm-5.1`

[opencode.ai](https://opencode.ai) adresinde mevcut modelleri inceleyin.

### 10. [OpenCode Go](https://opencode.ai/)

[opencode.ai/auth](https://opencode.ai/auth) adresinden bir API anahtarı alın (OpenCode Zen ile aynı).

Yönetici arayüzünde, `OPENCODE_API_KEY` kullanın, ardından `MODEL`'i `opencode_go/minimax-m2.7` gibi bir OpenCode Go model slug'ına ayarlayın.

OpenCode Go, kendi derlenmiş kataloğuna ve `https://opencode.ai/zen/go/v1` adresinde OpenAI uyumlu uç noktasına sahip bir abonelik ağ geçididir. Zen ile **aynı OpenCode API anahtarını** paylaşır; yalnızca slug öneki (`opencode_go/` yerine `opencode/`) ve yukarı akış yolu farklıdır.

Popüler örnekler:

- `opencode_go/minimax-m2.7`

Mevcut modelleri [opencode.ai](https://opencode.ai) adresinden inceleyin.

### 11. [Z.ai](https://z.ai/)

[Z.ai/manage-apikey/apikey-list](https://z.ai/manage-apikey/apikey-list) adresinden bir API anahtarı alın.

Yönetici arayüzünde, anahtarı `ZAI_API_KEY` alanına yapıştırın, ardından `MODEL` alanını `zai/glm-5.1` gibi bir Z.ai model slug'ına ayarlayın.

Z.ai, GLM modellerini OpenAI uyumlu Kodlama Planı uç noktası aracılığıyla `https://api.z.ai/api/coding/paas/v4` adresinden sağlar.

Popüler örnekler:

- `zai/glm-5.1`
- `zai/glm-5-turbo`

Modelleri [Z.ai](https://z.ai) adresinden inceleyebilirsiniz.

### 12. Model Katmanına Göre Sağlayıcıları Karıştırma

Her model katmanı, Yönetici Arayüzünde `MODEL_OPUS`, `MODEL_SONNET` ve `MODEL_HAIKU` ayarlarını yaparak farklı bir sağlayıcı kullanabilir. Bir katmanı boş bırakarak `MODEL` sağlayıcısını devralabilirsiniz.

Örneğin, Opus'u `nvidia_nim/moonshotai/kimi-k2.5`'e, Sonnet'i `open_router/deepseek/deepseek-r1-0528:free`'ye, Haiku'yu `lmstudio/unsloth/GLM-4.7-Flash-GGUF`'e yönlendirebilir ve yedek `MODEL` sunucusunu `zai/glm-5.1`'de tutabilirsiniz.

## Claude Code'a Bağlanma

### 1. Claude Code CLI

Terminal kullanımı için, kurulu başlatıcıyı tercih edin:

```bash
fcc-claude
```

Çalışırken `fcc-server`'ı çalışır durumda tutun. Yönetici Arayüzü proxy yapılandırmasını yönetir, çalışma zamanı ayarları değiştiğinde sunucuyu yeniden başlatır ve `fcc-claude`, her başlatıldığında mevcut Yönetici Arayüzü tarafından yönetilen portu ve kimlik doğrulama belirtecini okur. Ayrıca otomatik sıkıştırma için `CLAUDE_CODE_AUTO_COMPACT_WINDOW` değerini `190000` olarak ayarlar.

### 2. VS Code Eklentisi

Ayarları açın, `claude-code.environmentVariables` ifadesini arayın, **settings.json dosyasında düzenle** seçeneğini seçin ve aşağıdakileri ekleyin:

```json
"claudeCode.environmentVariables": [
{ "name": "ANTHROPIC_BASE_URL", "value": "http://localhost:8082" },

{ "name": "ANTHROPIC_AUTH_TOKEN", "value": "freecc" },

{ "name": "CLAUDE_CODE_ENABLE_GATEWAY_MODEL_DISCOVERY", "value": "1" },

{ "name": "CLAUDE_CODE_AUTO_COMPACT_WINDOW", "value": "190000" }
]
```

Eklentiyi yeniden yükleyin. Eğer eklenti bir giriş ekranı gösteriyorsa, Anthropic Console yolunu bir kez seçin; ortam değişkenleri etkinleştirildikten sonra bile yerel proxy model trafiğini yönetmeye devam eder.

### 3. JetBrains ACP

Kurulu Claude ACP yapılandırmasını düzenleyin:

- Windows: `C:\Users\%USERNAME%\AppData\Roaming\JetBrains\acp-agents\installed.json`
- Linux/macOS: `~/.jetbrains/acp.json`

`acp.registry.claude-acp` için ortam değişkenlerini ayarlayın:

```json
"env": {

"ANTHROPIC_BASE_URL": "http://localhost:8082",

"ANTHROPIC_AUTH_TOKEN": "freecc",

"CLAUDE_CODE_ENABLE_GATEWAY_MODEL_DISCOVERY": "1",

"CLAUDE_CODE_AUTO_COMPACT_WINDOW": "190000"
}
```

Dosyayı değiştirdikten sonra IDE'yi yeniden başlatın.

### 4. Model Seçici

<div align="center">

<img src="assets/cc-model-picker.png" alt="Claude Code model seçici, ağ geçidi modellerini gösteriyor" width="700">
</div>

## İsteğe Bağlı Entegrasyonlar

Aşağıdaki her entegrasyon için, **yönetilen proxy ayarlarını** yalnızca `/admin` adresindeki **Yönetici Arayüzünde** değiştirin: alanları düzenleyin, **Doğrula**'ya tıklayın, ardından **Uygula**'ya tıklayın. Altbilgi, yönetilen yapılandırmanın nerede saklandığını gösterir; bu README, bu dosyayı elle düzenlemeyi adım adım anlatmaz.

### 1. Discord ve Telegram Botları

Bot sarmalayıcı, Claude Code oturumlarını uzaktan çalıştırır, ilerlemeyi yayınlar, yanıt tabanlı konuşma dallarını destekler ve görevleri durdurabilir veya temizleyebilir.

**Discord**

1. Botu [Discord Geliştirici Portalı](https://discord.com/developers/applications) üzerinden oluşturun.

2. **Mesaj İçerik Amacı**'nı etkinleştirin.

3. Botu okuma, gönderme ve mesaj geçmişi izinleriyle davet edin.

4. Botun yanıt vermesi gereken kanal kimliğini (veya kimliklerini) ve bot token'ını kopyalayın.

**Telegram**

1. [@BotFather](https://t.me/BotFather) ile bir bot oluşturun ve bot token'ını kopyalayın.

2. Sadece sizin botu kullanabilmeniz için [@userinfobot](https://t.me/userinfobot) adresinden sayısal kullanıcı kimliğinizi alın.

**Yönetici Arayüzünde Yapılandırma**

1. `fcc-server` çalışırken, terminal çıktısından **Yönetici Arayüzü** URL'sini açın.

2. Yan çubukta **Mesajlaşma**'yı seçin.

3. **Mesajlaşma Platformu**'nu **Discord** veya **Telegram** olarak ayarlayın.

4. Discord için **Discord Bot Token** ve **İzin Verilen Discord Kanalları**'nı yapıştırın. Telegram için **Telegram Bot Token** ve **İzin Verilen Telegram Kullanıcı Kimliği**'ni yapıştırın.

5. **İzin Verilen Dizin**'i, proxy'yi çalıştıran makinedeki mutlak bir yola (botun kullanabileceği çalışma alanı kökü) ayarlayın.

6. **Doğrula**'ya, ardından **Uygula**'ya tıklayın. Kullanıcı arayüzü yeniden başlatmanın gerekli olduğunu söylüyorsa sunucuyu yeniden başlatın.

<div align="center">

<img src="assets/admin-messaging.png" alt="Bot ve ses ayarlarıyla Yönetici Arayüzü Mesajlaşma görünümü" width="700">
</div>

<p align="center"><em>Yönetici Arayüzü
→ Mesajlaşma (platform, botlar ve Ses)</em></p>

**Faydalı komutlar**

- `/stop` bir görevi iptal eder; yalnızca o dalı durdurmak için bir görev mesajına yanıt verin.

- `/clear` oturumları sıfırlar; bir dalı temizlemek için yanıt verin.

- `/stats` oturum durumunu gösterir.

### 2. Sesli Notlar

Sesli notlar, [Free Claude Code kurulumunuzu](#1-fast-install) ilgili isteğe bağlı eklerle genişlettikten sonra Discord ve Telegram'da çalışır.

macOS/Linux:

```bash
# NVIDIA NIM transkripsiyonu (Riva gRPC)
curl -fsSL "https://github.com/Alishahryar1/free-claude-code/blob/main/scripts/install.sh?raw=1" | sh -s -- --voice-nim

# Yerel Whisper (CPU veya CUDA)
curl -fsSL "https://github.com/Alishahryar1/free-claude-code/blob/main/scripts/install.sh?raw=1" | sh -s -- --voice-local

# Her iki arka uç
curl -fsSL "https://github.com/Alishahryar1/free-claude-code/blob/main/scripts/install.sh?raw=1" | sh -s -- --voice-all

# CUDA ile Yerel Whisper
curl -fsSL "https://github.com/Alishahryar1/free-claude-code/blob/main/scripts/install.sh?raw=1" | sh -s -- --voice-local --torch-backend cu130
```

Windows PowerShell:

```powershell
# NVIDIA NIM transkripsiyonu (Riva gRPC)
& ([scriptblock]::Create((irm "https://github.com/Alishahryar1/free-claude-code/blob/main/scripts/install.ps1?raw=1"))) -VoiceNim

# Yerel Whisper (CPU veya CUDA)
& ([scriptblock]::Create((irm "https://github.com/Alishahryar1/free-claude-code/blob/main/scripts/install.ps1?raw=1"))) -VoiceLocal

# Her iki arka uç
& ([scriptblock]::Create((irm "https://github.com/Alishahryar1/free-claude-code/blob/main/scripts/install.ps1?raw=1"))) -VoiceAll

# CUDA ile Yerel Fısıltı
& ([scriptblock]::Create((irm "https://github.com/Alishahryar1/free-claude-code/blob/main/scripts/install.ps1?raw=1"))) -VoiceLocal -TorchBackend cu130
```

Yeniden yükledikten sonra `fcc-server`'ı yeniden başlatın.

**Yönetici Arayüzünde**, **Mesajlaşma**'yı açın ve **Ses**'e kaydırın. **Sesli Notlar**'ı açın, **Fısıltı Aygıtı**'nı (`cpu`, `cuda` veya `nvidia_nim`) seçin, **Fısıltı Modeli**'ni ayarlayın ve kurulumunuz gerektirdiğinde **Sarılma Yüzü Belirteci**'ni girin. **nvidia_nim** transkripsiyonu için, `voice` eklentisini yükleyin ve **Sağlayıcılar** görünümünde **NVIDIA NIM API Anahtarı**'nı ayarlayın. Yukarıdaki ekran görüntüsü, aynı görünümde **Ses** bloğunu göstermektedir.

## Nasıl Çalışır

<div align="center">

<img src="assets/how-it-works.svg" alt="Free Claude Code istek akışı mimarisi" width="900">
</div>

Diyagram kaynağı: [`assets/how-it-works.mmd`](assets/how-it-works.mmd).

Önemli noktalar:

- FastAPI, `/v1/messages`, `/v1/messages/count_tokens` ve `/v1/models` gibi Anthropic uyumlu rotaları sunar.

- Model yönlendirmesi, Claude model adını `MODEL_OPUS`, `MODEL_SONNET`, `MODEL_HAIKU` veya `MODEL` olarak çözümler.

- NIM, OpenCode Zen, OpenCode Go, Z.ai, OpenAI sohbet akışını Anthropic SSE'ye çevirerek kullanır.

- Wafer, OpenRouter, DeepSeek, LM Studio, llama.cpp ve Ollama, Anthropic Mesajlar tarzı taşıma protokollerini kullanır.

- Proxy, düşünme bloklarını, araç çağrılarını, token kullanım meta verilerini ve sağlayıcı hatalarını Claude Code'un beklediği şekle dönüştürür.

- İstek optimizasyonları, gecikmeyi ve kotayı azaltmak için önemsiz Claude Code sorgularını yerel olarak yanıtlar.

## Geliştirme

### 1. Proje Yapısı

```text
free-claude-code/
├── server.py # ASGI giriş noktası
├── api/ # FastAPI rotaları, servis katmanı, yönlendirme, optimizasyonlar
├── core/ # Paylaşılan Antropik protokol yardımcıları ve SSE yardımcı programları
├── providers/ # Sağlayıcı taşıma mekanizmaları, kayıt defteri, hız sınırlama
├── messaging/ # Discord/Telegram adaptörleri, oturumlar, ses
├── cli/ # Paket giriş noktaları ve Claude işlem yönetimi
├── config/ # Ayarlar, sağlayıcı kataloğu, günlük kaydı
└── tests/ # Birim ve sözleşme testleri
```

### 2. Kaynaktan Çalıştırma

Geliştirme yapıyorsanız veya doğrudan çalıştırmak istiyorsanız bu yolu kullanın. Çıkış:

```bash
git clone https://github.com/Alishahryar1/free-claude-code.git
cd free-claude-code
uv run uvicorn server:app --host 0.0.0.0 --port 8082
```

### 3. Komutlar

```bash
uv run ruff format
uv run ruff check
uv run ty check
uv run pytest
```

Push işleminden önce bu komutları bu sırayla çalıştırın. CI aynı kontrolleri uygular.

### 4. Paket Komut Dosyaları

`pyproject.toml` şunları kurar:

- `fcc-server`: yapılandırılmış ana bilgisayar ve port ile proxy'yi başlatır.

- `fcc-init`: `~/.fcc/.env` için isteğe bağlı gelişmiş iskelet; normal yapılandırma için **Yönetici Arayüzü**'nü tercih edin.

- `fcc-claude`: Yapılandırılmış yerel proxy URL'si, kimlik doğrulama belirteci, model keşif bayrağı ve otomatik sıkıştırma için 190k `CLAUDE_CODE_AUTO_COMPACT_WINDOW` ile Claude Code'u başlatır.

- `free-claude-code`: `fcc-server` için uyumluluk takma adı.

### 5. Genişletme

- `OpenAIChatTransport`'u genişleterek OpenAI uyumlu sağlayıcılar ekleyin.

- `AnthropicMessagesTransport`'u genişleterek Anthropic Mesaj sağlayıcıları ekleyin.

- Sağlayıcı meta verilerini `config.provider_catalog`'da ve fabrika bağlantılarını `providers.registry`'de kaydedin.

- `messaging/`'da `MessagingPlatform` arayüzünü uygulayarak mesajlaşma platformları ekleyin.

## Katkıda Bulunma

- [`.env.example`](.env.exampl)
  e) Katkıda bulunanlar için ortam anahtar adlarını salt okunur bir referans olarak listeler; yönetilen proxy ayarlarını değiştirmek için **Yönetici Arayüzünü** kullanın.

- Hataları ve özellik isteklerini [Sorunlar](https://github.com/Alishahryar1/free-claude-code/issues) bölümünde bildirin.

- Değişiklikleri küçük tutun ve odaklanmış testlerle kapsayın.

- Docker entegrasyonu için PR açmayın.

- README değişikliği için PR açmayın, bunun için bir sorun bildirin.

- Bir çekme isteği açmadan önce tam kontrol dizisini çalıştırın.

- `except X, Y` sözdizimi Python 3.14 final sürümünde geri getirildi (3.14 alfa sürümünde değil). PR açmadan önce bunu aklınızda bulundurun.

## Lisans

MIT Lisansı. Ayrıntılar için [LICENSE](LICENSE) bölümüne bakın.
