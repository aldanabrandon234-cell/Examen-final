[Respuestas.txt](https://github.com/user-attachments/files/28914283/Respuestas.txt)
SERIE III: 

¿Qué error(es) tiene este código al ejecutarlo? ¿Por qué ocurre?

Al revisar el código identifiqué dos errores principales. El primero es un error de sintaxis bastante simple en la clase Mascota, específicamente dentro del método Mostrar(), donde a la línea "Console.WriteLine("Mascota: " + nombre)" le hace falta el punto y coma final, lo que va a evitar que el proyecto compile. El segundo es un error de lógica más grave que saltará en tiempo de ejecución como un NullReferenceException. Esto pasa en la función Main cuando se intenta hacer "m1.tipo.ToUpper()". Como el constructor que se usó para instanciar a m1 solo recibe el nombre y la edad, el atributo "tipo" nunca se inicializa y se queda como null. Al intentar aplicarle un método de cadena a un objeto nulo, el programa se rompe por completo.

¿Cómo mejoraría la clase Mascota en términos de encapsulamiento?

Para cumplir realmente con el principio de encapsulamiento, lo correcto es que los atributos no estén expuestos de forma pública. Actualmente cualquier otra clase podría meterse y cambiar los valores de nombre, edad o tipo de manera directa y sin ningún control, lo cual es una mala práctica.

La solución es pasar todos los atributos a privados y controlar su acceso por medio de propiedades públicas usando bloques get y set. De esta manera podemos meter validaciones, como por ejemplo, asegurarnos de que no nos ingresen una edad negativa.

Qué implicaciones tiene que Mascota sea una clase y no una struct y cual recomienda?

La diferencia clave está en cómo manejan la memoria. Al definir Mascota como una clase, se convierte en un tipo de referencia que se guarda en el Heap. Esto significa que si asignamos esta variable a otra, lo que se copia es la dirección de memoria, por lo que ambas variables apuntarían al mismo objeto y si modificamos una, se altera la otra. Por otro lado, si fuera un struct, funcionaría como un tipo de valor almacenado en el Stack, donde cada asignación genera una copia completamente independiente en la memoria. Además, las clases permiten herencia y admiten valores nulos, cosa que los structs no hacen de forma nativa.

Para este caso en concreto, mi recomendación es mantenerlo como una clase. Los structs en C# se deberían usar solo para almacenar estructuras de datos muy pequeñas, primitivas e inmutables, como un punto en un plano cartesiano. Una mascota representa una entidad del mundo real que va a estar cambiando de estado constantemente (como cumplir años) y que muy probablemente en el futuro necesite heredar de una clase base o extenderse a subtipos como Perro o Gato.

¿Cómo usaría ToString() correctamente para mostrar toda la información?

La mejor forma de hacerlo es aprovechar el polimorfismo sobreescribiendo el método ToString() que ya viene por defecto en la clase base Object. Esto nos permite deshacernos de métodos propios como Mostrar() y estandarizar la salida del objeto de una forma mucho más limpia.

Primero, dentro de la clase Mascota añadiría la sobreescritura utilizando interpolación de cadenas para que sea más legible:

public override string ToString() {
return $"Mascota: {nombre} | Edad: {edad} años | Tipo: {tipo ?? "No especificado"}";
}

Luego, para usarlo en el Main, ya no hace falta llamar a métodos extra. Basta con pasarle el objeto directamente al Console.WriteLine, ya que el sistema detecta de forma automática que queremos imprimir la representación en texto del objeto:

Mascota m1 = new Mascota("Luna", 3, "Perro");
Console.WriteLine(m1);
