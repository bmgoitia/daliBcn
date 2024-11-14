<script>
  import { onMount } from 'svelte';
  import zoo from '../src/assets/img/zoo.jpg';

  const blockSize = 80; // Tamaño del bloque incluyendo el espacio entre manzanas
  let columns = 0;
  let rows = 0;
  let startX = 0; // Posición X inicial de la línea roja
  let startY = 0; // Posición Y inicial de la línea roja
  let lineLength = 0; // Longitud total de la línea para la animación
  let dashOffset = 0; // Offset de la línea, actualizado al hacer scroll
  let isFixed = true; // Controla si el SVG debe estar en posición fixed
  let showImage = false; // Controla si la imagen debe aparecer al final del trazado

  // Función para actualizar la cuadrícula en función del tamaño de la ventana
  function updateGridDimensions() {
    columns = Math.ceil(window.innerWidth / blockSize);
    rows = Math.ceil(window.innerHeight / blockSize);

    // Configurar el punto de inicio de la línea y longitud de la línea
    startX = blockSize - 5;
    startY = blockSize;
    lineLength = 3 * blockSize + 2 * blockSize; // Longitud total de la línea (3 bloques hacia abajo, 2 bloques a la derecha)
    dashOffset = lineLength; // Inicialmente, la línea está "invisible"
  }

  // Función para gestionar el cambio de posición del SVG al hacer scroll
  function handleScroll() {
    const scrollY = window.scrollY;
    const viewportHeight = window.innerHeight;
    const svgHeight = viewportHeight * 2; // Ajustamos el alto del SVG para hacer scroll en el doble de la pantalla (200vh)

    // Cambia el SVG de fixed a absolute cuando se haya alcanzado el final de su altura
    isFixed = scrollY < svgHeight - viewportHeight;

    // Calcular la nueva posición de `dashOffset` para el trazado progresivo de la línea
    const maxScroll = svgHeight - viewportHeight;
    dashOffset = Math.max(lineLength - (scrollY / maxScroll) * lineLength, 0);

    // Mostrar la imagen al final del trazado de la línea
    if (dashOffset === 0) {
      showImage = true;
    }
  }

  onMount(() => {
    updateGridDimensions();
    window.addEventListener('resize', updateGridDimensions);
    window.addEventListener('scroll', handleScroll);

    // Limpiar los eventos al desmontar el componente
    return () => {
      window.removeEventListener('resize', updateGridDimensions);
      window.removeEventListener('scroll', handleScroll);
    };
  });
</script>

<!-- Contenedor de scroll para permitir desplazamiento -->
<div class="scroll-container">
  <!-- SVG con posición fija al inicio, que cambia a absolute al hacer scroll -->
  <div class="lv_svg" class:fixed={isFixed}>
    <svg width="100%" height="100%" xmlns="http://www.w3.org/2000/svg" viewBox={`0 0 ${columns * blockSize} ${rows * blockSize}`}>
      <!-- Definición de un bloque con chaflán corto -->
      <defs>
        <polygon id="lv_block" points="10,0 60,0 70,10 70,60 60,70 10,70 0,60 0,10" />
      </defs>
    
      <!-- Generación dinámica de bloques en la cuadrícula -->
      <g fill="#292929" stroke="#555" stroke-width="1">
        {#each Array(rows) as _, row}
          {#each Array(columns) as _, col}
            <use href="#lv_block" x={col * blockSize} y={row * blockSize} />
          {/each}
        {/each}
      </g>

      <!-- Línea roja que se dibuja con el scroll -->
      <path
        d={`M${startX},${startY} v${3 * blockSize - 5} h${2 * blockSize}`}
        stroke="#cfc363"
        stroke-width="5"
        fill="none"
        stroke-dasharray={lineLength} 
        stroke-dashoffset={dashOffset} 
      />

      <!-- Imagen centrada que aparece al final del trazado -->
      {#if showImage}
        <image href={zoo} x="50%" y="50%" width="300" height="300" transform="translate(-150, -150)" />
      {/if}
    </svg>
  </div>
</div>

<div class="lv_textBlock">
  <!-- Contenido del artículo -->
  <p class="lv_Par">Lorem ipsum dolor sit amet...</p>
  <!-- Repite o añade más contenido según sea necesario -->
</div>

<style>
  /* Contenedor para hacer scroll */
  .scroll-container {
    height: 300vh; /* Aumenta la altura para permitir el scroll */
    position: relative;
  }

  /* Clase para cuando el SVG está en posición fixed */
  .lv_svg.fixed {
    position: fixed;
    top: 0;
    left: 0;
    width: 100vw;
    height: 100vh;
    background-color: #585858;
  }

  /* Clase para cuando el SVG está en posición absolute */
  .lv_svg {
    position: absolute;
    width: 100vw;
    height: 200vh; /* Ajuste para cubrir todo el fondo del scroll */
    background-color: #585858;
  }

  /* Ajuste para que el SVG cubra toda la pantalla */
  svg {
    display: block;
    width: 100%;
    height: 100%;
  }

  .lv_textBlock {
    margin-top: 60px;
  }

  .lv_Par {
    padding-bottom: 40px;
    padding-left: 15px;
    padding-right: 15px;
    max-width: 850px;
    margin: 0 auto;
    font-family: 'TRegular', serif;
    -webkit-font-smoothing: antialiased;
    font-size: 18px;
    line-height: 27px;
    font-weight: 400;
    letter-spacing: -0.1px;
    word-wrap: break-word;
  }
</style>
