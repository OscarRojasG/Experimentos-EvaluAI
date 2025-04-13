# Framework para experimentos EvaluAI

El objetivo de este framework consiste en proporcionar un conjunto de funciones predefinidas para facilitar la evaluación y comparación de prompts en el contexto del proyecto EvaluAI.

Antes de comenzar, en la pestaña **Secrets** de Google Colab se debe establecer la variable de entorno **OPENAI_API_KEY** para poder acceder a los servicios de la API.

![Sección Secrets de Google Colab](/images/secrets.png)

Los pasos básicos para la utilización del framework se resumen a continuación:

1. Ejecutar las celdas de código de la sección *Implementación*.
2. Cargar el dataset con los datos de preguntas y respuestas.
3. Especificar el o los prompts a utilizar para el experimento.
4. Ejecutar el experimento.

Las instrucciones detalladas se describen en las secciones siguientes.

## Cargar dataset

Se debe subir un archivo *xlsx* con al menos las siguientes columnas:
* **Contexto**: Conocimiento previo que necesita el modelo para evaluar la respuesta del estudiante.
* **Pregunta**: Pregunta evaluada.
* **Respuesta**: Respuesta del estudiante.
* **Evaluación manual**: Puntaje de referencia dado por uno o más evaluadores humanos.
* **Dataset de origen**: Dataset del cual provienen los datos para una fila en particular. Podría hacer referencia a diferentes evaluaciones, categorías de alumnos, cursos, etc.

El nombre de las columnas se debe indicar en un diccionario con la siguiente estructura:

```
column_data = {
    "context": ...,
    "question": ...,
    "answer": ...,
    "real_eval": ...,
    "dataset": ...
}
```

Por último, se debe cargar el dataset con la función `load_dataset(path, sheet_name, column_data)`

- **path** - Ruta del archivo *xlsx* a utilizar.
- **sheet_name** - Nombre de la hoja donde se encuentran los datos.
- **column_data** - Diccionario con el nombre de las columnas.

## Definir prompts a utilizar

Los prompts se componen de varios *mini-prompts*. Cada *mini-prompt* representa una parte del prompt completo, como la pregunta, la respuesta del estudiante, la instrucción para solicitar el puntaje, etc.

El framework cuenta con algunos *mini-prompts* predefinidos en la ruta *GPTEvaluator/Experiments/Miniprompts_v2*, aunque también se pueden modificar o agregar nuevos *mini-prompts* en caso de ser necesario.

El prompt a evaluar se debe construir mediante un diccionario, donde la clave corresponde al nombre del *mini-prompt* o componente (question, examples, feedback...) y el valor corresponde a la variante a utilizar (feedback_minimal, feedback_full, feedback_normal...). Se debe respetar la estructura de archivos, donde los componentes son las carpetas y las variantes son archivos *txt* dentro de cada carpeta.

Usar como guía el siguiente ejemplo:

```
prompt_data = {
    "examples_basic": "examples_XXXX_basic.txt",
    "context": "...",
    "question": "...",
    "answer": "...",
    "instructions": {
        "analysis": "...",
        "feedback": "...",
        "score": "...",
    }
}
```

Además, se puede utilizar el comodín * como valor para probar con todos los miniprompts de una carpeta en particular.

Finalmente, cargar el o los prompts con la función `generate_prompts(prompt_data, prompt_folder, key_pos)`

- **prompt_data** - Diccionario con la estructura del prompt.
- **prompt_folder** - Ruta de la carpeta donde se encuentra la colección de miniprompts.
- **key_pos** - Indica el orden de las claves en el diccionario de salida. Un valor de `before` solicitará el análisis y feedback antes que el score. Por el contrario, un valor de `after` pedirá estos valores posterior al score.
