
<script>
    import { onMount } from 'svelte';
    import { writable } from "svelte/store";


    import zoo from '../assets/img/zoo.jpg';
    import tio_Anselm from '../assets/img/tio_Anselm.jpg';
  
    const blockSize = 80;
    let columns = 0, rows = 0;
    let startX = 0, startY = 0, lineLength = 0, dashOffset = 0;
    let startX2 = 0, startY2 = 0, lineLength2 = 0, dashOffset2 = 0;
    let showImage = false, showImage2 = false;

    let startScroll = false;
    let scrollPosition = 0;

    let path1Length = 0;
    let path2Length = 0;

    let viewportW;

    let horzImgSize = 0.9; 
    let pathColor = "#ffe500";

    let img1_posX;
    let img1_posY;
    let img2_posX;
    let img2_posY;

    let img2Height = 400;

    









    onMount(() => {



      

      let svgCont = document.querySelector('.scroll-container');
      let imgSvg2 = document.getElementById("imgSvg2");


      /* Generador de grilla */

      function updateGridDimensions() {
        columns = Math.ceil(window.innerWidth / blockSize);
        rows = Math.ceil(svgCont.offsetHeight / blockSize);
        console.log('rows' + rows)

        startX = blockSize - 5;
        startY = blockSize;
        lineLength = 3 * blockSize + 2 * blockSize;
        dashOffset = lineLength;

        startX2 = 5 * blockSize;
        startY2 = 11 * blockSize - 5;
        lineLength2 = 3 * blockSize;
        dashOffset2 = lineLength2;

        viewportW = window.innerWidth;


        img1_posX = "20%";
        img1_posY = "10%";
        img2_posX = "30%";
        img2_posY = "50%";


        /* Ajustar recorrido paths en función del ancho del viewport */

        if(window.innerWidth < 600){
          startX = blockSize - 5;
          startY = blockSize;
          lineLength = 3 * blockSize + 2 * blockSize;
          dashOffset = lineLength;

          startX2 = 5 * blockSize;
          startY2 = 11 * blockSize - 5;
          lineLength2 = 3 * blockSize;
          dashOffset2 = lineLength2;

        }else if(window.innerWidth > 600 && window.innerWidth < 900){
          startX = 2*blockSize - 5;
          startY = blockSize;

          startX2 = 7 * blockSize;
          startY2 = 13 * blockSize - 5;
          img2_posY = "60%";
          img2Height = 500;
          
        }else if(window.innerWidth > 901 && window.innerWidth < 1200){
          startX = 3*blockSize - 5;
          startY = blockSize;
          img1_posX = "30%";

          startX2 = 12 * blockSize;
          startY2 = 13 * blockSize - 5;
          img2_posX = "50%";
          img2_posY = "60%";

          horzImgSize = 0.6;
          img2Height = 600;

        }else{
          startX = 5*blockSize - 5;
          startY = blockSize;
          img1_posX = "30%";

          startX2 = 14 * blockSize;
          startY2 = 13 * blockSize - 5;
          img2_posX = "40%";
          img2_posY = "60%";

          horzImgSize = 0.4;

          img2Height = 600;

        }




      }


      updateGridDimensions();
      window.addEventListener('resize', updateGridDimensions);







        const path1 = document.getElementById("svgPath1");
        const path2 = document.getElementById("svgPath2");

        if (path1) path1Length = path1.getTotalLength();
        if (path2) path2Length = path2.getTotalLength();

        console.log(path2Length)



      /* INTERSECTION OBSERVER */

        const observer = new IntersectionObserver(
            ([entry]) => {
                if (entry.isIntersecting) {
                  console.log("está entrando")
                  startScroll = true;
                  window.addEventListener("scroll", updateScroll);
                } else {
                  startScroll = false;
                    window.removeEventListener("scroll", updateScroll);
                }
            },
            { 
              rootMargin: "0px 0px -50% 0px",
              threshold: 0 
            }
        );

        observer.observe(document.querySelector("#lv_svgWrapper"));

        return () => {
            window.removeEventListener("scroll", updateScroll);
            observer.disconnect();
        };
    });




    function updateScroll() {

      let pathLengthTotal = path1Length + path2Length;

      



        if (startScroll) {
            scrollPosition = window.scrollY - document.querySelector("#lv_svgWrapper").offsetTop - window.innerHeight * 0.5;
            console.log(scrollPosition);

            if (scrollPosition > path1Length +50 && !showImage){
              showImage = true;
            }

            if (scrollPosition > pathLengthTotal +550 && !showImage2){
              showImage2 = true;
            }


        }
  }


























</script>





<div class="scroll-container">
  <div id="lv_svgWrapper" class="lv_svg">
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
        stroke={pathColor}
        stroke-width="10"
        fill="none"
        stroke-dasharray={path1Length}
        stroke-dashoffset={scrollPosition > 0 ? Math.max(path1Length - scrollPosition, 0) : path1Length}
      />

      {#if showImage}
        <image 
          id="imgSvg1" 
          href={zoo} 
          x={img1_posX} 
          y={img1_posY} 
          width={`${viewportW*horzImgSize}px`} 
          height="auto" 
          transform={`translateX(-${viewportW / 2}px)`} 
        />
      {/if}

      <path
        id="svgPath2"
        d={`M${startX2},${startY2} h-${3 * blockSize}`}
        stroke={pathColor}
        stroke-width="10"
        fill="none"
        stroke-dasharray={path2Length}
        stroke-dashoffset={scrollPosition > path1Length + 500 ? Math.max(path2Length - (scrollPosition - path1Length-500), 0) : path2Length}
      />

      {#if showImage2}
        <image 
          id="imgSvg2" 
          href={tio_Anselm} 
          x={img2_posX} 
          y={img2_posY}  
          width="auto" 
          height={img2Height} 
          transform="translate(-150, -150)" 
        />
      {/if}
    </svg>
  </div>
</div>






<style>
  .scroll-container {
    height: 200vh;
    position: relative;
  }
  .lv_svg {
    padding-left: 10px;
    width: 100vw;
    height: 100%;
    background-color: #a7a197;
  }
  svg {
    display: block;
    width: 100%;
    height: 100%;
  }


  #imgSvg1{
    height: auto;

  }


  #imgSvg2{
    width: auto;

  }


</style>
