# LLM-Based Text Translator

An LLM-based API for translating EPUBs and other text using local or API-hosted language models.

## Why & What

I like reading... a lot. The problem is that many books aren't accessible to everyone because of language barriers. With the rise of increasingly capable LLMs, I saw an opportunity to improve upon traditional machine translation and make translated books more enjoyable and consistent.

This project takes source text and translates it using an LLM. In addition to the translation itself, you can provide a dictionary containing specific terms, names, expressions, or other terminology that should be translated consistently throughout the text.

The goal isn't simply to produce a literal translation, but to give the LLM enough context and guidance to produce a more natural, consistent and story-appropriate result. Anyone who has read a machine translated book knows the frustration of constant gender swaps, name changes and so on 

## Why just one HTML file? 
Honestly, I didn't see the need to setup a whole python or other environment for this. Browsers these days are powerful enough to do a lot of things. I also wanted something I can just start hassle free within a few seconds without having to worry about updating packages or running into compatibility errors. In theory, if you know how to double-click a html-file and download Ollama, you're all set on using it. (Do be sure to adjust the settings to your needs)

## Translation Modes
The translator currently supports several different modes depending on how much accuracy, consistency, and processing time you want to spend.

Auditing can be disabled for all modes by just not selecting a model.

### Sequential Dual — Default
The default mode uses a three-model pipeline:
1. The first LLM produces a translation.
2. The second LLM independently produces another translation.
3. A third LLM compares the results and performs a final audit, producing the final version.

This approach generally produces better results because the final model has multiple translations to compare against instead of having to rely entirely on a single output. 
The downside is speed. Because the models run sequentially, this mode can take a considerable amount of time.

Recommended for individual chapters or smaller amounts of text where translation quality is more important than speed.
I would generally not recommend this mode for translating an entire book, unless you have a lot of processing time available.

### Parallel Dual
Parallel Dual follows the same basic approach as Sequential Dual, but the two initial translations are run in parallel before being passed to the final auditing model.

In theory, this should significantly reduce the total processing time compared to Sequential Dual. However, I haven't been able to properly test the performance myself because my GPU isn't powerful enough to run the models simultaneously.

Recommended for users with enough GPU resources to run multiple models at the same time.

### Single Model
This is the mode I use most often.

It uses a single LLM to perform the translation, making it significantly faster than the dual-model approaches. The trade-off is that you lose the additional comparison in the auditing step (it still compares the original text to what was translated). For large-scale translation, such as translating an entire book, this is generally the most practical option.

When I want to translate a large amount of text quickly, I will also sometimes disable the final audit to save even more processing time. To disable it you just don't choose a model.

Recommended for bulk translation, casual translations and entire books where processing time is a major consideration.

## Models
Choosing the right model depends heavily on the languages you are translating from and to. Model size, prompting, dictionary entries, and the model itself can all have a significant impact on the final result.

My testing has focused primarily on Korean translations, so some of the prompts, placeholders, and recommendations are specifically geared toward Korean. Results may vary considerably for other language pairs.

### Model Size
There is a noticeable difference between models in the 14B and 27B range. You can try lower models too but I haven't had great results with those. A 14B model can be considerably faster and less demanding on hardware than a 27B while still producing a good translation. For large projects, this difference in speed can become very significant.

For auditing, I also recommend using a different model from the one used for translation** when possible. Having a different model review the translation can provide more variation and may help catch mistakes that the original model would otherwise repeat.

## Models I Tested
These are the models I have personally tested. The results below are based primarily on my Korean translation testing.

### Translation Models
- TranslateGemma 12B / 27B — One of the best models I tested for translation.
- Qwen 3 14B / Qwen 3.6 27B — Not great, not terrible. Performance depended heavily on the prompt and dictionary.
- HY-MT2 — The results were underwhelming in my testing.
- DeepSeek — Has potential, but I wasn't able to get particularly good results with my current prompts.
- Mistral — Translation quality wasn't particularly good in my testing.
- Gemma 3 — Also produced less-than-ideal results for my use case.
- EXAONE — Produced a lot of mistakes in my testing.

### Auditing Models
Auditing turned out to be more difficult than expected. With most of the models I tested, having the model "improve" an existing translation actually made the translation worse.

The two models that mostly avoided this problem in my testing were:
- Qwen 3 14B
- Qwen 3.6 27B

These were the only models I found that reliably improved or preserved the quality of the translation during the auditing step rather than introducing additional errors.

Here the prompt is also super important and not that easy to get right for all models. You need to make the model output only what it decided was the better text. A lot of model began explaining why they chose it or inserted their opinion which was a real headache. 

## A Note on Results
These model recommendations are based on my own testing and are not intended to be definitive benchmarks. With how fast models are currently evolving and every language output will differ, only see the above models as an aid when looking for your own models. Do your research to find the model you need to do your translations.

Translation quality can vary significantly depending on:
- The source language
- The target language
- The type of text being translated
- The model being used
- The model's quantization
- The system prompt
- The translation prompt
- The dictionary provided
- Available context
- Hardware and inference settings

A model that performs poorly for one language pair may perform extremely well for another.

If you are getting poor results, experimenting with the prompt and dictionary may be just as important as changing the model.

## Who Is This For?
This project is primarily aimed at people who want to read something that has no active human translators on it. There are many amazing stories where the translators have dropped it or just never got translated in the first place. It's also for those who want to experiment with LLM-based book and text translation, particularly when traditional machine translation doesn't produce the style or consistency they are looking for.


## Current Focus
The project has primarily been developed and tested around Korean to English translation, so expect some Korean-specific assumptions and placeholders in the current prompts and configuration.

Other languages will work, but your results will likely depend on the models available for your particular language pair.

Most of this project was also coded with Claude. I did add code where it kept running into walls or was unable to fix errors appropriately, but other than that, Claude did a lot here. Because of this, some features it implemented are untested. For example the API with other online models are untested. In theory these will deliver the best translation results but my goal was to be able to do everything locally.

While it's mostly a finished thing and fully usable, I probably will do some more fine tuning and adding some other features when I have the time.
