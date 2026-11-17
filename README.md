# Mâze esp. med.

**Mâze esp. med.** es una aplicación web para la lectura distante (*distant reading*) y el análisis cuantitativo de textos en español medieval, en verso o en prosa. Lee ediciones en texto plano, TEI-XML y texto anotado, y ofrece frecuencias, riqueza léxica, leyes de Zipf y Heaps, análisis de verso con asonancia y tiradas, concordancias, colocaciones, keyness, estilometría y análisis con léxicos propios.

Es un único archivo `index.html`, sin dependencias externas. Todo el cálculo se hace en el navegador, en un Web Worker, y los textos no salen del ordenador del usuario. La interfaz está disponible en español, alemán e inglés.

### El nombre

En alto alemán medio, *mâze* (también *mâȥe*) significa «medida, proporción», y *diu mâze* es la mesura, la virtud cortesana del equilibrio y la justa medida. Su equivalente castellano es la *mesura*, la virtud que el *Cantar de mio Cid* elogia en su héroe. La aplicación mide textos y busca en ellos proporciones; esta edición la adapta al español medieval.

Escríbase siempre con circunflejo, *Mâze*: sin él, en inglés se lee *maze*, «laberinto».

## Uso

- **En línea**: `https://javiermunoz-acebes.github.io/maze_esp_med/`.
- **En local**: descarga `index.html` y ábrelo con el navegador. No necesita instalación ni conexión.

### Publicar en GitHub Pages

1. Crea un repositorio llamado `maze_esp_med` y sube a su raíz `index.html`, `README.md` y `CITATION.cff`.
2. En **Settings → Pages → Build and deployment**, elige *Deploy from a branch*, rama `main`, carpeta `/ (root)`.
3. La aplicación quedará en `https://javiermunoz-acebes.github.io/maze_esp_med/`.

## Configuración para el español medieval

Al abrir la aplicación ya viene preparada para el español medieval:

- **Lengua fija**: palabras vacías del español medieval con sus variantes gráficas (*e*, *et*, *non*, *nin*, *commo*, *quando*, *pora*, *fasta*, *maguer*, *ge*, *gelo*, *uos*, *ouo*…), y cómputo silábico del español.
- **ſ → s** activado en la tokenización.
- **Capa regularizada de TEI** por defecto (`reg`, `corr`, `expan`: abreviaturas desarrolladas), con la diplomática disponible.
- **Negaciones** del español medieval (*non*, *no*, *nin*, *ni*, *nunca*, *nunqua*, *iamas*, *nada*, *nadi*, *ninguno*…) para invertir la polaridad cuando se usa un léxico.
- **Plantilla de normalización** con grafías habituales (ſ → s, *vn* → *un*, u/v consonántica, *qua*/*quo* → *cua*/*cuo*, ç → z, nn → ñ y, opcionalmente, ss → s e y vocálica → i), que se revisa y adapta a cada edición.
- **Sin fórmulas de legibilidad**, que están pensadas para prosa moderna.

## Ayuda integrada

Cada pestaña empieza con un recuadro plegable, «¿Para qué sirve este análisis?», que resume qué mide el análisis, qué preguntas permite responder y qué precauciones conviene tener al interpretarlo (por ejemplo, el efecto de la ortografía sin normalizar, o que la puntuación de las ediciones suele ser del editor). Al abrir o cerrar uno se abren o cierran todos, y la elección se recuerda.

## Verso y prosa

La opción **Tipo de texto** decide cómo se leen los saltos de línea. Por defecto se detecta automáticamente (en TEI, por la presencia de `<l>`; en texto plano, por la longitud de las líneas y los guiones a final de línea), y el resumen indica qué se ha detectado; se puede fijar a mano.

- **Verso**: cada línea es un verso (en TEI, cada `<l>`; sin `<l>`, cada `<lb/>`). Se analizan rima, **asonancia y tiradas**, inicio de verso, longitud de verso y la ley de Menzerath por verso, y las citas se dan por verso.

### Asonancia y tiradas

Para cada verso se calcula la asonancia de la palabra final: la vocal tónica y la última vocal (*llorando* → á-o, *Campeador* → ó, *tierra* → é-a). La tónica se toma del acento gráfico y, si no lo hay, de las reglas actuales de acentuación; los diptongos se reducen a su vocal plena, la *u* muda de *que*/*qui* y *gue*/*gui* se ignora, y en posición final *i* cuenta como *e* y *u* como *o*. Se muestran las asonancias más frecuentes y las **tiradas**: series de al menos tres versos seguidos con la misma asonancia, con su longitud y el verso en que empiezan.

Es una aproximación: no reconoce la *-e* paragógica (*Campeadore* sale como ó-e y no como ó), ni la acentuación medieval cuando difiere de la actual, ni los versos que rompen la serie por errores de transmisión, así que conviene revisarla con la concordancia.
- **Prosa**: los saltos de línea simples se leen como espacios y se unen las palabras partidas a final de línea (`-`, `¬`, `⸗`); en TEI, `<lb/>` es un espacio. El texto se organiza en párrafos (líneas en blanco o `<p>`) y oraciones, las citas se dan por párrafo u oración, y no se muestran los análisis de verso.

## Formatos de entrada

- **Texto plano** (`.txt`), con cualquier codificación habitual. Conviene conservar un verso por línea para el análisis de verso y rima.
- **TEI-XML** (p. ej. ediciones con criterios CHARTA o transcripciones convertidas a TEI):
  - Se elige la **capa de texto**: regularizada (`reg`, `corr`, `expan`, por defecto), diplomática (`orig`, `sic`, `abbr`, `am`) o normalizada (`@norm`).
  - Cada `<l>` es un verso (su `@n` se usa en las citas) y cada `<div>` una sección.
  - Los saltos `<lb break="no"/>` no cortan palabras, y por defecto se excluyen `note`, `metamark`, `fw`, `del`, las iniciales guía (`cue`) y el `teiHeader`.
  - Con la opción **dividir en textos por divisiones**, cada `<div>` de primer nivel se analiza como un texto aparte, llamado `n_archivo` (p. ej. `M_Erek_A_Wien` y `E_Erek_A_Wien`): sirve para comparar partes de un mismo códice o edición.
  - Las entidades MUFI declaradas en un DTD externo se resuelven con una tabla integrada y el texto se compone en Unicode NFC.
  - Si los `<w>` llevan `@lemma` (y `@pos`), se usan para analizar por lema y en la pestaña Gramática.
- **Salida de etiquetadores**: la de FreeLing (forma, lema, etiqueta EAGLES y probabilidad, separadas por espacios), la del TreeTagger y los CSV con cabecera se reconocen solos, aunque el archivo termine en `.txt`. El separador y las columnas se detectan automáticamente (también por la cabecera: *token*, *lemma*, *pos*…), y la aplicación indica lo que ha detectado.
- **CoNLL-U** (`.conllu`), por ejemplo de UDPipe: forma, lema y categoría; las oraciones vienen del archivo.
- **Tabulado** (`.tsv`, `.vrt`, `.csv`): también con columnas indicadas a mano; admite el formato VRT de CWB (p. ej. exportaciones de ANNIS).

Con texto anotado se puede analizar por **lema** en lugar de por forma, lo que neutraliza buena parte de la variación gráfica y flexiva.

### Lematizar

Mâze no lematiza por sí misma: lee los lemas que traiga el archivo. Para el español medieval hay varias vías:

- **FreeLing**, que tiene una adaptación al español antiguo (Sánchez-Marco, Boleda y Padró 2011, sobre los textos del Hispanic Seminary of Medieval Studies). Su salida (`forma lema etiqueta probabilidad`) se carga directamente.
- **UDPipe** u otros etiquetadores que produzcan CoNLL-U. Los modelos para español actual lematizan peor las formas medievales: conviene normalizar antes o revisar el resultado.
- **Corpus ya lematizados**: si exportas forma, lema y categoría en columnas (TSV o CSV con cabecera), Mâze las reconoce.

Elige **lema** como unidad de análisis. Con lemas, la concordancia busca todas las formas de un lema (y muestra el lema y la categoría de cada aparición) y la pestaña Gramática muestra categorías y formas por lema.

### Revisar la lematización

La pestaña **Anotación** muestra el texto verso a verso con el lema y la categoría debajo de cada palabra, marca las que se han quedado sin lema y permite resaltar o filtrar por lema, forma o categoría (con comodines). Debajo, dos tablas exportables: **lemas y sus formas**, para ver qué ha agrupado el lematizador bajo cada lema, y **formas con más de un lema**, que reúne homógrafos y posibles errores. Al hacer clic en un lema o una forma, el texto se filtra para mostrar solo sus apariciones.

**Convención de nombres** (la de Stylo; los nombres se pueden cambiar en la lista de archivos antes de analizar): `Autor_Titulo.txt`, p. ej. `Berceo_Milagros.txt`, `JuanRuiz_LibroBuenAmor.txt`. Lo que va antes del primer guion bajo es la clase (autor). Los archivos que empiezan por `anon_` o `desconocido_` se tratan como de autoría desconocida y la aplicación propone su atribución.

## Qué calcula

**Por texto**
- Recuentos y longitudes de palabra, oración y verso.
- Unas 30 medidas de riqueza léxica: TTR, Guiraud, Herdan, Maas, Brunet, Honoré, Sichel, Yule K/I, Simpson, MATTR, MSTTR, MTLD, HD-D, entropía, etc. Incluyen intervalos de confianza por bootstrap de bloques y un gráfico de estabilidad frente a la longitud.
- Zipf (mínimos cuadrados y Zipf-Mandelbrot), ley de potencia por máxima verosimilitud (Clauset, Shalizi y Newman 2009), espectro de frecuencias y ley de abreviación.
- Heaps con comparación frente al texto barajado; rarefacción exacta y extrapolación del vocabulario con el estimador de Chao (Chao et al. 2014); Chao1.
- **Estructura**: medidas por segmento (partes iguales, expresión regular, divisiones TEI o párrafos), densidad de hápax por segmento, palabras y terminaciones de rima, asonancias y tiradas, palabras iniciales de verso y ley de Menzerath-Altmann.
- Frecuencias con dispersión D de Juilland, tendencias y gráfico de dispersión, concordancias KWIC con referencia de verso y ordenación por dos claves.
- Colocados (MI, t-score, logDice, G²), red de coapariciones, n-gramas para localizar fórmulas, y nube de palabras.
- **Gramática** (con texto anotado): distribución de categorías, densidad léxica y formas por lema.
- **Anotación** (con texto anotado): el texto con lema y categoría bajo cada palabra y tablas para revisar la lematización (véase más arriba).

**Corpus**
- **Comparación**: tabla de medidas con rarefacción a un tamaño común, intervalos de confianza y curvas de Zipf, Heaps y rarefacción superpuestas.
- **Keyness**: log-likelihood (G²), BIC, %DIFF y log ratio frente a otro texto o al resto del corpus; útil también para comparar testimonios o redacciones.
- **Estilometría**: Delta de Burrows, de Eder, del coseno y cuadrática sobre las MFW, con culling y muestreo. Los nombres propios (palabras que en los textos van casi siempre con mayúscula fuera de inicio de verso u oración) se excluyen por defecto de los rasgos, y se puede añadir una lista propia de palabras excluidas. Ofrece dendrograma (Ward, UPGMA, completo, simple), PCA con cargas, matriz de distancias, clasificación con validación cruzada (vecino más próximo y centroide) y atribución de textos anónimos.
- **Rolling Delta**: distancia de una ventana móvil del texto objetivo a cada autor de referencia, para detectar cambios de mano, de fuente o interpolaciones. El eje horizontal se da en versos (o párrafos), el recuadro al pasar el ratón indica el tramo que abarca cada ventana y las divisiones del texto se marcan con líneas verticales. En la transmisión manuscrita la señal mezcla autor y copista (y, en los textos copiados en otra zona, rasgos dialectales): conviene trabajar con texto normalizado o por lema.

**Léxicos**: esta edición no incluye ningún léxico, pero se puede cargar cualquiera en TSV (categoría + formas) o una lista de palabras, en UTF-8 o Windows-1252: léxicos de sentimiento o emociones, campos semánticos, vocabulario jurídico o religioso, etc. Se obtienen la densidad por categoría, el índice de polaridad (si hay categorías positiva y negativa), el arco a lo largo del texto, las palabras que más contribuyen y la comparación entre textos; opcionalmente, comparación por lema e inversión de la polaridad tras una negación. Las palabras que no funcionan en contexto se pueden desactivar, y la elección se guarda.

**Normalización ortográfica**: reglas `origen => destino` (también con expresiones regulares). Las reglas y opciones se pueden guardar como perfiles reutilizables, que también se exportan e importan en JSON.

**Exportación**: cada tabla en CSV o Excel (.xlsx); cada gráfico en SVG o PNG; todas las métricas en JSON o Excel; y un informe HTML con parámetros, tablas y gráficos, que también puede imprimirse como PDF.

## Cómo citar

El archivo `CITATION.cff` permite que GitHub muestre el botón *Cite this repository*. Para obtener un DOI permanente:

1. Conecta la cuenta de GitHub en <https://zenodo.org> y activa el repositorio en *GitHub → Repositories*.
2. Crea una *release* en GitHub (p. ej. `v2.0.0`): Zenodo la archiva y le asigna un DOI.
3. Añade el DOI a `CITATION.cff` (campo `doi:`) y a este README.

## Licencia

El código de Mâze se publica con licencia MIT (véase `LICENSE`).
