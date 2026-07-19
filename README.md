# PS1 ModChip (A)

PCB para modchips basados en el PIC12C508 (modernamente el PIC12F508), diseñado para consolas Sony PS1 Fat y Slim. Este repositorio indicará compatibilidades, esquemas de instalación (gracias a [Quade](https://quade.co/ps1-modchip-guide/)), y también proporcionará los códigos hexadecimales y el archivo Gerber de la PCB listos para fabricación.

## Prefacio

Los modchips en la PS1 se utilizan para poder leer juegos de distintas regiones (originales y alternativos), y para lanzar copias de seguridad de juegos que ya tienes (¿cierto?). Como tal, no hacen nada más.

Al ser un accesorio para juegos físicos mediante CD-ROM, **el láser de la consola debe estar completamente funcional**. De hecho, al usar un chip es cuando más se nota si el láser está en buenas condiciones o si ya está mostrando sus años.

## Parte técnica

### ¿Cómo funciona técnicamente?

La protección original de la consola se basa en leer un código regional que viene "grabado" físicamente en la base de los discos originales (conocido como *wobble groove*). Las grabadoras de CD comunes de PC no pueden replicar esta pista física. Los códigos son:
*   **SCEA:** América (NTSC-U)
*   **SCEE:** Europa (PAL)
*   **SCEI:** Japón (NTSC-J)

Lo que hacen estos códigos programados en el PIC12F508 (como el MM3 o Mayumi v4) es conectarse directamente al controlador del lector de CD (el chip Mechacon). Cuando la consola intenta verificar la autenticidad del disco durante el arranque, el modchip interviene y **"spamea" o inyecta continuamente la cadena de texto** de la región correspondiente. 

Al recibir este código inyectado por el PIC, la consola es engañada, asume que está leyendo la pista física de un disco original válido, y permite el arranque del juego o copia de seguridad.

Esto en los primeros chips era ideal, ya que el juego/consola no verificaba si se seguía inyectando la región mientras jugabas. Sin embargo, en juegos como *Spyro*, *Dino Crisis* e incluso *MediEvil 2*, existe una segunda verificación antipiratería. Si el modchip no la pasa, salta la clásica pantalla de "SOFTWARE TERMINATED" (una con un símbolo rojo al centro y fondo negro). Ahí es donde entran los códigos proporcionados aquí, al detectar correctamente el inicio del juego y apagando el chip (el famoso modo *stealth* o sigilo).

> [!NOTE]
> Como experiencia personal, he visto que hasta en una consola y juego original de la misma región puede saltar la pantalla de "SOFTWARE TERMINATED" si usas un chip antiguo (no stealth), ya que el chip está permanentemente inyectando el código y hace disparar la protección del propio juego legítimo.

### ⚠️ Advertencia sobre consolas Japonesas (NTSC-J)

Es importante destacar que las consolas japonesas (desde la serie SCPH-3000 en adelante) tienen una capa de seguridad adicional. Además de la verificación del lector de CD, **tienen un bloqueo de región directamente integrado en la BIOS**. 

Si instalas este modchip en una consola japonesa:
*   Podrás jugar **copias de seguridad de juegos japoneses** sin problemas.
*   **NO podrás arrancar juegos americanos (NTSC-U) ni europeos (PAL)** directamente. El chip engaña al lector, pero la BIOS detectará que el juego no es de su región y bloqueará el arranque. 

Para jugar títulos de otras regiones en una consola japonesa con modchip, necesitarás usar un disco de arranque (como el *Import Player*), utilizar *FreePSXBoot* y un juego japonés original o directamente reemplazar el chip BIOS de la consola.

### 📺 El problema del color en juegos de otras regiones (PAL vs NTSC)

El modchip te permite saltar el bloqueo del CD y arrancar un juego europeo (PAL) en una consola americana (NTSC) o viceversa, pero **no modifica el hardware de video de la consola**.

La placa madre de la PS1 tiene un único oscilador de cristal (el reloj de cuarzo principal) soldado de fábrica, el cual dicta la frecuencia exacta de la subportadora de color para su región nativa. Si fuerzas el arranque de un juego europeo (que corre a 50Hz) en una consola americana (que es de 60Hz), el codificador de video no logra sincronizar la frecuencia cromática correcta.

**¿El resultado?** La imagen se verá en **blanco y negro** o rodando verticalmente si utilizas cables de video compuesto tradicionales (los clásicos RCA Amarillo, Blanco y Rojo) o S-Video.

**¿Cómo se soluciona esto?**

**Vía Hardware (Avanzado):** Instalar un mod adicional conocido como **DFO** (*Dual Frequency Oscillator*) o **MFO**. Esto implica desoldar el reloj original de la placa madre y reemplazarlo por una pequeña placa inteligente que cambia la frecuencia de reloj al vuelo dependiendo de la región del juego insertado.

---

## Archivos y Compatibilidad

Recopilación de códigos originales diseñados para el PIC12C508 (el **PIC12F508** sí sirve, es simplemente la versión con memoria flash del 12C508). Los archivos `.hex` listos para programar están en la sección de **Releases**.

*   **MM3:** No utiliza el PIN 2 del chip. Personalmente recomendado para placas **PU-20, PU-22 y PU-23** (americanas, japonesas o europeas); y para **PM-41 y PM-41(2)** (americanas y japonesas).
*   **Mayumi V4:** Utiliza la misma instalación base del MM3, más la conexión del PIN 2 del chip (reloj). Se recomienda para los mismos modelos de placa que el MM3.
*   **OneChip:** Diseñado **SOLO** para las placas **PM-41 y PM-41(2)** de región europea (PAL).
*   **Stealth 2.8a:** Un código clásico orientado a las placas más antiguas, **PU-7 a PU-8**. Lo he usado para probar su funcionamiento, pero no para dejarlo de forma permanente instalado.

> [!NOTE]
> Para las placas PU-7, PU-8 y PU-18 recomiendo instalar PsNee. Cubierto aquí en [PS1 ModChip (B)](https://github.com/artbookconsolas/PS1-ModChip-B).

> *Crédito y agradecimientos a los creadores originales de estos códigos.*

### Placa de montaje (Hardware)
En la sección de *Releases* también se agrega el archivo Gerber (`PS1.ModChip.A.100526.zip`) de la PCB custom para montar el chip en su versión superficial (SOIC-8) junto con un espacio para su condensador de desacoplo de 100nF (o 0.1uF).

## Esquemas de Instalación

A continuación se detallan los diagramas de instalación para los diferentes modelos de placa base de PlayStation 1. La mayor parte de estos esquemas son fruto del excelente trabajo de documentación/recopilación de [Quade](https://quade.co/ps1-modchip-guide/). Créditos y agradecimientos para él.

Crédito a [fatcat.co](https://www.fatcat.co.nz/psx/install/102/ONEchip8.html) para la instalación de OneChip en las PM-41 PAL.

Creditos a [Consoles Unleashed](https://www.consolesunleashed.com/guides/sony-playstation-stealth-2-8a-modchip-install-guide/?v=7885444af42e) para la instalación de Stealth 2.8a en las PU-8.

Para mantener este documento organizado y no convertirlo en una galería de imágenes interminable, he colocado todos los diagramas de soldadura separados por código de chip (MM3, Mayumi V4, OneChip y Stealth 2.8a).

📂 **[Ver todos los esquemas de instalación en la carpeta `Esquemas`](./Esquemas/)**


