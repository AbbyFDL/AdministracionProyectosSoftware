# 1. Evolución de los modelos

*Tarea 2 · Administración de Proyectos de Software (1151055) · UAM Azcapotzalco · 26-O*

## 1.1 Qué es un modelo de lenguaje

Un **modelo de lenguaje** es un modelo probabilístico que asigna probabilidades a secuencias de texto. En la práctica, dado un contexto (las palabras o *tokens* anteriores), estima cuál es el siguiente token más probable [3]. Los primeros modelos eran estadísticos: contaban qué tan seguido aparecían secuencias cortas de palabras (modelos de *n*-gramas) [3]. Hoy se usan redes neuronales que aprenden esas probabilidades a partir de enormes cantidades de texto.

## 1.2 Cómo se llegó al modelo grande (LLM)

El paso decisivo fue la arquitectura **Transformer**, basada en mecanismos de atención, que permite entrenar modelos mucho más grandes y procesar el texto en paralelo [8]. A partir de ahí, la receta fue escalar: más parámetros, más datos y más cómputo. Se observó que el error de los modelos de lenguaje mejora de forma regular y predecible cuando crecen estos tres factores [4]. GPT-3, con 175 000 millones de parámetros, mostró que un modelo de ese tamaño puede resolver tareas nuevas con solo unos ejemplos incluidos en el propio texto de entrada, sin reentrenarlo [1].

Después se mejoró su utilidad: el ajuste con retroalimentación humana hizo que los modelos siguieran mejor las instrucciones de las personas [6].

| Etapa | Qué cambió | Fuente |
|---|---|---|
| Modelos de *n*-gramas | Probabilidades de palabras a partir de conteos | [3] |
| Transformer | Atención y entrenamiento en paralelo | [8] |
| Escalado | Más parámetros, datos y cómputo mejoran el modelo de forma predecible | [4] |
| Modelos de miles de millones de parámetros | Aprendizaje con pocos ejemplos en el contexto | [1] |
| Ajuste con retroalimentación humana | Mejor seguimiento de instrucciones | [6] |

## 1.3 Modelos con razonamiento explícito

Un **modelo con razonamiento explícito** genera pasos intermedios (una "cadena de pensamiento") antes de dar su respuesta final. Se observó primero que pedirle a un modelo que razonara paso a paso mejoraba su desempeño en problemas de lógica y matemáticas [9].

**Esa capacidad no aparece solo por hacer más grande el modelo.** Viene de dos ingredientes:

1. **Técnicas de entrenamiento.** Se entrena al modelo con aprendizaje por refuerzo, premiando que llegue a respuestas correctas, de modo que aprende a producir razonamientos largos antes de responder [2][5].
2. **Cómputo adicional al responder.** Dejar que el modelo "piense" más tiempo en cada pregunta puede dar mejores resultados que simplemente agrandar el modelo [7]. OpenAI describe que el rendimiento de su modelo o1 mejora tanto con más aprendizaje por refuerzo durante el entrenamiento como con más tiempo de pensamiento al responder [5].

| Factor | Cuándo ocurre | Qué aporta |
|---|---|---|
| Escalar el modelo | Entrenamiento | Más capacidad general [4] |
| Aprendizaje por refuerzo para razonar | Entrenamiento | Cadenas de razonamiento más largas y correctas [2][5] |
| Cómputo al responder | Uso del modelo | Más "tiempo de pensamiento" por pregunta [7][5] |

## Referencias

[1] Brown, T. B., Mann, B., Ryder, N., Subbiah, M., Kaplan, J., Dhariwal, P., Neelakantan, A., Shyam, P., Sastry, G., Askell, A., Agarwal, S., Herbert-Voss, A., Krueger, G., Henighan, T., Child, R., Ramesh, A., Ziegler, D. M., Wu, J., Winter, C., … Amodei, D. (2020). Language models are few-shot learners. *Advances in Neural Information Processing Systems, 33*. https://arxiv.org/abs/2005.14165

[2] DeepSeek-AI. (2025). *DeepSeek-R1: Incentivizing reasoning capability in LLMs via reinforcement learning* (arXiv:2501.12948). arXiv. https://arxiv.org/abs/2501.12948

[3] Jurafsky, D., & Martin, J. H. (2025). *Speech and language processing* (3.ª ed., borrador). Stanford University. https://web.stanford.edu/~jurafsky/slp3/

[4] Kaplan, J., McCandlish, S., Henighan, T., Brown, T. B., Chess, B., Child, R., Gray, S., Radford, A., Wu, J., & Amodei, D. (2020). *Scaling laws for neural language models* (arXiv:2001.08361). arXiv. https://arxiv.org/abs/2001.08361

[5] OpenAI. (2024, 12 de septiembre). *Learning to reason with LLMs*. https://openai.com/index/learning-to-reason-with-llms/

[6] Ouyang, L., Wu, J., Jiang, X., Almeida, D., Wainwright, C. L., Mishkin, P., Zhang, C., Agarwal, S., Slama, K., Ray, A., Schulman, J., Hilton, J., Kelton, F., Miller, L., Simens, M., Askell, A., Welinder, P., Christiano, P., Leike, J., & Lowe, R. (2022). *Training language models to follow instructions with human feedback* (arXiv:2203.02155). arXiv. https://arxiv.org/abs/2203.02155

[7] Snell, C., Lee, J., Xu, K., & Kumar, A. (2024). *Scaling LLM test-time compute optimally can be more effective than scaling model parameters* (arXiv:2408.03314). arXiv. https://arxiv.org/abs/2408.03314

[8] Vaswani, A., Shazeer, N., Parmar, N., Uszkoreit, J., Jones, L., Gomez, A. N., Kaiser, Ł., & Polosukhin, I. (2017). Attention is all you need. *Advances in Neural Information Processing Systems, 30*. https://arxiv.org/abs/1706.03762

[9] Wei, J., Wang, X., Schuurmans, D., Bosma, M., Ichter, B., Xia, F., Chi, E., Le, Q., & Zhou, D. (2022). Chain-of-thought prompting elicits reasoning in large language models. *Advances in Neural Information Processing Systems, 35*. https://arxiv.org/abs/2201.11903

## Declaración de uso de inteligencia artificial
Utilicé Claude (Anthropic) para realizar esta investigación.