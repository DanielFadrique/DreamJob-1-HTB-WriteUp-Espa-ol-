# DreamJob-1-HTB-WriteUp-Español
Writeup del sherlock retirado DreamJob-1 de la plataforma HackThebox, el cual se centra en el dominio del Threat Intelligence sobre grupos grandes de ciber criminales. 

*Este writeup es una traducción directa en español del material oficial, el cual se encuentra en inglés.*


---------------

![](img/Pasted image 20251003121152.png)

El laboratorio Sherlock **DreamJob-1** de la plataforma **HackTheBox**, es un laboratorio con una temática bastante importante sobre el *Threat Intelligence* dentro del campo de la ciberseguridad, este laboratorio nos hace utilizar herramientas como la pagina de [MITRE ATT&CK](https://attack.mitre.org/) Y [Virustotal](https://www.virustotal.com/gui/home/upload), para así ver como estas herramientas son fundamentales en la investigación de posibles ataques, ya sean del mismo grupo investigado o de otros algo mas pequeños, pero que utilizan la misma metodología de ataque(también conocido como Cyber Kill Chain). Con estas investigaciones realizadas, nos dará una imagen de lo que puede ocurrir y como podemos prevenir esos ataques usando métodos de defensa en nuestros sistemas para que no caiga en manos equivocadas.

Pero sin mas dilación, empecemos con la resolución del Sherlock.

-------------------

# Descripción y primer análisis

## Descripción

Si nos ponemos a leer lo que nos cuenta el laboratorio veremos los siguiente escrito:

```
It is Friday afternoon and the SOC at Edny Consulting Ltd has received alerts from the workstation of Jason Longfield, a software engineer on the development team, regarding the
execution of some discovery commands. Jason has just gone on holiday and is not available by phone. The workstation appears to have been switched off, so the only evidence we have 
at the moment is an export of his mailbox containing today's messages. As the company was recently the victim of a supply chain attack, this case is being taken seriously and the 
Cyber Threat Intelligence team is being called in to determine the severity of the threat.
```

Si lo traducimos al español nos podemos quedar con esto:

```
Es Viernes por la tarde y el equipo SOC de Edny Consulting Ltd ha recibido alertas desde el ordenador de Jason Longfield, un ingeniero de softwware del equipo de desarrollo, 
mencinonando unas alertas que observan una ejecucion de comandos de descubrimiento. Jason justo esta de vacaciones y no contesta al telefono. El ordenador parace que esta apagado, asi que la unica 
evidencia que tenemos de momento es una exportacion de la bandeja de entrada del mail que contiene los mensajes de hoy. Como la compañia ha sido victima de un supply chain attack, 
este caso se esta tomando de forma seria y el equipo de Threat Intelligence ha sido asignado con la tarea de determinar la seriedad del problema.
```

## Primer análisis

Nos dan un archivo *.zip* para poder comenzar la investigacion asi que vamos a darle un repaso rapido.

Primero nos descargaremos el *.zip* y listaremos los contenidos:

![[img/Foto_1.jpg]]

Como podemos ver, dentro de este archivo nos podemos encontrar un *.txt*, por lo que vamos a descomprimirlo y ver que contiene.

![[img/Pasted image 20251003132858.png]]

Completada esa acción, podemos ver que este archivo en concreto tiene solo, lo que parece tres hashes **sha256**, esto parece que nos será útil para preguntas posteriores de la investigaciones, así que, veamos dichas preguntas.

---------------------

# Preguntas y resolución del laboratorio

Las preguntas que nos hacen para completar en este laboratorio son las siguientes:

## 1. Quien llevo a cabo la Operation Dream Job

Empezaremos nuestra investigacion entrando en [MITRE ATT&CK](https://attack.mitre.org/), Desde ahi, nos dirigiremos a la esquina superior derecha en *CTI* -> *Campaigns*
![[img/Pasted image 20251003135158.png]]

Una vez dentro tendremos que buscar por la *Operation Dream Job* y ver su ficha
![[img/Pasted image 20251003135336.png]]
![[img/Foto_3.jpg]]
Y ahi podemos encontrar nuestra primera respuesta:

#### **Respuesta:** *Lazarus Group*

## 2. Cuando se observo por primera vez dicha operacion

Sin irnos de la misma ficha de la operacion tambien la podemos contemplar la respuesta a esta pregunta
![[img/Foto_4.jpg]]

#### **Respuesta:** *September 2019*

## 3. Hay otras dos campañas asociadas con la Operation Dream Job. Una de ellas *Operation North Star*, ¿Cual es la otra?

Bajando un poco mas abajo de la información general nos encontramos con todas las campañas asociadas con esta operación

![[img/Foto_5.jpg]]
#### **Respuesta:** *Operation Interception*

## 4. Durante la Operation Dream Job, hubo dos binarios del sistema utilizados para la ejecucion de proxies. Uno fue *Regsvr32*, ¿Cual Fue el otro?

Bajando mas la pagina nos encontramos con el apartado de *Techniques used*, en las cuales se detallan todos los ataques recogidos por el **MITRE** que utilizaron los ciber criminales, explorando esta parte aparece una serie de subtecnicas que nos da la información que buscamos:

![[img/Pasted image 20251003181115.png]]

Seguido del ya mencionado *Regsvr32* tenemos abajo en la Technique ID: **T1218.011** la respuesta que necesitabamos:

#### **Respuesta:** *Rundll32*

## 5. ¿Que tecnica de movimientos laterales han usado los adversarios?

Desde la tabla que hemos visto anteriormente sera muy dificil poder encontrar el tipo de ataques que andamos buscando, por lo que, encima de la tabla a la derecha se encuentra una opcion de poder desplegar en un mapa mental todas las tecnicas usadas y sus tacticas:
![[img/Pasted image 20251003181735.png]]

Una vez dentro vamos a buscar la táctica de *Lateral Movement* y ver las tecnicas con azul en ellas, en la cual encontraremos la respuesta:

![[img/Pasted image 20251003182104.png]]

#### **Respuesta:** *Internal Spearphishing*

## 6. ¿Que Identificador de tecnica(ID Technique) es la utilizada en la pregunta anterior?

Esta pregunta tambien la tenemos resuelta si vemos la imagen de la pregunta anterior, aun asi la vuelvo a poner por aqui para que quede constatado:

![[img/Foto_6.jpg]]

#### **Respuesta:** *T1534*

## 7. ¿Que Trojano de acceso remoto uso el *Lazarus Group* en la *Operation Dream Job*?

Los software usados en campañas estan al final de las fichas de dichas campañas, en este caso vemos que hay 3 software distintos:

![[img/Pasted image 20251003182731.png]]

Si entramos en la ficha del primer software (*DRATzarus*) podremos ver esto:

![[img/Pasted image 20251003182830.png]]

Parece ser la respuesta que buscamos, incluso si vemos las fichas de los otros software no nos pone que sean realmente un **RAT** 

#### **Respuesta:** *DRATzarus*

## 8. ¿Que tecnica usa el malware para las tecnicas de ejecucion?

Asi como hicimos en la pregunta 5 donde nos metiamos a la *navigation layer* de **MITRE**, vamos a hacer lo mismo para esta pregunta, lo que nos da el siguiente resultado:

![[img/Pasted image 20251003183750.png]]

#### **Respuesta:** *Native API*

## 9. ¿Que tecnica uso el malware para evitar la deteccion de un sandbox?

Para esta pregunta haremos lo mismo que en la anterior, buscaremos ahora por la tactica en *Defense Evasion*

![[img/Pasted image 20251003183916.png]]

#### **Respuesta:** *Time Based Evasion*

## 10. Para responder a las siguientes preguntas, usa el archivo IOC.txt en Virustotal, ¿Que nombre viene asociado conel primer hash del archivo?

Vamos a coger el hash que nos comentan y lo vamos a poner en el buscador dandonos el siguiente resultado:

![[img/Pasted image 20251003184517.png]]

#### **Respuesta:** *IEXPLORE.EXE*

## 11. ¿Cuando fue creado el archivo relacionado con el segundo hash del IOC?

Introduciendo el segundo hash a Virustotal, y entrando en la pestaña de *Details* nos encontramos con esto:

![[img/Foto_7.jpg]]

#### **Respuesta:** *2020-05-12 19:26:17*

## 12. ¿Cuál es el nombre de la ejecucion padre del archivo asociado con el segundo hash proporcionado?

Vamos a seguir en esa pagina de la pregunta anterior, pero, ahora nos moveremos a la pestaña de *Relations* buscando el apartado *Execution parents*:

![[img/Foto_8.jpg]]

#### **Respuesta:** *BAE_HPC_SE.iso*

## 13. Examina el tercer hash proporcionado. ¿Cual es el nombre del archivo que pueda haber sido usado en la campaña que concuerda que las tacticas recopiladas del adversario?

En la pagina de los resultados que nos da Virustotal del tercer hash, en la pestaña de *Details* podemos ver, dentro de varios nombres uno que parece estar relacionado:

![[img/Foto_9.jpg]]

Al hablar bastante sobre el tema de trabajos, ver un nombre sobre *Job opportunities* levanta bastantes sospechas en concreto, mas sobre una empresa tan grande como lo es **Lockheed Martin** 

#### **Respuesta:** *Salary_Lockheed_Martin_job_opportunities_confidential.doc*

## 14. ¿Que URL fue contactada en 2022-08-03 por el archivo asociado al tercer hash del archivo de los IOCs?

Ahora nos vamos a mover a la peestaña de *Relations*, ahi, encontramos el apartado de *Names* catalogadas por dias, perfecto para lo que intentamos buscar:

![[img/Foto_10.jpg]]

#### **Respuesta:** *https.//markettrendingcenter.com/lk_job_oppor.docx*

-------------------

