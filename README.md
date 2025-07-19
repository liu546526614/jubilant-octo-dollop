<!DOCTYPE html>
<html lang="zh-CN">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>爱心跳动动画</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <link href="https://cdn.jsdelivr.net/npm/font-awesome@4.7.0/css/font-awesome.min.css" rel="stylesheet">
  
  <script>
    tailwind.config = {
      theme: {
        extend: {
          colors: {
            primary: '#FF4D6D',
            secondary: '#FF8FA3',
            dark: '#C9184A',
            light: '#FFB3C1',
          },
        },
      }
    }
  </script>
  
  <style type="text/tailwindcss">
    @layer utilities {
      .heart-beat {
        animation: heartBeat 1.5s ease-in-out infinite;
      }
      .heart-beat-fast {
        animation: heartBeat 1s ease-in-out infinite;
      }
      .heart-beat-slow {
        animation: heartBeat 2s ease-in-out infinite;
      }
      .float {
        animation: float 6s ease-in-out infinite;
      }
    }
    
    @keyframes heartBeat {
      0%, 100% { transform: scale(1); }
      14% { transform: scale(1.2); }
      28% { transform: scale(1); }
      42% { transform: scale(1.2); }
      70% { transform: scale(1); }
    }
    
    @keyframes float {
      0%, 100% { transform: translateY(0) rotate(0deg); }
      50% { transform: translateY(-20px) rotate(5deg); }
    }
  </style>
</head>
<body class="bg-gradient-to-br from-pink-50 to-purple-100 min-h-screen overflow-hidden m-0 p-0">
  <div id="hearts-container" class="fixed inset-0 pointer-events-none z-10"></div>
  
  <div class="fixed inset-0 flex flex-col items-center justify-center p-4 z-0">
    <h1 class="text-[clamp(2rem,5vw,4rem)] font-bold text-transparent bg-clip-text bg-gradient-to-r from-primary to-dark mb-4 text-center">爱心跳动</h1>
    <p class="text-[clamp(1rem,2vw,1.25rem)] text-gray-700 mb-8 text-center max-w-xl">点击屏幕任何地方，添加更多爱心</p>
    <div class="flex flex-wrap gap-4 justify-center">
      <button id="add-heart-btn" class="bg-primary hover:bg-dark text-white font-bold py-3 px-6 rounded-full transition-all duration-300 shadow-lg hover:shadow-xl transform hover:-translate-y-1 flex items-center">
        <i class="fa fa-heart mr-2"></i> 添加爱心
      </button>
      <button id="change-color-btn" class="bg-secondary hover:bg-primary text-white font-bold py-3 px-6 rounded-full transition-all duration-300 shadow-lg hover:shadow-xl transform hover:-translate-y-1 flex items-center">
        <i class="fa fa-paint-brush mr-2"></i> 改变颜色
      </button>
    </div>
  </div>
  
  <script>
    // 爱心颜色集合
    const heartColors = ['#FF4D6D', '#FF8FA3', '#C9184A', '#FFB3C1', '#FF5C8A', '#FF758F', '#FF9B9B'];
    let currentColorIndex = 0;
    
    // 初始化时创建一些爱心
    for (let i = 0; i < 50; i++) {
      setTimeout(() => createHeart(), i * 100);
    }
    
    // 点击屏幕添加爱心
    document.addEventListener('click', (e) => {
      createHeart(e.clientX, e.clientY);
    });
    
    // 添加爱心按钮
    document.getElementById('add-heart-btn').addEventListener('click', () => {
      for (let i = 0; i < 10; i++) {
        setTimeout(() => createHeart(), i * 100);
      }
    });
    
    // 改变颜色按钮
    document.getElementById('change-color-btn').addEventListener('click', () => {
      currentColorIndex = (currentColorIndex + 1) % 3;
      const hearts = document.querySelectorAll('.heart');
      hearts.forEach(heart => {
        const colorSet = getColorSet(currentColorIndex);
        heart.style.color = colorSet[Math.floor(Math.random() * colorSet.length)];
      });
    });
    
    // 创建爱心元素
    function createHeart(x, y) {
      const container = document.getElementById('hearts-container');
      const heart = document.createElement('div');
      
      // 获取颜色集合
      const colorSet = getColorSet(currentColorIndex);
      const color = colorSet[Math.floor(Math.random() * colorSet.length)];
      
      // 设置位置（如果没有提供位置，则随机生成）
      if (x && y) {
        heart.style.left = `${x}px`;
        heart.style.top = `${y}px`;
      } else {
        heart.style.left = `${Math.random() * 100}vw`;
        heart.style.top = `${Math.random() * 100}vh`;
      }
      
      // 设置样式
      heart.className = 'heart absolute text-[clamp(1rem,3vw,2.5rem)] pointer-events-none';
      heart.innerHTML = '<i class="fa fa-heart"></i>';
      heart.style.color = color;
      heart.style.opacity = (Math.random() * 0.7 + 0.3).toString();
      heart.style.transform = `scale(${Math.random() * 0.5 + 0.5})`;
      
      // 添加动画类
      const animations = ['heart-beat', 'heart-beat-fast', 'heart-beat-slow'];
      heart.classList.add(animations[Math.floor(Math.random() * animations.length)]);
      
      // 添加浮动效果
      if (Math.random() > 0.5) {
        heart.classList.add('float');
        heart.style.animationDelay = `${Math.random() * 2}s`;
      }
      
      container.appendChild(heart);
      
      // 一段时间后移除爱心
      setTimeout(() => {
        heart.style.transition = 'opacity 1s ease-out';
        heart.style.opacity = '0';
        setTimeout(() => heart.remove(), 1000);
      }, 8000 + Math.random() * 4000);
    }
    
    // 根据索引获取颜色集合
    function getColorSet(index) {
      const colorSets = [
        ['#FF4D6D', '#FF8FA3', '#C9184A', '#FFB3C1', '#FF5C8A', '#FF758F', '#FF9B9B'], // 粉色系
        ['#FF9F1C', '#FFBF69', '#FFC300', '#FFDD00', '#FFE569', '#FFF3B0', '#FFEE93'], // 黄色系
        ['#3A86FF', '#8338EC', '#9D4EDD', '#C77DFF', '#E0AAFF', '#BD93F9', '#B5EAEA']  // 蓝色系
      ];
      return colorSets[index % colorSets.length];
    }
  </script>
</body>
</html>
    
