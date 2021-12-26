# pvpc

_Data extraction library/wrapper for the [Esios PVPC](https://www.esios.ree.es/es/pvpc) API. Only exports PVPC price for a given day (€/kWh for each hour), from June 1st, 2021._

Librería/wrapper para extracción de datos del [portal Esios PVPC](https://www.esios.ree.es/es/pvpc). Únicamente exporta el precio [PVPC (Precio voluntario para el pequeño consumidor)](https://es.wikipedia.org/wiki/Precio_voluntario_para_el_pequeño_consumidor) para un día concreto (€/kWh por cada hora), a partir del 1 de junio del 2021.

## Usage

```python
import pvpc

# Obtener valores para un día
r = pvpc.get_pvpc_day("2021-12-12")
# Argumento de función: fecha, que puede ser objeto datetime.date, o string en formato YYYY-MM-DD
# Devuelve un objeto models.PVPCDay (basado en Pydantic)

# Si una fecha todavía no tiene valores disponibles, lanza excepción propia PVPCNoDataForDay.
# REE actualiza los precios a última hora de la tarde del día anterior.
try:
    r = pvpc.get_pvpc_day("3001-12-12")
except pvpc.PVPCNoDataForDay:
    print("No data for day!")

# Obtener todos los valores, por hora...
# ...para Peninsula/Canarias/Baleares:
data = r.data.pcb.hours
# ...para Ceuta/Melilla:
data = r.data.cm.hours

# "hours" es un diccionario, donde clave = horas del día, valores = coste en €/kWh
for hour, cost in data.items():
    print(f"{hour}h: {cost} €/kWh")
    # Ejemplo: "0h: 0.33694 €/kWh"

# Convertir a diccionario
r.dict()
# Convertir a string JSON
r.json()
```

Salida JSON/diccionario:

```json5
{
  "day": "2021-12-12", // si es diccionario Python, devuelve objeto datetime.date
  "data": {
    "pcb": { // Peninsula, Canarias, Baleares
      "hours": {
        // Claves son las horas del dia en formato 24h ("0" = from 0:00 to 1:00)
        // Valores son el coste en €/kWh
        "0": 0.33694,
        "1": 0.32597,
        "2": 0.31901,
        "3": 0.30213,
        "4": 0.28941,
        "5": 0.29458,
        "6": 0.30343,
        "7": 0.32168,
        "8": 0.31098,
        "9": 0.32455,
        "10": 0.31445,
        "11": 0.30529,
        "12": 0.31768,
        "13": 0.31913,
        "14": 0.31193,
        "15": 0.30304,
        "16": 0.32298,
        "17": 0.35688,
        "18": 0.37038,
        "19": 0.37381,
        "20": 0.36729,
        "21": 0.36467,
        "22": 0.36055,
        "23": 0.32983
      }
    },
    "cm": { // Ceuta, Melilla
      "hours": {
        "0": 0.33694,
        "1": 0.32597,
        "2": 0.31901,
        "3": 0.30213,
        "4": 0.28941,
        "5": 0.29458,
        "6": 0.30343,
        "7": 0.32168,
        "8": 0.31098,
        "9": 0.32455,
        "10": 0.31445,
        "11": 0.30529,
        "12": 0.31768,
        "13": 0.31913,
        "14": 0.31193,
        "15": 0.30304,
        "16": 0.32298,
        "17": 0.35688,
        "18": 0.37038,
        "19": 0.37381,
        "20": 0.36729,
        "21": 0.36467,
        "22": 0.36055,
        "23": 0.32983
      }
    }
  }
}
```

## TODO

- Tests
- CLI (interfaz línea de comandos)

## Disclaimer

La fuente de datos proviene del portal [PVPC | Esios electricidad](https://www.esios.ree.es/es/pvpc) de Red Eléctrica de España.
La reutilización de estos datos se permite bajo ciertas condiciones especificadas en el [Aviso legal - Condiciones respecto al contenido](https://www.esios.ree.es/es/aviso-legal-y-politica-de-privacidad#condiciones-respecto-al-contenido).
