# Justificaciones

## P1

El déficit total no se obtiene del déficit neto global (demanda total − generación total), ya que los excedentes de unos registros restarían a los déficits reales de otros y los ocultarían. Para obtenerlo, se deja en 0 la sobreproducción de cada registro y luego se suman solo los déficits reales.

El segmentador de región actúa directamente sobre la cantidad de registros con los que se hacen los cálculos, y la cobertura se recalcula con el déficit y la demanda de esos registros. Por eso no se puede obtener sumando ni promediando las coberturas de las regiones.

## P2

Se validaron 8 sedes y se excluyó 1 porque tenía coordenadas incorrectas: S09 La Unión, con una longitud fuera del rango establecido. La sede con mayor déficit válido es San Salvador, con 77,336.41 kWh.

Mostrar coordenadas erróneas puede hacer que los puntos aparezcan en lugares equivocados del mapa y causar confusión a la hora del análisis.

## P3

Al regenerar el informe cambian los KPIs (energía 2,913,624 kWh, demanda 3,146,867 kWh y déficit 286,904 kWh), el gráfico, el hallazgo, el periodo y la fecha, porque `generar_reporte.py` recalcula todo con Pandas desde `operacion_sedes.csv`, sin ingresar cifras a mano.

La concordancia con el dashboard la prueban `registros_entrada` (1,620) y `sha256_fuente`, que muestran que ambos usaron el mismo archivo, y los 3 KPIs coinciden con `indicadores.csv`.

La principal limitación operativa es que el informe solo está tan actualizado como el CSV, que hay que reemplazar y volver a ejecutar el script. Además, el déficit se mide por sede-día, sin compensar excedentes.

## P4

La prueba va del 2026-05-26 al 2026-06-29 (35 días). El MAE del baseline fue 1194.57 kWh y el del modelo 298.77 kWh, una mejora de 74.99 %.

No se debe entrenar con datos del futuro porque habría fuga: el modelo vería información que en la vida real no tendría al pronosticar, y las métricas saldrían abultadas. Por eso el split es cronológico y `media_movil_7` usa `shift(1)`, para no incluir la demanda del día que se predice.

El pronóstico solo sirve si se conoce la demanda de ayer y de hace 7 días y el calendario de eventos. Además, con solo 173 días de historia el modelo no ve un año completo y no captura estacionalidad anual, así que es una estimación, no una certeza.


