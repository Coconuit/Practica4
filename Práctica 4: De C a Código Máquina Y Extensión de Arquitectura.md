---


---

<h1 id="práctica-4-de-c-a-código-máquina-y-extensión-de-arquitectura">Práctica 4: De C a Código Máquina Y Extensión de Arquitectura</h1>
<p><img src="https://lh7-us.googleusercontent.com/mefolp8oAVDaWOvo0FuAohTn4xehH5zP4M0_YLSSdU0vrzvJrETQ_Hf7UKdg-kHaaj4T6pMx21O8KKjOl4FiDhmVXWeapIYu6MNHnUZLGg0kFLMDOiHWiVNvN0JoAGvbln2bnJx6mPHXxzIE_qQixBQ" alt=""></p>
<p>02 / 05 / 2024</p>
<blockquote>
<p>Tomás Juan Usón, Juan Alfonso y Fernando Azlor<br>
Alu: 156527 - 161014 - 166054</p>
</blockquote>
<p>Sistemas Lógicos<br>
Doble Grado Ciberseguridad E Ingeniería Informática<br>
Universidad San Jorge</p>
<h2 id="índice">ÍNDICE</h2>
<ul>
<li><a href="#resumen">Resumen</a></li>
<li><a href="#introducci%C3%B3n">Introducción</a></li>
<li><a href="#decisiones-de-dise%C3%B1o">Decisiones de Diseño</a>
<ul>
<li><a href="#primera-parte-compilaci%C3%B3n-y-ensamblaje-del-c%C3%B3digo-en-c">Primera Parte: Compilación y Ensamblaje del Código en C</a>
<ul>
<li><a href="#traducci%C3%B3n-a-lenguaje-ensamblador">Traducción a lenguaje ensamblador</a></li>
<li><a href="#traducci%C3%B3n-a-lenguaje-m%C3%A1quina">Traducción a lenguaje máquina</a></li>
</ul>
</li>
<li><a href="#segunda-parte-extensi%C3%B3n-de-isa">Segunda Parte: Extensión de ISA</a>
<ul>
<li><a href="#dise%C3%B1o-de-un-restador">Diseño de un Restador</a></li>
<li><a href="#modificaciones-a-la-unidad-de-procesos">Modificaciones a la Unidad de Procesos</a></li>
<li><a href="#modificaciones-a-la-unidad-de-control">Modificaciones a la Unidad de Control</a></li>
</ul>
</li>
</ul>
</li>
<li><a href="#resultados">Resultados</a>
<ul>
<li><a href="#implementaci%C3%B3n-en-la-m%C3%A1quina-del-c%C3%B3digo">Implementación en la máquina del código</a></li>
<li><a href="#extra-implementaci%C3%B3n-en-la-m%C3%A1quina-del-restador">Extra: Implementación en la máquina del restador</a></li>
</ul>
</li>
<li><a href="#conclusi%C3%B3n">Conclusión</a></li>
<li><a href="#referencias">Referencias</a></li>
</ul>
<hr>
<h2 id="resumen">Resumen</h2>
<p>Este documento presenta los resultados de la ejecución de varios códigos en máquina sencilla, y su posterior modificación para añadir una nueva instrucción a la colección. Buscando aprender el funcionamiento de la arquitectura y organización de un procesador simple.</p>
<p>El planteamiento de las modificaciones se realizó analógicamente antes de su implementación en Logisim utilizando los pasos universales descritos. Es decir, tradujimos el código expuesto en C a Ensamblador, para posteriormente pasarlo a Hexadecimal con su previa conversión a binario. Una vez finalizado tal proceso, las instrucciones y datos fueron añadidos a la RAM del procesador de Logisim y comprobados de manera que realice la función del código inicial. Finalmente, se redactó esta memoria técnica para transcribir la metodología y resultados del ejercicio.</p>
<p>Durante el desarrollo de la práctica, hemos aprendido la importancia de una unidad de control de circuitos secuenciales, para manejar de manera ordenada y efectiva los procesos de un sistema combinacional. Además, hemos conseguido afianzar la comprensión de las máquinas sencillas, al practicar la ejecución y la introducción de nuevas instrucciones a un procesador simple.</p>
<p>Este documento fue redactado con el fin de cumplir con los requisitos de la Práctica 4 de la asignatura Sistemas Lógicos presentada por el profesor Enrique Torres Sanchez.</p>
<h2 id="introducción">Introducción</h2>
<p>Esta memoria técnica presenta el proceso completo de interpretación y modificación de un procesador (máquina sencilla), con el propósito de aprender sobre su funcionamiento y ser capaz de interpretarlo hasta el punto de poder modificar y añadir instrucciones a conveniencia. La práctica se enmarca en un contexto educativo, destinada a proporcionar a los estudiantes una comprensión práctica y profunda sobre estos conceptos fundamentales.</p>
<p>La motivación detrás de este proyecto radica en la importancia de comprender el funcionamiento interno de los microprocesadores, así como en adquirir habilidades prácticas en interpretación de procesadores y entendimiento de máquinas. A lo largo de esta memoria, se estructura el proceso de desarrollo en distintas secciones:</p>
<ol>
<li>
<p><a href="#decisiones-de-dise%C3%B1o">Decisiones de Diseño:</a> Se detalla el proceso de traducción del código, así como la implementación de la nueva instrucción en el ALU, junto con justificaciones sobre las decisiones tomadas en cada etapa.</p>
</li>
<li>
<p><a href="#resultados">Resultados:</a> Se presentan los resultados extraídos de las modificaciones de la máquina sencilla proporcionada en Logisim, acompañados de las comprobaciones realizadas para garantizar su correcto funcionamiento según las especificaciones establecidas en la sección de Decisiones de diseño.</p>
</li>
<li>
<p><a href="#conclusi%C3%B3n">Conclusión:</a> Se resumen las conclusiones desarrolladas durante la implementación de la práctica, destacando resultados notables y dificultades encontradas, así como los aprendizajes adquiridos a lo largo del proceso.</p>
</li>
<li>
<p><a href="#referencias">Referencias:</a> Material adicional usado para la realización de esta práctica.</p>
</li>
</ol>
<p>A través de esta estructura, se busca proporcionar una guía completa y detallada que sirva como recurso educativo para futuros proyectos relacionados con la implementación de nuevas instrucciones a un procesador, así como la traducción de código previa e implementada en Logisim.</p>
<p>Durante el transcurso de la práctica se ha comprendido la importancia del diseño de un procesador por medio de la máquina sencilla. Además, se ha ganado experiencia con el simulador Logisim otorgando una herramienta para el diseño sencillo de circuitos secuenciales usado en esta ocasión para entender y modificar el circuito de un procesador. Finalmente, se han asentado las lecciones de sistemas lógicos para añadir nuevas instrucciones a una máquina con todo lo que ello implica de manera eficiente y práctica.</p>
<h2 id="decisiones-de-diseño">Decisiones de Diseño</h2>
<p>En este apartado se expondrán las decisiones de diseño tomadas durante el transcurso de la práctica para garantizar que las decisiones tomadas durante el trabajo estén justificadas y fundamentadas. Esto incluye la traducción a lenguaje máquina del código para el procesador, y el rediseño del ALU para poder ejecutar una nueva instrucción (además del planteamiento previo de la misma instrucción).</p>
<p>Debido a que se han realizado todos las partes de la práctica es necesario dividir este apartado en dos secciones: <a href="#primera-parte-compilaci%C3%B3n-y-ensamblaje-del-c%C3%B3digo-en-c">Parte 1. Compilación y Ensamblaje de Código C</a>, y <a href="#segunda-parte-extensi%C3%B3n-de-isa">Parte 2. Extensión de ISA</a>.</p>
<p>En la primera parte, se realizará la “compilación” y el posterior ensamblado de tres piezas de código sencillas implementadas en el lenguaje C. Estas piezas de código deben ser convertidas al lenguaje máquina de nuestra arquitectura sencilla. Esta parte se divide en tres procesos distintos:</p>
<p><strong>1. Traducción a Lenguaje Ensamblador:</strong> Primero, se traducirá el código en C al lenguaje ensamblador propio de la arquitectura de nuestra máquina sencilla.</p>
<p><strong>2. Traducción a Lenguaje Máquina:</strong> Después, se traducirá el código en lenguaje a ensamblador a lenguaje máquina, es decir, binario y posteriormente se convertirá a hexadecimal para poder introducirlo en la RAM de Logisim, y comprobar su funcionamiento.</p>
<p><strong>3. Implementación a Logisim:</strong>  Finalmente, se introducen los códigos en la RAM y se comprueban los resultados. Este apartado formará parte de los Resultados.</p>
<p>En la segunda parte, se mostrará el proceso por el cual se ha añadido la instrucción SUB a la ISA. Para ello se han seguido los siguientes pasos:</p>
<p><strong>1. Diseño de un restador:</strong> Para añadir la nueva instrucción a la máquina, primero ha de tenerse clara su implementación, por ello se ha diseñado un restador desde cero para su posterior.</p>
<p><strong>2. Modificaciones a la Unidad de Proceso:</strong> Para que el ALU pueda procesar la nueva instrucción, se ha de modificar añadiendo el restador.</p>
<p><strong>3. Modificaciones a la Unidad de Control:</strong> Para que los cambios se vean reflejados, han de realizarse ciertos cambios en la máquina (ROM de transiciones y salidas, nuevo ALU, comprobación…).</p>
<p><strong>4. Comprobar Resultados:</strong>  Como en la anterior parte, este apartado se encontrará junto a los resultados. Se desarrollará un código para probar la nueva instrucción y se podrá ver el funcionamiento de la máquina sencilla con una nueva instrucción.</p>
<h3 id="primera-parte-compilación-y-ensamblaje-del-código-en-c">Primera Parte: Compilación y Ensamblaje del Código en C</h3>
<h4 id="traducción-a-lenguaje-ensamblador">Traducción a lenguaje ensamblador</h4>
<p>Para comenzar con el ejercicio, se debía traducir el código en C al lenguaje ensamblador de la máquina. El lenguaje ensamblador de máquina sencilla contiene las siguientes cuatro instrucciones:</p>
<p align="middle">
  <img align="middle" src="https://lh7-us.googleusercontent.com/docsz/AD_4nXeDCsIPW6rK7y9hYUgxzisHptBRVmNYHHWSP1BppVjj5sR4SF6zlWl5YtetgKnvZ7_p61perhN_1Nyho4pJJe0exeinqqC6ESfjKPL7UEPNWWMS4KHWz6NapDvTc50_gElkCZVr0zjUzbM6rja0w8kbU54?key=QZjS5k0dJUR0swluZunyVA">
 </p>
<p>Para traducir un código en C a ensamblador, primero hay que declarar/escoger en qué posición de memoria se van a guardar las variables. Después se escribe el código en el ensamblador.</p>
<p>Quizá lo más importante a destacar en este apartado son las posiciones de memoria escogidas para guardar tanto las instrucciones como las variables. En los tres casos se ha decidido comenzar a guardar los datos desde las mismas posiciones. Evidentemente, las instrucciones se guardarán en la posición @0 ya que la máquina comenzará a leer por dicha posición.</p>
<p>Sin embargo, se ha tomado la decisión de comenzar a guardar las variables desde la posición @32 de memoria ya que se encuentra a suficiente distancia de las instrucciones como para poder añadir más de ser necesario y es potencia de 2, por lo que los datos no se encontrarán entre dos posiciones de memoria.</p>
<p><em>Apartado A:</em></p>
<p align="middle">
  <img align="middle" src="https://lh7-us.googleusercontent.com/docsz/AD_4nXfvC2KOA5LKU9gLvPH9KeWR9NFjsFHgaaPn111sOIMhFMSnorkpfEdjd2cNLdm_6KwKWiBxNFzTYbUQlS9HgmGtxtDNMu4Vdad9MsEbtRUGIcV-PtXKN2NMfELZyRnD5MVXOJuBLnVfGL-kpUBuGa3G3m76?key=QZjS5k0dJUR0swluZunyVA">
  <img align="middle" src="https://lh7-us.googleusercontent.com/docsz/AD_4nXc7q7cZhpkN2mipbKMmwLPm9z-drIxil894GB6dA1KgXS2A2_NOeLfjBa8JC3pHZI0P8G75US6smyG0xxrFK6IeHlNw-o0uXj61ZVqwoDoFMlnXCBYPkwGqF6ZCHlzpra-Hletb8jtBKu1xMATEHi2ecH79?key=QZjS5k0dJUR0swluZunyVA"> 
</p>
<p>En este caso la traducción ha sido simple, únicamente se han sumado las variables y guardado el resultado en ambas variables.</p>
<p><em>Apartado B</em></p>
<p align="middle">
  <img align="middle" src="https://lh7-us.googleusercontent.com/docsz/AD_4nXc1JFPvg09ooN9mFIqUx2kqokI2xigj-EtKrwtzY3byltVrfLiE2E-l6gm0eErCwotPYF_yrkmWVX0YH6Op6mNfMOx4nEQIp4CIdEa1f8wvu3QLLSU2AFLK7MKdKHgFtrDZ1KSEnk9HZCCFclhVSYfdGgni?key=QZjS5k0dJUR0swluZunyVA"> 
  <img align="middle" src="https://lh7-us.googleusercontent.com/docsz/AD_4nXerZIRDncCo_1GTgxPHgDex8kuN1vQi9Wxd6pjv6wRxtBnIPxH6JXoVT5E_tQbhOC_BPJAx8Zf6rL0V8mE-gtlHTWbDfQ4BVO2XUwDmu1EdzsVTfZQJsoedGcYSdm_Lv3o0NKtccNEsBTyNF8-NGchkRkI?key=QZjS5k0dJUR0swluZunyVA"> 
</p>
<p>El segundo caso es un poco más complicado. Primero, se han creado dos variables auxiliares para poder guardar el resultado momentáneamente y poder hacer la comparación del condicional if.</p>
<p>Las primeras cuatro instrucciones del código en ensamblador se encargan de realizar el siguiente código en C:</p>
<blockquote>
<p>a = b + c</p>
</blockquote>
<p>Ocupa tanto pues se busca que tanto b y c tras finalizar la operación conserven sus valores iniciales, lo cual es imposible de realizar con una única instrucción en ensamblador y se necesita una variable auxiliar.</p>
<p>En la segunda parte, para poder hacer frente a la condición if se ha tenido que realizar una estructura que en un principio puede parecer extraña pues el caso else se encuentra antes que la del if. De esta manera, en caso de que la condición se cumpliera el BEQ mandará la RAM a la instrucción dentro de la condición if y de no cumplirse, la máquina continuará su curso ejecutando la del else.</p>
<p><em>Apartado C</em></p>
<p align="middle">
  <img align="middle" src="https://lh7-us.googleusercontent.com/docsz/AD_4nXez7kNrAeGgi88zz0tl-08zKnQ-5MI7_rVTauHYLRqcZEcNOGWuEBOBt0BqQjdUaHFIsYEejt29utIt1WiLc2LhiVzvweX0ks8x9I0Q8tIS8oGiN-1Hc9fwiQQuHwaL9-mKCa7uzl0tJmKY2Vfx4ijanf4?key=QZjS5k0dJUR0swluZunyVA">
  <img align="middle" src="https://lh7-us.googleusercontent.com/docsz/AD_4nXfBd52XBZp0Tk6usfYuMlU4kOwiVa82HUvu3QYANIfJmS_PGcEy_9JCl9306EDoO2fKWQjARBwIbkYmqAa8GBYMD-zBFq3r4mTBa8xSnLpBReu84FKlY-wzcoT3edV_ajACLGbOLkhzKAyIfnd-ReOsWveH?key=QZjS5k0dJUR0swluZunyVA"> 
</p>
En el tercer caso, hay que hacer frente a un bucle while y a una resta. Para ello, simplemente se hace uso de una combinación entre las instrucciones CMP y BEQ para simular el comportamiento de un bucle while. Mientras que para realizar la resta se usa la instrucción ADD entre el minuendo y el sustraendo en Complemento a 2.
<p>Para realizar el complemento a 2 en vez de transformar el dato se ha preferido guardar directamente como variable para no tener que ejecutar tantas instrucciones.</p>
<h4 id="traducción-a-lenguaje-máquina">Traducción a lenguaje máquina</h4>
<p>Una vez se ha traducido el código en C a lenguaje ensamblador se ha de volver a traducir, en este caso, a lenguaje máquina. Para ello, primero se traducirá a binario y después a hexadecimal.</p>
<p>Para comenzar la traducción se necesita conocer la estructura de las instrucciones y su equivalente en binario:</p>
<p align="middle">
  <img align="middle" src="https://lh7-us.googleusercontent.com/docsz/AD_4nXeCx-xfZFH8DJOhr8rj9C_0Gg0N6LRkcSPlFzAqin5cmtDWzWyrStHxvUB8tI62nzUo9qgg4gMrq073Pjm_gy6X6JtaBkC9HFudWgpN-nE4YXJ4acjuOQh1qTGd7HYohuF_yllqYCC7087xilvWM3BL2I-R?key=QZjS5k0dJUR0swluZunyVA">
 </p>
<p>Como se puede ver, las instrucciones están divididas en tres secciones:</p>
<p><strong>COP:</strong> Instrucciones → 2 Bits</p>
<p><strong>@F:</strong> Dirección del Primer Elemento → 7 Bits</p>
<p><strong>@D:</strong> Dirección del Segundo Elemento → 7 Bits</p>
<p><img src="https://lh7-us.googleusercontent.com/docsz/AD_4nXewB81lwbuqLXBz5joONQqIJs7neuNwB3j5Z9pIZBUBhuXsOMs9O4TA5iKdlE0tTgYJkSCg7wUItsbtTGHuNcTl1UYgm3kXoLKv6leuBYQo9in-UOaLE-3sCY7Y9FB4gNYZxqeV2HpkGPP_wyfkt28DX-4?key=QZjS5k0dJUR0swluZunyVA" alt=""></p>
<p>Para traducir de binario a hexadecimal se dividen los 16 bits en 4 sets de 4 bits y traduce siguiendo la siguiente tabla:</p>
<p align="middle">
  <img align="middle" src="https://lh7-us.googleusercontent.com/docsz/AD_4nXdFzO6lall-mfCKi1LierKHOVTlhhrVL6DLS7clrcSOm7HgGXDI6BGTjSu4jpo1Yly1i2kwDVBgcKsdgdQoc9KAmyRvq9IsGrEi49fTHrbMyJRb0m0JE4EorA5ee0mrJAJhBUPrGTW6GjwKL46_rG_Fkq3B?key=QZjS5k0dJUR0swluZunyVA">
 </p>
<h3 id="segunda-parte-extensión-de-isa">Segunda Parte: Extensión de ISA</h3>
<p>Contenido de la segunda parte…</p>
<h4 id="diseño-de-un-restador">Diseño de un Restador</h4>
<p>Contenido del diseño de un restador…</p>
<h4 id="modificaciones-a-la-unidad-de-procesos">Modificaciones a la Unidad de Procesos</h4>
<p>Contenido de las modificaciones a la unidad de procesos…</p>
<h4 id="modificaciones-a-la-unidad-de-control">Modificaciones a la Unidad de Control</h4>
<p>Contenido de las modificaciones a la unidad de control…</p>
<h2 id="resultados">Resultados</h2>
<h3 id="implementación-en-la-máquina-del-código">Implementación en la máquina del código</h3>
<p>Contenido de la implementación en la máquina del código…</p>
<h3 id="extra-implementación-en-la-máquina-del-restador">Extra: Implementación en la máquina del restador</h3>
<p>Contenido de la implementación en la máquina del restador…</p>
<h2 id="conclusión">Conclusión</h2>
<p>Contenido de la conclusión…</p>
<h2 id="referencias">Referencias</h2>
<p>Listado de referencias…</p>

