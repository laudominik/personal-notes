# Safety of Large Language Models Beyond English

Measuring language resources [1]:

- number of unlabeled data
- number of labeled data for SFT
- dominated by english
- jailbreaks for lower resource languages - higher risks [2]
- unequal cost across languages [3]

Naive solution:

- NMT for all the labeled english data
- misses cultural nuance, has translation artifacts

SLR findings [4]
- nonenglish usually specialized datasets, not multilingual
- mostly benchmark-only
- mostly LLM-as-a-judge

RefusEU [5]
- rainbow teaming methodology
- multilingual
- 12 EU languagues
- safety ablation results: 
  - 70B models -> balanced languages decrease the ASR
  - takeaway: use balanced data for multilingual model

Multilingual Guardrails
- PL-Guard [6]

Multilingual llms
- Apertus
- EuroLLM

Polish-Silesian translation:
- rule based translation outperforms Google Translate

---

[1] Joshi, P., Santy, S., Budhiraja, A., Bali, K., & Choudhury, M. (2020, July). The state and fate of linguistic diversity and inclusion in the NLP world. In Proceedings of the 58th annual meeting of the association for computational linguistics (pp. 6282-6293).

[2] Rebedea, T., Derczynski, L., Ghosh, S., Sreedhar, M. N., Brahman, F., Jiang, L., ... & Choi, Y. (2025, July). Guardrails and security for LLMs: Safe, secure and controllable steering of LLM applications. In Proceedings of the 63rd Annual Meeting of the Association for Computational Linguistics (Volume 5: Tutorial Abstracts) (pp. 13-15).

[3] Ahia, O., Kumar, S., Gonen, H., Kasai, J., Mortensen, D. R., Smith, N. A., & Tsvetkov, Y. (2023, December). Do all languages cost the same? tokenization in the era of commercial language models. In Proceedings of the 2023 Conference on Empirical Methods in Natural Language Processing (pp. 9904-9923).

[4] Krasnodębska, A., Dziewulska, K., Seweryn, K., Chrabaszcz, M., & Kusa, W. (2026, March). Safety of large language models beyond English: A systematic literature review of risks, biases, and safeguards. In Proceedings of the 19th Conference of the European Chapter of the Association for Computational Linguistics (Volume 1: Long Papers) (pp. 1003-1034).

[5] Krasnodębska, A., Kusa, W., & Lipani, A. (2026, July). Multilingual Refusal Alignment for Safer Large Language Models. In Findings of the Association for Computational Linguistics: ACL 2026 (pp. 30769-30790).

[6] Krasnodębska, A., Seweryn, K., Łukasik, S., & Kusa, W. (2025, July). Pl-guard: Benchmarking language model safety for polish. In Proceedings of the 10th Workshop on Slavic Natural Language Processing (Slavic NLP 2025) (pp. 25-37).