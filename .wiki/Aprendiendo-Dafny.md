# Introducción

A continuación se va a introducir _Dafny_, o al menos la parte de _Dafny_ que se va a utilizar en la materia.

_Dafny_ es un lenguaje imperativo que soporta clases genéricas, asignación dinámica de memoria, y tipos de datos inductivos. A su vez, y su característica más importante, es su soporte para escribir especificaciones y la verificación automática de las mismos.

La verificación que realiza _Dafny_ es estática (no requiere la ejecución del programa), y está basada en _Satisfiability Modulo Theories Solvers_.

En los ejemplos vamos a seguir la [guía de estilo para _Dafny_](https://github.com/dafny-lang/dafny/blob/master/docs/StyleGuide/Style-Guide.md).

Para más información pueden referirse a la [guía de _Dafny_](https://github.com/dafny-lang/dafny/blob/master/docs/OnlineTutorial/guide.md) provista en su [repositorio](https://github.com/dafny-lang/dafny).

En conjunto con la guía anterior, tienen una [Guía de usuario](https://github.com/dafny-lang/dafny/blob/master/docs/DafnyRef/UserGuide.md). Aunque intentaremos dar toda la información relevante a la materia en las siguientes secciones.

## Métodos y especificaciones

Un método va a tener la siguiente estructura:

```
METODO -> method NOMBRE([TIPOS]) returns (TIPOS)
              [SPECS]
          BLOQUE
NOMBRE -> <nombre comenzando con letras y en camelCase, y pueden llevar una tilde al final>
TIPOS -> TIPO
TIPOS -> TIPO, TIPOS
TIPO -> NOMBRE : <tipo>
SPECS -> reads VAR
         [SPECS]
SPECS -> requires <BOOL_EXPR>
         [SPECS]
SPECS -> ensures <BOOL_EXPR>
         [SPECS]
VAR -> NOMBRE
BOOL_EXPR -> <una expresión booleana>
BLOQUE -> {
              SENTENCIA
          }
SENTENCIA -> BLOQUE
SENTENCIA -> SENTENCIA
             SENTENCIA
SENTENCIA -> DECLARACION_VARIABLE
SENTENCIA -> ASIGNACION
SENTENCIA -> ACCESO_ARREGLO := <una expresión del tipo de los elementos del arreglo>
SENTENCIA -> while <BOOL_EXPR>
                 [WHILE_SPECS]
             BLOQUE
SENTENCIA -> IF
SENTENCIA -> ASSERT
SENTENCIA -> LEMMA_CALL
SENTENCIA -> RETORNO
DECLARACION_VARIABLE -> var ASIGNACION
ASIGNACION -> VAR := <una expresión del tipo de VAR>
ACCESO_ARREGLO -> VAR[<entero>]
ACCESO_ARREGLO -> ACCESO_ARREGLO[<entero>]
IF -> if (BOOL_EXPR) then
      BLOQUE
      [ELSE]
ELSE -> else {
            SENTENCIA
        }
ASSERT -> assert BOOL_EXPR
LEMMA_CALL -> NOMBRE([VARS])
VARS -> VAR
VARS -> VAR, VARS
RETORNO -> return
RETORNO -> return NOMBRE
RETORNO -> return <expresión del tipo que el método retorna>
WHILE_SPECS -> decreases <expresión de tipo entero>
               [WHILE_SPECS]
WHILE_SPECS -> invariant <BOOL_EXPR>
```

Los tipos incluyen: `int`,`nat`,`bool`,`array<TIPO>`,`array?<TIPO>`. un `array?<int>` indica un arreglo que puede ser `null`.
El `return` puede usarse para detener la ejecución en un punto, no es necesario retornar, el último valor que adquieren las variables de retorno va a ser el "retorno" del método. El uso de `reads VAR` hace referencia a que se puede acceder a una variable para leer su contenido, esto se usa para arreglos donde es necesario acceso a componentes internos del arreglo (los elementos).

Un ejemplo de un método _Dafny_ es:

```
method sumArray(a:array<int>) returns (r:int)
    ensures r == sum(a,0,a.Length)
{
    r := 0;
    var i := 0;
    while i < a.Length
        invariant 0 <= i <= a.Length
        invariant r == sum(a,0,i)
        decreases a.Length-i
    { 
        sumLemma2(a,i); // Indicamos a Dafny que puede usar el sumLemma
        r := r + a[i];
        i := i+1;
    }
}
```


## Funciones y predicados

Las funciones y los predicados permiten abstraer especificaciones complejas, ambos son considerados declaraciones "fantasmas" o "ghost" en el sentido de que son auxiliares a las especificaciones o no forman parte del programa.

Las funciones se diferencian de los métodos en que retornan un valor y permiten recursion. Los predicados abstraen expresiones booleanas.

La gramática para una función es la siguiente, para simplificar se van a reutilizar definiciones en la gramática para métodos:

```
FUNCION -> function NOMBRE([TIPOS]):<tipo>
           [decreases <expresión de tipo entero>]
           [SPECS]
           BLOQUE_FUNCION
BLOQUE_FUNCTION -> {
                       SENTENCIA_FUNCION
                   }
SENTENCIA_FUNCION -> ASIGNACION
SENTENCIA_FUNCION -> DECLARACION_VARIABLE
SENTENCIA_FUNCION -> ACCESO_ARREGLO := <una expresión del tipo de los elementos del arreglo>
SENTENCIA_FUNCION -> EXPRESION
EXPRESION -> <expresión del mismo tipo que el retorno de la función, incluye llamadas a funciones>
EXPRESION -> IF_EXPRESION
IF_EXPRESION -> if BOOL_EXPR then EXPRESION else EXPRESION
```

Un ejemplo de una función en _Dafny_ es:

```
function sum(a:array<int>,i:nat,j:nat):int
    decreases j-i
    reads a
    requires 0 <= i <= j 
    requires 0 <= j <= a.Length
{
    if j == 0 || i>=j  then 0
    else  a[i] + sum(a,i+1,j) 
}
```

//TODO: predicados

## Verificando un programa

//TODO