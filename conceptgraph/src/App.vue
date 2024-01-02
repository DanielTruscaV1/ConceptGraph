<script setup>
let isDragging = false;

const data = [
    {
        id: 1,
        r: 50,
        x: 50,
        y: 350,
    },
    {
        id: 2,
        r: 50,
        x: 250,
        y: 250,
    }
    ,{
        id: 3,
        r: 50,
        x: 450,
        y: 450,
    }
    ,{
        id: 4,
        r: 50,
        x: 650,
        y: 150,
    }
    ,{
        id: 5,
        r: 50,
        x: 850,
        y: 550,
    }
    ,
    {
        id:6,
        r:50,
        x:950,
        y:350,
    }
]

const svg = d3.select("#graph-container")
    .append("svg")
    .attr("width", 1500)
    .attr("height", 700);


const line = d3.line()
    .x(d => d.x)
    .y(d => d.y);

// Create a line between the first and second circles
const lines = svg.selectAll("line")
    .data([
        { source: data[0], target: data[1] },
        { source: data[0], target: data[2] },
        { source: data[1], target: data[3] },
        { source: data[2], target: data[4] },
        { source: data[2], target: data[5] },
    ])
    .enter()
    .append("line")
    .attr("x1", d => d.source.x)
    .attr("y1", d => d.source.y)
    .attr("x2", d => d.target.x)
    .attr("y2", d => d.target.y)
    .attr("stroke-width", 15)
    .attr("stroke", "rgb(150, 150, 150)");


    svg.selectAll("circle")
        .data(data)
        .enter()
        .append("circle")
        .attr("cx", d => d.x) 
        .attr("cy", d => d.y)
        .attr("r", d => d.r)      
        .attr("fill", "rgb(30, 30, 30)")
        .on("mousedown", handleMouseDown)
        .call(d3.drag()
                .on("start", dragStarted)
                .on("drag", dragging)
                .on("end", dragEnded));

    // Drag behavior
    const drag = d3.drag()
        .on("start", dragStarted)
        .on("drag", dragging)
        .on("end", dragEnded);

        function handleMouseDown(event, d) {
            if (!isDragging) {
                console.log("Clicked on node " + d.id);
                // Add your custom action here for 
                selectedId = d.id;
            }
        }

        function dragStarted(event, d) {
            isDragging = true;
        }

        function dragging(event, d) {
            d3.select(this)
                .attr("cx", d.x = event.x)
                .attr("cy", d.y = event.y);

            // Update the lines during dragging
            lines.attr("x1", d => d.source.x)
                .attr("y1", d => d.source.y)
                .attr("x2", d => d.target.x)
                .attr("y2", d => d.target.y);
        }

        function dragEnded(event, d) {
            isDragging = false;
        }

let selectedId = 0;

import {overviews} from "./MockDB.js";

</script>

<template>
  <div id="app">
    <div id="overview">
        <h1>
            Concept: {{ overviews[selectedId].concept }}
        </h1>
        <h2>
            Filozof: {{ overviews[selectedId].filozof }}
        </h2>
        <img :src="overviews[selectedId].image"/>
        <p>
            {{ overviews[selectedId].abstract }}
        </p>
    </div>
  </div>
</template>

<style scoped>
    #overview 
    {
        position:fixed;
        top:10vh;
        right:30px;
        width:30vw;
        height:80vh;
        background-color:rgba(230, 230, 230, 0.5);
        border-radius:50px;
        font-family: 'Montserrat', sans-serif;
    }
    h1, h2, p 
    {
        margin-left:30px;
    }
    h1 
    {
        font-size:25px;
    }
    h2 
    {
        font-size:23px;
    }
    p 
    {
        width:400px;
        text-align:justify;
        text-indent:50px;
    }
    img 
    {
        margin-left:25%;
        width:50%;
        height:200px;
    }
</style>
