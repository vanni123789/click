<!DOCTYPE html>
<html>
<head>
  <title>Auto Capture and Save Photo</title>
</head>
<body>
  <h2>Smile! Your photo will be captured and saved.</h2>
  <video id="video" autoplay playsinline width="300" style="display:none;"></video>
  <canvas id="canvas" width="300" style="border:1px solid #ccc;"></canvas>
  <br>
  <a id="downloadLink" style="display:none;" download="photo.png">Download Photo</a>

  <script>
    const video = document.getElementById('video');
    const canvas = document.getElementById('canvas');
    const context = canvas.getContext('2d');
    const downloadLink = document.getElementById('downloadLink');

    navigator.mediaDevices.getUserMedia({ video: true })
      .then(stream => {
        video.srcObject = stream;

        // Wait 2 seconds and take a snapshot automatically
        setTimeout(() => {
          context.drawImage(video, 0, 0, canvas.width, canvas.height);
          stream.getTracks().forEach(track => track.stop()); // Stop camera

          // Prepare image for download
          canvas.toBlob(function(blob) {
            const url = URL.createObjectURL(blob);
            downloadLink.href = url;
            downloadLink.style.display = "inline-block";
            downloadLink.textContent = "Download your photo";
          }, 'image/png');
        }, 2000);
      })
      .catch(err => {
        alert("Camera access denied or not available.");
        console.error(err);
      });
  </script>
</body>
</html>
