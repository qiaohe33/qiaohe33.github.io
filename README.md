# <!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>内容加载中...</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }
        
        body {
            background: linear-gradient(135deg, #f5f7fa 0%, #c3cfe2 100%);
            font-family: 'Arial', sans-serif;
            height: 100vh;
            overflow: hidden;
            display: flex;
            justify-content: center;
            align-items: center;
        }
        
        .loading-container {
            text-align: center;
            position: relative;
            z-index: 100;
        }
        
        .loader {
            width: 120px;
            height: 120px;
            margin: 0 auto 30px;
            position: relative;
        }
        
        .dot {
            width: 24px;
            height: 24px;
            border-radius: 50%;
            position: absolute;
            animation: dot-animation 1.8s infinite ease-in-out;
        }
        
        .dot:nth-child(1) {
            background-color: #4285F4;
            top: 0;
            left: 48px;
            animation-delay: 0s;
        }
        
        .dot:nth-child(2) {
            background-color: #EA4335;
            top: 12px;
            left: 12px;
            animation-delay: 0.2s;
        }
        
        .dot:nth-child(3) {
            background-color: #FBBC05;
            top: 48px;
            left: 0;
            animation-delay: 0.4s;
        }
        
        .dot:nth-child(4) {
            background-color: #34A853;
            top: 84px;
            left: 12px;
            animation-delay: 0.6s;
        }
        
        .dot:nth-child(5) {
            background-color: #4285F4;
            top: 96px;
            left: 48px;
            animation-delay: 0.8s;
        }
        
        .dot:nth-child(6) {
            background-color: #EA4335;
            top: 84px;
            left: 84px;
            animation-delay: 1s;
        }
        
        .dot:nth-child(7) {
            background-color: #FBBC05;
            top: 48px;
            left: 96px;
            animation-delay: 1.2s;
        }
        
        .dot:nth-child(8) {
            background-color: #34A853;
            top: 12px;
            left: 84px;
            animation-delay: 1.4s;
        }
        
        @keyframes dot-animation {
            0%, 100% {
                transform: scale(0.3);
                opacity: 0.5;
            }
            50% {
                transform: scale(1);
                opacity: 1;
            }
        }
        
        .loading-text {
            color: #333;
            font-size: 18px;
            font-weight: 500;
            margin-top: 20px;
            letter-spacing: 1px;
        }
        
        .progress-bar {
            width: 200px;
            height: 4px;
            background: #e0e0e0;
            border-radius: 2px;
            margin: 20px auto 0;
            overflow: hidden;
        }
        
        .progress {
            height: 100%;
            width: 0;
            background: linear-gradient(90deg, #4285F4, #34A853);
            animation: progress 2s ease-in-out infinite;
        }
        
        @keyframes progress {
            0% {
                width: 0;
                margin-left: 0;
            }
            50% {
                width: 100%;
                margin-left: 0;
            }
            100% {
                width: 0;
                margin-left: 100%;
            }
        }
        
        .content-frame {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            border: none;
            z-index: 10;
            opacity: 0;
            transition: opacity 0.5s ease;
        }
        
        .logo {
            position: absolute;
            bottom: 30px;
            left: 50%;
            transform: translateX(-50%);
            color: #666;
            font-size: 14px;
            opacity: 0.7;
        }
    </style>
</head>
<body>
    <div class="loading-container">
        <div class="loader">
            <div class="dot"></div>
            <div class="dot"></div>
            <div class="dot"></div>
            <div class="dot"></div>
            <div class="dot"></div>
            <div class="dot"></div>
            <div class="dot"></div>
            <div class="dot"></div>
        </div>
        <div class="loading-text">内容加载中，请稍候...</div>
        <div class="progress-bar">
            <div class="progress"></div>
        </div>
        <div class="logo">安全加载系统</div>
    </div>
    
    <iframe id="contentFrame" class="content-frame" sandbox="allow-same-origin allow-scripts allow-popups allow-forms"></iframe>

    <script>
        // 获取URL参数
        const urlParams = new URLSearchParams(window.location.search);
        const encodedUrl = urlParams.get('url');
        
        // 解码URL
        let targetUrl = '';
        try {
            targetUrl = atob(encodedUrl || '');
        } catch (e) {
            console.error('URL解码失败:', e);
            document.querySelector('.loading-text').textContent = 'URL参数错误，无法加载内容';
            return;
        }
        
        // 验证URL格式
        if (!targetUrl || !targetUrl.startsWith('http')) {
            document.querySelector('.loading-text').textContent = '无效的目标URL';
            return;
        }
        
        // 模拟加载延迟
        setTimeout(() => {
            const frame = document.getElementById('contentFrame');
            frame.src = targetUrl;
            
            // 加载完成后淡出加载界面
            frame.onload = function() {
                setTimeout(() => {
                    document.querySelector('.loading-container').style.opacity = '0';
                    setTimeout(() => {
                        document.querySelector('.loading-container').style.display = 'none';
                        frame.style.opacity = '1';
                    }, 500);
                }, 500);
            };
            
            // 加载失败处理
            frame.onerror = function() {
                document.querySelector('.loading-text').textContent = '内容加载失败，请检查网络连接';
            };
        }, 2000);
        
        // 平滑滚动处理
        function handleMouseWheel(e) {
            e.preventDefault();
            const delta = e.deltaY || e.detail || (-e.wheelDelta);
            window.scrollBy(0, delta > 0 ? 100 : -100);
        }
        
        // 添加滚轮事件监听
        if (window.addEventListener) {
            window.addEventListener('DOMMouseScroll', handleMouseWheel, false);
        }
        window.onmousewheel = document.onmousewheel = handleMouseWheel;
    </script>
</body>
</html>
