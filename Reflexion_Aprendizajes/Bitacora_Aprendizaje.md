---
# [⬅️](../Reflexion_Aprendizajes)Contenidos
# 💡Bitacora Aprendizajes


 Bitácora de Aprendizaje — Unidad 2: Inferencia Estadística
**Estudiante:** Mateo Sebastian Pucha Carrera
**Grupo:** G — Teoría de la Distribución y Probabilidad
**Proyecto Integrador:** Análisis regional — Provincia de Loja
**Período cubierto:** APE 06 – APE 11
 
---
 
## 1. Introducción
 
Esta bitácora resume mi proceso de aprendizaje a lo largo de la Unidad 2, en la que pasamos de describir el comportamiento probabilístico de una sola variable (distribución Normal) hasta comparar formalmente grupos completos dentro del dataset regional de Loja (ANOVA y Tukey). La documento en orden cronológico, práctica por práctica, señalando qué entendí, dónde me trabé y cómo lo resolví.
 
---
 
## 2. APE 06 — Distribuciones Continuas Notables (Normal, Z-scores, Shapiro-Wilk)
 
**Qué aprendí:**
Aquí entendí por primera vez la diferencia entre trabajar con una probabilidad puntual (que siempre es cero en una variable continua) y trabajar con áreas bajo la curva. Aprendí a estandarizar mis datos regionales con puntajes Z y a interpretarlos: un Z=-2.45, por ejemplo, no es solo un número, es "qué tan lejos está este cantón del promedio, en unidades de desviación estándar".
 
**Dificultad algorítmica superada:**
Mi primer instinto fue asumir que la variable `% de viviendas sin alcantarillado` se comportaba de forma normal solo porque "se veía como una campana" en el histograma. La prueba de Shapiro-Wilk me demostró lo contrario (p ≈ 0.00004): el cantón Loja, al ser la capital, actúa como un valor atípico que rompe la simetría del resto de cantones. Tuve que aprender a no confiar solo en la inspección visual y a complementar siempre el gráfico Q-Q con una prueba analítica antes de sacar conclusiones.
 
**Reflexión:**
Esta práctica me enseñó una lección que se repitió en toda la unidad: *validar los supuestos antes de aplicar cualquier fórmula*, en vez de aplicar la prueba primero y preguntar después.
 
---
 
## 3. APE 07 — Teorema del Límite Central y Remuestreo (Bootstrapping)
 
**Qué aprendí:**
Aquí resolví la contradicción que dejó pendiente la APE 06: si mis datos NO son normales, ¿cómo puedo seguir haciendo inferencia sobre ellos? La respuesta fue el Teorema del Límite Central. Al generar 500 muestras bootstrap de tamaño n=40 sobre mi variable regional, comprobé con mis propios ojos que la distribución de las *medias* muestrales sí se aproxima a una normal, aunque la población original (con su fuerte asimetría) nunca lo haga.
 
**Dificultad algorítmica superada:**
Al principio confundía "muestra" con "población" dentro del bucle de remuestreo, lo que me generaba una distribución muestral idéntica a la original (sin efecto de suavizado). Tuve que revisar con cuidado que cada iteración tomara una submuestra aleatoria nueva (`np.random.choice`) y calculara su propia media, en vez de reutilizar el array completo. También me costó entender por qué el error estándar disminuye con la raíz de n y no linealmente — repasar la Ley de los Grandes Números me ayudó a visualizarlo con la simulación.
 
**Reflexión:**
Entendí que el TLC no "arregla" la falta de normalidad de los datos individuales, sino que me da permiso metodológico para trabajar con las medias muestrales como si fueran normales, siempre que el tamaño de muestra sea razonable.
 
---
 
## 4. APE 09 — Prueba de Hipótesis Unimuestral (T de Student)
 
**Qué aprendí:**
Formulé por primera vez una hipótesis nula y alterna aplicada a un problema real de mi región ($H_0: \mu = 15$ vs $H_1: \mu > 15$, sobre el déficit de alcantarillado) y ejecuté una prueba T de Student de una cola. Aprendí a diferenciar cuándo usar Z (sigma poblacional conocida) frente a T (sigma desconocida, muestra pequeña) — algo que antes de esta práctica manejaba solo de memoria, sin entender el porqué.
 
**Dificultad algorítmica superada:**
Como Shapiro-Wilk ya me había advertido en la APE 06 que mis datos no eran normales, dudé si la prueba T seguía siendo válida. Tuve que investigar y agregar una prueba de respaldo no paramétrica (Wilcoxon) para confirmar que la conclusión no dependía de ese supuesto. Ver que ambas pruebas —paramétrica y no paramétrica— coincidían en el resultado (p prácticamente cero) me dio mucha más confianza en la robustez del hallazgo.
 
**Reflexión:**
Comprendí que el valor-p no es "la probabilidad de que mi hipótesis sea cierta", sino la probabilidad de observar un resultado tan extremo como el mío si la hipótesis nula fuera verdadera. Esa distinción, aunque parece sutil, cambió completamente cómo redacto mis conclusiones.
 
---
 
## 5. APE 10/11 — Comparación de Grupos (ANOVA de un factor y Tukey HSD)
 
**Qué aprendí:**
Pasé de comparar mi región contra un único valor teórico (APE 09) a comparar tres subgrupos de cantones entre sí (Grande, Intermedio, Pequeño) usando ANOVA de un factor, y a identificar con Tukey HSD exactamente entre qué pares de grupos se concentraban las diferencias.
 
**Dificultad algorítmica superada:**
Inicialmente no tenía claro por qué no podía simplemente correr tres pruebas T por pares en lugar de un ANOVA. Investigar el problema de la inflación del error Tipo I al hacer comparaciones múltiples me hizo entender por qué el ANOVA controla ese riesgo con un solo estadístico F antes de pasar al post-hoc. También tuve que aprender a leer la tabla de `anova_lm` de `statsmodels` — al principio no distinguía los grados de libertad del factor (k-1) de los residuales (N-k), y confundía cuál correspondía a cuál fila de la tabla.
 
**Reflexión:**
Aprendí que un resultado "no significativo" en el ANOVA también es información válida: no todo análisis tiene que terminar rechazando H0 para ser un buen análisis.
 
---
 
## 6. Autoevaluación General de la Unidad
 
**Lo que domino con confianza ahora:**
- Formular H0/H1 de forma correcta según el tipo de pregunta (una muestra, dos muestras, más de dos grupos).
- Elegir la prueba adecuada (Z, T, ANOVA) según lo que realmente sé o no sé de mis datos (varianza poblacional, tamaño de muestra, número de grupos).
- Verificar los supuestos (normalidad, homocedasticidad) antes de confiar en cualquier resultado, y saber a qué prueba alternativa recurrir si esos supuestos fallan.
**Lo que más me costó de la unidad:**
Sin duda, entender que la estadística inferencial exige un orden metodológico estricto: primero se valida, después se prueba, y solo al final se concluye. En las primeras prácticas quería "saltarme" la validación de supuestos para llegar directo al resultado, y eso me llevó a sacar conclusiones apresuradas más de una vez.
 
**Cómo lo superé:**
Repitiendo el mismo patrón de trabajo en cada práctica —hipótesis, supuestos, estadístico, valor-p, decisión— hasta que dejó de sentirse como una lista de pasos memorizada y empezó a sentirse como una forma lógica de razonar sobre datos reales.
 
**Aplicación al Proyecto Integrador:**
Todo lo trabajado en esta unidad se integró en el análisis final del déficit de alcantarillado en la provincia de Loja: partimos describiendo la variable (APE 06), justificamos por qué podíamos inferir sobre ella pese a su no-normalidad (APE 07), probamos una hipótesis puntual sobre la región (APE 09) y finalmente comparamos subgrupos territoriales entre sí (APE 10/11). Ver cómo cada práctica se apoyó en la anterior fue, para mí, la parte más valiosa del proceso.
