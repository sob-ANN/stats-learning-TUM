<template>
  <div ref="chart" @mousemove="onMouseMove" @mouseleave="onMouseLeave" @click="addPoint"></div>
</template>

<script setup>
import * as d3 from 'd3';
import { ref, onMounted, watch } from 'vue';

// Reference to the chart div
const chart = ref(null);

// Data points for Bayesian optimization (initially empty)
const data = ref([]);

// Dimensions and margins for the chart
const margin = { top: 20, right: 30, bottom: 50, left: 60 };
const width = 800 - margin.left - margin.right;
const height = 400 - margin.top - margin.bottom;

// Create the SVG element
let svg;
let xScale, yScale;
let cursorLine;

onMounted(() => {
  svg = d3.select(chart.value)
    .append('svg')
    .attr('width', width + margin.left + margin.right)
    .attr('height', height + margin.top + margin.bottom)
    .append('g')
    .attr('transform', `translate(${margin.left},${margin.top})`);

  // Set up scales
  xScale = d3.scaleLinear()
    .domain([0, 6])
    .range([0, width]);

  yScale = d3.scaleLinear()
    .domain([-1, 9])
    .range([height, 0]);

  // Create axes
  svg.append('g')
    .attr('class', 'x-axis')
    .attr('transform', `translate(0,${height})`)
    .call(d3.axisBottom(xScale))
    .selectAll("text")
    .style("font-size", "18px");  // Customize x-axis font size

  svg.append('g')
    .attr('class', 'y-axis')
    .call(d3.axisLeft(yScale))
    .selectAll("text")
    .style("font-size", "18px");  // Customize y-axis font size

  // Add x-axis label
  svg.append("text")
    .attr("class", "x label")
    .attr("text-anchor", "end")
    .attr("x", width * 0.52)
    .attr("y", height + 40)
    .style("font-size", "18px")
    .style("font-style", "italic")
    .text("x");

  // Add y-axis label
  svg.append("text")
    .attr("class", "y label")
    .attr("text-anchor", "end")
    .attr("x", -145)
    .attr("y", -50)
    .attr("dy", ".75em")
    .attr("transform", "rotate(-90)")
    .style("font-size", "18px")
    .style("font-style", "italic")
    .text("f(x)");

  // Create the cursor line
  cursorLine = svg.append('line')
    .attr('stroke', '#cf7b0e')  // Set cursor line color
    .attr('stroke-width', 2)
    .attr('x1', 0)
    .attr('x2', 0)
    .attr('y1', 0)
    .attr('y2', height)
    .attr('opacity', 0);

  // Initial rendering of the chart
  updateChart();
});

const updateChart = () => {
  // Bind data and create circles for each data point
  const circles = svg.selectAll('circle')
    .data(data.value, d => d.x);

  circles.enter()
    .append('circle')
    .attr('cx', d => xScale(d.x))
    .attr('cy', d => yScale(d.y))
    .attr('r', 5)
    .attr('fill', '#1b52bf');  // Set point color

  circles
    .attr('cx', d => xScale(d.x))
    .attr('cy', d => yScale(d.y))
    .attr('fill', '#1b52bf');  // Ensure existing points maintain the color

  circles.exit().remove();
};

// Watch for data changes and update the chart accordingly
watch(data, updateChart, { deep: true });

// Define the underlying function
const underlyingFunction = x => 4 * (Math.sin((x - 0.5) * 1.6) * Math.sqrt(x + 1) / 3 + 1);

// Function to add a new point on click
const addPoint = (event) => {
  const coords = d3.pointer(event, svg.node());
  const x = xScale.invert(coords[0]);
  const y = underlyingFunction(x);
  data.value.push({ x, y });
};

// Function to handle mouse movement
const onMouseMove = (event) => {
  const coords = d3.pointer(event, svg.node());
  const xPos = coords[0];
  cursorLine
    .attr('x1', xPos)
    .attr('x2', xPos)
    .attr('opacity', 1);
};

// Function to handle mouse leaving the chart area
const onMouseLeave = () => {
  cursorLine.attr('opacity', 0);
};

</script>

<style scoped>
svg {
  font-family: Arial, sans-serif;
}
</style>
