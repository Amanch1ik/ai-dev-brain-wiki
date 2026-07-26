# Озвучка (VO) и аудио-микс для промо

Как получить профессиональную закадровую озвучку и свести звук для промо-роликов: скрипт под голос, SSML/voice-settings, нормализация громкости (LUFS), ducking, EQ и борьба с «роботностью». Аудио-слой промо-видео ([[tool-hyperframes]]).

## Содержание

### Скрипт под VO
- Предложения **≤15 слов**, разговорный тон — писать «для уха», не «для глаза».
- Читать вслух при написании: споткнулся — переписал.
- **Line breaks = паузы** (надёжнее пунктуации: TTS не всегда держит паузу на точке).
- Пометки `(pause)` / `(beat)` для ритма.

### SSML (Azure — эталон разметки)
Azure Speech поддерживает наиболее полный SSML:
- **`<break time="750ms"/>`** — пауза; или `strength`: x-weak ≈250ms … weak … medium … strong … x-strong ≈1250ms.
- **`<prosody rate pitch volume contour>`** — `rate` 0.5–2× (или %), `pitch` ± (Hz/%/st), `contour` задаёт интонационную кривую по фразе (например `(0%,+20%) (50%,-10%)`).
- **`<emphasis level="strong|moderate|reduced">`** — акцент на слове.
- **`<mstts:express-as style styledegree role>`** — эмоциональный стиль; `styledegree` 0.01–2; стили: `advertisement_upbeat`, `cheerful`, `narration-professional`, `newscast` и др.

**ElevenLabs** — почти только `<break time>`; остальное задаётся через **voice settings** и (в v3) audio-tags вида `[excited]`, `[whispers]`.

⚠ Проверять теги под конкретную модель/голос — набор поддерживаемых стилей и тегов различается.

### Voice settings (ElevenLabs)
- **stability** 0.0–1.0: ниже = эмоциональнее/вариативнее, выше = ровнее. Монотонно звучит → **понижать stability первым**.
- **similarity** ≈ 0.75 (близость к оригиналу голоса).
- **style** 0.0 по умолчанию (усиление стилевой экспрессии).
- Пресет news / professional: **stability 0.8, similarity 0.6, style 0.0**.

**HeyGen:** `speed` 0.5–1.5, `pitch` ±50, emotion-теги.

### LUFS — целевая громкость
Нормализуй под площадку (integrated loudness):
- **YouTube / TikTok / Reels ≈ −14 LUFS**, True Peak ≤ **−1.5 dBTP**.
- **Apple Podcasts −16 LUFS**.
- **Broadcast −23 LUFS** (EBU R128).

ffmpeg, двухпроходный loudnorm (точнее однопроходного):
```bash
# проход 1 — измерить
ffmpeg -i in.wav -af loudnorm=I=-14:TP=-1.5:LRA=11:print_format=json -f null -
# проход 2 — применить измеренные значения (measured_*), linear=true для точности
ffmpeg -i in.wav -af loudnorm=I=-14:TP=-1.5:LRA=11:measured_I=...:measured_TP=...:measured_LRA=...:measured_thresh=...:linear=true out.wav
```

### Ducking (музыка под голосом)
Музыка проседает **6–12 dB** под голосом. sidechain-компрессия: ratio **3–6**, attack **30–150ms**, release **500–800ms** (плавный возврат музыки).
```bash
# голос как триггер: делим VO на «звук в микс» и «сигнал детектора»
ffmpeg -i music.wav -i voice.wav -filter_complex \
"[1:a]asplit=2[vo][sc]; \
 [0:a][sc]sidechaincompress=threshold=0.05:ratio=4:attack=50:release=600[duck]; \
 [duck][vo]amix=inputs=2:duration=longest" out.wav
```
⚠ проверить синтаксис под свою версию ffmpeg.

### EQ голоса (порядок: сначала cuts, потом boosts)
1. **High-pass 80Hz** — срез низкочастотного гула/рокота.
2. **−3dB @ ~350Hz** — убрать «муть»/бубнёж.
3. **De-esser** — приглушить свистящие «с/ш» (~5–8kHz).
4. **+2–3dB @ 2–3kHz** — presence, разборчивость.
5. **Air @ 10kHz+** — студийный «блеск».

### Anti-AI-tells озвучки
- **Монотонность** — вари rate/pitch по блокам: hook быстрее/выше, CTA медленнее/увереннее. Понизить stability.
- **Неверные ударения** — транскрипция брендов/имён через фонетику или SSML `<phoneme>`.
- **Flat-стиль на весь ролик** — разные `express-as` / эмоции по секциям.
- Fade музыки **in/out 2–4с** — резкий старт/обрыв выдаёт непрофессиональный монтаж.

## Связано с

- [[tool-hyperframes]] — аудио-дорожка промо-видео, собираемого в MP4
- [[method-premium-creative-web]] — общий стандарт «дорого, не дёшево» на звук
- [[method-presentation-design]] — синхронизация озвучки со слайдами
- [[method-motion-design-craft]] — sync движения с озвучкой и битом

## Источник

- learn.microsoft.com/azure/ai-services/speech-service/speech-synthesis-markup-voice · .../speech-synthesis-markup-structure — Azure SSML (эталон)
- elevenlabs.io/docs/api-reference/voices/settings/update — voice settings
- elevenlabs.io/blog/how-to-make-text-to-speech-sound-less-robotic — анти-роботность
- help.heygen.com/en/articles/11202248 — HeyGen voice controls
- ffmpeglab.com/articles/ffmpeg-audio-mixing-amix-guide — ffmpeg amix/ducking ⚠ вторичный
- 343labs.com/vocal-eq-cheat-sheet — vocal EQ ⚠ вторичный
- clickyapps.com — LUFS targets ⚠ вторичный
