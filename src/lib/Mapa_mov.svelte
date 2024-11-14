
<script>
import { onMount } from 'svelte';
import mapboxgl from 'mapbox-gl';

import { mapaLlocsDali } from '../data/daliLlocs';


import albeniz from '../assets/img/albeniz.jpg';
import ateneu from '../assets/img/ateneu.jpeg';
import batllo from '../assets/img/batllo.jpg';
import caracoles from '../assets/img/caracoles.jpg';
import guell from '../assets/img/guell.jpg';
import llongueras from '../assets/img/llongueras2.png';
import masia from '../assets/img/masia.jpeg';
import mnac from '../assets/img/mnac.jpg';
import pedrera from '../assets/img/pedrera80.jpg';
import ritz_joyeros from '../assets/img/ritz_joyeros.jpeg';
import ritz_uri_geller from '../assets/img/ritz_uri_geller.jpg';
import ritz1 from '../assets/img/ritz1.jpg';
import ritz2 from '../assets/img/ritz2.jpg';
import ritz3 from '../assets/img/ritz3.jpg';
import SA_taxi from '../assets/img/SA_taxi.png';
import sagrada from '../assets/img/sagrada.jpg';
import sala_pares from '../assets/img/sala_pares.jpg';
import sant_pau from '../assets/img/sant_pau.jpg';
import tarantos from '../assets/img/tarantos.jpg';
import tGoya from '../assets/img/tGoya.jpg';
import tinell from '../assets/img/tinell.jpg';
import tio_Anselm from '../assets/img/tio_Anselm.jpg';
import toros from '../assets/img/toros.jpg';
import via_Veneto from '../assets/img/via_Veneto.jpg';
import zoo from '../assets/img/zoo.jpg';








let lv_map;
const mapboxToken = 'pk.eyJ1IjoibGF1cjA1IiwiYSI6ImNpbmtmM2FjazAwODF2eG0yNjhteTcxdHIifQ.l7uzjVe2b1L8dHh_Z9JjoQ';



onMount(() => {

    const imgDetail = document.getElementById('imgDetail');
    const detailTitle = document.getElementById('detailTitle');
    const detailPar = document.getElementById('detailPar');





    /* INICIALIZACIÓN MAPA */

    mapboxgl.accessToken = mapboxToken;



    const bounds = new mapboxgl.LngLatBounds();
    mapaLlocsDali.forEach(item => {
      const coord = new mapboxgl.LngLat(item.coordEdificio[0], item.coordEdificio[1]);
      bounds.extend(coord);
    });



    lv_map = new mapboxgl.Map({
        container: 'lv_map',
        style: 'mapbox://styles/mapbox/dark-v10',
        /* center: [2.166, 41.387], */
        /* zoom: 15, */
        /* pitch: 45,  */
        bearing: 97, 
        scrollZoom: false,
    });




    lv_map.addControl(new mapboxgl.NavigationControl());


    mapaLlocsDali.forEach(item => {

        const marker = new mapboxgl.Marker({ color: '#ffe500' })
            .setLngLat(item.coordEdificio)
            .addTo(lv_map)
            .getElement();

        marker.style.cursor = 'pointer';
           
        marker.addEventListener('click', () => {
        imgDetail.src = item.imgEdificio; 
        detailTitle.textContent = item.nombreEdificio; 
        detailPar.textContent = item.descrEdificio; 
    });
        
    });


    lv_map.fitBounds(bounds, {
        padding: 50, 
        maxZoom: 15, 
        duration: 0 
    });

    lv_map.once('idle', () => {
        lv_map.rotateTo(-45); 
    });







   /*  mapaLlocsDali.forEach(item => {
        
    

        const marker = new mapboxgl.Marker({ color: '#db5e30' })
            .setLngLat(item.coordsLoc)
            .addTo(lv_map)
            .getElement();

            marker.addEventListener('click', () => {
                handleCarouselItemClick(item);
            });
        
    }); */







    lv_map.on('styledata', () => {
      const layers = lv_map.getStyle().layers;
      for (const layer of layers) {
        if (layer.type === 'symbol' && layer.layout['text-field']) {
          lv_map.setLayoutProperty(layer.id, 'text-field', [
            'coalesce',
            ['get', 'name_ca'], 
            ['get', 'name']
          ]);
        }
      }
    });



    lv_map.on('load', () => {
        // Añadir edificios en 3D
        lv_map.addLayer({
            'id': '3d-buildings',
            'source': 'composite',
            'source-layer': 'building',
            'filter': ['==', 'extrude', 'true'],
            'type': 'fill-extrusion',
            'minzoom': 15,
            'paint': {
            'fill-extrusion-color': '#aaa',
            'fill-extrusion-height': ['get', 'height'],
            'fill-extrusion-base': ['get', 'min_height'],
            'fill-extrusion-opacity': 0.6
            }
        });
    });







});


</script>

















<div class="lv_mapaMov">
    <div class="lv_mapaMovWrapper">

       


        <div class="lv_detail">
            <div class="lv_detailWrapper">

                <div class="detailImg">
                    <img id="imgDetail" src={ritz1} alt="">
                    <span class="imgCredit">© Fundació Gala-Salvador Dalí</span>
                </div>

                


                <div class="detailText">
                    <div class="detailTextWrapper">
                        <h4 id="detailTitle">Hotel Ritz</h4>
                        <p id="detailPar"> Entre 1948 y 1979, cada vez que Dalí se hospedaba en Barcelona, lo hacía en la suite 108 de este lujoso hotel. A menudo, también alquilaba la habitación contigua, que servía de sala de recepción.</p>
                    </div>
                    
                </div>
                

            </div>
        </div>




        <div class="lv_mapWrapper">
            <div id="lv_map" class="lv_map-container"></div>
        </div>
        
    </div>
</div>
    









<style>



.lv_mapaMov{
    width: 100%;
    max-width: 850px;
    height: 100dvh;
    margin: 0 auto;
}


.lv_mapaMovWrapper{
    width: 100%;
    height: 100%;
    display: flex;
    flex-direction: column;
   /*  justify-content: center;
    align-items: center; */
    
}


.dali_mapaHeader{
    width: 100%;
    display: flex;
    justify-content: center;
    margin-bottom: 15px;
}



.dali_mapaHeader p{
    width: 90%;
    text-align: center;
    font-family: 'Roboto', sans-serif;
    font-weight: 300;
    font-style: italic;
    color: #fff;


    
}

/* DETAIL */

.lv_detail{
    width: 100%;
    height: 60%;
    padding-bottom: 50px;

}



.lv_detailWrapper{
    width: 100%;
    height: 100%;
    position: relative;
    display: flex;
    flex-direction: column;
}


.detailImg{
    width: 100%;
    height: 100%;
    margin: 0 auto;
    display: flex;
    justify-content: center;
    align-items: center;
    overflow: hidden;
    position: relative;
}

#imgDetail{
    max-width:100%;
    max-height:100%;
    object-fit:contain;
    

}

.imgCredit{
    position: absolute;
    right: 0;
    bottom: 0;
    padding: 4px 6px;
    background-color: #000;
    color:#fff;
    font-size: .7rem;
    font-style: italic;
    font-family: 'Roboto', sans-serif;
}







.detailText{

    width: 100%;
    margin: 0 auto;
    display: flex;
    justify-content: center;

}

.detailTextWrapper{
    width: 100%;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    padding: 12px;
    background-color: #000000ac;
    border-radius: 0px;

}



#detailTitle{
    color: #fff;
    text-transform: uppercase;
    font-weight: bold;
    margin-bottom: 10px;
    font-family: 'Roboto', sans-serif;
    padding-bottom: 8px;
    border-bottom: 1px solid yellow;
    margin-bottom: 16px; 
    font-size: 1.2rem;
}



#detailPar{
    color: #fff;
    font-family: 'Roboto', sans-serif;
    font-weight: 200;
    text-align: center;
}













/* MAPA */






.lv_mapWrapper{
    position: relative;
    height: 40%;
}


.lv_map-container {
    width: 100%;
    height: 100%;
}

.lv_inicio{
    position: absolute;
    top: 15px;
    left: 10px;
    z-index: 10002;
    cursor:pointer;
}

.lv_inicio span{
    display: inline-block;
    padding: 5px 10px;
    border: 1px solid #000;
    border-radius: 4px;
    background: #fff;
    text-transform: uppercase;
    font-family: 'TMedium', serif;
    -webkit-font-smoothing: antialiased;
    font-size: .9rem;
    letter-spacing: .7px;
}



</style>