# Eksekusi-Penjadwalan
<!DOCTYPE html>
<html lang="id">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1" />
<title>Simulasi Penjadwalan Proses Dasar</title>
<link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;600;700&amp;display=swap" rel="stylesheet" />
<style>
  :root {
    --color-bg: #ffffff;
    --color-text: #6b7280;
    --color-primary: #111827;
    --color-accent: #000000;
    --radius: 0.75rem;
    --shadow-light: 0 4px 12px rgb(0 0 0 / 0.05);
    --transition: 0.3s ease;
  }
  *, *::before, *::after {
    box-sizing: border-box;
  }
  body {
    margin: 0;
    background: var(--color-bg);
    color: var(--color-text);
    font-family: 'Inter', system-ui, sans-serif;
    font-weight: 400;
    font-size: 18px;
    line-height: 1.6;
    min-height: 100vh;
    display: flex;
    flex-direction: column;
    align-items: center;
    padding: 4rem 1rem 5rem;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
  }
  main {
    max-width: 1200px;
    width: 100%;
    display: flex;
    flex-direction: column;
    gap: 3rem;
    user-select: none;
  }
  h1 {
    font-weight: 700;
    font-size: 48px;
    color: var(--color-primary);
    text-align: center;
    margin: 0;
    line-height: 1.05;
    user-select: text;
  }
  p.subtitle {
    font-weight: 400;
    font-size: 20px;
    color: var(--color-text);
    text-align: center;
    margin-top: 0.25rem;
    margin-bottom: 3rem;
    user-select: text;
  }
  section.card {
    background: #fafafa;
    border-radius: var(--radius);
    box-shadow: var(--shadow-light);
    padding: 2rem 2.5rem;
    display: flex;
    flex-direction: column;
    gap: 1.5rem;
  }
  label {
    display: block;
    font-weight: 600;
    color: var(--color-primary);
    margin-bottom: 0.5rem;
    cursor: pointer;
  }
  input[type="text"],
  input[type="number"],
  select {
    width: 100%;
    padding: 0.75rem 1.25rem;
    font-size: 1.125rem;
    border-radius: var(--radius);
    border: 1.5px solid #d1d5db;
    color: var(--color-primary);
    background: white;
    transition: border-color var(--transition), box-shadow var(--transition);
    font-family: 'Inter', system-ui, sans-serif;
    appearance: none;
    user-select: text;
  }
  input[type="text"]:focus,
  input[type="number"]:focus,
  select:focus {
    outline: none;
    border-color: var(--color-accent);
    box-shadow: 0 0 0 3px rgba(0, 0, 0, 0.15);
  }
  button {
    background-color: var(--color-accent);
    color: white;
    border: none;
    border-radius: var(--radius);
    font-weight: 700;
    font-size: 1.25rem;
    padding: 1rem 0;
    cursor: pointer;
    transition: background-color var(--transition), transform 0.2s ease;
    width: 100%;
    max-width: 220px;
    align-self: flex-start;
    user-select: none;
  }
  button:hover,
  button:focus {
    background-color: #333333;
    transform: scale(1.05);
    outline: none;
  }
  button:disabled,
  button[disabled] {
    background-color: #9ca3af;
    cursor: not-allowed;
    transform: none;
  }
  form > div {
    display: flex;
    gap: 1.5rem;
    flex-wrap: wrap;
    align-items: flex-start;
  }
  .form-group {
    flex: 1 1 200px;
    min-width: 200px;
  }
  ul#process-list {
    list-style: none;
    margin: 0;
    padding-left: 0;
    max-height: 220px;
    overflow-y: auto;
    border: 1.5px solid #e5e7eb;
    border-radius: var(--radius);
    font-weight: 600;
    color: var(--color-primary);
    user-select: text;
  }
  ul#process-list li {
    padding: 0.7rem 1rem;
    border-bottom: 1px solid #e5e7eb;
    word-break: break-word;
    cursor: default;
  }
  ul#process-list li:last-child {
    border-bottom: none;
    font-style: italic;
    color: #9ca3af;
  }
  .gantt-chart {
    margin-top: 2rem;
    border: 1.5px solid #e5e7eb;
    border-radius: var(--radius);
    overflow-x: auto;
    background: white;
    padding: 0.75rem 1rem;
    white-space: nowrap;
    user-select: none;
    min-height: 40px;
  }
  .gantt-bar {
    display: inline-block;
    font-weight: 700;
    color: white;
    border-radius: var(--radius);
    text-align: center;
    margin-right: 6px;
    font-size: 1rem;
    line-height: 2.25rem;
    padding: 0 0.5rem;
    position: relative;
    vertical-align: middle;
    user-select: none;
  }
  .gantt-bar.fifo {
    background-color: #2563eb;
  }
  .gantt-bar.sjf {
    background-color: #f97316;
  }
  .gantt-bar.srf {
    background-color: #ef4444;
  }
  .gantt-bar.roundrobin {
    background-color: #10b981;
  }
  .time-labels {
    display: flex;
    margin-top: 0.5rem;
    color: var(--color-text);
    font-weight: 600;
    font-size: 1rem;
    user-select: none;
    gap: 0.875rem;
  }
  .time-labels span {
    display: inline-block;
    min-width: 30px;
    text-align: center;
  }
  table {
    width: 100%;
    border-collapse: collapse;
    margin-top: 2rem;
    font-size: 1rem;
    user-select: text;
  }
  table thead th {
    font-weight: 700;
    color: var(--color-primary);
    border-bottom: 2px solid #d1d5db;
    padding: 0.75rem 1rem;
    text-align: left;
  }
  table tbody td {
    padding: 0.6rem 1rem;
    border-bottom: 1px solid #e5e7eb;
    color: var(--color-primary);
    font-weight: 600;
  }
  #time-quantum-container {
    margin-top: 0.75rem;
    margin-bottom: 1rem;
    max-width: 360px;
  }
  @media (max-width: 600px) {
    form > div {
      flex-direction: column;
    }
    .form-group {
      min-width: 100%;
    }
    button {
      width: 100%;
      max-width: none;
    }
  }
  .sr-only {
    position: absolute !important;
    width: 1px !important;
    height: 1px !important;
    padding: 0 !important;
    margin: -1px !important;
    overflow: hidden !important;
    clip: rect(0, 0, 0, 0) !important;
    white-space: nowrap !important;
    border: 0 !important;
  }
</style>
</head>
<body>
<main>
  <h1>Simulasi Penjadwalan Proses Dasar</h1>
  <p class="subtitle">Uji algoritma penjadwalan proses: FIFO, SJF, SRF, dan Round Robin</p>

  <section class="card" aria-labelledby="process-form-title">
    <h2 id="process-form-title" style="font-weight:700; font-size:1.5rem; margin-bottom:1.5rem;">Input Proses</h2>
    <form id="process-form" aria-describedby="process-form-desc" novalidate>
      <div id="process-form-desc" class="sr-only">Masukkan nama proses, waktu kedatangan, dan waktu burst (eksekusi)</div>
      <div class="form-group">
        <label for="process-name">Nama Proses</label>
        <input type="text" id="process-name" name="process-name" autocomplete="off" placeholder="Contoh: P1" required aria-required="true" />
      </div>
      <div class="form-group">
        <label for="arrival-time">Waktu Kedatangan (detik)</label>
        <input type="number" id="arrival-time" name="arrival-time" placeholder="Misal: 0" min="0" required aria-required="true" />
      </div>
      <div class="form-group">
        <label for="burst-time">Waktu Burst (detik)</label>
        <input type="number" id="burst-time" name="burst-time" placeholder="Misal: 5" min="1" required aria-required="true" />
      </div>
      <div style="flex-basis: 220px; align-self: flex-end;">
        <button type="submit" id="add-process-btn" aria-label="Tambah proses">Tambah Proses</button>
      </div>
    </form>

    <ul id="process-list" aria-live="polite" aria-relevant="additions removals" role="list" style="margin-top:1.5rem;">
      <li>Belum ada proses yang ditambahkan.</li>
    </ul>
  </section>

  <section class="card" aria-labelledby="scheduler-selection-title">
    <h2 id="scheduler-selection-title" style="font-weight:700; font-size:1.5rem; margin-bottom:1.5rem;">Pilih Algoritma Penjadwalan</h2>
    <select id="algorithm-select" aria-required="true" style="padding:0.75rem 1rem; font-size:1.125rem; border-radius:var(--radius); border:1.5px solid #d1d5db; max-width: 360px;">
      <option value="fifo">FIFO (First In First Out)</option>
      <option value="sjf">SJF (Shortest Job First)</option>
      <option value="srf">SRF (Shortest Remaining First)</option>
      <option value="roundrobin">Round Robin</option>
    </select>

    <div id="time-quantum-container" style="display:none;">
      <label for="time-quantum" style="font-weight:600; margin-top:1rem; display:block;">Time Quantum (detik) untuk Round Robin</label>
      <input type="number" id="time-quantum" min="1" step="1" value="2" style="width:100%; padding:0.75rem 1rem; font-size:1.125rem; border-radius:var(--radius); border:1.5px solid #d1d5db;" />
    </div>

    <div style="margin-top:2rem; display:flex; gap:1rem; flex-wrap: wrap; max-width: 360px;">
      <button id="start-simulation-btn" style="flex-grow: 1;">Mulai Simulasi</button>
      <button id="reset-btn" style="flex-grow: 1; background:#6b7280; color:#f9fafb;">Reset</button>
    </div>
  </section>

  <section class="card" aria-label="Hasil simulasi penjadwalan proses">
    <h2 style="font-weight:700; font-size:1.5rem; margin-bottom:1.5rem;">Hasil Simulasi</h2>
    <div id="gantt-container" class="gantt-chart" aria-live="polite" aria-atomic="true" aria-label="Diagram Gantt penjadwalan proses"></div>
    <div class="time-labels" id="gantt-timings" aria-hidden="true"></div>

    <table aria-label="Tabel ringkasan proses">
      <thead>
        <tr>
          <th>Proses</th>
          <th>Waktu Kedatangan (s)</th>
          <th>Waktu Burst (s)</th>
          <th>Waktu Tunggu (s)</th>
          <th>Waktu Penyelesaian (s)</th>
          <th>Turnaround (s)</th>
        </tr>
      </thead>
      <tbody id="process-summary-body"></tbody>
    </table>
  </section>
</main>

<script>
(() => {
  const deepClone = obj => JSON.parse(JSON.stringify(obj));
  const formatSec = sec => sec + 's';

  // Scheduler algorithms (FIFO, SJF, SRF, Round Robin)
  function scheduleFIFO(processes) {
    const procs = deepClone(processes).sort((a,b) => a.arrival - b.arrival);
    let currentTime = 0;
    const sequence = [];
    for(const proc of procs) {
      if(currentTime < proc.arrival) currentTime = proc.arrival;
      proc.start = currentTime;
      proc.finish = currentTime + proc.burst;
      proc.wait = proc.start - proc.arrival;
      proc.turnaround = proc.finish - proc.arrival;
      sequence.push({id: proc.id, name: proc.name, start: proc.start, duration: proc.burst});
      currentTime = proc.finish;
    }
    return {processes: procs, sequence};
  }

  function scheduleSJF(processes) {
    const procs = deepClone(processes).sort((a,b) => a.arrival - b.arrival);
    let currentTime = 0;
    const completed = [];
    const sequence = [];
    const readyQueue = [];
    while(completed.length < procs.length) {
      procs.forEach(p => {
        if(p.arrival <= currentTime && !completed.includes(p) && !readyQueue.includes(p)) {
          readyQueue.push(p);
        }
      });
      if(readyQueue.length === 0) {
        currentTime++;
        continue;
      }
      readyQueue.sort((a,b) => a.burst - b.burst);
      const proc = readyQueue.shift();
      proc.start = currentTime;
      proc.finish = currentTime + proc.burst;
      proc.wait = proc.start - proc.arrival;
      proc.turnaround = proc.finish - proc.arrival;
      sequence.push({id: proc.id, name: proc.name, start: proc.start, duration: proc.burst});
      currentTime = proc.finish;
      completed.push(proc);
    }
    return {processes: procs, sequence};
  }

  function scheduleSRF(processes) {
    const procs = deepClone(processes).map(p => ({...p, remaining: p.burst, start: null}));
    let currentTime = 0;
    const completed = new Set();
    const sequence = [];
    let lastProcId = null;

    while(completed.size < procs.length) {
      const ready = procs.filter(p => p.arrival <= currentTime && p.remaining > 0);
      if(ready.length === 0) {
        currentTime++;
        continue;
      }
      ready.sort((a,b) => a.remaining - b.remaining);
      const proc = ready[0];
      if(lastProcId !== proc.id) {
        sequence.push({id: proc.id, name: proc.name, start: currentTime, duration: 1});
      } else {
        sequence[sequence.length - 1].duration++;
      }
      if(proc.start === null) proc.start = currentTime;
      proc.remaining--;
      if(proc.remaining === 0) {
        proc.finish = currentTime + 1;
        proc.turnaround = proc.finish - proc.arrival;
        proc.wait = proc.turnaround - proc.burst;
        completed.add(proc);
      }
      currentTime++;
      lastProcId = proc.id;
    }
    return {processes: procs, sequence};
  }

  function scheduleRoundRobin(processes, quantum) {
    const procs = deepClone(processes).map(p => ({...p, remaining: p.burst, start: null}));
    let currentTime = 0;
    const sequence = [];
    const queue = [];
    const arrivedSet = new Set();
    let completedCount = 0;
    const totalProcs = procs.length;

    function enqueueArrived() {
      procs.forEach(p => {
        if(p.arrival <= currentTime && !arrivedSet.has(p.id)) {
          queue.push(p);
          arrivedSet.add(p.id);
        }
      });
    }

    enqueueArrived();

    while(completedCount < totalProcs) {
      if(queue.length === 0){
        currentTime++;
        enqueueArrived();
        continue;
      }
      const proc = queue.shift();
      if(proc.start === null) proc.start = currentTime;
      const execTime = Math.min(proc.remaining, quantum);
      sequence.push({id: proc.id, name: proc.name, start: currentTime, duration: execTime});
      proc.remaining -= execTime;
      currentTime += execTime;
      enqueueArrived();
      if(proc.remaining === 0){
        proc.finish = currentTime;
        proc.turnaround = proc.finish - proc.arrival;
        proc.wait = proc.turnaround - proc.burst;
        completedCount++;
      } else {
        queue.push(proc);
      }
    }
    return {processes: procs, sequence};
  }

  // UI Elements & state
  const form = document.getElementById('process-form');
  const list = document.getElementById('process-list');
  const algSelect = document.getElementById('algorithm-select');
  const startBtn = document.getElementById('start-simulation-btn');
  const resetBtn = document.getElementById('reset-btn');
  const ganttCont = document.getElementById('gantt-container');
  const ganttTiming = document.getElementById('gantt-timings');
  const timeQuantumCont = document.getElementById('time-quantum-container');
  const timeQuantumInput = document.getElementById('time-quantum');

  let processes = [];
  let nextId = 1;

  function updateList() {
    list.innerHTML = '';
    if(processes.length === 0) {
      const li = document.createElement('li');
      li.textContent = 'Belum ada proses yang ditambahkan.';
      list.appendChild(li);
      return;
    }
    processes.forEach(p => {
      const li = document.createElement('li');
      li.textContent = \`\${p.name} - Arrival: \${p.arrival}s, Burst: \${p.burst}s\`;
      list.appendChild(li);
    });
  }

  function reset() {
    processes = [];
    nextId = 1;
    updateList();
    startBtn.disabled = false;
    resetBtn.disabled = true;
    ganttCont.innerHTML = '';
    ganttTiming.innerHTML = '';
    form.querySelectorAll('input').forEach(i => i.disabled = false);
    algSelect.disabled = false;
    timeQuantumInput.disabled = false;
    timeQuantumCont.style.display = algSelect.value === 'roundrobin' ? 'block' : 'none';
    form.reset();
    form.elements['process-name'].focus();
  }

  algSelect.addEventListener('change', () => {
    timeQuantumCont.style.display = algSelect.value === 'roundrobin' ? 'block' : 'none';
  });

  form.addEventListener('submit', e => {
    e.preventDefault();
    const name = form.elements['process-name'].value.trim();
    const arrivalRaw = form.elements['arrival-time'].value;
    const burstRaw = form.elements['burst-time'].value;
    const arrival = Number(arrivalRaw);
    const burst = Number(burstRaw);

    if(!name) {
      alert('Nama proses harus diisi.');
      return;
    }
    if(arrivalRaw === '' || isNaN(arrival) || arrival < 0) {
      alert('Waktu kedatangan harus angka >= 0.');
      return;
    }
    if(burstRaw === '' || isNaN(burst) || burst <= 0) {
      alert('Waktu burst harus angka > 0.');
      return;
    }
    if(processes.some(p => p.name.toLowerCase() === name.toLowerCase())) {
      alert('Nama proses harus unik.');
      return;
    }

    processes.push({id: nextId++, name, arrival, burst});
    updateList();
    form.reset();
    form.elements['process-name'].focus();
  });

  function drawGantt(sequence, algorithm) {
    ganttCont.innerHTML = '';
    ganttTiming.innerHTML = '';
    if(!sequence || sequence.length === 0) return;

    let totalTime = 0;
    sequence.forEach(seg => {
      const endT = seg.start + seg.duration;
      if(endT > totalTime) totalTime = endT;
    });

    const pxSec = 30;

    sequence.forEach(seg => {
      const bar = document.createElement('div');
      bar.className = 'gantt-bar ' + algorithm;
      bar.style.width = (seg.duration * pxSec) + 'px';
      bar.style.marginLeft = (seg.start * pxSec) + 'px';
      bar.textContent = seg.name;
      bar.title = \`Proses \${seg.name}\\nMulai: \${formatSec(seg.start)}\\nDurasi: \${formatSec(seg.duration)}\`;
      ganttCont.appendChild(bar);
    });

    const labelGap = totalTime > 20 ? 2 : 1;
    for(let t = 0; t <= totalTime; t += labelGap) {
      const span = document.createElement('span');
      span.textContent = t;
      span.style.minWidth = pxSec * labelGap + 'px';
      ganttTiming.appendChild(span);
    }
  }

  function showSummary(procs) {
    const tbody = document.getElementById('process-summary-body');
    tbody.innerHTML = '';
    procs.forEach(p => {
      const tr = document.createElement('tr');
      tr.innerHTML = \`
        <td>\${p.name}</td>
        <td>\${p.arrival}</td>
        <td>\${p.burst}</td>
        <td>\${p.wait}</td>
        <td>\${p.finish}</td>
        <td>\${p.turnaround}</td>
      \`;
      tbody.appendChild(tr);
    });
  }

  startBtn.addEventListener('click', () => {
    if(processes.length === 0) {
      alert('Tambahkan minimal satu proses sebelum memulai simulasi.');
      return;
    }
    startBtn.disabled = true;
    resetBtn.disabled = false;
    form.querySelectorAll('input').forEach(i => i.disabled = true);
    algSelect.disabled = true;
    timeQuantumInput.disabled = true;

    let result;
    const algo = algSelect.value;

    if(algo === 'fifo'){
      result = scheduleFIFO(processes);
    }
    else if(algo === 'sjf'){
      result = scheduleSJF(processes);
    }
    else if(algo === 'srf'){
      result = scheduleSRF(processes);
    }
    else if(algo === 'roundrobin'){
      const quantum = parseInt(timeQuantumInput.value, 10);
      if(isNaN(quantum) || quantum < 1){
        alert('Time Quantum harus angka >= 1.');
        startBtn.disabled = false;
        resetBtn.disabled = true;
        form.querySelectorAll('input').forEach(i => i.disabled = false);
        algSelect.disabled = false;
        timeQuantumInput.disabled = false;
        return;
      }
      result = scheduleRoundRobin(processes, quantum);
    }
    else{
      alert('Algoritma tidak dikenal.');
      return;
    }

    drawGantt(result.sequence, algo);
    showSummary(result.processes);
  });

  resetBtn.addEventListener('click', reset);

  reset();
})();
</script>
</body>
</html>

