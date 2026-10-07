<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>FastAPI Application Startup Time Benchmark</title>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <style>
        body {
            font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Helvetica, Arial, sans-serif;
            background-color: #ffffff;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            margin: 0;
            padding: 20px;
            box-sizing: border-box;
        }
        .container {
            width: 100%;
            max-width: 800px;
            background: #ffffff;
            padding: 30px;
            border-radius: 12px;
            box-shadow: 0 4px 20px rgba(0,0,0,0.08);
        }
        .title {
            text-align: center;
            font-size: 18px;
            color: #333333;
            margin-bottom: 25px;
            font-weight: 500;
        }
        .chart-box {
            position: relative;
            height: 450px;
            width: 100%;
        }
    </style>
</head>
<body>

<div class="container">
    <div class="title">Measured time taken for the FastAPI application to become ready to accept requests.</div>
    <div class="chart-box">
        <canvas id="benchmarkChart"></canvas>
    </div>
</div>

<script>
    const ctx = document.getElementById('benchmarkChart').getContext('2d');
    
    // Custom plugin to draw top labels (e.g. "2.961 s")
    const topLabelsPlugin = {
        id: 'topLabels',
        afterDraw(chart) {
            const { ctx } = chart;
            chart.data.datasets.forEach((dataset, datasetIndex) => {
                const meta = chart.getDatasetMeta(datasetIndex);
                meta.data.forEach((bar, index) => {
                    const value = dataset.data[index];
                    const label = value.toFixed(3) + ' s';
                    
                    ctx.save();
                    ctx.fillStyle = '#000000';
                    ctx.font = 'bold 15px -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif';
                    ctx.textAlign = 'center';
                    ctx.textBaseline = 'bottom';
                    ctx.fillText(label, bar.x, bar.y - 8);
                    ctx.restore();
                });
            });
        }
    };

    new Chart(ctx, {
        type: 'bar',
        data: {
            labels: ['Virtual Machine', 'Docker Container'],
            datasets: [{
                data: [2.961, 7.246],
                backgroundColor: [
                    '#2563eb', // Vivid Blue
                    '#10b981'  // Vibrant Green
                ],
                borderRadius: 4,
                barPercentage: 0.5,
                categoryPercentage: 0.8
            }]
        },
        options: {
            responsive: true,
            maintainAspectRatio: false,
            plugins: {
                legend: {
                    display: false
                },
                tooltip: {
                    callbacks: {
                        label: function(context) {
                            return context.raw + ' seconds';
                        }
                    }
                }
            },
            scales: {
                x: {
                    grid: {
                        display: false
                    },
                    ticks: {
                        font: {
                            size: 14,
                            weight: '500'
                        },
                        color: '#333333'
                    }
                },
                y: {
                    beginAtZero: true,
                    max: 10,
                    ticks: {
                        stepSize: 2,
                        font: {
                            size: 13
                        },
                        color: '#666666'
                    },
                    grid: {
                        color: '#f0f0f0'
                    }
                }
            }
        },
        plugins: [topLabelsPlugin]
    });
</script>

</body>
</html>
