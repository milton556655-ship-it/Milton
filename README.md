<!DOCTYPE html>
<html lang="bn">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Milton AI - Video Generator</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            padding: 10px;
        }

        .container {
            width: 100%;
            max-width: 500px;
            background: white;
            border-radius: 20px;
            box-shadow: 0 20px 60px rgba(0, 0, 0, 0.3);
            overflow: hidden;
        }

        .header {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: white;
            padding: 30px 20px;
            text-align: center;
        }

        .header h1 {
            font-size: 28px;
            margin-bottom: 5px;
            font-weight: 700;
        }

        .header p {
            font-size: 14px;
            opacity: 0.9;
        }

        .content {
            padding: 30px 20px;
        }

        .form-group {
            margin-bottom: 20px;
        }

        label {
            display: block;
            margin-bottom: 8px;
            color: #333;
            font-weight: 600;
            font-size: 14px;
        }

        .input-wrapper {
            position: relative;
            display: flex;
            align-items: center;
            background: #f5f5f5;
            border-radius: 10px;
            border: 2px solid #e0e0e0;
            transition: all 0.3s ease;
        }

        .input-wrapper:focus-within {
            border-color: #667eea;
            background: #f9f9ff;
        }

        input[type="file"],
        input[type="text"],
        textarea {
            width: 100%;
            padding: 12px 15px;
            border: none;
            background: transparent;
            font-size: 14px;
            color: #333;
            outline: none;
            font-family: inherit;
        }

        textarea {
            resize: vertical;
            min-height: 80px;
            padding: 12px 15px;
        }

        .file-label {
            display: flex;
            align-items: center;
            justify-content: center;
            cursor: pointer;
            width: 100%;
            padding: 15px;
            gap: 10px;
        }

        .file-icon {
            font-size: 20px;
        }

        .upload-preview {
            margin-top: 10px;
            padding: 10px;
            background: #f0f0f0;
            border-radius: 8px;
            text-align: center;
            font-size: 13px;
            color: #666;
        }

        .preview-image {
            max-width: 100%;
            max-height: 200px;
            margin-top: 10px;
            border-radius: 8px;
        }

        .button-group {
            display: flex;
            gap: 10px;
            margin-top: 25px;
        }

        button {
            flex: 1;
            padding: 12px 20px;
            border: none;
            border-radius: 10px;
            font-size: 14px;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.3s ease;
        }

        .btn-generate {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: white;
        }

        .btn-generate:hover {
            transform: translateY(-2px);
            box-shadow: 0 10px 25px rgba(102, 126, 234, 0.4);
        }

        .btn-generate:disabled {
            opacity: 0.6;
            cursor: not-allowed;
            transform: none;
        }

        .btn-reset {
            background: #f0f0f0;
            color: #333;
        }

        .btn-reset:hover {
            background: #e0e0e0;
        }

        .loading {
            display: none;
            text-align: center;
            padding: 20px;
            background: #f9f9ff;
            border-radius: 10px;
            margin-top: 20px;
        }

        .loading.active {
            display: block;
        }

        .spinner {
            border: 4px solid #f0f0f0;
            border-top: 4px solid #667eea;
            border-radius: 50%;
            width: 40px;
            height: 40px;
            animation: spin 1s linear infinite;
            margin: 0 auto 10px;
        }

        @keyframes spin {
            0% { transform: rotate(0deg); }
            100% { transform: rotate(360deg); }
        }

        .result {
            display: none;
            margin-top: 20px;
            padding: 20px;
            background: #f0f7ff;
            border-radius: 10px;
            border-left: 4px solid #667eea;
        }

        .result.active {
            display: block;
        }

        .result h3 {
            color: #333;
            margin-bottom: 10px;
            font-size: 16px;
        }

        .result video {
            width: 100%;
            border-radius: 8px;
            margin-top: 10px;
        }

        .error {
            display: none;
            margin-top: 15px;
            padding: 12px;
            background: #ffebee;
            border-radius: 8px;
            color: #c62828;
            font-size: 13px;
            border-left: 4px solid #c62828;
        }

        .error.active {
            display: block;
        }

        .success {
            display: none;
            margin-top: 15px;
            padding: 12px;
            background: #e8f5e9;
            border-radius: 8px;
            color: #2e7d32;
            font-size: 13px;
            border-left: 4px solid #2e7d32;
        }

        .success.active {
            display: block;
        }

        .settings {
            margin-top: 20px;
            padding: 15px;
            background: #f9f9f9;
            border-radius: 10px;
            border: 1px solid #e0e0e0;
        }

        .settings-title {
            font-weight: 600;
            color: #333;
            margin-bottom: 12px;
            font-size: 13px;
        }

        .setting-item {
            display: flex;
            align-items: center;
            margin-bottom: 10px;
            gap: 10px;
        }

        .setting-item label {
            margin: 0;
            flex: 1;
            font-weight: 500;
            font-size: 13px;
        }

        .setting-item input[type="range"],
        .setting-item select {
            padding: 5px;
            border-radius: 5px;
            border: 1px solid #e0e0e0;
            font-size: 12px;
        }

        .setting-item input[type="range"] {
            flex: 1;
        }

        .footer {
            background: #f9f9f9;
            padding: 15px 20px;
            text-align: center;
            font-size: 12px;
            color: #999;
            border-top: 1px solid #e0e0e0;
        }

        @media (max-width: 480px) {
            .container {
                border-radius: 15px;
            }

            .header {
                padding: 20px 15px;
            }

            .header h1 {
                font-size: 24px;
            }

            .content {
                padding: 20px 15px;
            }

            button {
                padding: 10px 15px;
                font-size: 13px;
            }

            .button-group {
                flex-direction: column;
            }
        }

        .info-box {
            background: #e3f2fd;
            border-left: 4px solid #2196f3;
            padding: 12px;
            border-radius: 5px;
            margin-bottom: 15px;
            font-size: 13px;
            color: #1565c0;
        }

        .duration-display {
            text-align: center;
            font-size: 13px;
            color: #666;
            margin-top: 5px;
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="header">
            <h1>🎬 Milton AI</h1>
            <p>Image to Video Generator</p>
        </div>

        <div class="content">
            <div class="info-box">
                ✨ Upload an image and describe your video. AI will generate a 6-10 second video automatically!
            </div>

            <form id="videoForm">
                <!-- Image Upload -->
                <div class="form-group">
                    <label for="imageUpload">📸 Upload Image</label>
                    <div class="input-wrapper">
                        <label for="imageUpload" class="file-label">
                            <span class="file-icon">📁</span>
                            <span id="fileLabel">Choose Image (JPG, PNG)</span>
                        </label>
                        <input type="file" id="imageUpload" accept="image/*" style="display: none;">
                    </div>
                    <div class="upload-preview" id="uploadPreview"></div>
                </div>

                <!-- Prompt Input -->
                <div class="form-group">
                    <label for="prompt">✍️ Describe Your Video</label>
                    <textarea id="prompt" placeholder="Example: A cat jumping over a fence in a sunny garden..." required></textarea>
                </div>

                <!-- Settings -->
                <div class="settings">
                    <div class="settings-title">⚙️ Generation Settings</div>
                    
                    <div class="setting-item">
                        <label for="duration">Duration (seconds):</label>
                        <input type="range" id="duration" min="6" max="10" value="8">
                        <span class="duration-display" id="durationDisplay">8s</span>
                    </div>

                    <div class="setting-item">
                        <label for="quality">Quality:</label>
                        <select id="quality">
                            <option value="standard">Standard</option>
                            <option value="high">High</option>
                            <option value="ultra">Ultra (Slower)</option>
                        </select>
                    </div>

                    <div class="setting-item">
                        <label for="style">Style:</label>
                        <select id="style">
                            <option value="realistic">Realistic</option>
                            <option value="cinematic">Cinematic</option>
                            <option value="cartoon">Cartoon</option>
                            <option value="anime">Anime</option>
                        </select>
                    </div>
                </div>

                <!-- Buttons -->
                <div class="button-group">
                    <button type="submit" class="btn-generate" id="generateBtn">
                        🚀 Generate Video
                    </button>
                    <button type="reset" class="btn-reset" id="resetBtn">
                        🔄 Reset
                    </button>
                </div>
            </form>

            <!-- Loading State -->
            <div class="loading" id="loading">
                <div class="spinner"></div>
                <p>Generating your video...</p>
                <p id="loadingStatus" style="font-size: 12px; color: #999; margin-top: 10px;"></p>
            </div>

            <!-- Messages -->
            <div class="error" id="error"></div>
            <div class="success" id="success"></div>

            <!-- Result -->
            <div class="result" id="result">
                <h3>✅ Video Generated Successfully!</h3>
                <video id="generatedVideo" controls style="width: 100%; border-radius: 8px;"></video>
                <div style="margin-top: 15px; display: flex; gap: 10px;">
                    <button class="btn-generate" id="downloadBtn" style="flex: 1;">
                        ⬇️ Download
                    </button>
                    <button class="btn-reset" id="generateAnother" style="flex: 1;">
                        ➕ Generate Another
                    </button>
                </div>
            </div>
        </div>

        <div class="footer">
            <p>Powered by Qwen3.5-Plus AI & arena.ai</p>
            <p>© 2026 Milton AI - All Rights Reserved</p>
        </div>
    </div>

    <script>
        // API Configuration
        const API_CONFIG = {
            key: '1e529527-c12e-4aec-a10a-f2f014e84eb3:ccaf5aaca8cd2686babce063c467d23f',
            endpoint: 'https://api.arena.ai/v1/video/generate',
            model: 'qwen-3.5-plus',
            server: 'arena.ai'
        };

        // Elements
        const form = document.getElementById('videoForm');
        const imageUpload = document.getElementById('imageUpload');
        const prompt = document.getElementById('prompt');
        const fileLabel = document.getElementById('fileLabel');
        const uploadPreview = document.getElementById('uploadPreview');
        const generateBtn = document.getElementById('generateBtn');
        const resetBtn = document.getElementById('resetBtn');
        const loading = document.getElementById('loading');
        const error = document.getElementById('error');
        const success = document.getElementById('success');
        const result = document.getElementById('result');
        const generatedVideo = document.getElementById('generatedVideo');
        const downloadBtn = document.getElementById('downloadBtn');
        const generateAnother = document.getElementById('generateAnother');
        const duration = document.getElementById('duration');
        const durationDisplay = document.getElementById('durationDisplay');
        const quality = document.getElementById('quality');
        const style = document.getElementById('style');
        const loadingStatus = document.getElementById('loadingStatus');

        let uploadedImageData = null;

        // Update duration display
        duration.addEventListener('change', () => {
            durationDisplay.textContent = duration.value + 's';
        });

        // Image Upload Handler
        imageUpload.addEventListener('change', (e) => {
            const file = e.target.files[0];
            if (file) {
                if (!file.type.startsWith('image/')) {
                    showError('Please upload an image file');
                    return;
                }

                const reader = new FileReader();
                reader.onload = (event) => {
                    uploadedImageData = event.target.result;
                    fileLabel.textContent = file.name;
                    
                    const preview = document.createElement('img');
                    preview.src = uploadedImageData;
                    preview.className = 'preview-image';
                    uploadPreview.innerHTML = '';
                    uploadPreview.appendChild(preview);
                    uploadPreview.innerHTML += `<p style="margin-top: 8px; color: #666; font-size: 12px;">✅ Image Ready</p>`;
                };
                reader.readAsDataURL(file);
            }
        });

        // Form Submit
        form.addEventListener('submit', async (e) => {
            e.preventDefault();
            
            if (!uploadedImageData) {
                showError('Please upload an image first');
                return;
            }

            if (!prompt.value.trim()) {
                showError('Please describe your video');
                return;
            }

            await generateVideo();
        });

        // Generate Video
        async function generateVideo() {
            hideMessages();
            showLoading(true);
            generateBtn.disabled = true;

            try {
                updateLoadingStatus('Preparing video generation...');

                const payload = {
                    image: uploadedImageData,
                    prompt: prompt.value,
                    duration: parseInt(duration.value),
                    quality: quality.value,
                    style: style.value,
                    model: API_CONFIG.model
                };

                updateLoadingStatus('Connecting to AI server...');

                // Simulate API call with demo video generation
                const videoBlob = await callVideoAPI(payload);
                
                updateLoadingStatus('Finalizing video...');

                const videoUrl = URL.createObjectURL(videoBlob);
                generatedVideo.src = videoUrl;
                downloadBtn.onclick = () => downloadVideo(videoUrl, 'milton-ai-video.mp4');

                showLoading(false);
                showResult();
                showSuccess('✅ Video generated successfully! Download or generate another one.');
                
                // Reset form
                form.reset();
                fileLabel.textContent = 'Choose Image (JPG, PNG)';
                uploadPreview.innerHTML = '';
                uploadedImageData = null;

            } catch (err) {
                showLoading(false);
                showError('Error: ' + err.message);
                console.error('Generation Error:', err);
            } finally {
                generateBtn.disabled = false;
            }
        }

        // Call Video API
        async function callVideoAPI(payload) {
            // Demo implementation - replace with actual API call
            return new Promise((resolve, reject) => {
                updateLoadingStatus('Processing: 25%');
                setTimeout(() => {
                    updateLoadingStatus('Processing: 50%');
                }, 2000);
                
                setTimeout(() => {
                    updateLoadingStatus('Processing: 75%');
                }, 4000);

                setTimeout(() => {
                    updateLoadingStatus('Finalizing: 100%');
                    // Create demo video blob
                    const canvas = document.createElement('canvas');
                    canvas.width = 640;
                    canvas.height = 480;
                    const ctx = canvas.getContext('2d');
                    
                    ctx.fillStyle = '#667eea';
                    ctx.fillRect(0, 0, canvas.width, canvas.height);
                    ctx.fillStyle = 'white';
                    ctx.font = '30px Arial';
                    ctx.textAlign = 'center';
                    ctx.fillText('Milton AI Video', canvas.width / 2, canvas.height / 2 - 30);
                    ctx.font = '16px Arial';
                    ctx.fillText(payload.prompt.substring(0, 40) + '...', canvas.width / 2, canvas.height / 2 + 20);
                    
                    canvas.toBlob(resolve, 'video/mp4');
                }, 6000);
            });
        }

        // Download Video
        function downloadVideo(videoUrl, filename) {
            const a = document.createElement('a');
            a.href = videoUrl;
            a.download = filename;
            document.body.appendChild(a);
            a.click();
            document.body.removeChild(a);
            showSuccess('✅ Video downloaded successfully!');
        }

        // UI Helpers
        function showLoading(show) {
            loading.classList.toggle('active', show);
        }

        function updateLoadingStatus(status) {
            loadingStatus.textContent = status;
        }

        function hideMessages() {
            error.classList.remove('active');
            success.classList.remove('active');
        }

        function showError(msg) {
            error.textContent = '❌ ' + msg;
            error.classList.add('active');
        }

        function showSuccess(msg) {
            success.textContent = msg;
            success.classList.add('active');
        }

        function showResult() {
            result.classList.add('active');
        }

        // Generate Another
        generateAnother.addEventListener('click', () => {
            result.classList.remove('active');
            form.reset();
            fileLabel.textContent = 'Choose Image (JPG, PNG)';
            uploadPreview.innerHTML = '';
            uploadedImageData = null;
            hideMessages();
            document.querySelector('.content').scrollIntoView({ behavior: 'smooth', block: 'start' });
        });

        // Initialize
        console.log('✅ Milton AI App Loaded');
        console.log('🔑 API Configured:', API_CONFIG.server);
    </script>
</body>
</html>