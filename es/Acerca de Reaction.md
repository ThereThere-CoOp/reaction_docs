![[icon_version_2.png]] 

**Reaction** es un plugin para el motor gráfico  Godot 4.x que implementa  el  [Sistema de respuesta de Valve](https://developer.valvesoftware.com/wiki/Response_System) usados en el juego **Left 4 Dead**.  El sistema ha sido implementado por otros desarrolladores como **Naughty Dog** en en el videojuego **Last Of Us** y por los desarrolladores **Campo Santo** en **Firewatch**. 

### Antecedentes
Durante nuestra búsqueda de como implementar un sistema para la gestión de diálogos para nuestro videojuego [Shadow's Keep](https://2therecoop.itch.io/shadows-keep), unos de los principales desafíos que teníamos era como manejar diálogos y narrativa altamente ramificadas dependiente de una gran cantidad de variables. El acercamiento común es implementarlo con condicionales *(sentencias if/else)*. Pero si se desea construir diálogos que den la ilusión de ser realmente reactivos necesitas verificar y modificar una enorme cantidad de variables y construir gran cantidad de posibles rutas que ha medida que la longitud del videojuego crezca indudablemente puede ser la causa del problema conocido como *infierno if/else*(como se muestra en el bloque de código a continuación) que tirara por la borda la escabilidad del código.

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

 De manera fortuita gracias a la internet terminamos encontrando [una charla GDC de Valve](https://www.youtube.com/watch?v=tAbBID3N64A) acerca del sistema que implementaron y usaron en varios de sus videojuegos que termino convirtiéndose en la solución que tanto estábamos buscando. Después de una rápida búsqueda para encontrar si otros desarrolladores habían implementado un plugin con un sistema similar en Godot no arrojo ningún resultado, por lo que decidimos implementar uno propio.

