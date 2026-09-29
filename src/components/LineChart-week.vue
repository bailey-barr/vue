<template>
  <div>
    <h2>weekly Visitors - Line Chart</h2>
    <svg ref="chart"></svg>
  </div>
</template>

<script setup>
import { ref, onMounted } from "vue";
import * as d3 from "d3";

const chart = ref(null);

const data = [
  { week: "first", value: 30 },
  { week: "second", value: 45 },
  { week: "third", value: 38 },
  { week: "fourth", value: 65 },
  { week: "fifth", value: 72 },
  
];

onMounted(() => {

  const width =500;
  const height = 400;

  const margin = {
    top: 20,
    right: 30,
    bottom: 50,
    left: 50
  };

  const svg = d3
    .select(chart.value)
    .attr("width", width)
    .attr("height", height);

  // X scale
  const x = d3
    .scalePoint()
    .domain(data.map(d => d.week))
    .range([margin.left, width - margin.right]);

  // Y scale
  const y = d3
    .scaleLinear()
    .domain([0, d3.max(data, d => d.value)])
    .nice()
    .range([height - margin.bottom, margin.top]);

  // Line generator
  const line = d3
    .line()
    .x(d => x(d.week))
    .y(d => y(d.value));

  // Draw line
  svg
    .append("path")
    .datum(data)
    .attr("fill", "none")
    .attr("stroke", "steelblue")
    .attr("stroke-width", 3)
    .attr("d", line);

  // Draw data points
  svg
    .selectAll("circle")
    .data(data)
    .join("circle")
    .attr("cx", d => x(d.week))
    .attr("cy", d => y(d.value))
    .attr("r", 5)
    .attr("fill", "orange");

  // X Axis
  svg
    .append("g")
    .attr(
      "transform",
      `translate(0,${height - margin.bottom})`
    )
    .call(d3.axisBottom(x));

  // Y Axis
  svg
    .append("g")
    .attr(
      "transform",
      `translate(${margin.left},0)`
    )
    .call(d3.axisLeft(y));
});
</script>