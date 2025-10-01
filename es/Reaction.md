---
dg-publish: true
dg-home: true
---
# Reaction

![[reaction_banner.png|]]

**Reaction** es un plugin para el motor gráfico  Godot 4.x que implementa  el  [Sistema de respuesta de Valve](https://developer.valvesoftware.com/wiki/Response_System) usados en el juego **Left 4 Dead**.  El sistema ha sido implementado por otros desarrolladores como **Naughty Dog** en en el videojuego **Last Of Us** y por los desarrolladores **Campo Santo** en **Firewatch**. 

## Antecedentes

Durante nuestra búsqueda de como implementar un sistema para la gestión de diálogos para nuestro videojuego [Shadow's Keep](https://2therecoop.itch.io/shadows-keep), unos de los principales desafíos que teníamos era como manejar diálogos y narrativa altamente ramificadas dependiente de una gran cantidad de variables o estados del juego. El acercamiento común es implementarlo con condicionales *(sentencias if/else)*. Pero si se desea construir diálogos que den la ilusión de ser realmente reactivos necesitas verificar y modificar una enorme cantidad de variables y construir gran cantidad de posibles rutas que ha medida que la longitud del videojuego crezca indudablemente puede ser la causa del problema conocido como *infierno if/else*(como se muestra en el bloque de código a continuación) que tirara por la borda la escabilidad del código.

```
func get_dialog_response(player_input: String) -> String:
    if player_input == "hello":
        return "Hi there, traveler!"
    else:
        if player_input == "how are you?" and guard_health == 100:
            return "I’m just guarding the gate as usual."
        else:
            if player_input == "quest" and is_sick_guard:
	            doSomething()
                return "Yes, I have a task for you. Bring me 5 herbs."
            else:
                if player_input == "reward":
	                doAnotherThing()
                    return "The reward is 100 gold coins."
                else:
                    if player_input == "bye":
                        return "Farewell, stranger."
                    else:
                        # it just keep going!!!!!!!!
```

 De manera fortuita gracias a la internet terminamos encontrando [una charla GDC de Valve](https://www.youtube.com/watch?v=tAbBID3N64A) acerca del sistema que implementaron y usaron en varios de sus videojuegos que termino convirtiéndose en la solución que tanto estábamos buscando. Después de una rápida búsqueda para encontrar si otros desarrolladores habían implementado un plugin con un sistema similar en Godot y no encontrar ningún resultado, decidimos implementar nuestra propia solución.

## Conceptos generales

**Reaction** puede ser usado para decidir que recurso devolver (o si no se devuelve nada) en una escena por cualquier componente. En nuestro caso la necesidad inicial era de devolver dialogo según el estado o contexto actual de la partida. Pero puede ser empleado para devolver cualquier recurso en Godot. Ejemplo: audio, animaciones, scripts, etc.

> [!TIP] 
> # Mucho más que un gestor de diálogos!!!
> La gestión de diálogos es una de las aplicaciones comunes para **Reaction**, pero puede ser
>  usado como un patrón altamente condicional para devolver recursos.

El funcionamiento de manera general es que existe una **pizarra** o almacén de datos que contiene todo el estado actual del juego o de la partida *(como un diccionario o tabla hash )* , la cual es única *"fuente de verdad"* de **Reaction**. Cuando un componente ejecuta un evento de **Reaction** revisara todas las **reglas** definidas para ese evento. Cada regla tiene definidos distintos **criterios** ( los criterios son condiciones que debe cumplirse en la **pizarra** ) , la primera regla que cumpla todos sus condiciones será la que devuelva su **respuesta** o recursos asociados.

Para más detalles revisar [[Como funciona Reaction]].


