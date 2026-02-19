# Sensitivity Decay & Regime Rotation in Trading Systems

# Decay чувствительности и ротация режимов в торговых системах

---

## English

A formal framework that models **edge decay** and **strategy–regime ceiling** (the "biological barrier") and provides double-blind experiment design for validation and live trading.

### Idea

Repeated use of the same strategy on the same market regime leads to **sensitivity decay**: edge fades with exposure. No amount of "more technique" (indicators, optimization) within the same strategy–regime pair can sustainably break the ceiling—only **regime rotation** and **overload** (stronger signals in noisy regimes) restore responsiveness. The math is derived from behavioral models (habituation, ceiling effect, power-law adaptation) and mapped one-to-one to trading. The framework incorporates **2024–2026 research**: power-law decay, optimal information gain, statistical jump models, hybrid ML regime detection, and pre-registration with pre-analysis plans.

### Contents

| File | Description |
|------|-------------|
| [docs/WHITEPAPER_EN.md](docs/WHITEPAPER_EN.md) | Full whitepaper (English): Jessicka formulation, market mapping, **extended decay models** (exponential + power-law), **design of experiments** (double-blind + PAP) |
| [docs/WHITEPAPER_RU.md](docs/WHITEPAPER_RU.md) | Full whitepaper (Russian) |
| [LICENSE](LICENSE) | MIT (EN) |
| [LICENSE_RU.md](LICENSE_RU.md) | MIT (RU, unofficial translation) |

### Main Formulas

- **Sensitivity decay (exponential):** \(\sigma_{S,R}(n) = e^{-\lambda_{S,R} n}\)
- **Sensitivity decay (power-law, 2024+):** \(\sigma_{S,R}(n) = (1 + \beta n)^{-\eta}\)
- **Effective edge:** \(\mu_{S,R}(t) = \bar\mu_{S,R} \cdot \sigma_{S,R}(n_{S,R}(t))\)
- **Rotation rule:** Switch when \(\sigma < \theta\)
- **Overload threshold:** \(\tau_t = \tau_0(1 + \alpha V_t/\bar{V})\); position size \(w \propto \sigma\)

### Experiment Design

1. **Initial validation (double-blind):** Training / calibration / holdout split; anonymous strategy–regime labels; blinded parameter estimation; **pre-registration with Pre-Analysis Plan (PAP)**.
2. **Trading (double-blind):** Strategy–regime anonymization; regime classifier out-of-sample only; no look-ahead; evaluator blinded until period locked.

### Author

**Eduard Samokhvalov**

### License

MIT. See [LICENSE](LICENSE).

---

## Русский

Формальный фреймворк, моделирующий **затухание edge** и **потолок стратегия–режим** («биологический барьер»), с дизайном двойного слепого эксперимента для валидации и живой торговли.

### Идея

Повторное использование одной и той же стратегии в одном рыночном режиме приводит к **затуханию чувствительности**: edge ослабевает с экспозицией. Никакие дополнительные «техники» (индикаторы, оптимизация) в рамках той же пары стратегия–режим не могут устойчиво пробить потолок — только **ротация режимов** и **перегрузка** (более сильные сигналы в шумных режимах) восстанавливают отзывчивость. Математика выведена из поведенческих моделей (привыкание, эффект потолка, степенная адаптация) и отображена один-к-одному на торговлю. Фреймворк опирается на **исследования 2024–2026**: степенной decay, оптимальный информационный выигрыш, статистические jump-модели, гибридное ML-детектирование режимов, пре-регистрация с планом пре-анализа (PAP).

### Содержимое

| Файл | Описание |
|------|----------|
| [docs/WHITEPAPER_EN.md](docs/WHITEPAPER_EN.md) | Полный whitepaper (английский) |
| [docs/WHITEPAPER_RU.md](docs/WHITEPAPER_RU.md) | Полный whitepaper (русский): формулировка Джессики, маппинг на рынки, **расширенные модели decay** (экспоненциальная + степенная), **дизайн экспериментов** (двойной слепой + PAP) |
| [LICENSE](LICENSE) | MIT (EN) |
| [LICENSE_RU.md](LICENSE_RU.md) | MIT (RU, неофициальный перевод) |

### Основные формулы

- **Decay чувствительности (экспоненциальный):** \(\sigma_{S,R}(n) = e^{-\lambda_{S,R} n}\)
- **Decay чувствительности (степенной, 2024+):** \(\sigma_{S,R}(n) = (1 + \beta n)^{-\eta}\)
- **Эффективный edge:** \(\mu_{S,R}(t) = \bar\mu_{S,R} \cdot \sigma_{S,R}(n_{S,R}(t))\)
- **Правило ротации:** Переключаться при \(\sigma < \theta\)
- **Порог перегрузки:** \(\tau_t = \tau_0(1 + \alpha V_t/\bar{V})\); размер позиции \(w \propto \sigma\)

### Дизайн эксперимента

1. **Начальная валидация (двойной слепой):** Разделение train/calibration/holdout; анонимные метки стратегия–режим; слепая оценка параметров; **пре-регистрация с планом пре-анализа (PAP)**.
2. **Торговля (двойной слепой):** Анонимизация пар стратегия–режим; классификатор режимов только out-of-sample; без look-ahead; оценщик слеп до завершения периода.

### Автор

**Эдуард Самоквалов**

### Лицензия

MIT. См. [LICENSE](LICENSE).
