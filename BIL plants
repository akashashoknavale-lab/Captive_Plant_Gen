<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>Solar Plant Dashboard</title>

<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
<script src="https://cdn.jsdelivr.net/npm/papaparse@5.4.1/papaparse.min.js"></script>

<style>
body {
  font-family: Arial;
  background:#f4f6f8;
  margin:0;
  padding:20px;
}
h1 {
  text-align:center;
  color:green;
}
.filters {
  display:flex;
  gap:20px;
  justify-content:center;
  margin-bottom:20px;
}
select {
  width:220px;
  height:120px;
}
.cards {
  display:grid;
  grid-template-columns: repeat(4,1fr);
  gap:15px;
  margin-bottom:20px;
}
.card {
  background:white;
  padding:15px;
  border-radius:8px;
  text-align:center;
  box-shadow:0 2px 5px rgba(0,0,0,0.1);
}
.charts {
  display:grid;
  grid-template-columns: 1fr 1fr;
  gap:20px;
}
canvas {
  background:white;
  padding:10px;
  border-radius:8px;
}
</style>
</head>

<body>

<h1>Bhageria GM Plants Generation Dashboard</h1>

<div class="filters">
  <div>
    <b>Select Plant</b><br>
    <select id="plantSelect" multiple></select>
  </div>
  <div>
    <b>Select Month</b><br>
    <select id="monthSelect" multiple></select>
  </div>
</div>

<div class="cards">
  <div class="card"><h3>Total Generation</h3><div id="gen">0</div></div>
  <div class="card"><h3>Avg PR %</h3><div id="pr">0</div></div>
  <div class="card"><h3>Avg CUF %</h3><div id="cuf">0</div></div>
  <div class="card"><h3>Plant Availability</h3><div id="avail">0</div></div>
</div>

<div class="charts">
  <canvas id="genChart"></canvas>
  <canvas id="prChart"></canvas>
</div>

<script>
const csvUrl = "PASTE_CSV_LINK_HERE";

let rawData = [];
let genChart, prChart;

Papa.parse(csvUrl, {
  download: true,
  header: true,
  complete: res => {
    rawData = res.data;
    populateFilters();
    updateDashboard();
  }
});

function populateFilters() {
  const plantSet = [...new Set(rawData.map(r => r["Site Name"]))];
  const monthSet = [...new Set(rawData.map(r => r["Month"]))];

  const plantSel = document.getElementById("plantSelect");
  const monthSel = document.getElementById("monthSelect");

  plantSet.forEach(p => plantSel.innerHTML += `<option selected>${p}</option>`);
  monthSet.forEach(m => monthSel.innerHTML += `<option selected>${m}</option>`);

  plantSel.onchange = monthSel.onchange = updateDashboard;
}

function updateDashboard() {
  const plants = [...plantSelect.selectedOptions].map(o=>o.value);
  const months = [...monthSelect.selectedOptions].map(o=>o.value);

  const filtered = rawData.filter(r =>
    plants.includes(r["Site Name"]) &&
    months.includes(r["Month"])
  );

  let totalGen = 0, totalPR = 0, totalCUF = 0;

  filtered.forEach(r=>{
    totalGen += Number(r["Actual Gen"]||0);
    totalPR += Number(r["PR"]||0);
    totalCUF += Number(r["CUF"]||0);
  });

  document.getElementById("gen").innerText = totalGen.toLocaleString();
  document.getElementById("pr").innerText = (totalPR/filtered.length || 0).toFixed(2)+"%";
  document.getElementById("cuf").innerText = (totalCUF/filtered.length || 0).toFixed(2)+"%";
  document.getElementById("avail").innerText = "98%";

  drawCharts(filtered);
}

function drawCharts(data) {
  const labels = data.map(r=>r.Month);
  const genData = data.map(r=>Number(r["Actual Gen"]||0));
  const prData = data.map(r=>Number(r["PR"]||0));

  if(genChart) genChart.destroy();
  if(prChart) prChart.destroy();

  genChart = new Chart(document.getElementById("genChart"), {
    type:'bar',
    data:{labels,datasets:[{label:'Generation (kWh)',data:genData}]}
  });

  prChart = new Chart(document.getElementById("prChart"), {
    type:'pie',
    data:{labels,datasets:[{data:prData}]}
  });
}
</script>

</body>
</html>
