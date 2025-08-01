# Lab3_DL

---

# Respuestas a preguntas:

## Parte 1

* ¿Qué interpretación le da a la separación de las graficas de training y validation?

R// La brecha creciente —donde la training loss (roja) baja más rápido y se mantiene por debajo de la validation loss (azul)— indica que el modelo está ajustando mejor los ejemplos de entrenamiento que los de validación. Esto suele señalar un inicio de sobreajuste: la red memoriza patrones del set de entrenamiento que no generalizan tan bien al set de validación.

* ¿Cree que es un buen modelo basado solamente en el loss?

R// Aún cuando ambas pérdidas descienden de forma consistente, la diferencia entre ellas (~1.5–2 NLL al final) sugiere que el modelo está aprendiendo, pero está capturando ruido o detalles demasiado específicos del entrenamiento. Para considerarlo “bueno” solo por el loss, querríamos ver una brecha menor; aquí probablemente convendría más regularización (dropout, early stopping) o menos épocas.

* ¿Cómo deberían de verse esas gráficas en un modelo ideal?

R// Curvas muy cercanas: training y validation convergerían casi solapadas.

Descenso suave y paralelo: ambas perderían valor a ritmos similares, sin que ninguna se estanque muy por debajo de la otra.

Estabilidad al final: tras suficientes épocas, se estabilizarían en un nivel bajo de NLL, señalando buena generalización sin sobreajuste.

## Parte 2

* ¿Qué modelo funcionó mejor? ¿RNN tradicional o el basado en LSTM? ¿Por qué?
  R// En este experimento la RNN “vanilla” obtuvo curvas de pérdida más suaves y un nivel de NLL final menor (≈6 – 6.5) tras ~2 000 épocas, mientras que el LSTM mostró pérdidas muy altas y ruidosas (picando hasta 60) antes de descender hacia ≈7 – 8 en 500 épocas. Por lo tanto, la RNN tradicional resultó “mejor” en términos de valores de loss y estabilidad con los hiperparámetros actuales.

* Observen la gráfica obtenida arriba, ¿en qué es diferente a la obtenida a RNN? ¿Es esto mejor o peor? ¿Por qué?

  R// Oscilaciones y picos extremos: El LSTM presenta subidas bruscas y altísimos NLL (30–60) en muchas épocas, mientras que la RNN clásica tiene una curva monótona y suavemente decreciente.

Brechas variables: En LSTM la diferencia training/validation varía mucho, reflejando posible inestabilidad de gradientes o learning rate muy agresivo.

Conclusión: Este comportamiento es peor, pues indica que el LSTM no está convergiendo de forma estable bajo estos ajustes; la RNN alcanzó pérdidas más bajas y consistentes.

  
* ¿Por qué LSTM puede funcionar mejor con secuencias largas?
  
  R// Gates y estado de celda: Sus “forget”, “input” y “output” gates regulan qué información retener/olvidar, frenando el desvanecimiento o explosión de gradiente.

Memoria a largo plazo: El cell state c_t introduce un “canal” casi lineal por el cual la red puede transportar gradientes sin saturarse en activaciones tanh o sigmoide.

Dependencias remotas: Gracias a ese diseño, un LSTM puede aprender relaciones entre tokens muy distantes en la secuencia, donde una RNN estándar se pierde.

## Parte 3

* Compare las graficas obtenidas en el LSTM "a mano" y el LSTM "usando PyTorch, ¿cuál cree que es mejor? ¿Por qué?

  R// El LSTM con PyTorch es significativamente mejor porque utiliza implementaciones optimizadas en C++/CUDA que son más rápidas y numéricamente estables, maneja automáticamente la inicialización de pesos y gradient clipping, y puede aprovechar GPU para acelerar el entrenamiento. Las gráficas de pérdida con PyTorch típicamente muestran convergencia más suave y rápida, mientras que la implementación manual tiende a tener más ruido y convergencia inestable debido a errores de implementación y falta de optimizaciones

* Compare la secuencia target y la predicha de esta parte, ¿en qué parte falló el modelo?

  R// El modelo falló principalmente en la transición crítica de 'a' a 'b' (alrededor de la posición 10-11) y en la predicción del token especial 'EOS' al final de la secuencia. Esto ocurre porque el modelo tiene dificultades para "contar" exactamente cuántos tokens 'a' ha procesado antes de cambiar a 'b', y el token 'EOS' aparece con poca frecuencia en el dataset, haciendo que sea difícil de predecir correctamente.

* ¿Qué sucede en el código donde se señala "NOTA 1" y "NOTA 2"? ¿Para qué son necesarias estas líneas?

  R// net.eval() (NOTA 1) pone la red en modo evaluación, desactivando dropout y usando estadísticas fijas para batch normalization, lo que garantiza predicciones consistentes durante validación. net.train() (NOTA 2) reactiva el modo entrenamiento con dropout y batch normalization dinámicos para regularización. Estas líneas son necesarias porque sin ellas, las métricas de validación serían inconsistentes y el modelo no se regularizaría correctamente durante el entrenamiento.
---

## Dependencias y como correr

Se puede usar Google Colab o usar un entorno virtual.

```bash
pip install virtualenv
```

Con venv

```bash
python -m venv venv
source venv/bin/activate
```

Dentro del entorno virtual:

```bash
pip install jupyterlab
```

Instalar librerías:
```bash
pip install -r requirements.txt
```


Levanta el servidor:

```bash
jupyter lab
```
