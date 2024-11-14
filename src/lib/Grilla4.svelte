<script>
    import { onMount } from 'svelte';
    import zoo from '../assets/img/zoo.jpg';
    import tio_Anselm from '../assets/img/tio_Anselm.jpg';
  
    const blockSize = 80;
    let columns = 0, rows = 0;
    let startX = 0, startY = 0, lineLength = 0, dashOffset = 0;
    let startX2 = 0, startY2 = 0, lineLength2 = 0, dashOffset2 = 0;
    let showImage = false, showImage2 = false;

    function updateGridDimensions() {
      columns = Math.ceil(window.innerWidth / blockSize);
      rows = Math.ceil(window.innerHeight / blockSize);

      startX = blockSize - 5;
      startY = blockSize;
      lineLength = 3 * blockSize + 2 * blockSize;
      dashOffset = lineLength;

      startX2 = 5 * blockSize;
      startY2 = 9 * blockSize;
      lineLength2 = 3 * blockSize;
      dashOffset2 = lineLength2;
    }

    function handleScroll() {
      const scrollY = window.scrollY;
      const viewportHeight = window.innerHeight;
      const svgHeight = viewportHeight * 2;
      const maxScroll = svgHeight - viewportHeight;

      dashOffset = Math.max(lineLength - (scrollY / maxScroll) * lineLength, 0);
      if (dashOffset === 0 && !showImage) showImage = true;

      if (showImage) {
        const additionalScrollStart = maxScroll + 200;
        const relativeScroll = Math.max(scrollY - additionalScrollStart, 0);
        dashOffset2 = Math.max(lineLength2 - (relativeScroll / maxScroll) * lineLength2, 0);
      }

      if (dashOffset2 === 0 && !showImage2) showImage2 = true;
    }

    onMount(() => {
      updateGridDimensions();
      window.addEventListener('resize', updateGridDimensions);
      window.addEventListener('scroll', handleScroll);

      return () => {
        window.removeEventListener('resize', updateGridDimensions);
        window.removeEventListener('scroll', handleScroll);
      };
    });
</script>





<div class="scroll-container">
  <div class="lv_svg">
    <svg width="100%" height="100%" xmlns="http://www.w3.org/2000/svg" viewBox={`0 0 ${columns * blockSize} ${rows * blockSize}`}>
      <defs>
        <polygon id="lv_EixampleBlock" points="10,0 60,0 70,10 70,60 60,70 10,70 0,60 0,10" />
      </defs>

      <g fill="#292929" stroke="#555" stroke-width="1">
        {#each Array(rows) as _, row}
          {#each Array(columns) as _, col}
            <use href="#lv_EixampleBlock" x={col * blockSize} y={row * blockSize} />
          {/each}
        {/each}
      </g>

      <path
        id="svgPath1"
        d={`M${startX},${startY} v${3 * blockSize - 5} h${2 * blockSize}`}
        stroke="red"
        stroke-width="5"
        fill="none"
        stroke-dasharray={lineLength} 
        stroke-dashoffset={dashOffset} 
      />

      {#if showImage}
        <image href={zoo} x="50%" y="30%" width="300" height="300" transform="translate(-150, -150)" />
      {/if}

      <path
        id="svgPath2"
        d={`M${startX2},${startY2} h-${3 * blockSize}`}
        stroke="red"
        stroke-width="5"
        fill="none"
        stroke-dasharray={lineLength2}
        stroke-dashoffset={dashOffset2}
      />

      {#if showImage2}
        <image href={tio_Anselm} x="30%" y="70%" width="300" height="300" transform="translate(-150, -150)" />
      {/if}
    </svg>
  </div>
</div>






<style>
  .scroll-container {
    height: 350vh;
    position: relative;
  }
  .lv_svg {
    padding-left: 10px;
    position: sticky;
    top:0;
    width: 100vw;
    height: 100dvh;
    background-color: #a7a197;
  }
  svg {
    display: block;
    width: 100%;
    height: 100%;
  }
</style>
