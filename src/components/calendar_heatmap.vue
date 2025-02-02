<template>
    <div ref="calendarHeatmap"></div>
</template>

<script setup>
import { ref, onMounted, onBeforeUnmount } from 'vue';
import * as d3 from 'd3';

const calendarHeatmap = ref(null);

const xOffset = 10; //margin-left
const yOffset = 10; //margin-top
const year_width = 400;

onMounted(() => {

    d3.csv("/dataset/full_data.csv", (d) => {
        return { 
            date: d3.timeParse("%Y-%m-%d")(d.tran_date), 
            profit: parseInt(d.total_amt), 
            sales: parseInt(d.Qty),
            cust_id: d.cust_id,
            prod_cat: d.prod_cat,
            prod_subcat: d.prod_subcat
        }
    }, (data) => {
        
        let svg = d3.select(calendarHeatmap.value).append("svg")
            .attr("width", "420px")
            .attr("viewBox","0 0 " + (xOffset + year_width) + " 540")
    });
});

// Clean up when component is destroyed
onBeforeUnmount(() => {
    if (calendarHeatmap.value) {
        d3.select(calendarHeatmap.value).selectAll('*').remove();
    }
});
</script>