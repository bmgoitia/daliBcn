<script>
  import { onMount } from 'svelte';
  import { fly, scale } from 'svelte/transition';
  import mapboxgl from 'mapbox-gl';

  import Grilla3ok from '../src/lib/Grilla3ok.svelte';
  import Grilla4 from '../src/lib/Grilla4.svelte';
  import Grilla5 from '../src/lib/Grilla5.svelte';
  import Mapa_mov from '../src/lib/Mapa_mov.svelte';
  import BloqueMapa from '../src/lib/BloqueMapa.svelte';


  import taxiMov from '../src/assets/img/taxi_mov.png';
  import taxiDesk from '../src/assets/img/SA_taxi.png';
  import taxiMedio from '../src/assets/img/taxiMedio.png';
  import taxiCapa from '../src/assets/img/taxiCapa.png';
  import taxiSinLados from '../src/assets/img/taxiSinLados.png';
  import taxiOk from '../src/assets/img/taxiOk.png';
  import portada from '../src/assets/img/portadaPix.png';


 
  


  let lv_isMov;
  let scrollPos;




  function lv_checkScreenW(){
    lv_isMov = window.innerWidth < 900;
  }








  onMount(() => {


    lv_checkScreenW();
    window.addEventListener('resize', lv_checkScreenW);
    window.addEventListener("scroll", scrollChecker);
   

 
  });

  function scrollChecker(){
    scrollPos = window.scrollY;
    
    /* let newScrollPos = Math.max((window.scrollY - document.querySelector("#dali_text").offsetTop),0);
    console.log('Esto debe ser cero hasta que entre el texto: ' + newScrollPos); */

  }

  $: zoomLevel = lv_isMov ? 1 + scrollPos / 900 : 1 + scrollPos / 2000; 
  $: offsetX = (scrollPos / 50) * -1; 
  $: offsetY = (scrollPos / 15) * -1; 








  
</script>



















<main>

  <div id="bcnDali" class="lv_piece">
    <div class="lv_pieceWrapper">

      <section id="dali_intro">
        <div class="dali_introWrapper">

          <div class="dali_introTitle">
            <div class="introImg">
              <img 
                src={taxiOk} 
                alt="Dalí en un taxi en Barcelona"
                class="lv_zoomImage"
                style="transform:scale({zoomLevel}) translateX({lv_isMov ? -offsetX : 0}%);"
              >
            </div>

            <div class="introTxt">
              <div class="introTxtWrapper">
                  <h1>La Barcelona de</h1> 
                  <span class="Dalient">Dalí</span> 
                </div>
            </div>



          </div>


          <!-- <Grilla3ok /> -->
           <!-- <Grilla4 /> -->
            <Grilla5 />


            <div class="lv_gradient" style="width: 100%; height: 200px; background: linear-gradient(180deg, rgba(167,161,151,1) 0%, rgba(255,255,255,1) 100%);"></div>




        </div>

      </section> <!-- END of intro -->







      <article>

        <div id="lv_Authors">

          <div class="lv_authorsWrapper">
              <span class="lv_authorName">
                  <a href="https://www.lavanguardia.com/autores/placid-garcia-planas.html" target="_blank">
                      <h4>Plàcid Garcia-Planas</h4>
    
                  </a>
              </span>
    

    
    
    
              <span class="lv_authorName">
                
                    <h5>Desarrollo web: <br> Begoña M. Goitia</h5>
                
              </span>
    
    
    
          </div>
    
          <div class="lv_xarxesWrapper">
                  <a href="" target="_blank" class="btn rounded-circle"><i class="fab fa-facebook-f"></i></a>
                  <a href="" target="_blank" class="btn rounded-circle"><i class="fab fa-twitter"></i></a>
                  <a href="" target="_blank" class="btn rounded-circle"><i class="fab fa-whatsapp"></i></a>
          </div>
    
        
    
        
        
        </div>

        <section id="dali_text">
          <div class="dali_textWrapper">

            <div class="lv_textBlock">
              <p class="lv_Par">–¡Taxi! A la calle Salvador Dalí. Y rápido. </p>
              <p class="lv_Par">  –Lo siento, en Barcelona no existe la calle, ni la plaza, ni la avenida Salvador Dalí. Pero, si quiere, le puedo llevar al paseo Picasso, al parque Joan Miró o incluso al carrer de les Tàpies. </p>
              <p class="lv_Par"> –Bueno, pues lléveme a algún monumento daliniano. </p>
              <p class="lv_Par"> –Le puedo llevar al monumento Homenaje a Picasso, de Antoni Tàpies, o a una escultura gigantesca de Miró en su propio parque… ¡Incluso a un sinfín de obritas públicas de Joan Brossa! </p>
              <p class="lv_Par">  –Pero, vamos a ver… Media familia Dalí nació en Barcelona, y la otra se crió y residió aquí. Dalí fue el primer difusor internacional del modernismo catalán, especialmente de Gaudí, promocionó la ciudad cuando todavía no había turismo, en revistas del prestigio de Vogue o Paris Match, ayudó a triunfar a los artistas catalanes en Nueva York, incluso dio a conocer internacionalmente a Copito de Nieve. Barcelona fue uno de sus grandes escenarios de Dalí… ¡En 1962 intentó entrar en la peluquería Llongueras rompiendo su cristal con un mazo! Estudió cuatro años en Madrid y allí tiene Plaza y monumento. Tiene calles en el cinturón de Barcelona –Cornellà, Sant Cugat o Badalona–, en Estados Unidos o en Rusia… </p>
              <p class="lv_Par"> –Mire, lo más parecido a lo que me pide, ahora mismo, es una exposición titulada La Barcelona de Dalí, en el museo Diocesà. Ahí, los mejores fotoperiodistas del momento dejaron su testimonio de lo que me acaba de comentar. </p>


            </div>



            <div class="lv_separator"></div>

            <div class="lv_destacado">
                <h5>Dalí promocionó Barcelona en revistas como Vogue o Paris Match cuando todavía no había turismo</h5>
            </div>

            <div class="lv_separator"></div>




            <div class="lv_textBlock">

              <p class="lv_Par">Este diálogo con el taxista es imaginario. Lo único real es la exposición, comisariada por Ricard Mas y abierta en el museu Diocesà, avenida de la Catedral, 4. Hasta el 1 de diciembre.</p>

              <p class="lv_Par">La muestra recoge –a través de la mirada de grandes fotorreporteros como Català-Roca, Brangulí, Carlos Pérez de Rozas, Postius, Sans Condeminas, Sáenz Guerrero o González Albiach– las prodigiosas acciones de Dalí por la trama urbana de Barcelona… El asalto de un gorila albino a un maniquí con vestido de novia. Forzudos bedeles cargando en plena canícula un caballo disecado por la escalinata del Ritz. Modelos en remojo bombardeadas a caracolazos borgoñeses. Dalí subido en una silla brindando, junto a un retrato en cartón de Manolete, por la guitarra del Pescaílla. Dos multitudinarios happening con alquitrán en pleno Park Güell. Una antigua prostituta posando, empapada, para un retrato de María Magdalena… e incluso, para los más fetichistas, los últimos cabellos de Dalí que le peinó el estilista Llongueras. </p>

              <div class="lv_miniImg">
                <img src={portada} alt="">
              </div>


              <p class="lv_Par">También se incluyen carteles como el que pintó para “salvar” al club de futbol Sant Andreu, litografías, libros, revistas y catálogos, originales de Lorca y Dalí, así como numerosas cartas y notas que documentan la amplia relación del genio ampurdanés con el Cap i Casal.</p>


            </div>



          </div>
        </section>




        <div class="lv_gradient" style="width: 100%; height: 200px; background: linear-gradient(180deg, rgba(255,255,255,1) 0%, rgba(88,88,88,1) 100%);"></div>



        <section id="dali_mapa">
          <div class="dali_mapaWrapper">

            <div class="dali_mapaHeader">
              
              <p>Haz click sobre el mapa para consultar los lugares vinculados con el maestro del surrealismo</p>
            </div>

            


            <div class="dali_mapBlock">
              <!-- {#if lv_isMov}
                <Mapa_mov />
              {:else}
                <Mapa_desk />
              {/if} -->

              <Mapa_mov />

            </div>









          </div>

        </section>

      </article> 
  

    </div>
  </div>

    

</main>






<style>





  #bcnDali{

  }

  .lv_pieceContainer{
    
  }




  /* INTRO */


  #dali_intro{
   /*  background-color: var(--lightBlack); */

  }

  .dali_introWrapper{

  }




  .dali_introTitle{
    width: 100%;
    height: 100vh;
    display: flex;
    flex-direction: column;
    /* background-color: #e3e1de; */
    position: relative;
    background: linear-gradient(180deg, rgba(227,225,222,1) 0%, rgba(167,161,151,1) 100%);

  }

  .introImg{
    width: 100%;
    height: 100%;
    display: flex;
    justify-content: center;
    align-items: center;
    overflow-x: hidden;
    overflow-y: hidden;
  }

  .introImg img{
    width: auto;
    height: 100%;
    transition:transform 0.1s;
    transform-origin:center center;
    
  }








  .introTxt{
    position:absolute;
    top: 20px;
    background-color: transparent;
    width: 100%;
    display: flex;
    flex-direction: column;
    justify-content:flex-start;
    align-items: center;
    z-index: 3;
    
  }

  .introTxtWrapper{
    padding: 30px 0;
    width: 100%;
    text-align: center;
  }

  






  .introTxt h1{
    font-family: "The Nautigal", cursive;
    font-size: 4.5rem;



    
  }


  .introTxt span{
    display: block;
    font-family: "The Nautigal", cursive;
    font-size: 6.5rem;
    text-transform: uppercase;

  }





    /* Bloque autores */

    #lv_Authors{
    
    padding-top: 20px;
    margin-bottom: 80px;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;

  }

  .lv_authorsWrapper{

  }

  .lv_authorName{
    text-align: center;
  }

  .lv_authorName a{
      text-decoration: none;
      
  }

  .lv_authorName h4{
      text-transform: uppercase;
      color: #000;
      font-family: 'HMedium', serif;
      font-size: 16px;

  }

  .lv_authorName h5{
      margin: 12px 0;
      text-transform: uppercase;
      color: #000;
      font-family: 'HMedium', serif;
      font-size: 12px;

  }

  .lv_xarxesWrapper{
    display: flex;
    justify-content: center;
    margin-top: 10px;
  }

  .lv_xarxesWrapper .btn{
      width: 40px;
      margin-left: 6px;
      color: #fff;
      background-color: #bcbcbc;
      border-color: #bcbcbc;
      border: 1px solid transparent;
      padding: .375rem .75rem;
      font-size: 1rem;
      line-height: 1.5;
      border-radius: .25rem;
      border-radius: 50%

  }


  /* End of authors */






  /* TEXTO */

  #dali_text{

  }

  .dali_textWrapper{

  }


  .lv_textBlock{
    margin-top: 60px;
  }



  .lv_Par{
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
    letter-spacing: -.1px;
    word-wrap: break-word;
    
  }



  .lv_separator {
    width: 40%;
    max-width: 300px;
    height: 2px;
    background-color: #cecece; 
    margin: 20px auto; 
    border-radius: 5px; 
    display: none;
  }

  .lv_destacado h5{
    padding: 20px 15px;
    max-width: 750px;
    margin: 0 auto;
    font-family: 'HSemibold', serif;
    font-size: 1.7rem;
    
  }


  .lv_miniImg{
    width: 100%;
    max-width: 750px;
    margin: 0 auto;
    padding: 60px 0;
  }

  .lv_miniImg img{
    max-width:100%;
    max-height:100%;
    object-fit:contain;
  }




  /* MAPA */

  #dali_mapa{
    background-color: var(--lightBlack);
    padding-bottom: 250px;
  }

  .dali_mapaWrapper{

  }

  .dali_mapaHeader p{
    width: 100%;
    text-align: center;
    font-family: 'Roboto', sans-serif;
    font-weight: 300;
    font-style: italic;
    color: #fff;
    padding-top: 60px;
    padding-bottom: 50px;

    
}







  /* MEDIA QUERIES */

  @media (min-width: 750px) {

    .introTxtWrapper{
      padding: 0;
    }

    .introTxtWrapper h1{
      font-size: 5.5rem;

    }


    .introTxtWrapper span{
      font-size: 7.5rem;
    }



    .lv_separator{
        display: block;
    }

    .lv_destacado h5{
        font-size: 2rem;
    }
}

</style>
