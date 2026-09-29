<template>
  <div>
    <h2>Study Hours vs. Score - Scatter Plot</h2>

    <svg ref="chart"></svg>

    <p v-if="selectedData">
      Study Hours: {{ selectedData.study }},
      Score: {{ selectedData.score }}
    </p>
  </div>
</template>

<script setup>
import { ref, onMounted } from "vue";
import * as d3 from "d3";

const chart = ref(null);
const selectedData = ref(null);

const data = [
  { study: 1, score: 45 },
  { study: 2, score: 55 },
  { study: 3, score: 62 },
  { study: 4, score: 70 },
  { study: 5, score: 78 },
  { study: 6, score: 88 },
  { study: 7, score: 92 }
];

onMounted(() => {

  const width = 600;
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
    .scaleLinear()
    .domain([0, 8])
    .range([margin.left, width - margin.right]);

  // Y scale
  const y = d3
    .scaleLinear()
    .domain([0, 100])
    .range([height - margin.bottom, margin.top]);

  // Draw points
  svg
    .selectAll("circle")
    .data(data)
    .join("circle")
    .attr("cx", d => x(d.study))
    .attr("cy", d => y(d.score))
    .attr("r", 7)
    .attr("fill", "steelblue")

    // Mouse over
    .on("mouseover", function(event, d) {

      d3.select(this)
        .attr("r", 12)
        .attr("fill", "orange");

      selectedData.value = d;
    })

    // Mouse out
    .on("mouseout", function() {

      d3.select(this)
        .attr("r", 7)
        .attr("fill", "steelblue");

      selectedData.value = null;
    });

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