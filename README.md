<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Cosine Wave Visualizer</title>
  <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
  <style>
    body {
      font-family: Arial, sans-serif;
      text-align: center;
      padding: 20px;
    }
    canvas {
      margin-top: 20px;
    }
  </style>
</head>
<body>
  <h1>กราฟคลื่นความถี่เสียง (Cosine Wave)</h1>
  <form id="frequencyForm">
    <label for="frequency">กรอกค่าความถี่ (Hz): </label>
    <input type="number" id="frequency" name="frequency" min="20" max="20000" step="1" required>
    <button type="submit">แสดงกราฟ</button>
  </form>
  <canvas id="cosineChart" width="800" height="400"></canvas>

  <script>
    const ctx = document.getElementById('cosineChart').getContext('2d');
    let chart;

    document.getElementById('frequencyForm').addEventListener('submit', function(event) {
      event.preventDefault();

      const frequency = parseFloat(document.getElementById('frequency').value);
      const sampleRate = 44100; // ตัวอย่างการสุ่ม (Samples per second)
      const duration = 0.01; // ระยะเวลา (วินาที)
      const dataPoints = Math.floor(sampleRate * duration);

      // คำนวณค่าของคลื่นโคไซน์
      const xValues = [];
      const yValues = [];
      for (let i = 0; i < dataPoints; i++) {
        const t = i / sampleRate;
        xValues.push(t);
        yValues.push(Math.cos(2 * Math.PI * frequency * t));
      }

      // สร้างกราฟ
      if (chart) {
        chart.destroy();
      }
      chart = new Chart(ctx, {
        type: 'line',
        data: {
          labels: xValues,
          datasets: [{
            label: `Cosine Wave (Frequency: ${frequency} Hz)`,
            data: yValues,
            borderColor: 'blue',
            borderWidth: 2,
            fill: false,
          }]
        },
        options: {
          responsive: true,
          scales: {
            x: {
              type: 'linear',
              title: {
                display: true,
                text: 'Time (seconds)'
              }
            },
            y: {
              title: {
                display: true,
                text: 'Amplitude'
              }
            }
          }
        }
      });
    });
  </script>
</body>
</html>