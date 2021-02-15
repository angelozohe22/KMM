# KOTLIN MULTIPLATFORM MOBILE

# ¿Qué es?

Es una SDK para el desarrollo móvil multiplataforma suministrado por JetBrains que permite utilizar una única base de código para la lógica de las aplicaciones de iOS y Android. Solo necesitas escribir código específico de la plataforma cuando sea necesario.

![Notion_F7yQNQEUCU](https://user-images.githubusercontent.com/46237010/107978591-0fbb4300-6f8b-11eb-8707-dd7feb43a10a.png)

## Setup

- Android Studio: version 4.1 en adelante
    - Si necesitamos escribir codigo especifico y correr la aplicacion de iOS necesitamos instalar Xcode vesion 11.3 en adelante
- Kotlin: version 1.4.20 en adelante
- Instalar el plugin KMM en la IDE de Android

## Estructura

- *Multiplatform Library: U*n módulo Kotlin que contiene lógica común para aplicaciones Android e iOS. Se crea en una biblioteca de Android y un marco de iOS. Utiliza Gradle como sistema de compilación.
- *Aplicación Android: U*n módulo Kotlin que se integra en la aplicación Android. Utiliza Gradle como sistema de compilación.
- *Aplicación iOS: U*n proyecto Xcode que se integra en la aplicación iOS.

![Notion_zPQxMX08Vc](https://user-images.githubusercontent.com/46237010/107978667-311c2f00-6f8b-11eb-9f67-234a7d59ca14.png)

![Notion_ei58sAH2lO](https://user-images.githubusercontent.com/46237010/107978694-3bd6c400-6f8b-11eb-9500-53bf868da708.png)

La aplicación iOS se produce a partir de un proyecto Xcode. Se almacena en un directorio independiente dentro del proyecto raíz. Xcode utiliza su propio sistema de compilación; por lo tanto, el proyecto de aplicación iOS no está conectado con otras partes del proyecto KMM a través de Gradle. En su lugar, usa el módulo compartido como un artefacto externo

### Source sets

El módulo compartido contiene el código que es común para las aplicaciones Android e iOS. Sin embargo, para implementar la misma lógica en Android e iOS, a veces es necesario escribir dos versiones específicas de la plataforma de la misma. Para manejar estos casos, Kotlin ofrece la‎ mecanica **expect/actual**

- commonMain: ‎Almacena el código que funciona en ambas plataformas, incluidas las declaraciones‎ **expect**
- androidMain: ‎Almacena partes específicas de Android, incluyendo‎ implementaciones **actual**
- iosMain: Almacena partes específicas de iOS, incluyendo‎ implementaciones **actual**

‎KMM presume que la mejor experiencia de usuario es cuando es nativa de la propia plataforma, y por lo tanto no recomendamos compartirla en KMM. Sin embargo, puede compartir el código para el comportamiento de la interfaz de usuario que define lo que sucede con cualquier interacción del usuario y cómo el front-end se comunica con el back-end con la ayuda de patrones arquitectónicos compatibles.‎

## How does it work?

Primero, escribimos el código compartido en Kotlin. Para Android, se convierte al código de bytes Java que se puede ejecutar en la JVM. Por lo tanto, Android que ya tiene la máquina virtual (VM) es fácilmente capaz de ejecutarlo.‎

Para iOS, el código Kotlin debe convertirse en el código fuente (ejecutable de código nativo en iOS) que va dentro del marco de trabajo. Ese código fuente funciona en iOS.‎

Gracias a la infraestructura Kotlin/Native construida por los JetBrains. Básicamente, para Android, convierte el IR a Java Bytecode y para iOS, convierte el IR al código ejecutable de máquina nativo de iOS.‎

![Notion_apfldD1oVi](https://user-images.githubusercontent.com/46237010/107978755-5446de80-6f8b-11eb-92bb-a25c4226152d.png)

## Kotlin/Native

Kotlin-Native es la herramienta que se utiliza para compilar código Kotlin destinado a ejecutarse en plataformas que no sean JVM o plataformas basadas en Web como IOS, macOS, etc.‎ Se trata de una tecnología que permite compilar el código escrito en Kotlin directamente a binarios nativos. Procesa el código fuente en 2 pasos para llegar a los bits ejecutables.

Hay una parte de front-end y una parte de back-end para todo el flujo. La representación intermedia es un formato que el compilador LLVM entiende y esta representación podría convertirse en código nativo para todas las plataformas admitidas por LLVM.‎

![Notion_HNwmxYxRiN](https://user-images.githubusercontent.com/46237010/107978863-822c2300-6f8b-11eb-9178-64c6736cfd88.png)

interoperatibilidad → que podamos intercambiar informacion y podamos usar esa informacion intercambiada

Kotlin/Native ofrece interoperabilidad bidireccional con Objective-C/Swift. Los módulos de Kotlin pueden utilizarse por completo en Swift/Objective-C. Puede utilizar marcos de trabajo Objective-C y bibliotecas en código Kotlin, así como bibliotecas Swift si su API se ha exportado a Objective-C con @objc.

### ¿Qué es LLVM?

Es una biblioteca que se utiliza para construir, optimizar y producir código de máquina intermedio y/o binario.

LLVM es una biblioteca para crear mediante programación el código nativo de la máquina. Un desarrollador utiliza la API para generar instrucciones en un formato llamado representación intermedia o IR. LLVM puede luego compilar el IR en un binario autónomo, o realizar una compilación JIT (justo a tiempo) en el código para ejecutarse en el contexto de otro programa, como un intérprete para el idioma.

[https://www.infoworld.com/article/3187370/kotlin-compiles-directly-to-native-code-via-llvm.html](https://www.infoworld.com/article/3187370/kotlin-compiles-directly-to-native-code-via-llvm.html)

[https://kotlindays.com/2019/12/05/kotlin-going-native/index.html](https://kotlindays.com/2019/12/05/kotlin-going-native/index.html)

![Notion_o9f3U1oBNT](https://user-images.githubusercontent.com/46237010/107978905-9839e380-6f8b-11eb-9e6a-6301968ed6c3.png)

# expect / actual

expect

es una función, pero también podría ser una clase. Al igual que con las interfaces no se define el cuerpo de la función ya que este existirá en la implementación concreta para cada plataforma. Se utiliza dentro de commonMain para notificar al compilador que parte compartida del código requiere alguna implementación específica de la plataforma

![Notion_1awy3a7fw6](https://user-images.githubusercontent.com/46237010/107978937-a687ff80-6f8b-11eb-856c-9fddef61f2cb.png)

actual

Posteriormente en el sourceset de cada plataforma tendremos la implementación **actual** de la función, debe ir con la palabra clave actual. Es la forma de indicar al compilador que ha proporcionado una implementación específica de la plataforma y que el módulo compartido está listo para continuar.‎

![Notion_sWXHofVzCQ](https://user-images.githubusercontent.com/46237010/107978969-b273c180-6f8b-11eb-8853-e48ca0dcdbe0.png)

### Referencias
* https://medium.com/@otroprogramador/kotlin-multiplatform-mobile-el-futuro-del-desarrollo-movil-4f4d20be5fbc
* https://blog.jetbrains.com/es/kotlin/2020/09/kotlin-multiplatform-mobile-ahora-es-alpha/
* https://medium.com/@juegeorge/kotlin-multiplatform-hello-world-adfcfb09d685

### How does KMM work?
* http://xurxodev.com/kotlin-multiplatform-y-el-principio-de-inversion-de-dependencias/#:~:text=En%20kotlin%20multiplataforma%20existe%20un,plataforma%20escrito%20tambi%C3%A9n%20en%20Kotlin
* https://blog.mindorks.com/how-does-the-kotlin-multiplatform-work
