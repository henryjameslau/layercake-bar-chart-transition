<script>
  import { LayerCake, Svg } from 'layercake';
  import { scaleBand } from 'd3-scale';
  import { tweened } from 'svelte/motion';
  import { cubicOut } from 'svelte/easing';
  import Bar from './components/Bar.svelte';
  import AxisX from './components/AxisX.svelte';
  import AxisY from './components/AxisY.svelte';

  // This example loads csv data as json using @rollup/plugin-dsv
  import data from './data/groups.csv';

  let xKey = 'value';
  const yKey = 'year';

  let points = data.map(function(d){
    return {x:+d[xKey],y:d[yKey]};
  });

  const tweenedPoints = tweened(points,{
    duration:750,
    easing:cubicOut
  });

  function handleClick(){
		if(xKey == 'value'){
			xKey='chiffre'
		}else{
			xKey='value'
		}
	}

  function setTween(datakey){

    var temppoints=data.map(function(d){
      return {x:+d[xKey],y:d[yKey]};
    });
    tweenedPoints.set(temppoints);
  }

  $: setTween(xKey);



</script>

<style>
  .chart-container {
    width: 100%;
    height: 50%;
  }
</style>

<button on:click={handleClick}>
  all change
</button>
<div class="chart-container">
  <LayerCake
    padding={{ top: 0, bottom: 20, left: 30 }}
    x="x"
    y="y"
    yScale={scaleBand().paddingInner([0.05]).round(true)}
    yDomain={['1979', '1980','1981','1982','1983']}
    xDomain={[0, null]}
    data={$tweenedPoints}
  >
    <Svg>
      <AxisX
        gridlines={true}
        baseline={true}
        snapTicks={true}
      />
      <AxisY
        gridlines={false}
      />
      <Bar/>
    </Svg>
  </LayerCake>
</div>
