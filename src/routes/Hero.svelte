<script>
    import world from "./world-110m.json";
    import * as topojson from "topojson-client";
    import { geoOrthographic, geoPath } from "d3-geo";
    import Glow from "./Glow.svelte";
    import Scroll from "./Scrolly.svelte";
    import { tweened } from "svelte/motion"
    import { circOut } from 'svelte/easing';

    let width = 400;
    $: height = width;

    let countries = topojson.feature(world, world.objects.countries).features;
    let borders = topojson.mesh(world, world.objects.countries, (a, b) => a !== b);

    let currentStep;

    const tweenOptions = {
		duration: 1200,
		easing: circOut
	};

    let xRotation = tweened(20, tweenOptions);
    let yRotation = tweened(-30, tweenOptions);

    $: {
        if(currentStep == 0) {
            xRotation.set(120)
            yRotation.set(30)
        } else if(currentStep == 1) {
            xRotation.set(350)
            yRotation.set(50)
        } else if(currentStep == 2) {
            xRotation.set(580)
            yRotation.set(70)
        } else if(currentStep == 3) {
            xRotation.set(120)
            yRotation.set(90)
        } else {
            xRotation.set(90)
        }
    };

    $: projection = geoOrthographic()
        .scale(width / 2)
        .rotate([$xRotation, $yRotation])
        .translate([width / 2, height / 2]);

    $: path = geoPath(projection);



        
</script>


<section>
    <div id="chart">
        <svg width={width} height={height}>
            <Glow/>
            <!-- Globe -->
            <circle class="circle" r={width / 2} cx={width / 2} cy={height / 2} fill="grey" filter="url('#glow')"/>
        
            <!-- Countries -->
            {#each countries as country}
                <path class="countries" d={path(country)} fill="#FFF1E0" stroke="none" />
            {/each}
        
            <!-- Borders -->
            <path class="borders" d={path(borders)} fill="none" stroke="grey" />

            <circle class="circle" r={10} cx={200} cy={300} fill="green" opacity="0.4"/>
        </svg>
    </div>

    <!-- The scrolling container which includes each of the text elements -->
    <Scroll bind:value={currentStep}>   
        <div class='step'>
            <div class="step-content">
                <p>Step 0</p>
            </div>
        </div>
        <div class='step'>
            <div class="step-content">
                <p>Step 1</p>
            </div>
        </div>
        <div class='step'>
            <div class="step-content">
                <p>Step 2</p>
            </div>
        </div>
        <div class='step'>
            <div class="step-content">
                <p>Step 3</p>
            </div>
        </div>
        <div class='step'>
            <div class="step-content">
                <p>Step 4</p>
            </div>
        </div>
    </Scroll>
</section>



<style>

    @import url('https://fonts.googleapis.com/css2?family=Source+Sans+Pro:ital,wght@0,200;0,300;0,400;0,600;0,700;0,900;1,200;1,300;1,400;1,600;1,700;1,900&display=swap');
    @import url('https://fonts.googleapis.com/css2?family=Open+Sans&family=Playfair+Display:wght@700&family=Staatliches&display=swap');
    @import url('https://fonts.googleapis.com/css2?family=Open+Sans&family=Playfair+Display:wght@700&family=Roboto:wght@400;500&family=Staatliches&display=swap');
    
    #chart {
        position: sticky;
        margin: 80px auto;
        max-width: 468px;
        background: #FFF1E0;
        top: 10%;
    }

    svg {
        overflow: visible;
        display: block;
        z-index: 999;
    }

    .countries { 
        transition-delay: 1s;
    }


    /* Scrollytelling CSS */
    .step {
        height: 80vh;
        display: flex;
        place-items: center;
        justify-content: center;
        z-index:1;
    }

    .step-content {
        background: whitesmoke;
        color: #ccc;
        border-radius: 5px;
        padding: 0.5rem 1rem;
        display: flex;
        flex-direction: column;
        justify-content: center;
        transition: background 500ms ease;
        box-shadow: 1px 1px 10px rgba(0, 0, 0, 0.2);
        z-index: 10;
    }

    h1 {
        font-family: "Playfair Display",serif;
        font-size: 48px;
        line-height: 1.2;
        font-weight: 700;
        letter-spacing: -2px;
        max-width: 650px;
        text-align: center;
        color: black; 
    }

</style>