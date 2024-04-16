# GitHub

## Crear un usuario

Si no tienen una cuenta de usuario en GitHub, van a necesitar [crear una](https://github.com/signup). De ser posible dediquen un poco de tiempo a configurar su perfil, de esa forma es más fácil para otras personas reconocer su cuenta.

## Aplicar al paquete educativo de GitHub (opcional)

GitHub es un servicio _Freemium_, eso quiere decir que es posible usarlo de manera gratuita, pero hay varias características que solo pueden ser utilizadas mediante una suscripción paga. Hace tiempo que GitHub ofrece un programa tanto para estudiantes como para docentes. Estos programas ofrecen, entre otras cosas, acceso a GitHub Pro. Si quieren [pedir un paquete de estudiantes](https://education.github.com/pack), pueden hacerlo en cualquier momento.

## Acceso a un repositorio GitHub mediante git

Una vez que hayan creado un repositorio, o haya un repositorio con el que quieran trabajar, van a tener dos formas posibles de usarlo mediante git, https o ssh. Dentro de un repositorio GitHub, van a ver un botón que dice "Code" que al pulsarlo les va a ofrecer distintas opciones para clonar el repositorio y poder trabajar con el mismo, las opciones principales y más utilizadas son _HTTPS_ y _SSH_, al seleccionar una o la otra van a obtener un URL.

![Screenshot from 2022-03-14 18-50-25](https://user-images.githubusercontent.com/4238210/158267247-6ce8a51c-6f43-40ee-b26f-b19fa6c0bc63.png)

### Acceso mediante https

Si utilizan el URL marcado como _HTTPS_, el comando para clonar va a ser:

```
git clone https://github.com/ProgAv-UNRC/Playground.git
```

En este caso, git va a pedir un usuario y una contraseña, y van a tener que usar los de su cuenta de GitHub. Sin embargo, hace Octubre del 2021, GitHub ya no acepta el uso de la contraseña para su uso fuera de la web, en cambio es necesario usar un _Personal Access Token_.

Para crearlo, necesitan ir a la página de GitHub, loguearse en su cuenta, y dirigirse a _Settings -> Developer settings -> Personal access tokens_. Ahí van a tener que generar un nuevo token, darle un nombre apropiado junto a los permisos asociados a ese token, al contrario de la contraseña que da acceso completo a su cuenta de GitHub, los tokens pueden tener permisos más específicos, para trabajar con un repositorio como vamos a hacer en la materia, recomendamos seleccionar solamente los permisos bajo _repo_ (el primer recuadro de permisos).

***
![Screenshot from 2022-03-14 18-40-34](https://user-images.githubusercontent.com/4238210/158266016-6a1d0ffa-9db4-4b7b-b42d-d9bf54673072.png)
***

Es importante que el token que es generado lo copien en algún lugar, una vez que completen el proceso no van a poder volver a ver cual era.

Una opción puede ser usar alguna herramienta de manejo de contraseñas como [KeePassXC](https://keepassxc.org/) (la cual es gratis, de código abierto, y disponible para múltiples plataformas).

Finalmente, el token generado lo van a usar en el campo de contraseña cuando git pida usuario y contraseña.

### Acceso mediante ssh

Si utilizan el URL marcado como _SSH_, el comando para clonar va a ser:

```
git clone git@github.com:ProgAv-UNRC/Playground.git
```

En este caso, git no va a pedir usuario y contraseña, sino que va a buscar una clave ssh privada que se corresponda con la clave ssh pública que tienen en su cuenta de GitHub. Como aún no han configurado ninguna, [el siguiente paso es hacerlo](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent).

Las instrucciones, si bien descritas en el link anterior, son:

En la terminal, ejecutar `ssh-keygen -t ed25519 -C "your_email@example.com"`, alternativamente, pueden usar `ssh-keygen -t rsa -b 4096 -C "your_email@example.com"` si tienen un problema con el algoritmo usado por el primer comando.

Cuando sea necesario definir el archivo donde van a guardar la clave pueden no escribir nada y el archivo se va a guardar en la ruta por defecto (revisar link), o pueden dar un nombre y una ruta particular (se recomienda solo cambiar el nombre, manteniendo el resto de la ruta usada por defecto). Finalmente van a tener con dos archivos para la ruta elegida: _ruta_, la clave privada; y _ruta.pub_, la clave pública.

Ahora necesitan iniciar el agente ssh mediante el comando `eval "$(ssh-agent -s)"`. Este paso puede variar según el OS, revisen el link anterior que ofrece más información.

Luego de generar las claves, por ejemplo `~/.ssh/id_ed25519` y `~/.ssh/id_ed25519.pub`, van a crear o modificar el archivo `~/.ssh/config` Y agregar lo siguiente:

```
Host github.com
  HostName github.com
  IdentityFile ~/.ssh/id_ed25519
```

Para terminar van a tener que copiar la clave pública en su cuenta de GitHub, primer van a tener que abrir la clave pública `~/.ssh/id_ed25519.pub` (siguiendo el ejemplo anterior). Pueden usar _cat_ en la terminal si están en Linux/Unix, _nano_ en la terminal de Mac OS X, o algún editor de texto, se recomienda uno que muestre el texto verbatim, [Notepad++](https://notepad-plus-plus.org/) es una muy buena opción para Windows, gedit es una buena opción particularmente en _Ubuntu_, y [Atom](https://atom.io/) es una buena opción multiplataforma. Con la clave pública copiada van a ir a su cuenta de GitHub _Settings -> SSH and GPG keys_ y van a crear una nueva clave ssh, dándole un nombre significativo como _ubuntu@notebook_ y copiando la clave pública en el campo correspondiente.

***
![Screenshot from 2022-03-14 19-23-40](https://user-images.githubusercontent.com/4238210/158270980-7aed5fc0-6c3a-4661-a28e-1a8f56ea7328.png)
***

# Git

A continuación se presenta una breve y muy simple guía de comandos básicos de git suponiendo un repositorio existente, esto es, o bien se creo un repositorio mediante `git init` o se clonó un repositorio mediante el comando `git clone`.

## Estructura de un repositorio (local)

Un repositorio local tiene 3 partes:

 * El área de trabajo, donde tienen los archivos y realizan modificaciones.
 * El área de cambios o "staging area", donde están los cambios que se van a "commitear".
 * El repositorio local, este repositorio es luego sincronizado con el repositorio remoto (por ejemplo en GitHub).

Para ver el estado del repositorio local pueden ejecutar el comando `git status` que va a mostrar que archivos hay en el área de trabajo, cuantos en el "staging area", y cuantas modificaciones han hecho en el repositorio local que aún no han sido sincronizadas con el repositorio remoto.

## Pasos usuales de trabajo

Suponiendo un repositorio existente sin ningún cambio realizado, los pasos usuales de trabajo son los siguientes:

1. Realizar modificaciones (agregar nuevos archivos, borrar archivos, modificar archivos).
2. Los cambios anteriores aparecen en el área de trabajo, para pasarlos al "staging area" usan los comandos:
   1. `git add <archivo>` para agregar archivos nuevos o modificaciones al "staging area".
   2. `git rm <archivo>` para agregar/confirmar la eliminación de un archivo al "staging area".
3. Los cambios en el "staging area" ahora se introducen en el repositorio local mediante el comando `git commit -m <mensaje asociado entre comillas>` (ejemplo: `git commit -m "implementación de función buscar"`).
4. Para sincronizar nuestros cambios en el repositorio local con el remoto (subir nuestros cambios) se usa el comando `git push`.
   1. Si había cambios en el repositorio remoto que no teníamos, git va a informar que es necesario hacer un `git pull` para traer dichos cambios.
   2. Si los cambios remotos y locales se pueden sincronizar automáticamente, solo es necesario re-ejecutar `git push`, sino va a ser necesario elegir manualmente que cambios queremos dejar (eso se va a realizar en cada archivo para el cual git reporte que es necesario hacerlo) y luego realizar nuevamente los pasos *2* y *3*.
5. Para sincronizar nuestro repositorio local con cambios en el repositorio remoto (bajar cambios remotos) se usa el comando `git pull`.