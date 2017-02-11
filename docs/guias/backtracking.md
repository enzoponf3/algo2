Algoritmos BackTracking
=======================

Ejercicio Resuelto:
-------------------
Se tiene una lista de materias que deben ser cursadas en el mismo cuatrimestre, cada materia está representada con una lista de cursos/horarios posibles a cursar (solo debe elegirse uno de todos ellos). Cada materia puede tener varios cursos. Implementar el pseudocódigo de un algoritmo de backtracking que devuelva un listado con todas las combinaciones posibles que permitan asistir a un curso de cada materia sin que se solapen los horarios.
Considerar que existe una función son_compatibles(curso_1, curso_2) que dados dos cursos devuelve un valor booleano que indica si se pueden cursar al mismo tiempo.

Solucion:
--------

Lo primero que hay que tratar de entender, es que es necesario tratar de ir generando las combinaciones hasta encontrar alguna válida. Por fuerza bruta, sería obtener primero todas las combinaciones de materias (un curso por cada materia) y luego ver cuáles son incompatibles para filtrar.
Suponiendo que todas las materias tenga $k$ cursos, y que tenemos $n$ materias, tendríamos un orden de $O(k^n)$ para generar cada combinacion (pues hay $k^n$ combinaciones, supondremos que generar cada una costará a lo sumo O(n), despreciable al lado del otro término aunque k fuere constante o muy pequeño).
Luego, sería necesario filtrar las opciones incorrectas, pero esto lo vamos a obviar porque la idea es plantear que esto no sea necesario. La idea va a ser ir generando todas esas combinaciones posibles, pero en cuanto detectemos que una opcion no es válida, simplemente descartarla. 
Por ejemplo, si ya sabemos que el primer curso de Algoritmos y Programación II no es compatible con el primer curso de Álgebra II, por qué ver todas las combinaciones que incluyan estos dos cursos? Ya podemos descartarlas al encontrar esta colisión. En el peor de los casos, todos los cursos son compatibles entre sí sin colisión y mantendremos el orden (aunque siendo realistas, esto no sucede ni cuando las materias son de curso único).
Por lo tanto, allí estará la poda: cuando detectemos que un curso es incompatible con el resto, descartamos la opción. Simplemente con analizar si la ultima materia agregada en cada paso es compatible con las anteriores, será suficiente (luego, no es necesario validar la primera con la segunda, la priera con la tercera, etc... porque eso ya debería haberse hecho). De esta forma reducimos el costo de esa poda de $O(n^2)$ a $O(n)$.
```Python
def horarios_posibles(materias, solucion_parcial):
    # Si no nos quedan materias por ver
    if len(materias) == 0:
        if solucion_posible(solucion_parcial):
            return [solucion_parcial]
        else:
            return []
    
    # No es solucion total, pero es solucion parcial?
    if not solucion_posible(solucion_parcial):
        return []
    
    # Caso general, por ahora la solucion parcial es aceptada:
    materia_actual = materias.ver_primero()
    materias.borrar_primero()
    
    soluciones = []
    
    for curso in materia_actual:
        # Si devuelve una lista con soluciones parciales, se agregaran todas a esta lista. Si devuelve lista vacia, no hara nada
        soluciones.extend(horarios_posibles(materias_restantes, solucion_parcial + [curso]))
    
    # Si interesa volver a poner la materia que sacamos:
    materias.guardar_primero(materia_actual)
    
    return soluciones
    
def solucion_posible(horarios):
    ultimo = horarios.ver_ultimo()
    for curso in horarios (salvo el ultimo):
        if not son_compatibles(curso, ultimo):
            return False
    return True
```

Ejercicios Propuestos:
----------------------
1) Diseñar un algoritmo de backtracking que resuelva el siguiente problema: dado una pieza de caballo de ajedrez (knight, o caballero) dentro de un tablero, determinar los movimientos a hacer para que el caballo logre pasar por todos los casilleros, una única vez.
Recordar que el caballo se mueve en forma de L (dos casilleros en una dirección, y un casillero en dirección perpendicular).
2) a) Escribir en pseudocódigo un algoritmo de tipo Backtracking que reciba una cantidad de dados n y una suma s. La función debe imprimir todas las tiradas posibles de n dados cuya suma es s. Por ejemplo, con n = 2 y s = 7, debe imprimir [1, 6] [2, 5] [3, 4] [4, 3] [5, 2] [6, 1].
b) ¿De qué orden es el algoritmo en tiempo y memoria?
3) Escribir una función en pseudo-código que, utilizando backtracking, dada una lista de enteros positivos L y un entero n devuelva todos los subconjuntos de L que suman exactamente n. ¿Cómo cambia el comportamiento del algoritmo frente a una solución por fuerza bruta? Explicar con al menos dos ejemplos de L y n.
4) ¡Ejercicio difícil! Realizar el ejercicio anterior, si además se agregar como restricción que no se puede utilizar más de una vez un mismo elemento de la lista L, pero se quieren maximizar la cantidad de conjuntos obtenidos. 
