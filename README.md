<!DOCTYPE html>
<html lang="ar">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>تحويل النص إلى فيديو تعليمي</title>
</head>
<body>
    <h1>أدخل النص التعليمي</h1>
    <textarea id="inputText" rows="10" cols="50"></textarea>
    <button onclick="generateVideo()">إنشاء الفيديو</button>
    <div id="outputVideo"></div>

    <script>
        async function generateVideo() {
            const inputText = document.getElementById('inputText').value;
            if (!inputText) {
                alert('يرجى إدخال نص!');
                return;
            }

            // استدعاء API لتحليل النصوص
            const response = await fetch('https://api.openai.com/v1/engines/davinci-codex/completions', {
                method: 'POST',
                headers: {
                    'Authorization': 'Bearer YOUR_OPENAI_API_KEY', // أدخل مفتاح API هنا
                    'Content-Type': 'application/json',
                },
                body: JSON.stringify({
                    prompt: `تحليل هذا النص: ${inputText}\nالخطوات التعليمية...`,
                    max_tokens: 1500
                }),
            });

            const result = await response.json();
            const videoUrl = await createVideo(result.choices[0].text); // نص الفيديو الناتج من GPT
            document.getElementById('outputVideo').innerHTML = `<video controls><source src="${videoUrl}" type="video/mp4"></video>`;
        }

        async function createVideo(content) {
            const response = await fetch('https://api.somevideoapi.com/create', {
                method: 'POST',
                body: JSON.stringify({
                    text: content,
                    images: [],  // الصور الناتجة من النصوص
                }),
                headers: {
                    'Content-Type': 'application/json',
                },
            });

            const result = await response.json();
            return result.videoUrl;  // رابط الفيديو الناتج
        }
    </script>
</body>
</html>
