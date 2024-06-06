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
<p><em>Apartado A</em></p>
<p align="middle">
  <img align="middle" src="https://lh7-us.googleusercontent.com/docsz/AD_4nXcDUDIYr2HuBuEAv2AvgNqfjcAMoQ7NX3_eT550VjWRPP-dsVHOv00asHxYibSXMtCyCqhiHiwcoXTzEyxnik3vTZkYDh2kORZfRQocdlXwsjlvLq6ym2pr5yk4rffZ0sa12S9FR9NuLYSwToQbifX3PTYh?key=QZjS5k0dJUR0swluZunyVA"> 
    <img align="middle" src="https://lh7-us.googleusercontent.com/docsz/AD_4nXcWiFoiTf3vJp4RsAZ4k4jt7zeS4-1qMV_7V9Sc8TaOLeTXkXC-YMcHnyvvph4d4wuGSsvi_4r3RzOkSuaLm1QLJGpe52nHvx580YMzZDOJD8-6yoc-KBr-Hqnd7BIojNE-GGkmlBLGvB61AWZGXKfgCDBc?key=QZjS5k0dJUR0swluZunyVA"> 

</p><p><em>Apartado B</em></p>
<p align="middle">
  <img align="midlle" src="https://lh7-us.googleusercontent.com/docsz/AD_4nXfAgX9acWkzIg3DXP_jKtI5Gq1C7jnTjB4e9AC9z6GprPPaeEPBH4Xk0iNDsMWxMIeiThEDpbIr-KbKemilUlGTaHed7kpcy3wY4Rc8WlbcKSs9N75mp-zzbD0FbVzPCrFuilHeBrOBfS_tYY_nW2SYjaF9?key=QZjS5k0dJUR0swluZunyVA">
  <img src="https://lh7-us.googleusercontent.com/docsz/AD_4nXcXfZ-SUtI5Cm_-C40710Cmwfr4kJ2fLdSYewjn7oJoEKlyr4kyJI1pltuOmU3ulO8WKpzplsa4N9IkKGjp6yJpxU6LTXHCtOulRXd2DlKQ4VYpMvc_Gv0s4aTY5cxqTXzCJVRDBwbQEsJOfHxBQkfhTZ8?key=QZjS5k0dJUR0swluZunyVA"> 
</p>
<p><em>Apartado C</em></p>
<p align="middle">
  <img align="midlle" src="https://lh7-us.googleusercontent.com/docsz/AD_4nXeuIwpU_gBLWlXKB91qrAY_qDmqi4TZgtUGc1gOkowrtA_3MHZ3ur6Qql7zao5Q6F5PSCXJxQjSmunC4EdlTPQcMgLcfyay268zBdQ09M6kpAM8AxHVrv5ykLZvTQINvi2xN90qkzBrzlpzGs8UrZjJTzD3?key=QZjS5k0dJUR0swluZunyVA">
  <img src="https://lh7-us.googleusercontent.com/docsz/AD_4nXcsNkOQPodCEdSdMIi0dBTOmCjb24U342Em4CwnjRaETKNrNPRFm-MFlbr7aGrxyJjhHwo4ZYNbkfyvANTfMRAjqRYT044x9JJmVDiWi4TItWQfHUaXxWGDmzNRVahwtDU4L0aFwPTL8v7ntwKOIcls2VY?key=QZjS5k0dJUR0swluZunyVA"> 
</p>
<h3 id="segunda-parte-extensión-de-isa">Segunda Parte: Extensión de ISA</h3>
<p>En la parte extra del trabajo se ha de añadir una nueva instrucción a la ISA. En este caso, se implementará una extensión de la ISA y organización de la arquitectura sencilla que permita la ejecución de la instrucción SUB @F, @D. Esta instrucción leerá de memoria el operando F, leerá de memoria el operando D, y restará el operando F al operando D, guardando el resultado en la dirección del operando D.</p>
<p>Para ello se ha decidido optar por una estrategia Firmware, es decir, modificar tanto la Unidad de Proceso como la Unidad de Control. De esta manera, será más “sencillo” de implementar. La modificación en la UP consistirá en modificar la ruta para que sea capaz de soportar la extensión de codificación y añadir un restador a la ALU para que realice la operación.</p>
<p>Después, se modificará la Unidad de Control para que realice los cambios necesarios de estado y de salida.</p>
<h4 id="diseño-de-un-restador">Diseño de un Restador</h4>
<p>Se comienza diseñando un restador para añadirlo a la ALU, de manera que realice la instrucción adecuadamente. Para ello se decidió diseñar un FULL-SUBTRACTOR, que de la misma manera que un FULL-ADDER realiza la suma de 2 bits y guarda el carry, esta pieza realiza la resta de dos bits y guarda el carry para futuras iteraciones.</p>
<p>Para diseñarlo primero se desarrolló una tabla de verdad:</p>
<p align="middle">
  <img align="middle" src="https://lh7-us.googleusercontent.com/docsz/AD_4nXdWT98lQjtMNVYbCDBkyHWsIhM22o9cwUmGWNJprqqjVxOnFM0q46rxLVHa4aIoAH3zDtjsYaQrcwLVwP3JPKa9IOKCkgw6WTS4LNWjZNZsuYojDSlp23ABTOER9ePK7F44NixiRB0BLpZ7biElZ_XD4kg?key=QZjS5k0dJUR0swluZunyVA">
 </p>
<p>Y averiguamos las expresiones lógicas a través de mapas de Karnaugh:</p>
<p align="middle">
  <img align="middle" src="https://lh7-us.googleusercontent.com/docsz/AD_4nXd2OnIbwU5eTY3FsQUXrnNhBhCqibZ_OcgRO0yuF7fZpQh-emWOURApWOCiVA3q4-xm7hQ4z3KDs6ovNp82G-BTTtPf4XiWvmb-yJrKJJ1Ck2ctOXTrAnWZSdaJ4UQZTQFiF3lXP18zRoMccEoxRGu2Ygur?key=QZjS5k0dJUR0swluZunyVA">
  <img align="middle" src="https://lh7-us.googleusercontent.com/docsz/AD_4nXeqQzF5TypF2xpbnW9_gvP9ik9liQf45yjoezogUVNqo7gxxoH146FOokIX4JW4JuahHpK11p6AHsZ1Z73aMnuO4mXuMbpAtZ7LxsVJCu673xOuRyL1bfEbG9GE3-az1CIR6XicpBlA9JJZBR0yTioNCeos?key=QZjS5k0dJUR0swluZunyVA"> 
</p>
<p>Se simplificaron las expresiones:</p>
<p align="middle">
  <b>S = (a XOR b) XOR Cin 
</b></p>
<p align="middle">
  <b>Cout = Cin * (a XOR b)’ + (a’ * b)
</b></p>
<p>Finalmente, se implementó de el circuito combinacional desarrollado en Logisim:</p>
<p align="middle">
  <img align="middle" src="https://lh7-us.googleusercontent.com/docsz/AD_4nXdCfzE13BWoywW3AM5HxFfwaTMfwd9uyZDv6mUMIYrtPE4tPvP2zMXQ9NoMio7g-tMM5eI4RIEcbt2BvfawXC0TfYXhhQhcgSFlc5o91t4tYVWQe-2cgT5wdYusutzUyDDHbnB-O71Knqo3jVfz_QIx_JEz?key=QZjS5k0dJUR0swluZunyVA">
 </p>
Sin embargo, el FULL-SUBTRACTOR que se ha diseñado solo puede hacer frente a restas de dos elementos de 1 bit. Para ello se encadenaron 17 FULL-SUBTRACTOR’s para poder realizar restas de 2 elementos de 17 bits cada uno. (17 Bits debido a Codificación Extendida):
<p align="middle">
  <img align="middle" src="https://lh7-us.googleusercontent.com/docsz/AD_4nXdffybYKgXJWY3B9chBYxDYCx7aZLgpcXqpiM7qDf1ROQKwR6yRd_DQU3i57hWymILTFJsfURUMEuaKHgr8_rWnX1BeuHf2lFtTBg35Qgs_cs4C_AbQBCusbfEsS4YcAidq12pnNkQrZZ3KVbEVybwig9Gu?key=QZjS5k0dJUR0swluZunyVA">
 </p>
<p>Una vez creado un restador de 17 bits se organizaron todos los elementos para poder unirlo a la ALU de manera sencilla:</p>
<p align="middle">
  <img align="middle" src="https://lh7-us.googleusercontent.com/docsz/AD_4nXcqEJjSAaZrgEvSG4KsMetQPillgPHFK14LeS8kXJn-TfNZKLGYkLe2IvecQj24sCmQDhN2Vj4E1Gkr9Z3m2wGE4tt9qT3TUKJZP4laW6npLsmPePIBzB2KoqV5C633NbznO0dN2CVJbgy2bSa96ofHpiw?key=vcM-upaYYMEnmznRvyhKSA">
 </p>
<h4 id="modificaciones-a-la-unidad-de-procesos">Modificaciones a la Unidad de Procesos</h4>
<p>Una vez se ha organizado el nuevo restador de 17 bits, se añade a la ALU de la siguiente manera:</p>
<p align="middle">
  <img align="middle" src="https://lh7-us.googleusercontent.com/docsz/AD_4nXccmqFHKsmJDeZLh9SacWfbKzUpEfC_YcEMbx56xP1vuZ8Gpol-VZqGsAwPofpwsodp4C2rxiSep7T2hWUc1hJiaDdPrP-0hbyuyOyUsX_sK9RCChQA3niL53ff6qo-2B4LKEIQass9ucsrr88RreG3m-I?key=QZjS5k0dJUR0swluZunyVA">
 </p>
<p>El multiplexor del ALU tiene un hueco libre, en el que se puede introducir la salida de nuestro restador de 17 bits, de tal manera que la instrucción ya estará lista para ejecutada cuando el ALU select tome los valores 11.</p>
<p>Una vez se ha añadido el nuevo restador al ALU, lo único que queda es aplicar los cambios pertinentes a tanto la Unidad de Procesos como de la Unidad de Control. Estos cambios consisten en modificar todos los componentes del procesador para que puedan manejar palabras de 17 bits en lugar de 16, pues se ha añadido un bit para poder indicar la nueva instrucción (SUB), además de cambiar el ALU antiguo por el nuevo que contiene el restador:</p>
<p align="middle">
  <img align="middle" src="https://lh7-us.googleusercontent.com/docsz/AD_4nXek4kvxmbd9fClw73PWdM0w8aJjhIg2ceWtEMaJ9YhyJktzsyZcm4mxdnx7Jk4qZen_8q-bEYba7DIpyY47IUU4z9u2b-cvi_FURaXYOnrk4UECNQiHcc624iZzml9XDu27Yt0BX7mp3xdNis5ck3eL9_AG?key=QZjS5k0dJUR0swluZunyVA">
 </p>
<h4 id="modificaciones-a-la-unidad-de-control">Modificaciones a la Unidad de Control</h4>
<p>Debido a todas las modificaciones realizadas a la Unidad de Proceso, es necesario actualizar la Unidad de Control para poder ejecutar las instrucciones. Para ello, primero se ha decidido modificar el autómata de la Máquina Sencilla para obtener una base sólida desde la que trabajar.</p>
<p>Primero hay que tener en cuenta como la codificación extendida a afectado a la codificación de las instrucciones:</p>

<table>
<thead>
<tr>
<th>COP2</th>
<th>COP1</th>
<th>COP0</th>
<th>Instruccióm</th>
</tr>
</thead>
<tbody>
<tr>
<td>0</td>
<td>0</td>
<td>0</td>
<td>ADD</td>
</tr>
<tr>
<td>0</td>
<td>0</td>
<td>1</td>
<td>ADD</td>
</tr>
<tr>
<td>0</td>
<td>1</td>
<td>0</td>
<td>CMP</td>
</tr>
<tr>
<td>0</td>
<td>1</td>
<td>1</td>
<td>CMP</td>
</tr>
<tr>
<td>1</td>
<td>0</td>
<td>0</td>
<td>MOV</td>
</tr>
<tr>
<td>1</td>
<td>0</td>
<td>1</td>
<td>MOV</td>
</tr>
<tr>
<td><strong>1</strong></td>
<td><strong>1</strong></td>
<td><strong>0</strong></td>
<td><strong>SUB</strong></td>
</tr>
<tr>
<td>1</td>
<td>1</td>
<td>1</td>
<td>BEQ</td>
</tr>
</tbody>
</table><h2 id="resultados">Resultados</h2>
<h3 id="implementación-en-la-máquina-del-código">Implementación en la máquina del código</h3>
<p>Contenido de la implementación en la máquina del código…</p>
<h3 id="extra-implementación-en-la-máquina-del-restador">Extra: Implementación en la máquina del restador</h3>
<p>Contenido de la implementación en la máquina del restador…</p>
<h2 id="conclusión">Conclusión</h2>
<p>Contenido de la conclusión…</p>
<h2 id="referencias">Referencias</h2>
<p>Listado de referencias…</p>

