# Práctica 2
## 1. ¿Cómo hacer un respaldo de una máquina virtual? y ¿cómo levantar ese respaldo?

La forma más fácil para hacer un respaldo de una máquina virtual si estamos usando el software VirtualBox es hacerlo desde la aplicación, para esto damos clic derecho sobre la máquina virtual que deseamos respaldar y seleccionamos la opción clonar.

![alt text](https://github.com/CarlosGA20/Practica3_SO2/blob/main/Capturas/1.png)

En la ventana que se abre le asignaremos un nombre al respaldo de la máquina virtual y seleccionaremos el directorio donde deseamos guardarla, después seleccionamos Next.

![alt text](https://github.com/CarlosGA20/Practica3_SO2/blob/main/Capturas/2.png)

En la siguiente ventana seleccionamos la opción Clonación completa y presionamos el botón Clonar.

![alt text](https://github.com/CarlosGA20/Practica3_SO2/blob/main/Capturas/3.png)

Una vez finalizado el proceso podremos iniciar el respaldo de la máquina virtual como solemos hacerlo, seleccionando el botón Iniciar.

![alt text](https://github.com/CarlosGA20/Practica3_SO2/blob/main/Capturas/4.png)

## 2. Explicar la nomenclatura del kernel.

## 3. Enlistar paquetes requeridos para la compilación y ¿cómo instalarlos desde terminal?.

Los paquetes que necesitaremos para compilar el kernel son:

 - libncurses-dev 
 - flex 
 - bison 
 - openssl 
 - libssl-dev 
 - dkms
 - libelf-dev 
 - libudev-dev
 - libpci-dev 
 - libiberty-dev 
 - autoconf
 
Para instalar estos paquetes utilizaremos el comando `sudo apt-get install`.
```
sudo apt-get install libncurses-dev flex bison openssl libssl-dev dkms libelf-dev libudev-dev libpci-dev libiberty-dev autoconf
``` 

![alt text](https://github.com/CarlosGA20/Practica3_SO2/blob/main/Capturas/5.PNG)

##  4. ¿Cómo descargar una versión de kernel desde terminal?.

Tenemos que visitar la página [kernel.org](kernel.org) para revisar qué versión deseamos descargar.  En nuestro caso descargaremos la versión 5.11.15 como ejemplo en el directorio Documents. Copiamos el link e introducimos el comando:
```
wget https://cdn.kernel.org/pub/linux/kernel/v5.x/linux-5.11.15.tar.xz
```

![alt text](https://github.com/CarlosGA20/Practica3_SO2/blob/main/Capturas/6.PNG)

## 5. ¿Cómo extraer el código comprimido del kernel desde terminal?

Primero nos movemos al directorio donde se encuentra el archivo comprimido. Para extraer el kernel utilizamos el comando `tar xf`.

	tar xf linux-5.11.15.tar 

![alt text](https://github.com/CarlosGA20/Practica3_SO2/blob/main/Capturas/7.PNG)

Y nos movemos a la carpeta recién descomprimida

	cd linux-5.11.15 

![alt text](https://github.com/CarlosGA20/Practica3_SO2/blob/main/Capturas/8.PNG)

## 6. ¿Cómo configurar el kernel? 

Para configurar el kernel recién descargado podemos utilizar el archivo de configuración actual. Copiaremos el archivo de configuración que está en uso a la carpeta recién descomprimida.

	cp /boot/config-$(uname -r) .config   

![alt text](https://github.com/CarlosGA20/Practica3_SO2/blob/main/Capturas/9.PNG)

Después ejecutamos el comando:

	make menuconfig

![alt text](https://github.com/CarlosGA20/Practica3_SO2/blob/main/Capturas/10.PNG)

Con las flechas del teclado seleccionamos Save.

![alt text](https://github.com/CarlosGA20/Practica3_SO2/blob/main/Capturas/11.PNG)

Presionamos Ok.

![alt text](https://github.com/CarlosGA20/Practica3_SO2/blob/main/Capturas/12.PNG)

Para terminar seleccionamos Exit.

![alt text](https://github.com/CarlosGA20/Practica3_SO2/blob/main/Capturas/13.PNG)

Y volvemos a seleccionar Exit.

![alt text](https://github.com/CarlosGA20/Practica3_SO2/blob/main/Capturas/14.PNG)

Para finalizar este paso ingresamos el comando:

	scripts/config --set-str SYSTEM_TRUSTED_KEYS ""

![alt text](https://github.com/CarlosGA20/Practica3_SO2/blob/main/Capturas/15.PNG)

## 7. ¿Cómo compilar el código del kernel?

Para compilar el código insertamos el comando 
	
	sudo make -j 3
	
El -j 3 indica el número de núcleos que deseamos utilizar para la compilación, esto se especifica debido a que la compilación del kernel es un proceso muy tardado y utilizaremos todos los núcleos de procesador que tenga nuestra máquina virtual para acelerar el proceso, este campo varía con cada usuario.

Si no deseamos indicar cuántos núcleos usará la compilación únicamente ejecutamos:

	sudo make

Este paso tardará algunas horas y es normal, sólo debemos dejar que la compilación termine sin cerrar la terminal.

![alt text](https://github.com/CarlosGA20/Practica3_SO2/blob/main/Capturas/16.PNG)

## 8. ¿Cómo instalar módulos?

Después de que termine la compilación del kernel procederemos a instalar los módulos del mismo utilizando el comando:

	sudo make modules-install -j 3

De igual forma que el paso anterior, indicaremos el número de núcleos que deseamos usar para acelerar el proceso.

![alt text](https://github.com/CarlosGA20/Practica3_SO2/blob/main/Capturas/17.PNG)

## 9. ¿Cómo instalar el kernel?

Con el siguiente comando instalaremos el kernel y el archivo .config  en el directorio /boot
	
	sudo make install -j 3

![alt text](https://github.com/CarlosGA20/Practica3_SO2/blob/main/Capturas/18.PNG)

## 10. ¿Cómo indicarle a la computadora con cuál kernel debe iniciar?

Para indicar que versión de kernel deseamos utilizar al encender la computadora usamos el comando `sudo update-initramfs`. 
Nosotros indicaremos que use la versión 5.11.15, pero este campo depende de que versión hayamos instalado.
 
	sudo update-initramfs -c -k 5.11.15

![alt text](https://github.com/CarlosGA20/Practica3_SO2/blob/main/Capturas/19.PNG)

## 11. ¿Cómo verificar el cambio de kernel a partir de consola?

Utilizamos el comando siguiente para verificar los kernels que se encuentran instalados:
	
	sudo update-grub  

![alt text](https://github.com/CarlosGA20/Practica3_SO2/blob/main/Capturas/20.PNG)
