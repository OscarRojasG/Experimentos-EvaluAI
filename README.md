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
