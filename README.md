# tweedledumming
A manual Multi-Agent Debate technique. Give AI #1 your prompt. Give AI #2 the prompt and AI #1's response; ask it to fact-check, render a verdict addressed to AI #1, and generate a corrective prompt. Paste back. Repeat until both AIs converge. Two browser tabs. No code.

What it is
AI systems generate confident, well-organized, plausible-sounding nonsense. They fill gaps in their knowledge with assumptions and present speculation as fact. Most users can't catch this because they lack the expertise to verify what the AI tells them — which is often why they asked in the first place.
Tweedledumming solves this by recruiting a second AI to audit the first. Neither AI is trusted by default. Each becomes the other's skeptical reviewer. You act as the relay between them, pasting outputs back and forth until both systems converge on validated, grounded responses.
Researchers at MIT, Stanford, and elsewhere have been publishing on this since 2023 under the name Multi-Agent Debate (MAD). Their implementations are automated and require an engineering team. Tweedledumming is manual, slower, and teachable to anyone with a lunch break.
The method

Ask your question to AI #1.
Give AI #2 your original prompt and AI #1's response.
Ask AI #2 to identify unsupported assumptions and generate a corrective prompt addressed directly to AI #1. Use this instruction: "Phrase your verdict in the second person as if you're addressing the LLM directly. If you find hallucination, give me a corrective prompt I can paste back word-for-word. Skip any preface — give me the verdict and corrective prompt as one paste-ready block."
Paste AI #2's output into AI #1.
Send AI #1's revised response back to AI #2.
Repeat until both systems largely agree.
Ask AI #2 to answer the original question independently.
Compare both final responses and combine the strongest validated elements.

Why the second-person instruction matters
The critical prompt mechanic is "phrase your verdict in the second person as if you're addressing the LLM directly." This converts AI #2's critique into a paste-ready correction. You don't summarize it, translate it, or editorialize. You copy it and paste it. The human in this loop is the relay, not the editor.
What it catches

Hallucinated citations and invented facts
Confident assertions made without sufficient context
Logical overreach — conclusions that don't follow from the evidence provided
Missing context the AI should have requested before answering

What it doesn't do
Tweedledumming raises the floor on quality. It does not remove the ceiling on risk. It does not guarantee correctness. Human judgment remains essential, particularly for legal, regulatory, medical, scientific, or policy decisions.
Setup
Two browser tabs. Any two conversational AI systems. Claude and ChatGPT work well. Two instances of the same AI in separate sessions also work — the adversarial structure matters more than the brand.
References
Du, Y., Li, S., Torralba, A., Tenenbaum, J.B., & Mordatch, I. (2024). Improving factuality and reasoning in language models through multiagent debate. Proceedings of ICML 2024, Vol. 235, Article 467. https://dl.acm.org/doi/10.5555/3692070.3692537
Liang, T., He, Z., Jiao, W., et al. (2024). Encouraging divergent thinking in large language models through multi-agent debate. Proceedings of EMNLP 2024, pp. 17889–17904. https://aclanthology.org/2024.emnlp-main.992/
Author
Basil J. White with Beth A. Martin. Documented at https://github.com/basilwhite/tweedledumming
