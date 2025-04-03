<!DOCTYPE html>
<html lang="zh-TW">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Free Fire 創意地圖</title>
    <style>
        body { font-family: Arial, sans-serif; text-align: center; }
        .map-card { border: 1px solid #ddd; padding: 10px; margin: 10px; display: inline-block; width: 300px; }
        .map-card img { max-width: 100%; height: auto; }
        .delete-btn { background-color: red; color: white; border: none; padding: 5px; cursor: pointer; }
    </style>
</head>
<body>
    <h1>玩家上傳的 Free Fire 地圖</h1>
    <!-- 搜索功能 -->
    <input type="text" id="search-box" placeholder="輸入關鍵字搜尋..." onkeyup="searchMaps()">
    <div id="maps-container"></div>
    <script>
        // 使用新的 Google Apps Script API 連結
        const apiUrl = "https://script.google.com/macros/s/AKfycbxRs1qtaNJs58CW8k_XalVmTZYJ3kpiA4Drw-T9bx0XLBtZs1SG4rDi9aq1EHp1fb6YQQ/exec";
        async function fetchMaps() {
            try {
                const response = await fetch(apiUrl);
                const data = await response.json();
                displayMaps(data);
            } catch (error) {
                console.error("讀取地圖失敗", error);
            }
        }
        function displayMaps(data) {
            let container = document.getElementById("maps-container");
            container.innerHTML = "";
            data.forEach(item => {
                let mapCard = document.createElement("div");
                mapCard.className = "map-card";
                mapCard.innerHTML = `
                    <h3>${item.map_name}</h3>
                    <p><b>作者：</b>${item.name}</p>
                    <p><b>簡介：</b>${item.description}</p>
                    <p><b>地圖代碼：</b>${item.code}</p>
                    <img src="${item.image}" alt="地圖圖片">
                    <br>
                    <button class="delete-btn" onclick="deleteMap('${item.id}')">刪除</button>
                `;
                container.appendChild(mapCard);
            });
        }
        function searchMaps() {
            let searchText = document.getElementById("search-box").value.toLowerCase();
            let cards = document.getElementsByClassName("map-card");
            Array.from(cards).forEach(card => {
                let text = card.innerText.toLowerCase();
                card.style.display = text.includes(searchText) ? "block" : "none";
            });
        }
        async function deleteMap(id) {
            let password = prompt("請輸入管理者密碼刪除內容:");
            if (password !== "your_admin_password") {
                alert("密碼錯誤，無法刪除！");
                return;
            }
            await fetch(apiUrl + `?delete=${id}`, { method: "GET" });
            alert("已刪除！");
            fetchMaps(); // 重新載入資料
        }
        fetchMaps(); // 載入地圖資料
    </script>
</body>
</html>
