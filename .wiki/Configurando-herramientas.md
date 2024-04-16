En la materia vamos a utilizar las siguientes herramientas:

* **GHC** y **Haddock**, el segundo suele estar incluído en la instalación del primero.
* **git**
* **Dafny**

Opcionalmente pueden instalar **OpenSSH**.

## GHC

GCH o _Glasgow Haskell Compiler_ es un compilador e interprete para Haskell, un lenguaje funcional. En un lenguaje funcional solo se tienen funciones y constantes, si bien existen argumentos para las funciones, éstos no pueden cambiar su valor.

Mínimamente se requiere que tengan `ghc` y `ghci` disponibles en su computadora, aunque para ciertos OS puede que instalar un paquete más completo para Haskell sea más fácil.

### Instalación en Linux/Unix

En Linux/Unix deberían poder utilizar el manejador de paquetes por defecto, `apt` o `snap` para distros basadas en _Debian_, `pacman` para distros basadas en _Arch_, entre otros. Por ejemplo, en _Ubuntu_ pueden utilizar:

```
sudo apt install ghc
```

Aunque si quieren la última versión, primero deben agregar el siguiente repositorio (esto es para Ubuntu, si está en otra distro van a tener que buscar el repositorio apropiado):

```
sudo add-apt-repository ppa:hvr/ghc
sudo apt-get update
```

_Nota: este repositorio parece funcionar para versiones de Ubuntu 16-20.xx pero para Ubuntu 21.xx no parece funcionar. De todas formas, el repositorio por defecto que viene en Ubuntu tiene una versión razonablemente actualizada._

### Instalación en Windows

En Windows pueden instalar el siguiente [binario*](https://www.haskell.org/ghc/download_ghc_9_2_2.html#windows64).

_*: deberían usar el primer link_

### Instalación en macOS

En Mac OS X pueden instalar el [binario (Mac OS X 10.7+)](https://www.haskell.org/ghc/download_ghc_9_2_2.html#macosx_x86_64), o pueden utilizar alguno de los siguientes administradores de paquetes como [MacPorts](https://www.macports.org/) o [Homebrew](https://brew.sh/) y luego ver como instalar ghc desde esos administradores. El binario anterior requiere instalar el paquete de herramientas de terminal de [Xcode 4 o Xcode 5](http://developer.apple.com/).

### GHCup (instalador multiplataforma)

[GHCup](https://www.haskell.org/ghcup/#) es un instalador disponible para todas las plataformas anteriores (Windows, Linux, MacOSX, FreeBSD, y WSL2*). Es una buena opción que incluye la posibilidad de instalar GHC, HLS (un server para poder conectar distintos editores para tener características como autocompletado y detección de errores, para esta materia no es necesario), Cabal, y Stack, dos manejadores de dependencias que para esta materia no van a ser necesarios.

## GIT

Git es una herramienta de código abierto para control de versiones, siguiendo un modelo distribuido (al contrario de herramientas como SVN que siguen un modelo cliente-servidor) y descentralizado. Al trabajar con Git se tienen repositorios locales que _pueden_ usar un repositorio remoto para sincronizar los cambios. En general, lo más común es tener algún repositorio remoto, en esta materia vamos a utilizar GitHub como repositorio remoto.

### Instalación en Linux/Unix

En Linux/Unix deberían poder utilizar el manejador de paquetes por defecto, `apt` o `snap` para distros basadas en _Debian_, `pacman` para distros basadas en _Arch_, entre otros. Por ejemplo, en _Ubuntu_ pueden utilizar:

```
sudo apt install git
```

### Instalación en Windows

En Windows pueden seguir las [instrucciones en esta página](https://git-scm.com/download/win); pueden instalar [Git para Windows](https://gitforwindows.org/); y otra opción es usar [SmartGit](https://www.syntevo.com/smartgit/) que es gratis para el ámbito educativo.

### Instalación en macOS

Para Mac OS X pueden seguir las [instrucciones en esta página](https://git-scm.com/download/mac).

## Dafny

Dafny es un lenguaje desarrollado por el grupo _RiSE_ de _Microsoft_. Dafny es un lenguaje multi-paradigma que toma inspiración y características de diversos lenguajes, ofreciendo entre las mismas a: _herencia de clases y características_; _tipos de datos inductivos_ que pueden tener métodos asociados y son apropiados para _pattern matching_; _tipos de datos inductivos con evaluación perezosa_, estos permiten definir valores potencialmente infinitos (por ejemplo: una lista de 1s que no tiene un tamaño definido); _tipos restringidos por predicados_, dado un tipo `A`, es posible definir un tipo `B` como `B = {x : A | P(x)}` que significa _B es el conjunto de todos los valores en A tal que se cumple el predicado P para cada uno de ellos_, un ejemplo son los naturales definidos como `Nat = {x : Int | x >= 0}`; _lambdas_, que representan funciones anónimas, ej.: `\x, y -> x + y` como la función anónima que suma dos valores; y finalmente _estructuras de datos mutables e inmutables_, como conjuntos, listas, etc.

A su vez, y la característica de interés para esta materia, es que Dafny soporta no solamente escribir especificaciones formales junto al código de los programas, sino que además soporta verificación automática de las mismas.

A continuación se muestra un ejemplo de una función que calcula el valor absoluto _Abs_ con su especificación asociada.

![vs-code-dafny-2 0 0-demo](https://user-images.githubusercontent.com/3601079/140799975-f3ac0925-10d9-4c14-b1a9-cd449854c6ae.gif)

_Este ejemplo fue sacado del [repositorio GitHub de Dafny](https://github.com/dafny-lang/dafny)._

### Instalación de Dafny como extensión de Visual Studio Code

La instalación de Dafny como extensión de Visual Studio Code es simple, requiere ir a la sección de extensiones, buscar dafny, e instalar la extensión.

![dafnyvsc_install](https://user-images.githubusercontent.com/4238210/170554583-89a86aa7-3bc1-4fb4-a393-229898e5a65f.gif)

### Instalación en Linux/Unix

Las instrucciones se dan en el contexto de Ubuntu, para otras distros se pueden utilizar las páginas provistas para buscar los cambios necesarios.

#### Instalar .NET (necesario para tanto la versión de VSC como la versión por terminal)

En esta página web están las [instrucciones para instalar .NET](https://docs.microsoft.com/en-us/dotnet/core/install/linux-ubuntu) en distintas versiones de Ubuntu. Solo es necesaria la instalación de las librerías de Runtime.

#### Visual Studio Code

1. Descargar el instalador `.deb` desde la [página web de Visual Studio Code](https://code.visualstudio.com/).
2. Instalar Visual Studio Code utilizando el archivo `.deb` descargado:
   1. Mediante el instalador de software: click derecho en el archivo `.deb` -> "abrir con" -> Software Install.
   2. Por terminal:
```
        sudo dpkg -i /absolute/path/to/deb/file
        sudo apt-get install -f
```

#### Versión por terminal

Siga las [instrucciones para la instalación de Dafny para su uso por terminal](https://github.com/dafny-lang/dafny/wiki/INSTALL#linux-binary).

### Instalación en Windows

#### Instalar .NET (necesario para tanto la versión de VSC como la versión por terminal)

En esta página web están las [instrucciones para instalar .NET](https://docs.microsoft.com/en-us/dotnet/core/install/windows?tabs=net60) en distintas versiones de Windows. Solo es necesaria la instalación de las librerías de Runtime.

#### Visual Studio Code con extensión

En Windows pueden descargar el [instalador para Visual Studio Code](https://code.visualstudio.com/).

#### Versión por terminal

Siga las [instrucciones para la instalación de Dafny para su uso por terminal](https://github.com/dafny-lang/dafny/wiki/INSTALL#windows-binary).

### Instalación en macOS

#### Instalar .NET (necesario para tanto la versión de VSC como la versión por terminal)

En esta página web están las [instrucciones para instalar .NET](https://docs.microsoft.com/en-us/dotnet/core/install/macos) en distintas versiones de macOS (10.15+). Solo es necesaria la instalación de las librerías de Runtime.

#### Visual Studio Code con extensión

En macOS pueden descargar el [instalador para Visual Studio Code](https://code.visualstudio.com/).

#### Versión por terminal

Siga las [instrucciones para la instalación de Dafny para su uso por terminal](https://github.com/dafny-lang/dafny/wiki/INSTALL#mac-binary).

## OpenSSH

Una de las dos opciones para trabajar con repositorios remotos en GitHub usando git, es mediante el protocolo ssh junto al uso de claves públicas y privadas. Esto es completamente opcional y se puede usar otra alternativa que, quizás no tan conveniente, funciona bien y no requiere instalar herramientas extra.

En Linux/Unix o al menos las distros más conocidas, el cliente ssh viene ya instalado. Para Mac OS X, el cliente ssh también viene instalado por defecto.

### Instalación en Windows

En Windows 10 y 11, OpenSSH (tanto el cliente como el server) es una característica opcional que puede ser instalada fácilmente sin tener que descargar una aplicación aparte. Las instrucciones para hacerlo está en [este link](https://docs.microsoft.com/en-us/windows-server/administration/openssh/openssh_install_firstuse), se sugiere seguir las instrucciones relacionadas a la instalación mediante _Windows Settings_.