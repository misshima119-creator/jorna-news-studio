# jorna-news-studio

<!DOCTYPE html> 
<html lang="bn"> 
<head> 
    <meta charset="UTF-8"> 
    <meta name="viewport" content="width=device-width, initial-scale=1.0"> 
    <title>JORNA NEWS Studio Pro</title> 
    <style> 
        *{ margin:0; padding:0; box-sizing:border-box; font-family: 'Arial', sans-serif; } 
        body{ background:#0b0b0b; color:#fff; padding: 15px; display: flex; flex-direction: column; align-items: center; } 
        .header{ background:#c40000; padding:12px; text-align:center; font-size:20px; font-weight:bold; border-radius: 8px; width: 100%; max-width: 450px; margin-bottom: 10px; } 
        .studio{ width:100%; max-width:450px; } 
        
        .screen{ 
            position:relative; 
            width: 100%;
            height:580px; 
            background:#000; 
            border:3px solid #c40000; 
            border-radius:14px; 
            overflow:hidden; 
            box-shadow: 0px 5px 15px rgba(196, 0, 0, 0.3);
        } 
        #previewVideo{ width:100%; height:100%; object-fit:cover; } 
        
        .logo-container{ 
            position:absolute; 
            top:15px; 
            left:15px; 
            z-index:10; 
        } 
        .logo-circle {
            width: 60px;
            height: 60px;
            border-radius: 50%;
            background: #c40000;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 11px;
            font-weight: bold;
            border: 2px solid #fff;
            overflow: hidden;
            box-shadow: 0px 2px 5px rgba(0,0,0,0.5);
        }
        .logo-circle img {
            width: 100%;
            height: 100%;
            object-fit: cover;
        }
        
        .headline{ 
            position:absolute; 
            bottom:100px; 
            left:15px; 
            width:80%; 
            max-height:80px; 
            overflow:hidden; 
            background:rgba(0, 0, 0, 0.85); 
            padding:10px 14px; 
            font-size:16px; 
            font-weight:bold; 
            line-height:1.4; 
            border-left:5px solid #ff0000; 
            z-index:10; 
            border-radius:4px; 
        } 
        
        .caption-overlay {
            position: absolute;
            top: 45%;
            left: 5%;
            width: 90%;
            background: rgba(0, 0, 0, 0.7);
            color: #fff200; 
            padding: 10px;
            text-align: center;
            font-size: 19px;
            font-weight: bold;
            border-radius: 6px;
            z-index: 10;
            display: none;
            box-shadow: 0px 2px 8px rgba(0,0,0,0.5);
        }

        .breaking{ position:absolute; bottom:50px; left:0; width:100%; background:#ff0000; padding:10px; font-weight:bold; font-size:14px; z-index:10; letter-spacing: 0.5px; } 
        
        .ticker{ position:absolute; bottom:0; left:0; width:100%; background:#003cff; padding:10px; overflow:hidden; white-space:nowrap; z-index:10; height: 50px;} 
        .ticker span { 
            display: inline-block; 
            position: absolute;
            white-space: nowrap;
            font-size: 15px;
            font-weight: bold;
            color: #fff;
        } 
        
        .controls{ margin-top:15px; display:grid; gap:12px; background: #161616; padding: 15px; border-radius: 10px; border: 1px solid #333; } 
        .input-group { display: flex; flex-direction: column; gap: 5px; }
        .input-group label { font-size: 13px; color: #aaa; font-weight: bold; }
        input, textarea, button{ padding:12px; border:none; border-radius:8px; font-size:15px; width: 100%; background: #222; color: #fff; border: 1px solid #444; } 
        input:focus, textarea:focus { border-color: #c40000; outline: none; }
        textarea{ height:90px; resize:none; } 
        button{ background:#c40000; color:white; font-weight:bold; cursor:pointer; border: none; font-size: 16px; margin-top: 5px; transition: 0.2s; } 
        button:hover { background: #9e0000; }
        .recording-active { background: #00cc66 !important; }
    </style> 
</head> 
<body> 

    <div class="header"> JORNA NEWS SHORTS STUDIO </div> 
    
    <div class="studio"> 
        <div class="screen" id="newsScreen"> 
            <div class="logo-container"> 
                <div class="logo-circle" id="logoBox">LOGO</div>
            </div>
            
            <div class="headline"> <span id="headlineBox">এখানে আপনার হেডলাইন দেখাবে</span> </div> 
            
            <div class="caption-overlay" id="captionOverlay">ক্যাপশন...</div>

            <video id="previewVideo" loop playsinline muted></video> 
            
            <div class="breaking"> 🔴 BREAKING NEWS </div> 
            <div class="ticker"> <span id="tickerText">বাংলাদেশের সর্বশেষ সংবাদ আপডেট...</span> </div> 
        </div> 

        <div class="controls"> 
            <div class="input-group">
                <label>১. আপনার রিলস বা শর্টস ভিডিওটি আপলোড করুন:</label>
                <input type="file" id="videoInput" accept="video/*"> 
            </div>
            
            <div class="input-group">
                <label>২. আপনার চ্যানেলের গোল লোগো ছবি আপলোড করুন:</label>
                <input type="file" id="logoInput" accept="image/*"> 
            </div>

            <div class="input-group">
                <label>৩. মূল নিউজ হেডলাইন লিখুন (নিচে বামে থাকবে):</label>
                <input type="text" id="headlineInput" placeholder="এখানে হেডলাইন টাইপ করুন..." oninput="updateHeadline()"> 
            </div>

            <div class="input-group">
                <label>৪. স্ক্রল বা চলন্ত নিউজ টেক্সট লিখুন (একদম নিচে থাকবে):</label>
                <input type="text" id="tickerInput" placeholder="এখানে চলমান খবর লিখুন..." oninput="updateHeadline()"> 
            </div>
            
            <div class="input-group">
                <label>৫. ভিডিওর অটো-ক্যাপশন বা নিউজ স্ক্রিপ্ট লিখুন:</label>
                <textarea id="newsText" placeholder="এখানে আপনার খবর বা ক্যাপশনটি লিখুন..." oninput="processCaptions()"></textarea> 
            </div>
            
            <button id="recordBtn"> 🎥 স্টুডিও ফ্রেমসহ ভিডিও রেকর্ড করুন </button> 
        </div> 
    </div> 

    <canvas id="recordCanvas" width="720" height="1280" style="display:none;"></canvas>

<script> 
const video = document.getElementById('previewVideo');
const headlineBox = document.getElementById('headlineBox');
const tickerText = document.getElementById('tickerText');
const captionOverlay = document.getElementById('captionOverlay');
const canvas = document.getElementById('recordCanvas');
const ctx = canvas.getContext('2d');
const recordBtn = document.getElementById('recordBtn');

let mediaRecorder;
let recordedChunks = [];
let isRecording = false;
let animationFrameId;
let logoImage = null; 

let captionLines = [];
let currentCaptionText = "";

function updateHeadline(){ 
    let headline = document.getElementById("headlineInput").value; 
    let ticker = document.getElementById("tickerInput").value; 
    headlineBox.innerText = headline || "এখানে আপনার হেডলাইন দেখাবে"; 
    tickerText.innerText = ticker || "বাংলাদেশের সর্বশেষ সংবাদ আপডেট..."; 
} 

function processCaptions() {
    const fullText = document.getElementById("newsText").value.trim();
    if (!fullText) {
        captionLines = [];
        captionOverlay.style.display = "none";
        currentCaptionText = "";
        return;
    }
    
    captionOverlay.style.display = "block";
    const words = fullText.split(/\s+/);
    captionLines = [];
    let currentLine = [];
    
    words.forEach(word => {
        currentLine.push(word);
        if (currentLine.length >= 4) { 
            captionLines.push(currentLine.join(" "));
            currentLine = [];
        }
    });
    if (currentLine.length > 0) {
        captionLines.push(currentLine.join(" "));
    }
}

function updateCaptionTiming() {
    if (captionLines.length === 0 || !video.duration) return;
    let progress = video.currentTime / video.duration;
    let currentLineIndex = Math.floor(progress * captionLines.length);
    if (currentLineIndex >= captionLines.length) currentLineIndex = captionLines.length - 1;
    
    currentCaptionText = captionLines[currentLineIndex] || "";
    captionOverlay.innerText = currentCaptionText;
}

let tickerX = canvas.width;
function animateTicker() {
    tickerX -= 3.5; 
    ctx.font = "bold 26px Arial"; 
    if (tickerX < -ctx.measureText(tickerText.innerText).width - 200) {
        tickerX = canvas.width;
    }
    tickerText.style.transform = `translateX(${tickerX / 2.3}px)`; 
    
    updateCaptionTiming();
    drawToCanvas();
    
    if (isRecording) {
        animationFrameId = requestAnimationFrame(animateTicker);
    }
}

document.getElementById("logoInput").addEventListener("change", function(e) {
    let file = e.target.files[0];
    if (!file) return;
    logoImage = new Image();
    logoImage.src = URL.createObjectURL(file);
    logoImage.onload = function() {
        document.getElementById("logoBox").innerHTML = `<img src="${logoImage.src}">`;
        document.getElementById("logoBox").style.background = "transparent";
    }
});

document.getElementById("videoInput").addEventListener("change", function(e){ 
    let file = e.target.files[0]; 
    if(!file) return; 
    video.src = URL.createObjectURL(file); 
    video.muted = false; 
    
    video.onloadedmetadata = function() {
        video.play();
        processCaptions();
        tickerX = canvas.width;
        isRecording = true; 
        animateTicker();
        isRecording = false; 
    };
}); 

function drawToCanvas() {
    ctx.drawImage(video, 0, 0, canvas.width, canvas.height);
    
    if (logoImage) {
        ctx.save();
        ctx.beginPath();
        ctx.arc(70, 70, 45, 0, Math.PI * 2, true);
        ctx.closePath();
        ctx.clip();
        ctx.drawImage(logoImage, 25, 25, 90, 90);
        ctx.restore();
        
        ctx.beginPath();
        ctx.arc(70, 70, 45, 0, Math.PI * 2, true);
        ctx.strokeStyle = "#ffffff";
        ctx.lineWidth = 4;
        ctx.stroke();
    } else {
        ctx.save();
        ctx.beginPath();
        ctx.arc(70, 70, 45, 0, Math.PI * 2, true);
        ctx.fillStyle = "#c40000";
        ctx.fill();
        ctx.strokeStyle = "#ffffff";
        ctx.lineWidth = 4;
        ctx.stroke();
        ctx.fillStyle = "#ffffff";
        ctx.font = "bold 18px Arial";
        ctx.textAlign = "center";
        ctx.fillText("NEWS", 70, 76);
        ctx.restore();
    }
    
    if (currentCaptionText) {
        ctx.save();
        ctx.fillStyle = "rgba(0, 0, 0, 0.75)";
        ctx.font = "bold 32px Arial";
        let textWidth = ctx.measureText(currentCaptionText).width;
        ctx.fillRect((canvas.width - textWidth - 40) / 2, canvas.height / 2 - 40, textWidth + 40, 75);
        ctx.fillStyle = "#fff200"; 
        ctx.textAlign = "center";
        ctx.fillText(currentCaptionText, canvas.width / 2, canvas.height / 2 + 10);
        ctx.restore();
    }
    
    ctx.save();
    ctx.fillStyle = "rgba(0, 0, 0, 0.85)";
    ctx.fillRect(35, canvas.height - 310, canvas.width * 0.78, 110);
    ctx.fillStyle = "#ff0000";
    ctx.fillRect(35, canvas.height - 310, 10, 110);
    ctx.fillStyle = "#ffffff";
    ctx.font = "bold 28px Arial";
    ctx.fillText(headlineBox.innerText, 65, canvas.height - 245);
    ctx.restore();
    
    ctx.fillStyle = "#ff0000";
    ctx.fillRect(0, canvas.height - 170, canvas.width, 65);
    ctx.fillStyle = "#ffffff";
    ctx.font = "bold 26px Arial";
    ctx.fillText("🔴 BREAKING NEWS", 35, canvas.height - 128);
    
    ctx.fillStyle = "#003cff";
    ctx.fillRect(0, canvas.height - 105, canvas.width, 105);
    ctx.fillStyle = "#ffffff";
    ctx.font = "bold 28px Arial";
    ctx.fillText(tickerText.innerText, tickerX, canvas.height - 45);
}

recordBtn.addEventListener("click", () => {
    if (!isRecording) {
        recordedChunks = [];
        isRecording = true;
        video.currentTime = 0;
        video.play();
        const videoStream = canvas.captureStream(30);
        
        if (video.captureStream || video.mozCaptureStream) {
            const audioStream = video.captureStream ? video.captureStream() : video.mozCaptureStream();
            const audioTracks = audioStream.getAudioTracks();
            if (audioTracks.length > 0) { videoStream.addTrack(audioTracks[0]); }
        }

        let options = { mimeType: 'video/webm;codecs=vp9,opus', videoBitsPerSecond: 6000000 };
        if (!MediaRecorder.isTypeSupported(options.mimeType)) { options = { mimeType: 'video/mp4', videoBitsPerSecond: 6000000 }; }

        try { mediaRecorder = new MediaRecorder(videoStream, options); } catch (e) { mediaRecorder = new MediaRecorder(videoStream); }
        mediaRecorder.ondataavailable = (e) => { if (e.data.size > 0) recordedChunks.push(e.data); };

        mediaRecorder.onstop = () => {
            const blob = new Blob(recordedChunks, { type: "video/mp4" });
            const url = URL.createObjectURL(blob);
            
            let outputVideo = document.getElementById('outputVideo');
            if(!outputVideo) {
                outputVideo = document.createElement('video');
                outputVideo.id = 'outputVideo';
                outputVideo.controls = true;
                outputVideo.style.width = '100%';
                outputVideo.style.marginTop = '15px';
                outputVideo.style.border = '3px solid #00cc66';
                outputVideo.style.borderRadius = '10px';
                document.querySelector('.controls').appendChild(outputVideo);
            }
            outputVideo.src = url;
            
            const a = document.createElement("a");
            a.href = url;
            a.download = "JORNA_NEWS_24_REELS.mp4";
            document.body.appendChild(a);
            a.click();
            document.body.removeChild(a);
            alert("ভিডিওটি ফ্রেম ও সাউন্ডসহ এইচডি কোয়ালিটিতে তৈরি হয়েছে!");
        };

        animateTicker();
        mediaRecorder.start();
        recordBtn.innerText = "🛑 রিলস ডাউনলোড করুন";
        recordBtn.classList.add("recording-active");
    } else {
        isRecording = false;
        mediaRecorder.stop();
        cancelAnimationFrame(animationFrameId);
        recordBtn.innerText = "🎥 স্টুডিও ফ্রেমসহ ভিডিও রেকর্ড করুন";
        recordBtn.classList.remove("recording-active");
    }
});
</script> 
</body> 
</html>
