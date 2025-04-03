<!DOCTYPE html>
<html lang="zh-TW">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>玩家提交的地圖 - ISCG</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 0;
            padding: 0;
            background-color: #f4f4f4;
            text-align: center;
        }
        header {
            background-color: #333;
            color: white;
            padding: 15px 0;
            font-size: 1.5em;
        }
        #maps-container {
            display: flex;
            flex-wrap: wrap;
            justify-content: center;
            padding: 20px;
        }
        .map-card {
            background: white;
            padding: 15px;
            margin: 10px;
            border-radius: 8px;
            box-shadow: 2px 2px 10px rgba(0, 0, 0, 0.1);
            width: 300px;
            text-align: left;
        }
        .map-card img {
            width: 100%;
            border-radius: 5px;
        }
        .map-card h3 {
            margin-top: 0;
        }
    </style>
</head>
<body>
    <header>
        <h1>玩家提交的 Free Fire 地圖</h1>
    </header>
    <main>
        <div id="maps-container">載入中...</div>
    </main>
    <script>
        fetch("https://script.google.com/macros/s/AKfycbwywm2WdSsHxnl3gi8uKqGzg6Jcr5SY3e6Cez55e4uJ11c-imORT6nd_3Lqp5tekEG4Fg/exec")
          .then(response => response.json())
          .then(data => {
            let content = "";
            data.forEach(item => {
              content += `
                <div class="map-card">
                  <h3>${item.map_name}</h3>
                  <p><b>作者：</b>${item.name}</p>
                  <p><b>簡介：</b>${item.description}</p>
                  <p><b>地圖代碼：</b>${item.code}</p>
                  <img src="${item.image}" alt="地圖圖片">
                </div>
              `;
            });
            document.getElementById("maps-container").innerHTML = content;
          })
          .catch(error => console.error("載入失敗", error));
    </script>
</body>
</html>
