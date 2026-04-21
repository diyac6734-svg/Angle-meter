<!DOCTYPE html>
<html>
<head>
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Angle Meter</title>

<style>
body { text-align:center; font-family:Arial; background:#111; color:white; }
.angle { font-size:60px; margin:20px; }
button { padding:12px; margin:10px; font-size:16px; }
</style>
</head>

<body>

<h1>Angle Meter</h1>

<div class="angle" id="angle">0°</div>

<p>X: <span id="x">0</span> | Y: <span id="y">0</span> | Z: <span id="z">0</span></p>

<button onclick="start()">Start Sensor</button>
<button onclick="calibrate()">Calibrate</button>

<script>
let offset = 0;

function start() {
    if (typeof DeviceOrientationEvent.requestPermission === "function") {
        DeviceOrientationEvent.requestPermission()
        .then(response => {
            if (response === "granted") {
                window.addEventListener("deviceorientation", update);
            } else {
                alert("Permission denied");
            }
        });
    } else {
        window.addEventListener("deviceorientation", update);
    }
}

function update(e) {
    let x = e.beta || 0;
    let y = e.gamma || 0;
    let z = e.alpha || 0;

    let angle = x - offset;

    document.getElementById("angle").innerText = angle.toFixed(1) + "°";
    document.getElementById("x").innerText = x.toFixed(1);
    document.getElementById("y").innerText = y.toFixed(1);
    document.getElementById("z").innerText = z.toFixed(1);
}

function calibrate() {
    offset = parseFloat(document.getElementById("x").innerText);
}
</script>

</body>
</html>
