# Top 3 estrategias sistemáticas documentadas para operar el S&P 500 con el CFD US500

**Fecha de la investigación y de consulta de todas las fuentes:** 2026-10-12
**Alcance:** estrategias ya documentadas, sin diseñar ni modificar reglas. No hice backtests propios: todas las cifras vienen de fuentes abiertas o son cálculos simples marcados como [DERIVADO].

**Etiquetas:**
- [VERIFICADO]: el dato está en la fuente citada.
- [DERIVADO]: cálculo simple con datos citados; se muestra la fórmula.
- [INFERENCIA]: razonamiento propio sin dato directo.
- [NO REPORTADO]: la fuente no da el dato.

Las fuentes se citan como [F#]; la lista completa está en la sección 5.

**Convenciones usadas en los cálculos [DERIVADO]:**
- Puntos de CFD a puntos básicos (bp): bp = puntos / P × 10⁴. Uso P ≈ 6,000 como orden de magnitud. El SPY alcanzó un máximo de $608 en dic-2024 [VERIFICADO F1, FAQ Q23], y el índice ≈ 10 × SPY [INFERENCIA]. Con P = 6,000, un spread de 0.4 pt ≈ 0.67 bp. El coste en bp es inversamente proporcional al precio.
- Financiamiento nocturno del CFD (Pepperstone, jun-2026): noches × precio × tamaño × (ARR ± 2.5 %)/360. Con ARR = 4.31847 %, una posición larga paga 6.82 %/año [VERIFICADO F36]:
  - Frente al retorno total, la posición larga paga ≈ (4.31847 + 2.5) %/360 ≈ **1.89 bp por noche** [DERIVADO].
  - Frente al retorno en exceso (que ya descuenta la tasa libre de riesgo), el lastre es solo el margen del bróker: 2.5 %/360 ≈ **0.69 bp por noche** [DERIVADO]. Supone que ARR ≈ tasa libre de riesgo [INFERENCIA].

---

## 1. Top 3

| Rank | Estrategia | Puntuación (0–100) | Familia | Año del dato más reciente | Confianza |
|---|---|---|---|---|---|
| 1 | Prima por anuncios macroeconómicos programados (Savor & Wilson 2013; actualización Ai-Bansal-Guo 2023) | **73.0** | Calendario / anuncios macro | 2023 (ago-2023) [VERIFICADO F11] | Media |
| 2 | Momentum intradía "Noise Area" con trailing VWAP (Zarattini, Aziz & Barbon 2024/2025) | **69.0** | Momentum intradía | 2025 (ene-2025, actualización de los propios autores) [VERIFICADO F1] | Media |
| 3 | Momentum intradía de mercado en la última media hora (Gao et al. 2018; Baltussen et al. 2021) — **Estrategia más débil** | **68.0** | Momentum intradía (cierre) | 2020 a favor (may-2020, revisado por pares) [VERIFICADO F4]; 2024 condicional y desfavorable (documento de trabajo) [VERIFICADO F6] | Baja |

**Cómo se calculó la puntuación:** cada criterio recibe una nota de 0 a 5. Total = Σ(peso × nota) × 20. Las notas son un juicio propio [INFERENCIA] basado en la evidencia que cito.

| Estrategia | Evidencia (30 %) | Fuera de muestra (20 %) | Costos y ejecución (20 %) | Reglas (15 %) | Riesgo (15 %) | Total |
|---|---|---|---|---|---|---|
| 1. Anuncios macro | 4.0 | 3.5 | 3.5 | 4.0 | 3.0 | 73.0 |
| 2. Noise Area | 3.5 | 2.5 | 3.5 | 4.5 | 3.5 | 69.0 |
| 3. Última media hora | 3.5 | 2.0 | 3.0 | 5.0 | 4.0 | 68.0 |
| 4. (fuera) Cambio de mes | 3.0 | 2.5 | 3.5 | 5.0 | 3.0 | 66.0 |

**Advertencias del ranking:**
- La diferencia entre el puesto 3 (68.0) y el 4 (cambio de mes, 66.0) está dentro del margen de error de una rúbrica subjetiva [INFERENCIA]. El cambio de mes es la alternativa natural si la #3 se descarta en el backtest.
- Las estrategias #2 y #3 comparten mecanismo (cobertura de gamma y momentum intradía), así que es de esperar que estén correlacionadas [INFERENCIA]. Además, la #2 opera durante la ventana de 15:30 a 16:00 que usa la #3. No deberían contarse como dos fuentes independientes de ventaja.
- **Por qué la #3 es la "estrategia más débil":**
  - Su versión simple (señal = retorno de la primera media hora) dejó de funcionar fuera de muestra después de 2013, según un artículo revisado por pares [VERIFICADO F5].
  - Un documento de trabajo con datos 2018–2024 muestra que el momentum intradía desaparece o se invierte cuando los creadores de mercado tienen gamma positiva, régimen frecuente en la era de las opciones 0DTE [VERIFICADO F6].
  - Su ventaja bruta por operación es la más estrecha de las tres frente al spread del CFD [DERIVADO, ver ficha 3].

---

## 2. Fichas completas D0–D14

### FICHA 1 — Prima por anuncios macroeconómicos programados

#### D0. Identidad
- **Nombre y versión:** "Macroeconomic Announcement Premium" (MAP).
  - Versión original: Pavel Savor y Mungo Wilson, *JFQA* 48(2):343–375, 2013 [VERIFICADO F8].
  - Versión documentada aquí: el conjunto de anuncios de Ai & Bansal (2018), usado en la actualización 1961–2023 de Hengjie Ai, Ravi Bansal y Hongye Guo (NBER WP 31923, nov-2023, revisado dic-2023) [VERIFICADO F10, F11].
- **Familia:** efectos de calendario y anuncios macro.
- **Instrumento original:**
  - Savor & Wilson: índice CRSP ponderado por valor (NYSE/Nasdaq/Amex), con dividendos, en exceso sobre la tasa libre de riesgo [VERIFICADO F9].
  - Ai-Bansal-Guo: "U.S. stock market" en exceso; no identifiqué en el texto qué índice concreto usan [NO REPORTADO].
- **Instrumento adaptado:** CFD US500.
- **Horizonte:** un día, de cierre a cierre.
- **Dirección:** solo compras.
- **Frecuencia:** ≈ 44 días de anuncio al año [VERIFICADO F11], ≈ 3.7 por mes (44/12) [DERIVADO]. Serán menos operaciones si hay anuncios en días consecutivos, porque la posición simplemente se mantiene [INFERENCIA].

#### D1. Fundamento económico y estadístico
- **Hipótesis:** los inversores exigen una compensación por soportar el riesgo de los días en que se publica información macroeconómica clave. Por eso gran parte de la prima de riesgo de las acciones se obtiene en esos días [VERIFICADO F8, F11].
- **Ineficiencia explotada:** no es una ineficiencia, sino una **prima de riesgo**.
  - Ai-Bansal-Guo atribuyen la prima a que las preferencias de los inversores satisfacen la "generalized risk sensitivity" [VERIFICADO F10].
  - La contraparte es el inversor que reduce o cubre su exposición en esos días y renuncia a la prima a cambio de menos riesgo [INFERENCIA].
- **Datos que lo respaldan:**
  - 1958–2009: 11.4 bp de exceso medio en días de anuncio frente a 1.1 bp en el resto (diferencia 10.3 bp, t = 3.77) [VERIFICADO F9].
  - 1961–2023: 10.68 bp frente a 0.93 bp; ≈ 71 % de la prima anual (4.65 % de 6.59 %) [VERIFICADO F11].
- **Por qué la ventaja no ha desaparecido:** al ser compensación por riesgo, no tendría por qué arbitrarse [INFERENCIA]. Empíricamente, en ene-2020–ago-2023 la prima media fue de 16.33 bp por anuncio, por encima del promedio histórico [VERIFICADO F11].
- **Régimen en que debería fallar:**
  - Periodos de baja incertidumbre. La prima del FOMC fue baja entre ene-2016 y dic-2019 [VERIFICADO F11, citando a Kurov et al.; F15].
  - Días con sorpresas macro negativas, en los que la estrategia, al estar siempre comprada, pierde de lleno [INFERENCIA].
- **Crítica que matiza la evidencia:** Ernst, Gilbert y Hrdlicka (WP, 31-jul-2021) sostienen que la concentración "excesiva" de la prima se debe en parte a la selección de la muestra y al momento del mes en que caen los anuncios. Controlando por el día del mes, los anuncios explicarían ≈ la mitad de la prima, con Sharpe similares en días con y sin anuncio [VERIFICADO F13]. Esto sugiere que la estrategia comparte parte de su retorno con el efecto de cambio de mes [INFERENCIA].

#### D2. Arquitectura lógica y matemática
- **Datos de entrada:** precio de cierre diario.
  - Fuente original: cierre diario del índice CRSP con dividendos [VERIFICADO F9].
  - En el CFD: el precio del US500 a las 15:59–16:00 ET [INFERENCIA]. Si se usa precio medio, bid o ask no lo dice la fuente original, porque usa índices [NO REPORTADO].
- **Sesión:** horario regular NYSE, 9:30–16:00 ET; cierre anticipado a las 13:00 ET en días señalados (en 2026: 27-nov y 24-dic) [VERIFICADO F37].
  - ET (hora del Este) incluye el horario de verano de EE. UU.
  - Si un anuncio cae en día sin mercado, ese día no existe en la muestra: Savor & Wilson reportan 23 anuncios en días no hábiles, excluidos [VERIFICADO F9].
- **Convención de velas:** no aplica; se usan cierres diarios.
- **"Indicador":** variable de calendario A_t = 1 si el día hábil t tiene al menos uno de estos anuncios programados [VERIFICADO F11]:
  - comunicado del FOMC;
  - Non-farm payroll (informe de empleo);
  - PIB ("first and last" estimates);
  - ISM manufacturing PMI;
  - el primero del mes entre IPC (CPI) y PPI.
  
  Horas de publicación:
  - FOMC: reuniones 2026 publicadas por la Fed [VERIFICADO F38]; la hora del comunicado no aparece en esa página [NO REPORTADO en F38].
  - Empleo, CPI y PPI: 8:30 ET [VERIFICADO F39].
  - PIB: 8:30 ET [VERIFICADO F40].
  - ISM: 10:00 ET [VERIFICADO F11, Tabla 2].
- **Parámetros:** el único "parámetro" es el conjunto de anuncios. Ai & Bansal eligieron los 5 principales por atención de los inversores en Bloomberg [VERIFICADO F11]. No consta si se probaron otros conjuntos para elegirlo [NO REPORTADO]. Savor & Wilson usaron PPI/CPI, empleo y FOMC por disponibilidad de datos [VERIFICADO F9].
- **Entrada:** estar comprado al cierre del día hábil t−1 si t es día de anuncio. La medición es de cierre a cierre del día t [VERIFICADO F9, F11: retornos diarios].
- **Salida:** al cierre del día t, salvo que t+1 también sea día de anuncio; en ese caso se mantiene la posición [INFERENCIA: es la implementación directa de "estar comprado en días de anuncio"].
- **Stop, objetivo y trailing:** ninguno en la fuente [NO REPORTADO].
- **Filtros:** ninguno.
- **Casos especiales:**
  - Varios anuncios el mismo día: un solo día de exposición. Savor & Wilson reportan 51 días con más de un anuncio [VERIFICADO F9].
  - Anuncios no programados: excluidos [VERIFICADO F9].
  - Antes de feb-1994, Savor & Wilson asumen que la decisión del FOMC se conocía el día siguiente a la reunión [VERIFICADO F9].
  - Huecos: no aplica; se asume el riesgo del hueco nocturno.
  - Datos faltantes: [NO REPORTADO].
- **Pseudocódigo** [INFERENCIA, traducción literal de la regla]:

```text
CAL = {días hábiles NYSE con ≥1 de: FOMC, NFP, PIB (first/last), ISM manuf., min(CPI,PPI)}
en cada día hábil d, a las 15:58:30 ET (12:58:30 ET si hay cierre anticipado):
    objetivo = COMPRADO si siguiente_dia_habil(d) ∈ CAL, si no PLANO
    si posición ≠ objetivo: enviar orden a mercado para alcanzar el objetivo
```

- **Ambigüedades de la fuente:**
  1. Qué son exactamente "GDP (first and last)": ¿estimación adelantada y tercera estimación? [NO REPORTADO].
  2. Qué índice usa Ai-Bansal-Guo [NO REPORTADO].
  3. Cómo se tratan los anuncios reprogramados [NO REPORTADO].
  4. Cómo se tratan los anuncios en lunes, con el fin de semana dentro del retorno. Savor & Wilson solo tienen 29 [VERIFICADO F9].
  5. Si la cifra 1961–2023 sale de cierres oficiales (subasta) o de otra fuente [NO REPORTADO].

#### D3. Rendimiento y riesgo reportados
- **Win rate y número de operaciones:**
  - Win rate: [NO REPORTADO]. El tratamiento de resultados 0 tampoco aplica, porque no hay operaciones reportadas.
  - Número de días: 1,450 días de anuncio frente a 11,641 sin anuncio (1958–2009) [VERIFICADO F9]; ≈ 44 días/año (1961–2023) [VERIFICADO F11].
- **Payoff:** real [NO REPORTADO]. El R:R planificado no aplica (no hay stop ni objetivo).
- **Esperanza por día de anuncio (bruta, en exceso):**
  - 1961–2023: 10.68 bp [VERIFICADO F11].
  - 1958–2009: 11.4 bp; 11.7 bp sin valores extremos [VERIFICADO F9].
  - Neta estimada en el CFD: 10.68 − 0.69 (1 noche de margen de financiamiento) − 0.67 (spread de 0.4 pt) ≈ **9.3 bp** [DERIVADO]. No incluye deslizamiento.
- **Win rate de equilibrio** = 1/(1 + payoff): no calculable sin payoff [NO REPORTADO].
- **Máxima caída, duración, recuperación y rachas:** [NO REPORTADO].
- **Dispersión:** desviación típica diaria de 98.6 bp en días de anuncio frente a 94.6 bp en el resto [VERIFICADO F9].
- **Factor de beneficio:** [NO REPORTADO].
- **Sharpe:**
  - Savor & Wilson: "about ten times higher" en días de anuncio. Frecuencia diaria; exceso sobre una tasa libre de riesgo mensual (Kenneth French) supuesta constante en el mes [VERIFICADO F9].
  - Sharpe anual implícito con datos de Savor & Wilson ≈ 11.4/98.6 × √(1450/52) ≈ **0.61** [DERIVADO]. Supone retornos independientes y anualiza por √(días de anuncio por año).
- **Rendimiento anual:** 4.65 %/año en exceso, invertido solo 44 días al año (1961–2023) [VERIFICADO F11]; ≈ 3.2 %/año con datos de Savor & Wilson (11.4 bp × 27.9 días) [DERIVADO].
- **Resultado por año:** [NO REPORTADO]. Solo hay subperiodos: 2020–ago-2023 con 16.33 bp por anuncio [VERIFICADO F11], y resultados en 9 de 10 subperiodos de cinco años desde 1958 [VERIFICADO F9].
- **Bruto y neto:** todas las cifras son brutas, sin costos [VERIFICADO: las fuentes no modelan costos].
- **Apalancamiento:** ninguno; es el retorno del índice. Sin volatilidad objetivo ni reinversión explícita [VERIFICADO F9, F11].
- **Comparación con comprar y mantener (1961–2023) [VERIFICADO F11]:**

  | | Prima anual | Días de exposición por año |
  |---|---|---|
  | Mercado completo | 6.59 % | 252 |
  | Solo días de anuncio | 4.65 % | 44 |
  | Resto de días | 1.94 % | 208 |

  La exposición temporal de la estrategia es ≈ 17 % de los días (44/252) [DERIVADO].
- **Correlación con el mercado:** beta ≈ 1 en los días invertidos, porque compra el índice [INFERENCIA]. La correlación total [NO REPORTADO].

#### D4. Microestructura y ejecución
- **Modelo de costos de la fuente:** ninguno [VERIFICADO F9, F11].
- **Costos en el CFD [DERIVADO]:**
  - Spread: el CFD de IG cobra 0.4 pt entre 09:30 y 16:00 ET [VERIFICADO F35], ≈ 0.67 bp por operación completa.
  - Financiamiento: una noche ≈ 0.69 bp frente al retorno en exceso.
  - Fines de semana: el viernes se cobra triple [VERIFICADO F35]. Un anuncio en lunes implica 3 noches (≈ 2.1 bp).
- **Sensibilidad:** el costo total por evento (≈ 1.4 bp) es ≈ 13 % de la ventaja bruta de 10.68 bp [DERIVADO]. El margen frente a spread, financiamiento y deslizamiento es amplio.
- **Dividendos:** el CFD abona el ajuste por dividendos a las posiciones largas [VERIFICADO F35, F36]. Esto es coherente con que la fuente use retornos con dividendos [INFERENCIA].
- **Riesgo por momento de entrada:**
  - La exposición incluye el hueco nocturno y el momento de la publicación (8:30 ET antes de la apertura, 10:00 ET el ISM, el FOMC por la tarde).
  - No hay stop, así que un dato adverso se absorbe completo [INFERENCIA].
- **Tipos de orden:** en un CFD no hay orden "al cierre" con subasta. Se usa una orden a mercado en los últimos minutos de la sesión [INFERENCIA]. Precio del CFD de IG = futuro del mes más cercano ajustado por valor razonable [VERIFICADO F35].
- **Horarios críticos:** 15:58–16:00 ET (entrada y salida), 8:30 ET, 10:00 ET y la hora del comunicado del FOMC.

#### D5. Gestión del dinero y riesgo estructural
- **Tamaño original:** 100 % nocional, porque se mide el retorno del índice [VERIFICADO F9, F11].
- **Riesgo por operación como % fijo del capital:** sin stop, R no está definido.
  - Se puede usar una fracción fija del capital como nocional, o volatilidad objetivo. Ninguna de las dos está probada en la fuente [INFERENCIA].
- **Stop catastrófico:** no existe en la fuente. Cualquier stop añadido debe calibrarse en el backtest para que casi nunca se active. Por ejemplo, más allá del percentil 99 de |retorno| en días de anuncio, medido en la muestra de desarrollo. Además hay que comprobar que no reduce la esperanza [INFERENCIA].
- **Interruptores de seguridad [INFERENCIA]:**
  - No entrar si el spread al cierre supera un umbral (por ejemplo, el doble de 0.4 pt).
  - No entrar si el calendario no se pudo verificar ese día contra la Fed, el BLS, el BEA y el ISM.
  - No entrar si los precios del bróker y del índice de referencia divergen anormalmente.
  - Límite de pérdida diaria de la cuenta.
  - **No** filtrar por noticias: las noticias son la estrategia.

#### D6. Infraestructura
- **Latencia:** no requiere latencia baja ni servidor dedicado; hay una decisión al día [INFERENCIA].
- **Resiliencia:** órdenes idempotentes. Al reiniciar, consultar al bróker la posición abierta y recalcular el objetivo con el calendario [INFERENCIA].
- **Reconstrucción del estado** [INFERENCIA]:
  - Si hay posición abierta y hoy es día de anuncio: mantener hasta el cierre.
  - Si hay posición abierta y hoy no es día de anuncio pasadas las 16:00: cerrar.
- **Husos horarios:** todo en ET.
  - IG cobra el financiamiento a las 22:00 de Londres [VERIFICADO F35]; Pepperstone a las 17:00 de Nueva York [VERIFICADO F36].
  - Durante las semanas en que EE. UU. y Reino Unido no coinciden en horario de verano, las 22:00 de Londres no equivalen a las 17:00 ET [INFERENCIA]. Una posición abierta a las 16:00 ET cruza el corte del financiamiento en cualquier caso.

#### D7. Datos necesarios
- **Precios diarios:**
  - Historial diario del CFD del bróker.
  - Retornos diarios en exceso del mercado (Fama/French 3 Factors [Daily], gratuitos) [VERIFICADO F41]. La fecha de inicio del fichero no está en la página [NO REPORTADO en F41].
  - CRSP es de pago [INFERENCIA].
- **Calendarios con hora original:**
  - FOMC: Fed [VERIFICADO F38].
  - Empleo, CPI y PPI: BLS, con calendarios 2026–2027 y años previos [VERIFICADO F39].
  - PIB: BEA [VERIFICADO F40].
  - ISM: no abrí la fuente oficial del calendario ISM [NO VERIFICADO].
- **Periodo mínimo:** debe cubrir 2008, 2016–2019 (prima del FOMC baja), 2020 y 2022 [INFERENCIA]. Por ejemplo, de 2005 al presente.
- **Comparación con el CFD:** el CFD replica el índice de contado a partir del futuro ajustado por valor razonable [VERIFICADO F35]. Hay que medir la diferencia entre el US500 a las 15:59 y el cierre oficial del S&P 500 [INFERENCIA].

#### D8. Evidencia y su calidad
- **Fuentes:**

  | Fuente | Tipo | Muestra | Hallazgo |
  |---|---|---|---|
  | Savor & Wilson 2013, *JFQA* [F8, F9] | (a) revisado por pares | 1958–2009 | Prima en días de anuncio |
  | Ai, Bansal & Guo 2023, NBER [F10, F11] | (b) documento de trabajo, revisión de literatura | 1961–2023 | Actualización; ene-2020–ago-2023: 16.33 bp |
  | Hu, Pan, Wang & Zhu, *JFE* 2021/2022 [F12] | (a) | Futuro ES (E-mini S&P 500) 1997–2022 (según Tabla 2 de F11) | Retornos altos antes de NFP, ISM y PIB |
  | Ernst, Gilbert & Hrdlicka 2021 [F13] | (b) | Ene-1990–jun-2018 | Crítica: la prima está sobrestimada |
  | Kurov, Wolfe & Gilbert 2021, *FRL* [F15] | (a) | Hasta dic-2019 | La deriva previa al FOMC desapareció tras 2015 |

- **Periodos de desarrollo y prueba:** no hay separación formal [NO REPORTADO]. La evidencia posterior a la publicación (2013 → 2023) existe en la actualización 1961–2023 y en el subperiodo 2020–2023 [VERIFICADO F11].
- **Variantes probadas:** Savor & Wilson muestran robustez a incluir el CPI después de 1971, a excluir valores extremos y a controles por día de la semana [VERIFICADO F9]. El número total de variantes probadas: [NO REPORTADO].
- **Código y datos de replicación:** [NO REPORTADO].
- **Críticas:**
  - Ernst et al. (arriba).
  - Lucca & Moench (2015) no hallan deriva previa comparable en otros anuncios, solo en el FOMC [VERIFICADO F14]. Esto contradice parcialmente a Hu et al. [INFERENCIA].
- **Deterioro posterior a la publicación:** no se observa en conjunto (2020–2023 por encima de la media). Sí en el FOMC en 2016–2019 [VERIFICADO F11].

#### D9. Robustez
- **Sensibilidad al conjunto de anuncios:** días FOMC con 17.61 bp (t = 4.71) y días sin FOMC con 8.65 bp (t = 3.71); ambos significativos [VERIFICADO F11].
- **Estabilidad:** 9 de 10 subperiodos de cinco años [VERIFICADO F9].
- **Años concretos:** 2008, 2020, 2022 y el periodo más reciente por separado [NO REPORTADO]. Solo hay el agregado de 2020–ago-2023 [VERIFICADO F11].
- **Compras frente a ventas:** solo compras por diseño. Las ventas no aplican.

#### D10. Perfil de comportamiento
- **Frecuencia:** ≈ 3.7 operaciones/mes; ≈ 0.85/semana (44/52) [DERIVADO].
- **Rachas perdedoras normales y extremas:** [NO REPORTADO].
- **Duración de las caídas:** [NO REPORTADO].
- **Cuándo gana y cuándo pierde:** gana cuando la incertidumbre se resuelve favorablemente; pierde con sorpresas negativas o en mercados bajistas, porque está siempre comprada [INFERENCIA].

#### D11. Operación y gobierno
- **Demo de 2–4 semanas:** cubre solo ≈ 7–15 eventos [DERIVADO: 3.7 × 2 a 4 semanas], así que **no puede validar la ventaja**.
  - Detectar una media de 10.68 bp con desviación típica de 98.6 bp y t ≈ 2 exige n ≈ (2 × 98.6/10.68)² ≈ **341 eventos ≈ 7.8 años** [DERIVADO].
  - La demo sirve para validar [INFERENCIA]:
    1. el calendario (cada evento correcto);
    2. el deslizamiento frente al cierre;
    3. el financiamiento cobrado frente al teórico;
    4. el ajuste por dividendos;
    5. la ausencia de órdenes duplicadas.
- **Señales de degradación** [INFERENCIA]:
  - media móvil (por ejemplo, de 100 eventos) del exceso en días de anuncio no mayor que la del resto de días;
  - costo real por evento muy superior a 1.4 bp.
- **Criterios para apagarla** [INFERENCIA]:
  - errores de calendario;
  - costo real mayor que la mitad de la ventaja histórica;
  - estadístico t acumulado fuera de muestra menor que 0 tras ≥ 200 eventos.

#### D12. Ejecución con CFD
- **Diferencias frente al índice del estudio:**
  - El S&P 500 (500 grandes empresas) no es el CRSP ponderado (todo el mercado). La correlación es alta, pero no son idénticos [INFERENCIA].
  - El precio del CFD procede del futuro ajustado por valor razonable [VERIFICADO F35].
  - El financiamiento nocturno y el ajuste por dividendos son propios del CFD [VERIFICADO F35, F36].
- **Riesgos específicos:**
  - Spread más ancho fuera de las 09:30–16:00 ET: 0.6 pt en general y 1.5 pt de 16:15 a 16:30 y de 17:00 a 18:00 en IG [VERIFICADO F35].
  - Riesgo de contraparte con el bróker [INFERENCIA].
- **Validez:** no invalida la estrategia, porque los costos son ≈ 13 % de la ventaja bruta [DERIVADO].

#### D13. Capacidad y escalabilidad
- Para tamaños minoristas no hay límite relevante: al cierre la liquidez del índice es máxima [INFERENCIA].
- El límite real es el del bróker de CFD (exposición máxima, cobertura de su libro) [INFERENCIA].

#### D14. Pendiente para el backtest
- **Supuestos críticos:**
  1. Que la prima persista en 2024–2026 con el conjunto exacto de anuncios.
  2. Que la ejecución a las 15:59 en el CFD replique el cierre oficial.
  3. Cuánta prima es efecto de cambio de mes y no de anuncio (crítica de Ernst et al.). Hay que controlar por el día del mes.
- **Preguntas abiertas:**
  - definición de "GDP (first and last)";
  - qué índice usa Ai-Bansal-Guo;
  - anuncios reprogramados;
  - si conviene la variante que sale antes del anuncio (Hu et al.), no documentada en esta ficha.
- **[NO REPORTADO] a obtener:** win rate, payoff, máxima caída, duración de las caídas, rachas, factor de beneficio, resultados por año (2008, 2020, 2022, 2024–2026), correlación total con el índice y resultados netos.
- **Confianza global: MEDIA.**
  - A favor: dos fuentes revisadas por pares, 60 años de datos y prima posterior a la publicación (2020–2023).
  - En contra: no hay métricas de trading (máxima caída, rachas), la cifra más reciente viene de un documento de trabajo y existe una crítica metodológica seria.

---

### FICHA 2 — Momentum intradía "Noise Area" con trailing VWAP

#### D0. Identidad
- **Nombre y versión:** "Beat the Market: An Effective Intraday Momentum Strategy for S&P500 ETF (SPY)". Carlo Zarattini (Concretum Research), Andrew Aziz (Peak Capital Trading / Bear Bull Traders) y Andrea Barbon (Universidad de St. Gallen / Swiss Finance Institute). Primera versión: 10-may-2024; versión consultada: 3-feb-2025 [VERIFICADO F1].
  - Versión documentada: "Curr.Band + VWAP" con tamaño dinámico (la principal del paper).
- **Familia:** momentum intradía (serie temporal, un solo instrumento).
- **Instrumento original:** ETF SPY, velas de 1 minuto [VERIFICADO F1].
- **Instrumento adaptado:** CFD US500.
- **Horizonte:** intradía; todo se cierra a las 16:00 [VERIFICADO F1].
- **Dirección:** compras y ventas.
- **Frecuencia:** 1.8 operaciones por día (7,668 en ≈ 17 años) [VERIFICADO F1, Tabla 4], ≈ 38 por mes (1.8 × 21) [DERIVADO].

#### D1. Fundamento económico y estadístico
- **Hipótesis:** cuando el precio sale de una "zona de ruido" definida por el movimiento intradía medio de los últimos 14 días, existe un desequilibrio persistente de oferta y demanda que tiende a continuar durante la sesión [VERIFICADO F1].
- **Ineficiencia explotada y contraparte:**
  - Los autores invocan la subreacción a las noticias (efecto disposición, atención limitada, capital lento) [VERIFICADO F1].
  - Invocan también la cobertura de gamma de los dealers: con gamma negativa deben operar a favor de la tendencia [VERIFICADO F1, citando F4]. Esa contraparte acepta perder porque es una **restricción estructural**: debe mantener su delta neutral [VERIFICADO F4].
  - Sus datos: un RSI(5) bajo (aproximación de gamma negativa) predice más rentabilidad (β = −3.25, p = 0.001) [VERIFICADO F1].
- **Por qué no habría desaparecido (según los autores):** pocos institucionales pueden operar intradía con alta rotación y apalancamiento por sus límites regulatorios. Además, el momentum sería menos propenso a deteriorarse tras publicarse [VERIFICADO F1, FAQ Q16]. Es una afirmación de los autores, sin prueba independiente [INFERENCIA].
- **Régimen de fallo:**
  - Baja volatilidad: el Sharpe aumenta con el VIX [VERIFICADO F1, Fig. 8]. En 2016 (−12.8 %) y 2017 (−6.9 %) hubo pérdidas [VERIFICADO F1].
  - Días de reversión brusca intradía [VERIFICADO F1, ejemplo del 20-ene-2022].
  - Dealers con gamma positiva. Un documento de trabajo con E-mini 2018–2024 halla que a mayor gamma neta de los creadores de mercado, menores retornos de momentum de 10 minutos a 2 horas (cuartil alto ≈ −1 a −2 bp) [VERIFICADO F6].

#### D2. Arquitectura lógica y matemática
- **Datos:**
  - SPY y VIX en velas OHLCV de 1 minuto (IQFeed), may-2007 a abr-2024 [VERIFICADO F1].
  - Precio: último negociado (barras OHLCV) [VERIFICADO F1]. El código Python usa Polygon con `adjusted=false` [VERIFICADO F2].
- **Sesión:** 9:30–16:00 ET; el código conserva las barras de 9:30 a 15:59 [VERIFICADO F2]. En días de cierre anticipado (13:00) [NO REPORTADO].
- **Convención de las velas:** las barras llevan la marca de su minuto de inicio. Las decisiones se toman en las barras con minutos desde la apertura múltiplo de 30 (9:59, 10:29, ...), es decir, con el **cierre a las HH:00 y HH:30** [VERIFICADO F2].
- **Fórmulas** [VERIFICADO F1, F2]:
  - move_{t−i}(HH:MM) = |Close_{t−i,HH:MM} / Open_{t−i,9:30} − 1|, con i = 1..14.
  - σ_t(HH:MM) = media de los 14 valores previos para ese minuto. El código exige 13 como mínimo y desplaza un día.
  - UB_t(HH:MM) = max(Open_{t,9:30}, Close_{t−1,16:00}) × (1 + VM·σ_t).
  - LB_t(HH:MM) = min(Open_{t,9:30}, Close_{t−1,16:00}) × (1 − VM·σ_t).
  - El código ajusta el cierre previo restando el dividendo del día.
  - VWAP_t = Σ(volumen·(H+L+C)/3) / Σ volumen, acumulado desde las 9:30 [VERIFICADO F2]. El paper solo dice "only using market hours data".
  - Tamaño: Shares_t = AUM_{t−1} × min(4, 2 % / σ_SPY,t) / Open_{t,9:30}, donde σ_SPY,t = desviación típica de 14 retornos diarios [VERIFICADO F1].
- **Parámetros:**

  | Parámetro | Valor | Cómo se fijó |
  |---|---|---|
  | Retrospectiva | 14 días | Fijado; no es el óptimo (el óptimo ex post es 90 días) [VERIFICADO F1, Q6, Q14] |
  | VM (multiplicador de volatilidad) | 1 | Elegido "to maintain simplicity" (el óptimo ex post es ≈ 1.5) [VERIFICADO F1 §4.4] |
  | Volatilidad objetivo | 2 % diario | [VERIFICADO F1] |
  | Apalancamiento máximo | 4× | [VERIFICADO F1] |
  | Frecuencia de decisión | 30 min | [VERIFICADO F1] |

- **Entrada (en cada HH:00 y HH:30, desde las 10:00) [VERIFICADO F1, F2]:**
  - Comprado si Close > UB y Close > VWAP.
  - Vendido si Close < LB y Close < VWAP.
- **Salida [VERIFICADO F1, F2]:**
  - Trailing para compras = max(UB, VWAP); para ventas = min(LB, VWAP). Se evalúa solo cada 30 minutos.
  - Si el precio cruza la banda opuesta, se cierra y se abre en sentido contrario.
  - Cierre obligatorio a las 16:00.
  - Sin take profit.
- **Filtros:** ninguno en la versión base [VERIFICADO F1].
- **Casos especiales:**
  - Reentradas permitidas en cualquier decisión posterior [VERIFICADO F2: la señal se recalcula en cada barra de decisión].
  - Huecos: incorporados vía max/min(Open, cierre previo) [VERIFICADO F1].
  - Datos faltantes: [NO REPORTADO].
  - Señales simultáneas: no aplican (un solo instrumento).
- **Pseudocódigo** [VERIFICADO en lógica F1/F2; redacción propia]:

```text
pre-apertura día t:
  σ[m] = media_{i=1..14} |C[t−i,m]/O[t−i] − 1|   para cada minuto m
  vol_d = std(ret diarios últimos 14); lev = min(4, 0.02/vol_d)
a las 9:30: O = apertura; refUp = max(O, Cprev_aj); refDn = min(O, Cprev_aj)
            shares = AUM[t−1]·lev / O
cada minuto: actualizar VWAP con volumen
en τ ∈ {10:00, 10:30, …, 15:30} usando el cierre P de la barra que termina en τ:
  UB = refUp·(1+VM·σ[τ]); LB = refDn·(1−VM·σ[τ])
  objetivo = +1 si P > max(UB,VWAP); −1 si P < min(LB,VWAP); 0 en otro caso
  ajustar posición a objetivo·shares
a las 16:00: objetivo = 0
```

- **Ambigüedades y diferencias entre paper y código [VERIFICADO F1 vs F2]:**
  1. La volatilidad del tamaño usa en el código los retornos d−15..d−2 (excluye el último); el paper usa t−1..t−14.
  2. El código no modela deslizamiento; el paper usa $0.001 por acción.
  3. El código impone una comisión mínima de $0.35 por orden.
  4. Fórmula exacta del VWAP en el paper.
  5. Tratamiento de días con cierre anticipado [NO REPORTADO].
  6. Ejecución a la apertura del minuto siguiente o al cierre del minuto de decisión. El experimento de deslizamiento mide contra la apertura del minuto HH:MM [VERIFICADO F1 §4.6].
  7. **Filtro "stop < 0.1 %":** la distancia inicial entre el precio y max(UB, VWAP) puede ser muy pequeña. Su distribución no se reporta [NO REPORTADO]. El stop no es una orden en el libro sino una condición que se revisa cada 30 minutos. Por eso no la excluyo, pero el cumplimiento del filtro **no es verificable** con la fuente [INFERENCIA].

#### D3. Rendimiento y riesgo reportados (may-2007 a abr-2024, neto de costos de la fuente, salvo indicación)

| Variante | Rentabilidad total | Anual | Volatilidad | Sharpe | Hit ratio diario | Máx. caída | Beta |
|---|---|---|---|---|---|---|---|
| Banda opuesta, 100 % | 178 % | 6.2 % | 10.9 % | 0.61 | 54 % | 21 % | −0.05 |
| Banda actual + VWAP, 100 % (sin apalancamiento) | 380 % | 9.7 % | 7.7 % | 1.24 | 43 % | 12 % | −0.03 |
| **Banda actual + VWAP, tamaño dinámico** | **1,985 %** | **19.6 %** | **14.3 %** | **1.33** | **43 %** | **25 %** | **−0.07** |
| SPY comprar y mantener | 227 % | 7.2 % | 20.2 % | 0.45 | 54 % | 56 % | — |

Fuente de la tabla: [VERIFICADO F1, Tablas 2–3 y A1].

- **Win rate por operación:** 37 %, con 7,668 operaciones [VERIFICADO F1, Tabla 4]. Con datos hasta dic-2024: 7,964 operaciones [VERIFICADO F1, Q22]. El tratamiento de las operaciones con resultado 0 [NO REPORTADO].
- **Payoff real:** [NO REPORTADO]. Dato derivado: con 37 % de aciertos y esperanza positiva, el payoff real debe ser **> 1.70** = (1 − 0.37)/0.37 [DERIVADO].
- **R:R planificado:** no aplica (no hay objetivo).
- **Esperanza por operación:** $0.09 por acción [VERIFICADO F1]. Sobre el precio medio del SPY de $247 [VERIFICADO F1, Q23], ≈ **3.6 bp** por operación neta de costos de la fuente (0.09/247) [DERIVADO].
- **Win rate de equilibrio:** [NO REPORTADO]; no calculable sin payoff.
- **Máxima caída:** 25 % (tamaño dinámico) [VERIFICADO F1]. Duración y recuperación [NO REPORTADO].
  - Con la tabla mensual publicada: caída mensual de −19.5 %, desde dic-2015 hasta jul-2017, recuperada en abr-2018 (≈ 28 meses bajo el máximo) [DERIVADO: capital compuesto mensual].
- **Rachas:**
  - Por operación: [NO REPORTADO].
  - Mensuales: como máximo 4 meses negativos seguidos (nov-2014 a feb-2015); 74 de 213 meses negativos [DERIVADO].
- **Peor y mejor operación:** peor −2.9 % (31-jul-2013), mejor +9.1 % (10-oct-2018) [VERIFICADO F1]. Asimetría diaria 1.22; peor día −4.5 % [VERIFICADO F1].
- **Factor de beneficio:** [NO REPORTADO].
- **Sharpe:** la frecuencia de los retornos, el método de anualización y la tasa libre de riesgo [NO REPORTADO].
- **Resultado por año (%) [VERIFICADO F1, Q24]:**

  | 2007 (parcial) | 2008 | 2009 | 2010 | 2011 | 2012 | 2013 | 2014 | 2015 | 2016 | 2017 | 2018 | 2019 | 2020 | 2021 | 2022 | 2023 | 2024 | Ene-2025 |
  |---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
  | 4.5 | 63.4 | 19.6 | 1.3 | 25.7 | 21.1 | 9.6 | 12.6 | 18.4 | −12.8 | −6.9 | 61.1 | 6.9 | 26.8 | 34.8 | 24.4 | 37.2 | 32.2 | −1.2 |

- **Bruto y neto:**
  - Neto con comisión de $0.0035/acción (IB) y deslizamiento de $0.001/acción [VERIFICADO F1].
  - Con el modelo de impacto I-Star: Sharpe 1.17 [VERIFICADO F1, Q15].
  - Bruto: [NO REPORTADO] (solo un gráfico de sensibilidad a la comisión).
- **Apalancamiento:** la versión principal usa volatilidad objetivo, hasta 4×, y reinvierte (tamaño sobre el capital del día anterior) [VERIFICADO F1]. La versión sin apalancamiento es la fila "100 %".
- **Comparación con comprar y mantener:** en la tabla. Alfa de 19.6 %/año y beta de −0.07 frente al SPY [VERIFICADO F1].

#### D4. Microestructura y ejecución
- **Costos de la fuente:**
  - Comisión de $0.0035/acción; ≈ 20 % de los días a $0.0020 por volumen [VERIFICADO F1].
  - Deslizamiento de $0.001/acción, medido con más de 1,000 órdenes reales a mercado por ≈ $50 M en abr-2024: media $0.001, mediana $0.0005 [VERIFICADO F1].
  - Total por operación completa: ≈ 0.36 bp = 2 × (0.0035 + 0.001)/247 [DERIVADO].
- **CFD:**
  - Spread de 0.4 pt (≈ 0.67 bp) en 09:30–16:00 ET [VERIFICADO F35; DERIVADO bp].
  - Esperanza neta aproximada en el CFD ≈ 3.6 + 0.36 − 0.67 ≈ **3.3 bp/operación** [DERIVADO, sin deslizamiento del CFD].
  - Financiamiento nocturno: no aplica, porque se cierra antes de las 16:00 y el corte es a las 17:00 ET o 22:00 de Londres [VERIFICADO F35, F36; DERIVADO].
- **Sensibilidad:** con 1.8 operaciones al día, cada 0.1 pt adicional de spread cuesta ≈ 0.3 bp diarios (0.1/6000 × 10⁴ × 1.8) [DERIVADO].
- **Riesgo por momento de entrada:** la primera entrada es a las 10:00. Los datos de las 8:30 entran vía el hueco de apertura, y los días FOMC concentran rentabilidad: el miércoles tiene mejor promedio, 18 bp [VERIFICADO F1, Tabla 6].
- **Órdenes:** a mercado en HH:00/HH:30. En el estudio se enviaron milisegundos antes del minuto [VERIFICADO F1].
- **Horarios críticos:** 9:30 (apertura de referencia), cada HH:00/HH:30 y 15:59–16:00 (cierre forzado).

#### D5. Gestión del dinero y riesgo estructural
- **Tamaño original:** volatilidad objetivo del 2 % diario, con tope de 4× [VERIFICADO F1]. No crece tras pérdidas: es proporcional al capital y a la inversa de la volatilidad [VERIFICADO F1]. Cumple el filtro.
- **% fijo de riesgo por operación:** R no está definido por la fuente, porque el stop se revisa cada 30 minutos y la pérdida puede superar la distancia inicial.
  - Sustituir el tamaño por un % fijo del capital por operación cambiaría la estrategia. Se recomienda conservar el de la fuente y, si se cambia, someterlo a backtest [INFERENCIA].
- **Stop catastrófico:** no existe. Una orden de stop dentro de la vela, muy alejada (calibrada sobre el peor movimiento de 30 minutos de la muestra de desarrollo), protegería contra desconexiones. Hay que medir que no altere los resultados [INFERENCIA].
- **Interruptores [INFERENCIA]:**
  - spread mayor que 2 × 0.4 pt en el momento de decisión;
  - datos de volumen o precio del SPY/ES desfasados más de N segundos;
  - divergencia anormal entre el SPY y el US500;
  - pérdida diaria mayor que la peor operación histórica (−2.9 % en la versión dinámica);
  - límites de margen del bróker.

#### D6. Infraestructura
- **Latencia:** no exige latencia de milisegundos (decisiones cada 30 minutos), pero sí un reloj sincronizado y datos de 1 minuto con volumen [INFERENCIA].
- **Resiliencia:** σ, las bandas y el VWAP se pueden recalcular desde el historial. Al reiniciar a mitad de sesión, consultar la posición al bróker y re-evaluar en la siguiente decisión [INFERENCIA].
- **Husos:** todo en ET, incluido el cambio de horario de EE. UU. [INFERENCIA].

#### D7. Datos necesarios
- **SPY de 1 minuto con volumen:**
  - IQFeed (de pago);
  - Polygon (2 años gratis);
  - Alpaca (7 años gratis) [VERIFICADO F1, Q2].
- **VIX:** solo para análisis, no para la regla [VERIFICADO F1].
- **CFD:** bid/ask de 1 minuto del US500 en el bróker.
- **Periodo mínimo:** 2007 al presente, como la fuente (incluye 2008, 2020 y 2022) [INFERENCIA].
- **Comparación con el CFD:** el CFD no tiene volumen real. El VWAP debe construirse con el volumen del SPY o del ES [INFERENCIA]. Hay que medir minuto a minuto la base entre el US500 y 10 × SPY y el efecto de los dividendos [INFERENCIA].

#### D8. Evidencia y su calidad
- **Tipo:** documento de trabajo (b): SSRN, Swiss Finance Institute (24-97) y repositorio de la Universidad de St. Gallen. No está revisado por pares [VERIFICADO F1].
  - Dos de los tres autores tienen vínculos comerciales (Concretum Research, Bear Bull Traders) [VERIFICADO F1]. No hay réplica independiente revisada por pares [NO REPORTADO].
- **Muestra:** SPY, may-2007 a abr-2024. Sin separación entre periodo de desarrollo y de prueba [NO REPORTADO].
- **Evidencia posterior a la publicación:** may-2024 a ene-2025, +15.5 % compuesto (9 meses), reportado por los mismos autores [DERIVADO de F1, Q24].
- **Otros mercados (mismos autores):** las mismas reglas en 33 futuros (2007–sep-2024) dan un Sharpe medio de 0.60; en el futuro del S&P 500, Sharpe 1.32, crecimiento anual compuesto 19.8 %, máxima caída 23 % [VERIFICADO F1, Q13].
- **Variantes probadas:** 3 tipos de stop, 2 de tamaño, una rejilla de VM, 8 retrospectivas, stop solo con VWAP, 8 patrones diarios y días de la semana [VERIFICADO F1].
- **Código:** Matlab y Python publicados [VERIFICADO F1, F2]. Los datos no se comparten [VERIFICADO F1].
- **Críticas y contexto:**
  - Rosa (2022) muestra que la predictibilidad tipo Gao desaparece fuera de muestra [VERIFICADO F5].
  - Adams et al. (2026): el momentum se comprime con gamma alta [VERIFICADO F6].
  - Los autores responden a la acusación de sobreajuste (Q14) [VERIFICADO F1].
  - Pistas (e), no evidencia: Maróy (2025, foro de Bear Bull Traders) afirma Sharpe > 3 tras optimizar, con riesgo evidente de sobreajuste [F42].

#### D9. Robustez
- **Sensibilidad a parámetros [VERIFICADO F1]:**
  - Retrospectiva de 5–90 días: Sharpe 1.23–1.50.
  - VM: mejor Sharpe en ≈ 1.5; la rentabilidad cae al subir VM.
  - Los valores vecinos funcionan: no depende de un único valor exacto.
- **Por año:** 2 años negativos de 18 (2016, 2017) [VERIFICADO F1].
- **Por régimen:** el Sharpe crece con el VIX al abrir [VERIFICADO F1, Fig. 8]. Positiva en los 10 peores trimestres del S&P 500 [VERIFICADO F1, Q7].
- **Años concretos [VERIFICADO F1]:**

  | 2008 | 2020 | 2022 | 2024 | Ene-2025 |
  |---|---|---|---|---|
  | +63.4 % | +26.8 % | +24.4 % | +32.2 % | −1.2 % |

- **Compras frente a ventas:** ambas contribuyen positivamente; las compras algo más [VERIFICADO F1, Q5]. Las magnitudes, solo en un gráfico [NO REPORTADO].

#### D10. Perfil de comportamiento
- **Frecuencia:** ≈ 9 operaciones por semana (1.8 × 5) [DERIVADO].
- **Rachas:** como máximo 4 meses negativos seguidos [DERIVADO]. Racha de operaciones [NO REPORTADO].
- **Caídas:** la peor duró ≈ 28 meses bajo el máximo según la tabla mensual [DERIVADO].
- **Cuándo gana y cuándo pierde:** gana en días de tendencia y alta volatilidad; pierde en mercados laterales y de baja volatilidad, como 2016–2017 [VERIFICADO F1; caracterización INFERENCIA].

#### D11. Operación y gobierno
- **Demo de 4 semanas:** ≈ 36 operaciones (1.8 × 20) [DERIVADO]. No basta para validar la ventaja.
  - Con Sharpe 1.33 se necesitan ≈ (2/1.33)² ≈ 2.3 años para t ≈ 2 [DERIVADO].
- **Qué observar:**
  - coincidencia de señales con un modelo de referencia calculado sobre el SPY (debería ser del 100 %);
  - deslizamiento frente al cierre del minuto;
  - spread en HH:00;
  - diferencia entre el VWAP con volumen externo y el de la fuente [INFERENCIA].
- **Degradación:** Sharpe de 12 meses ≤ 0; ganancia media por operación ≤ costo real; caída > 25 % [INFERENCIA].
- **Apagado:** caída mayor que la histórica (25 %), o errores de datos o de ejecución persistentes [INFERENCIA].

#### D12. Ejecución con CFD
- **Restricciones:**
  1. No hay volumen real; el VWAP debe venir del SPY o del ES [INFERENCIA].
  2. El CFD cotiza 24 h. La "apertura 9:30" y el "cierre 16:00" deben tomarse a esas horas exactas, y no coinciden con las subastas oficiales [INFERENCIA].
  3. El índice del CFD se ajusta a la baja por los dividendos [VERIFICADO F35], de forma coherente con el ajuste del cierre previo del código [VERIFICADO F2].
  4. El apalancamiento de hasta 4× depende del margen del bróker. IG indica un margen del 0.50 % en su página [VERIFICADO F35]; hay que verificarlo según la jurisdicción [INFERENCIA].
- **Riesgos:**
  - spread variable en momentos volátiles;
  - riesgo de base entre la señal (SPY) y la ejecución (CFD);
  - pagos de dividendos mal reflejados en las bandas [INFERENCIA].
- **Validez:** la ventaja estimada (≈ 3.3 bp/operación) supera el spread del CFD (≈ 0.67 bp), así que no queda invalidada a priori [DERIVADO].

#### D13. Capacidad y escalabilidad
- El SPY es de los activos más líquidos. La fuente reconoce que el deslizamiento crecería con el tamaño y responde con el modelo I-Star (Sharpe 1.17) [VERIFICADO F1, Q15].
- En el CFD, el límite lo pone el bróker [INFERENCIA].

#### D14. Pendiente para el backtest
- **Supuestos críticos:**
  1. Que el VWAP con volumen externo reproduzca la señal.
  2. Que el deslizamiento real del CFD en HH:00 sea comparable a 0.4 pt.
  3. Que el rendimiento persista con 0DTE y gamma de dealers alta (F6).
  4. La distribución de la distancia inicial al stop frente al filtro del 0.1 %.
- **Preguntas abiertas:** días de cierre anticipado; desajuste paper/código en la volatilidad del tamaño; separación propia entre periodo de desarrollo y de prueba.
- **[NO REPORTADO] a obtener:** payoff, factor de beneficio, rachas por operación, duración de las caídas diarias, resultados brutos, magnitudes de compras y ventas por separado, y el método del Sharpe.
- **Confianza global: MEDIA.**
  - A favor: reglas completas, código público, 17 años con 2008, 2020 y 2022, y resultados posteriores a la publicación positivos.
  - En contra: no está revisada por pares, la evidencia procede de los propios autores (con intereses comerciales), no hay réplica independiente y depende de apalancamiento para la cifra principal.

---

### FICHA 3 — Momentum intradía de mercado en la última media hora (ESTRATEGIA MÁS DÉBIL)

#### D0. Identidad
- **Nombre y versiones:**
  - Versión original η(r1): "Market intraday momentum", Lei Gao, Yufeng Han, Sophia Zhengzi Li y Guofu Zhou, *JFE* 129(2):394–414, 2018. Las métricas citadas abajo son de la versión de trabajo de dic-2015 [VERIFICADO F3].
  - Versión ampliada η(r_ROD): Guido Baltussen, Zhi Da, Sten Lammers y Martin Martens, "Hedging demand and market intraday momentum", *JFE* 142(1):377–403, 2021 [VERIFICADO F4].
- **Familia:** momentum intradía (cierre).
- **Instrumento original:** SPY (Gao); futuros sobre índices, incluido el S&P 500 (Baltussen).
- **Instrumento adaptado:** CFD US500.
- **Horizonte:** 30 minutos (15:30–16:00 ET).
- **Dirección:** compras y ventas.
- **Frecuencia:** una operación por día hábil, ≈ 21 por mes [DERIVADO]. La versión que combina dos señales opera menos: no opera cuando las señales discrepan [VERIFICADO F3, F4].

#### D1. Fundamento económico y estadístico
- **Hipótesis:** el retorno del día hasta las 15:30, medido desde el cierre previo, predice con el mismo signo el retorno de la última media hora [VERIFICADO F3, F4].
- **Contraparte (restricción estructural):**
  - Quienes cubren gamma corta (creadores de mercado de opciones y ETF apalancados) deben comprar cuando sube y vender cuando baja, y concentran esa cobertura cerca del cierre [VERIFICADO F4].
  - Gao propone además el reajuste infrecuente de carteras (Bogousslavsky) y el "late-informed trading" [VERIFICADO F3].
  - El efecto revierte en los días siguientes, coherente con presión de precios transitoria [VERIFICADO F4].
- **Por qué podría persistir:** los ETF apalancados y las opciones siguen existiendo [INFERENCIA].
- **Datos en contra:**
  - El momentum solo aparece cuando la exposición gamma neta (NGE) de los dealers es negativa; con NGE positiva no es significativo (1996–may-2020, S&P 500) [VERIFICADO F4].
  - Con gamma alta de los creadores de mercado (E-mini 2018–2024), el momentum se comprime o se invierte [VERIFICADO F6].
- **Régimen de fallo:** gamma positiva de los dealers y baja volatilidad. En terciles de volatilidad de la primera media hora, el retorno anual fue de 0.54 % (bajo), 4.75 % (medio) y 14.73 % (alto) [VERIFICADO F3].

#### D2. Arquitectura lógica y matemática
- **Datos:**
  - Gao: precios de operaciones de SPY (base TAQ) en instantes de media hora; precios de compra y venta a las 15:30 para el análisis de costos [VERIFICADO F3].
  - Baltussen: retornos intradía de futuros [VERIFICADO F4]; el tipo de precio [NO REPORTADO en lo leído].
- **Sesión:** 9:30–16:00 ET. Gao excluye los días con menos de 500 operaciones [VERIFICADO F3]. Días de cierre anticipado [NO REPORTADO].
- **Convención:** precios puntuales en cada media hora (p_j a las 10:00, 10:30, ..., 16:00) con p₀ = cierre de las 16:00 del día anterior [VERIFICADO F3]. No son velas.
- **Fórmulas [VERIFICADO F3, F4]:**
  - r_j = p_j/p_{j−1} − 1.
  - Gao: r1 = p(10:00)/p(16:00 del día previo) − 1; r13 = p(16:00)/p(15:30) − 1.
  - Baltussen: r_ROD = retorno desde el cierre previo hasta 30 minutos antes del cierre; r_LH = retorno de la última media hora.
- **Parámetros:** ventanas fijas de 30 minutos y umbral 0. No están optimizados [VERIFICADO F3, F4].
- **Entrada:** a las 15:30 ET [VERIFICADO F3, F4].
  - Gao: comprado si r1 > 0, vendido si r1 ≤ 0.
  - Baltussen: comprado si r_ROD > 0, vendido en otro caso.
- **Salida:** al cierre (16:00). Sin stop ni objetivo [VERIFICADO F3].
- **Filtros:** ninguno en la versión base. Variante: solo cuando r1 y r12 (o r_ONFH y r_ROD) coinciden en signo [VERIFICADO F3, F4].
- **Casos especiales:** r = 0 cuenta como venta en Gao (r1 ≤ 0) [VERIFICADO F3]. Huecos: incluidos en la señal, porque se mide desde el cierre previo. Datos faltantes: excluye días con pocas operaciones [VERIFICADO F3].
- **Pseudocódigo** [INFERENCIA, traducción literal]:

```text
a las 15:30:00 ET del día t:
  r = P(15:30) / C(t−1, 16:00) − 1          # versión r_ROD (Baltussen)
  lado = +1 si r > 0 en otro caso −1
  abrir posición 'lado'
a las 15:59:xx ET: cerrar posición          # en el CFD no hay subasta de cierre
```

- **Ambigüedades:**
  1. Días de cierre anticipado [NO REPORTADO].
  2. Si el Sharpe de Gao resta la tasa libre de riesgo: para la medida M² suponen rf diaria = 0; para el Sharpe no lo dicen [NO REPORTADO].
  3. Sharpe concreto del futuro del S&P 500 solo en Baltussen [NO REPORTADO en lo leído; la Tabla 6 es una cartera 1/N].
  4. Uso del precio de subasta (16:00) en la salida frente a la ejecución continua.

#### D3. Rendimiento y riesgo reportados
- **Gao, SPY 1-feb-1993 a 31-dic-2013, η(r1), sin costos [VERIFICADO F3]:**
  - retorno medio 6.67 %/año; desviación típica 6.19 %;
  - Sharpe 1.08 (retornos diarios × 252);
  - asimetría 0.90; curtosis 15.65.
  - Comparación: siempre comprado en la última media hora, −1.11 %; comprar y mantener, 6.04 % (Sharpe 0.29; desviación 20.57 %).
- **Win rate:** "success rate" de 54.37 %. **Ojo:** la fuente lo define como el porcentaje de días con retorno **≥ 0**, así que los ceros cuentan como acierto [VERIFICADO F3]. Número de operaciones exacto [NO REPORTADO]; ≈ 5,200 (≈ 21 años × ≈ 250 días) [DERIVADO aproximado].
- **Esperanza por operación:**
  - bruta ≈ 6.67 %/252 ≈ **2.65 bp**; desviación por operación ≈ 6.19 %/√252 ≈ 0.39 % [DERIVADO].
  - Payoff [NO REPORTADO]. R:R planificado: no aplica.
- **Neto de costos [VERIFICADO F3]:**
  - Paga el spread solo en la entrada de las 15:30; la salida a las 16:00 es en subasta sin spread; ignora las comisiones.
  - 2001–2013: 4.46 %/año neto (−2.47 pp).
  - 2005–2013: 7.96 % bruto → 6.52 % neto (≈ 2.59 bp/operación neta) [DERIVADO].
- **Baltussen, cartera 1/N de futuros sobre índices, dic-1974 a may-2020, η(r_ROD), sin costos [VERIFICADO F4]:**
  - 6.86 %/año; desviación 3.96 %; Sharpe 1.73; acierto 0.55.
  - Siempre comprado: Sharpe 0.11. Comprar y mantener: 8.76 %, Sharpe 0.51.
  - En el futuro del S&P 500, con un costo de un tick, el Sharpe neto "is positive"; el valor [NO REPORTADO].
- **Máxima caída, duración, rachas, factor de beneficio y resultado por año:** [NO REPORTADO].
- **Apalancamiento:** ninguno; exposición de 1× solo 30 minutos al día [VERIFICADO F3]. Nota de la fuente: el Sharpe se infla porque solo se está expuesto 30 minutos [VERIFICADO F3].
- **Correlación con el mercado:** [NO REPORTADO].

#### D4. Microestructura y ejecución
- **Modelo de la fuente:** spread de compra y venta a las 15:30 (mediana de cotizaciones de ese segundo); salida en subasta [VERIFICADO F3]. El spread proporcional del SPY se estabilizó en ≈ 1.2 bp tras 2005 [VERIFICADO F3].
- **CFD:**
  - Paga el spread completo: entrada a las 15:30 y salida a las 15:59.
  - 0.4 pt ≈ 0.67 bp por operación completa, ≈ 25 % de la ventaja bruta histórica de 2.65 bp [DERIVADO].
  - Costo de equilibrio ≈ 2.65 bp ≈ 1.6 pt a P = 6,000 [DERIVADO].
  - Si la ventaja actual es menor (F5, F6), el margen desaparece rápido [INFERENCIA].
- **Financiamiento:** no aplica (intradía).
- **Riesgo de horario:** el cierre concentra el flujo de las subastas. El CFD no participa en la subasta, solo en la cotización continua derivada del futuro [VERIFICADO F35 sobre el origen del precio; resto INFERENCIA]. Los futuros del S&P siguen cotizando hasta las 16:15 [VERIFICADO F4].
- **Órdenes:** a mercado a las 15:30:00 y a las 15:59:xx [INFERENCIA].

#### D5. Gestión del dinero y riesgo estructural
- **Tamaño original:** 1× nocional [VERIFICADO F3, F4].
- **% fijo de riesgo:** sin stop, R no está definido. Se puede usar un nocional fijo o volatilidad objetivo, sin pruebas en la fuente [INFERENCIA].
- **Stop catastrófico:** no existe. Con 30 minutos de exposición, un stop amplio dentro de la vela solo protege de eventos extremos [INFERENCIA].
- **Interruptores [INFERENCIA]:**
  - no operar si el spread a las 15:30 supera un umbral;
  - no operar con feed caído;
  - no operar en días de cierre anticipado hasta definir la regla.

#### D6. Infraestructura
- Requiere puntualidad de segundos, no de milisegundos [INFERENCIA].
- Resiliencia: si se reinicia entre las 15:30 y las 16:00, consultar la posición y cerrarla a las 15:59 [INFERENCIA].
- Husos: ET [INFERENCIA].

#### D7. Datos necesarios
- **Precios:** del US500 a las 16:00 del día previo y a las 15:30 del día actual; minuto a minuto del CFD (bid/ask) para el backtest; SPY o ES de referencia [INFERENCIA].
- **Variante con gamma:** requiere la NGE estimada con OptionMetrics o SqueezeMetrics, datos de pago [VERIFICADO F4]. No forma parte de la regla base.
- **Periodo mínimo:** 2005 al presente, con 2008, 2020, 2022 y la era 0DTE desde 2022 [INFERENCIA].

#### D8. Evidencia y su calidad
- **Fuentes:**

  | Fuente | Tipo | Muestra | Hallazgo |
  |---|---|---|---|
  | Gao et al. 2018, *JFE* [F3] | (a) | SPY 1993–2013 | R² dentro de muestra 1.6 %; fuera de muestra 1.2 % |
  | Baltussen et al. 2021, *JFE* [F4] | (a) | >60 futuros, dic-1974 a may-2020 | Subperiodos 1974–1999 y 2000–2020 "similar" |
  | Li, Sakkas & Urquhart 2022, *JFM* [F7] | (a) | 16 mercados desarrollados | Momentum intradía significativo dentro y fuera de muestra en la mayoría |
  | Rosa 2022, *JFutM* [F5] | (a) | — | "The predictability disappears in the out-of-sample period"; con umbrales mejora |
  | Adams et al. 2026 [F6] | (b) | E-mini 2018–2024 | Momentum comprimido con gamma alta |

- **Periodos de desarrollo y prueba:** Gao usa regresiones recursivas fuera de muestra [VERIFICADO F3].
- **Variantes probadas:** 3 señales (r1, r12, combinada) en Gao; 3 en Baltussen [VERIFICADO F3, F4].
- **Código público:** [NO REPORTADO].
- **Deterioro tras la publicación:**
  - Rosa (revisado por pares) lo documenta [VERIFICADO F5].
  - Pista (e) de un vendedor de datos: pendiente incondicional ≈ 0 en 1,085 sesiones del SPX entre 2022 y 2026, positiva solo en cierres con gamma corta (≈ 15 % de las sesiones) [F43]. No cuenta como evidencia.

#### D9. Robustez
- **Parámetros:** no tiene parámetros optimizados [VERIFICADO F3, F4].
- **Por régimen:** mejor con alta volatilidad y alto volumen [VERIFICADO F3]; solo con NGE negativa [VERIFICADO F4].
- **Años concretos:**
  - 2008: "stronger... on recession days", sin cifra anual [VERIFICADO F3 / NO REPORTADO].
  - 2020: fuerte en feb–may-2020, solo cualitativo [VERIFICADO F4].
  - 2022 y periodo reciente: [NO REPORTADO] en fuentes revisadas por pares.
- **Compras frente a ventas:** [NO REPORTADO].

#### D10. Perfil de comportamiento
- **Frecuencia:** ≈ 5 operaciones por semana.
- **Rachas y caídas:** [NO REPORTADO].
- **Cuándo gana y cuándo pierde:** gana en días de tendencia con dealers en gamma corta; pierde o queda plana en días tranquilos con gamma larga [VERIFICADO F4, F6].

#### D11. Operación y gobierno
- **Demo de 4 semanas:** ≈ 20 operaciones; no valida la ventaja. Con Sharpe 1.08 se necesitarían ≈ (2/1.08)² ≈ 3.4 años para t ≈ 2 [DERIVADO].
- **Qué observar:** el spread real a las 15:30 y a las 15:59, y la diferencia entre el cierre del CFD y el cierre oficial [INFERENCIA].
- **Degradación y apagado:** retorno medio por operación ≤ costo real tras ≥ 250 operaciones [INFERENCIA].

#### D12. Ejecución con CFD
- **Restricciones:**
  - no hay subasta de cierre;
  - el precio deriva del futuro, que sigue cotizando hasta las 16:15 [VERIFICADO F4, F35];
  - se paga el spread dos veces frente a una sola en la medición neta de Gao [VERIFICADO F3; DERIVADO].
- **Riesgo:** que el costo del CFD supere la ventaja actual, que según F5 y F6 puede ser ≈ 0 en ausencia de gamma corta [INFERENCIA]. **Puede invalidarla.**

#### D13. Capacidad y escalabilidad
- En el subyacente, el volumen es máximo alrededor de la apertura y el cierre del índice [VERIFICADO F4].
- En el CFD, el límite es el bróker [INFERENCIA].

#### D14. Pendiente para el backtest
- **Supuestos críticos:**
  1. Que exista ventaja después de 2020 en el S&P 500.
  2. Que el costo del CFD a las 15:30 y a las 15:59 sea ≤ 0.4 pt.
  3. Que la versión r_ROD funcione mejor que r1 en datos recientes.
- **Preguntas abiertas:** reglas para medias sesiones; si una aproximación de gamma sin datos de pago (como el RSI de F1) puede filtrar sin sobreajuste.
- **[NO REPORTADO] a obtener:** número exacto de operaciones, payoff, máxima caída, rachas, factor de beneficio, resultados por año, compras frente a ventas, correlación y Sharpe neto del futuro S&P.
- **Confianza global: BAJA.**
  - A favor: dos artículos en *JFE* y un mecanismo convincente.
  - En contra: fallo fuera de muestra publicado (Rosa 2022), ventaja dependiente de la gamma de los dealers, evidencia de 2018–2024 desfavorable y margen estrecho frente al spread del CFD.

---

## 3. Otras candidatas evaluadas

| # | Candidata (fuente) | Resultado | Motivo |
|---|---|---|---|
| 4 | Cambio de mes, día −1 a +3 (McConnell & Xu 2008 *FAJ*; Etula et al. 2020 *RFS*) [F18, F19, F20] | 66.0, fuera del top 3 por poco | 1987–2005 fuera de muestra: 0.15 %/día en la ventana frente a −0.00 % el resto (t de la media = 4.35; t de la diferencia = 3.78) [VERIFICADO F19]. Mecanismo de pagos de fin de mes con datos hasta 2013 [VERIFICADO F20]. No hallé evidencia revisada por pares posterior a 2013. Una pista (e) indica que la ventana [0:3] "has largely disappeared" en la última década [F23]. Mantiene la posición 4 días: ≈ 6 noches de financiamiento ≈ 11.4 bp frente a ≈ 60 bp brutos (0.15 % × 4) [DERIVADO]. Pista (e): Quantpedia atribuye a la versión (−1,+3) una máxima caída de −20.79 % en 1926–2005, cifra no verificada en la fuente primaria [F22]. |
| 5 | Retorno antes de NFP, ISM y PIB (Hu, Pan, Wang & Zhu, *JFE* 2021/2022) [F12, F11] | 65.5 | Futuro ES 1997–2022, de 16:00 de t−1 a 5 min antes del dato: PIB 8.42 bp, NFP 8.48 bp, ISM 14.43 bp [VERIFICADO F11, Tabla 2]. Solo abrí el resumen y la tabla reproducida. No hay evidencia posterior a la publicación. Contradice a Lucca & Moench (F14). Variante de la familia de la #1. |
| 6 | Exposición gestionada por volatilidad (Moreira & Muir 2017; Cederburg et al. 2020) [F31] | 59.5 | Fuera de muestra no supera a la versión sin gestionar en la mayoría de 103 estrategias [VERIFICADO F31]. Posición casi permanente: financiamiento del CFD. |
| 7 | Timing con media móvil o momentum de serie temporal (MOP 2012; Zakamulin 2014) [F30, F29] | 58.5 | La evidencia de momentum de serie temporal es multiactivo [VERIFICADO F30]. En el índice, Zakamulin muestra resultados "highly overstated" fuera de muestra con costos [VERIFICADO F29]. Margen del CFD ≈ 2.5 %/año en largos. No abrí la fuente primaria de Faber (SSRN bloqueado). |
| 8 | Víspera de festivo (Ariel 1990; Ko & Yang) [F27] | 58.5 | En 1983–2019 el efecto persiste solo en empresas pequeñas; en grandes es insignificante, sobre todo tras 1990 [VERIFICADO F27]. |
| 9 | Deriva previa al FOMC (Lucca & Moench 2015) [F14, F15] | 57.0 | "Essentially disappeared after 2015" (muestra hasta 2019) [VERIFICADO F15]. |
| 10 | Mantener del cierre a la apertura (overnight) (BLW; Lou-Polk-Skouras 2019) [F16, F32] | 55.0 | Del cierre a la apertura, 4.6 %/año (2004–2020) [VERIFICADO F16], ≈ 1.83 bp/noche. Costo del CFD ≈ 0.69 + 0.67 ≈ 1.36 bp (≈ 74 % de la ventaja) [DERIVADO]. La componente nocturna principal ≈ 0 desde 2021 [VERIFICADO F17]. |
| 11 | Reglas técnicas clásicas de medias móviles y rupturas (Brock et al. 1992 → Sullivan, Timmermann & White 1999) [F28] | 55.0 | Fuera de muestra 1987–1996, "scant evidence" de valor económico [VERIFICADO F28]. |
| 12 | Reversión por IBS (Pagonidis 2014, NAAIM) [F33] | 51.0 | Solo verifiqué que figura en la lista de premios NAAIM 2014 [VERIFICADO F33]. Rendimiento y costos no verificados en fuente primaria. No hay evidencia revisada por pares fuera de muestra. |
| — | Deriva nocturna 2:00–3:00 y 1:30–3:30 (Boyarchenko, Larsen & Whelan 2023 *RFS*) [F16, F17] | **Excluida** | Costo > ventaja: Sharpe bruto 1.1 → **neto −0.5**; versión 1:30–3:30: 1.3 → 0.3 [VERIFICADO F16]. Además "close to zero since 2021" [VERIFICADO F17]. |
| — | "Buy-the-dip" nocturno condicionado (misma fuente) [F16] | **Excluida** | Requiere el volumen firmado al cierre (flujo de órdenes) [VERIFICADO F16]; el CFD no lo tiene. Depende del mismo efecto, desaparecido desde 2021 [F17]. |
| — | Ruptura del rango de apertura de 5 min en QQQ (Zarattini & Aziz 2023) [F24] | **Excluida** | No es el S&P 500. "We assumed no slippage" [VERIFICADO F24]. El stop es el extremo de la primera vela de 5 minutos, que puede quedar a menos del 0.1 % del precio [INFERENCIA]. Pista (e): una réplica en CFD del SPX (2015–2026) da +0.119 R bruto y **−0.081 R neto** [F25]. |
| — | Ruptura del rango de apertura clásica (Crabel; Holmberg et al. 2013 *FRL*) [F26] | **Excluida** | El resumen no identifica un mercado del S&P 500 [VERIFICADO F26]. No hallé evidencia aplicable verificable. |
| — | RSI(2) de Connors & Alvarez (libro, 2008) [F34] | **Excluida** | Libro no verificado. Las versiones secundarias difieren en umbrales y salidas: una web declara seguir "Quantified Strategies" y no el libro [VERIFICADO F34]. Reglas ambiguas y evidencia solo de vendedores. |
| — | Momentum de la última media hora condicionado a gamma (Baltussen NGE; F6) | **Excluida como estrategia independiente** | Requiere datos de gamma de dealers de pago o con metodología propietaria (OptionMetrics/SqueezeMetrics) [VERIFICADO F4]. Se considera una variante de la #3. |

---

## 4. Resumen de una página por estrategia

### #1 Prima por anuncios macroeconómicos (73.0, confianza media)
- **Por qué está en el top 3:**
  - Es la única de las tres con evidencia revisada por pares (*JFQA* 2013) **y** continuidad posterior a la publicación hasta 2023.
  - 1961–2023: 10.68 bp por día de anuncio frente a 0.93 bp [F11]; 2020–ago-2023: 16.33 bp [F11].
- **Fortalezas:**
  - Fundamento de prima de riesgo, no de ineficiencia arbitrable.
  - Reglas casi triviales de programar (calendario y una orden al día).
  - Costos en el CFD ≈ 13 % de la ventaja bruta [DERIVADO].
  - Pocas operaciones (≈ 3.7 al mes).
- **Riesgos:**
  - Está siempre comprada en los días de mayor incertidumbre, sin stop.
  - Sin métricas de máxima caída ni rachas en las fuentes.
  - Crítica metodológica (Ernst et al.: la prima sería ≈ la mitad y se solapa con el calendario del mes).
  - El componente FOMC fue débil en 2016–2019.
  - Validar la ventaja con datos propios exige ≈ 340 eventos (≈ 8 años) [DERIVADO]. Una demo corta solo valida la ejecución.

### #2 Momentum intradía "Noise Area" (69.0, confianza media)
- **Por qué está en el top 3:**
  - Es la más reciente (datos hasta ene-2025) y la más completa en reglas, con código público.
  - Resultados netos de costos en 17 años, con 2008, 2020 y 2022 positivos.
  - 2 años negativos de 18 [F1].
  - Opera solo intradía, así que no paga financiamiento en el CFD.
- **Fortalezas:**
  - Parámetros vecinos funcionan (retrospectiva 5–90: Sharpe 1.23–1.50).
  - Beta ≈ 0 o negativa con el mercado, y positiva en los 10 peores trimestres.
  - Ventaja estimada por operación (≈ 3.3 bp en el CFD) ≈ 5 veces el spread [DERIVADO].
- **Riesgos:**
  - Documento de trabajo no revisado por pares, de autores con interés comercial, sin réplica independiente.
  - La cifra principal (Sharpe 1.33, 19.6 %/año) usa hasta 4× de apalancamiento; sin apalancamiento: 9.7 %/año, Sharpe 1.24, máxima caída 12 %.
  - Necesita volumen externo para el VWAP.
  - Riesgo de que el régimen de gamma o 0DTE comprima el momentum (F6).
  - Caídas de ≈ 2 años (2016–2017).

### #3 Momentum de la última media hora (68.0, confianza baja — estrategia más débil)
- **Por qué está en el top 3:** la evidencia académica es de la máxima calidad (dos artículos en *JFE*) y el mecanismo es claro (cobertura de gamma y de ETF apalancados).
- **Fortalezas:**
  - Reglas sin parámetros optimizados.
  - Exposición de solo 30 minutos al día.
  - Asimetría positiva.
  - Robusta en 1974–2020 en más de 60 futuros [F4].
- **Riesgos:**
  - Fallo fuera de muestra documentado (Rosa 2022).
  - Dependencia de que los dealers tengan gamma corta: sin ella no hay efecto [F4], y con gamma alta en 2018–2024 se invierte [F6].
  - Ventaja bruta histórica ≈ 2.65 bp por operación frente a ≈ 0.67 bp de spread del CFD pagado dos veces [DERIVADO]: margen estrecho que puede desaparecer.
  - Correlacionada con la #2.
  - Prácticamente empatada con el cambio de mes (66.0): si el backtest reciente no confirma la ventaja, la sustituta natural es esa.

---

## 5. Fuentes (todas consultadas el 2026-10-12)

| ID | Fuente | Tipo |
|---|---|---|
| F1 | Zarattini, Aziz & Barbon, "Beat the Market: An Effective Intraday Momentum Strategy for S&P500 ETF (SPY)", v. 3-feb-2025 (1ª v. 10-may-2024). https://alexandria.unisg.ch/server/api/core/bitstreams/a99aba00-f967-49b3-aceb-f544dc386e0b/content | (b) |
| F2 | Concretum Group, "Python backtesting: Beat the Market…" (código). https://www.concretumgroup.com/python-backtesting-beat-the-market-an-effective-intraday-momentum-strategy-for-the-sp500-etf-spy/ | (b), código de los autores |
| F3 | Gao, Han, Li & Zhou, "Market Intraday Momentum", WP dic-2015 (publicado en *JFE* 129(2):394–414, 2018; DOI 10.1016/j.jfineco.2018.05.009). https://c.mql5.com/forextsd/forum/173/intraday_momentum_-_the_first_half-hour_return_predicts_the_last_half-hour_return.pdf | (a)/(b) |
| F4 | Baltussen, Da, Lammers & Martens, "Hedging demand and market intraday momentum", *JFE* 142(1):377–403, 2021. https://academicweb.nd.edu/~zda/intramom.pdf | (a) |
| F5 | Rosa, "Understanding intraday momentum strategies", *J. Futures Markets* 42(12):2218–2234, 2022. https://ideas.repec.org/a/wly/jfutmk/v42y2022i12p2218-2234.html | (a) |
| F6 | Adams, Dim, Eraker, Fontaine, Ornthanalai & Vilkov, "Do S&P 500 Options Increase Market Volatility? Evidence from 0DTEs", WP 15-jul-2026. https://www.jean-sebastienfontaine.com/papers/0dte-options-volatility.pdf | (b) |
| F7 | Li, Sakkas & Urquhart, "Intraday time series momentum: Global evidence…", *JFM* 57, 2022. https://ideas.repec.org/a/eee/finmar/v57y2022ics138641812100001x.html | (a) |
| F8 | Savor & Wilson, *JFQA* 48(2):343–375, 2013. https://ideas.repec.org/a/cup/jfinqa/v48y2013i02p343-375_00.html | (a) |
| F9 | Savor & Wilson, WP 28-nov-2011. https://faculty.wharton.upenn.edu/wp-content/uploads/2012/04/Draft20111128p_edited.pdf | (b) |
| F10 | Ai, Bansal & Guo, "Macroeconomic Announcement Premium", NBER WP 31923. https://www.nber.org/papers/w31923 | (b) |
| F11 | Ídem, PDF. https://www.nber.org/system/files/working_papers/w31923/w31923.pdf | (b) |
| F12 | Hu, Pan, Wang & Zhu, "Premium for Heightened Uncertainty…", NBER w25817 / *JFE* (DOI 10.1016/j.jfineco.2021.09.015). https://www.nber.org/papers/w25817 | (a)/(b) |
| F13 | Ernst, Gilbert & Hrdlicka, "More than 100% of the equity premium…", WP 31-jul-2021. http://www.aeaweb.org/conference/2022/preliminary/paper/ktNibY2Y | (b) |
| F14 | Lucca & Moench, "The Pre-FOMC Announcement Drift", *JF* 70(1):329–371, 2015. https://ideas.repec.org/a/bla/jfinan/v70y2015i1p329-371.html | (a) |
| F15 | Kurov, Wolfe & Gilbert, "The disappearing pre-FOMC announcement drift", *FRL* 40:101781, 2021. https://ideas.repec.org/a/eee/finlet/v40y2021ics1544612320315956.html | (a) |
| F16 | Boyarchenko, Larsen & Whelan, "The Overnight Drift", NY Fed Staff Report 917 (rev. ago-2022; *RFS* 2023). https://www.newyorkfed.org/medialibrary/media/research/staff_reports/sr917.pdf | (b)/(a) |
| F17 | Boyarchenko, Larsen & Whelan, "The Disappearing Overnight Drift", Liberty Street Economics, 1-jul-2026. https://libertystreeteconomics.newyorkfed.org/2026/07/the-disappearing-overnight-drift/ | (b) banco central |
| F18 | McConnell & Xu, "Equity Returns at the Turn of the Month", *FAJ* 64(2):49–64, 2008. https://ideas.repec.org/a/taf/ufajxx/v64y2008i2p49-64.html | (a) |
| F19 | Ídem, copia escaneada (páginas 49–52 leídas). https://apps.it.purdue.edu/sites/Home/DirectoryApi/Files/fed4cdba-2490-4cc5-893d-9f0d5efb626f/Download | (a) |
| F20 | Etula, Rinne, Suominen & Vaittinen, "Dash for Cash", *RFS* 33(1):75–111, 2020. https://acris.aalto.fi/ws/portalfiles/portal/40965155/hhz054.pdf | (a) |
| F22 | Quantpedia, "Turn of the Month in Equity Indexes". https://quantpedia.com/strategies/turn-of-the-month-in-equity-indexes | (e) pista |
| F23 | QuantSeeker, "Turn-of-the-Month Strategies: Do They Still Work?", 9-feb-2025. https://www.quantseeker.com/p/turn-of-the-month-strategies-do-they | (e) pista |
| F24 | Zarattini & Aziz, "Can Day Trading Really Be Profitable?" (SSRN 4416622). https://concretumgroup.com/wp-content/uploads/2026/02/Can-Day-Trading-Really-Be-Profitable.pdf | (b) |
| F25 | Krueger, "The opening-range breakout paper, replicated on five indices…", MQL5 blog, 25-sep-2026. https://www.mql5.com/en/blogs/post/776235 | (e) pista |
| F26 | Holmberg, Lönnbark & Lundström, *FRL* 10(1):27–33, 2013. https://ideas.repec.org/a/eee/finlet/v10y2013i1p27-33.html | (a) |
| F27 | Ko & Yang, "The Pre-Holiday Premium of Ariel (1990) Has Largely Become A Small-Firm Effect Out of Sample", *Critical Finance Review*. https://cfr.ivo-welch.org/published/papers/ko2021pre.pdf | (a) |
| F28 | Sullivan, Timmermann & White, "Data-Snooping, Technical Trading Rule Performance, and the Bootstrap", LSE FMG DP303 (*JF* 54(5), 1999). https://www.fmg.ac.uk/sites/default/files/2020-11/dp303.pdf | (a)/(b) |
| F29 | Zakamulin, *J. Asset Management* 15(4):261–278, 2014. https://ideas.repec.org/a/pal/assmgt/v15y2014i4d10.1057_jam.2014.25.html | (a) |
| F30 | Moskowitz, Ooi & Pedersen, "Time series momentum", *JFE* 104(2):228–250, 2012. https://ideas.repec.org/a/eee/jfinec/v104y2012i2p228-250.html | (a) |
| F31 | Cederburg, O'Doherty, Wang & Yan, *JFE* 138(1):95–117, 2020. https://ideas.repec.org/a/eee/jfinec/v138y2020i1p95-117.html | (a) |
| F32 | Lou, Polk & Skouras, "A tug of war…", *JFE* 134(1):192–213, 2019. https://ideas.repec.org/a/eee/jfinec/v134y2019i1p192-213.html | (a) |
| F33 | NAAIM, lista de trabajos premiados 2009–2018. https://www.naaim.org/wp-content/uploads/2018/05/ALL-founders-award-paper-list_2009-2018_web.pdf | (c) |
| F34 | Backtrex, "Connors RSI(2) on the Nasdaq 100". https://backtrex.com/en/backtests/connors-rsi-2-nasdaq-100 | (e) pista |
| F35 | IG, "What are IG's indices CFD product details". https://www.ig.com/en/help-and-support/cfds/fees-and-charges/what-are-igs-indices-cfd-product-details | (c) bróker |
| F36 | Pepperstone, "Costs and Charges Information & Examples", v7.0, jun-2026. https://eu-assets.contentstack.com/v3/assets/bltaec35894448c7261/bltb3f65365f20ef593/6a1ebf2ab3c954c1e626efea/Pepperstone_Limited_Cost_and_Charges_-_June_2026.pdf | (c) bróker |
| F37 | NYSE, Hours & Calendars. https://www.nyse.com/markets/hours-calendars | (c) bolsa |
| F38 | Federal Reserve, FOMC calendars. https://www.federalreserve.gov/monetarypolicy/fomccalendars.htm | oficial |
| F39 | BLS, Schedule of releases. https://www.bls.gov/schedule/news_release/ | oficial |
| F40 | BEA, Release schedule. https://www.bea.gov/news/schedule | oficial |
| F41 | Kenneth R. French, Data Library. https://mba.tuck.dartmouth.edu/pages/faculty/ken.french/data_library.html | (b) datos académicos |
| F42 | Maróy, foro Bear Bull Traders, 22-ene-2025. https://forums.bearbulltraders.com/topic/4002-improvements-to-intraday-momentum-strategies-using-parameter-optimization-and-different-exit-strategies/ | (e) pista |
| F43 | FirmTape (dev.to), "Is intraday momentum still alive?…". https://dev.to/firmtape/intraday-momentum-is-dead-in-the-0dte-era-we-measured-it-on-1085-spx-sessions-43g0 | (e) pista, vendedor |

## Limitaciones de esta investigación
- **Textos no leídos completos:** no pude abrir el texto completo de varias fuentes clave. SSRN y ScienceDirect devolvieron 403:
  - el texto publicado de Hu et al.;
  - Ai & Bansal (2018, *Econometrica*);
  - el texto completo de Rosa (2022), del que solo leí el resumen;
  - la versión final de *JFE* de Gao et al.: las cifras citadas son de su versión de trabajo de dic-2015.
- **Sin backtests propios:** toda métrica marcada [NO REPORTADO] debe obtenerse en el backtest.
- **Rúbrica subjetiva:** las notas son juicio propio y las diferencias de 2–3 puntos no son significativas.
- **Costos del CFD:** usan IG (spreads) y Pepperstone (financiamiento). Otros brókers difieren, y los valores deben confirmarse en la plataforma que se use.
