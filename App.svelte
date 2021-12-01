<head>
</head>

<script>
  /*import * from "d3";*/
  import {
    forceSimulation,
    forceLink,
    forceManyBody,
    forceCenter,
    forceCollide
  } from "d3-force";
  import { symbolCircle } from "d3-shape";
  import { data, datalines, datanodes } from "./Data.js";
  import { Graphic, Point, Line } from "@snlab/florence";
  import DataContainer from "@snlab/florence-datacontainer";

  const dataConNodes = new DataContainer(datanodes);
  const dataConLines = new DataContainer(datalines);

  let width = 800;
  let height = 800;

  let nodes = [...data.nodes];
  let links = [...data.lines];
  /*
  let nodes=dataConNodes
  let links=dataConLines
  */
  export let pointcolor = "";
  let loca = "All";

  const simulation = forceSimulation(nodes)
    .force("link", forceLink(links).id(d => d.Vakcode))
    .force("charge", forceManyBody())
    .force("center", forceCenter(width / 2, height / 2))
    .force("charge", forceCollide(25))
    .on("tick", () => {
      links = links;
      nodes = nodes;
    });

  function dotcolor(dotvaria, pointcolor) {
    console.log(dotvaria);
    if ((dotvaria = "Gemeenschappelijk")) {
      pointcolor = "green";
    } else if ((dotvaria = "Verbreding")) {
      pointcolor = "orange";
    } else if ((dotvaria = "Verbreding - aardwetenschappen")) {
      pointcolor = "yellow";
    } else if ((dotvaria = "Verbreding - sociaal-economische")) {
      pointcolor = "red";
    } else if ((dotvaria = "Wis- en natuurkunde")) {
      pointcolor = "purple";
    } else if ((dotvaria = "Business and innovation")) {
      pointcolor = "blue";
    } else if ((dotvaria = "Bachelorproef")) {
      pointcolor = "grey";
    }
    console.log(pointcolor);
    return pointcolor;
  }
  $: Course = "not found";
  $: VakIP = "not found";
  $: LocationIP = "not found";
  $: VakcodeIP = "not found";
  $: SpIP = "not found";
  $: JaarIP = "not found";
  $: SemesterIP = "not found";
  $: EctsIP = "not found";

  function information(node) {
    VakIP = node.Vak;
    LocationIP = node.Location;
    VakcodeIP = node.Vakcode;
    SpIP = node.SP;
    JaarIP = node.Jaar;
    SemesterIP = node.Semester;
    EctsIP = node.ECTS;
    return VakIP, LocationIP, VakcodeIP, SpIP, JaarIP, SemesterIP, EctsIP;
  }
</script>
<!--
<div id="main">

<p id="title">Orders of enrollment: a network visualization</p>
<h1>Background information</h1>
<p> Orders of enrollment can be very important for students to 
create there indivisual study programme. At the moment they need
a lot of clicks to have a good understanding of these.
As answers to this lack of general overview, a network visualization
can be part of the solution.</p>

<h1>Learning path</h1>
This assingment builds on the knowledge gained inside the course 
Interactive Data Visualization. On top of this some other resources 
have helped to develop this particular case. One of the resources <isindex>
<a href="https://flowingdata.com/2012/08/02/how-to-make-an-interactive-network-visualization/">
How to Make an Interactive Network Visualization</a>.
</div>
-->
<div id="all">
<div id="information" class="divs">
<p>Course: {VakIP}, workload of {SpIP} ECTS</p>
<p>Position in the Bachelor of Geography: {LocationIP}</p>
<p>Code: {VakcodeIP}</p>
<p>You best take this course in year {JaarIP} </p>
<p>This course is thaught in semester {SemesterIP}</p>
<p>Link to page in program book: <a href={EctsIP}> click here </a></p>
</div>
<div id="network" class="divs">
 <Graphic x={width} y={height} scaleX={[0, width]} scaleY={[0, height]}>
 {#each nodes as node, i}
 <Point x={node.x} y={node.y} radius={node.SP*2} fill={dotcolor(node.Location,pointcolor)}
 onMouseover={function() {information(node)}}/>
  
 {/each}
  {#each links as link}
   <Line
    x={[link.source.x, link.target.x]}
    y={[link.source.y, link.target.y]}
    strokeWidth={1}
  />
  {/each}  
  </Graphic>
</div>
 <!--
<select>
<select name="Location" id="Location" bind:value={loca}>
  <option value="">--Please choose an option--</option>
    {#each nodes as node}
    <option value={node.}>{region}</option>
    {/each}
</select>
<div id="bubbles">
<Graphic x={width} y={height} scaleX={[0, width]} scaleY={[0, height]}>

</Graphic>
</div>
 -->
</div>
<style>
  #main {
    padding-left: 5%;
    padding-right: 5%;
    text-align: justify;
  }
  #title {
    font-size: 30px;
    text-align: center;
  }

  #network {
    width: 400px;
    background-color: lightgray;
  }
  .divs {
    flex-direction: row;
    flex-wrap: wrap;
  }

  #information {
    width: 300px;
  }

  #all {
    padding-left: 5%;
  }
</style>