graph TD
    subgraph " "
        A[Input Text: "Би OpenAI-ийн 2-р model-ийг train хийлээ."]
    end

    subgraph "1. Text Frontend & Normalizer"
        B(Normalizer)
        C{Lexicons}
        C -- "Numbers, Abbreviations, English Tokens" --> B
    end

    subgraph "2. Word-level G2P"
        D(Tokenizer)
        E(WordG2P Engine)
        F{Rules & Exception Dictionary}
        F -- "Context rules, Vowel harmony, 'ь', 'е', etc." --> E
    end

    subgraph "3. Phrase-level Phonology"
        G(Phrase Processor)
        H{Junction Rules & Prosody}
        H -- "Pauses, Assimilation, Intonation" --> G
    end

    subgrap<ctrl60>Тэгье, `russian_g2p`-ээс санаа авсан **Монгол G2P архитектурын skeleton**-ийг зуръя. Энэ нь зөвхөн код биш, илүүтэйгээр **модуль, файл, класс, функц, өгөгдлийн урсгал**-ыг харуулсан зураглал/төлөвлөгөө байна. ([github.com](https://github.com/nsu-ai/russian_g2p?utm_source=openai))

Гол зорилго:
- **Modular** (засвар хийх, сайжруулахад амар)
- **Extensible** (шинэ дүрэм, үг нэмэхэд хялбар)
- **TTS/ASR-д ээлтэй** (phonetic dictionary, Coqui/VITS integration боломжтой)

---

# 1. Ерөнхий архитектурын зураглал

Энэ бол өгөгдөл ямар замаар урсаж, G2P pipeline ажиллахыг харуулсан high-level зураглал.

```mermaid
graph TD
    subgraph " "
        A[Input Text: "��и OpenAI-ийн 2-р model-ийг train хийлээ."]
    end

    subgraph "1. Text Frontend & Normalizer"
        B(Normalizer)
        C{Lexicons}
        C -- "Numbers, Abbreviations, English Tokens" --> B
    end

    subgraph "2. Word-level G2P"
        D(Tokenizer)
        E(WordG2P Engine)
        F{Rules & Exception Dictionary}
        F -- "Context rules, Vowel harmony, 'ь', 'е', etc." --> E
    end

    subgraph "3. Phrase-level Phonology"
        G(Phrase Processor)
        H{Junction Rules & Prosody}
        H -- "Pauses, Assimilation, Intonation" --> G
    end

    subgraph " "
        I[Output Phonemes: ["b", "i", "<sp>", "o", "ʊ", "p", "ə", "n", "e", "j", "a", "j", ...]]
    end

    A --> B
    B --> D
    D --> E
    E --> G
    G --> I
