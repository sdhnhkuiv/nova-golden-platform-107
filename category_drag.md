# 拖动物品分类游戏



## 说明

将屏幕上的物品拖到正确的分类框中（如水果→水果篮、动物→动物园）。学习 HTML drag-drop 与分类逻辑的结合。



## 代码

```html

<!DOCTYPE html>

<html lang="zh">

<head>

<meta charset="UTF-8">

<title>物品分类游戏</title>

<style>

  body { font-family: Arial; text-align: center; margin: 20px; }

  .items { display: flex; flex-wrap: wrap; justify-content: center; gap: 15px; margin: 20px; }

  .item {

    width: 90px; height: 90px; font-size: 40px;

    display: flex; align-items: center; justify-content: center;

    background: #fff3e0; border: 2px solid #ff9800; border-radius: 12px;

    cursor: grab; user-select: none;

  }

  .bins { display: flex; justify-content: center; gap: 40px; margin: 30px; }

  .bin {

    width: 180px; min-height: 150px;

    border: 4px dashed #2196F3; border-radius: 16px;

    padding: 10px; text-align: center; background: #e3f2fd;

  }

  .bin.over { background: #bbdefb; border-color: #0d47a1; }

  .bin h3 { margin: 5px 0; }

  .correct { border-color: #4CAF50 !important; background: #c8e6c9 !important; }

  #score { font-size: 24px; color: #2196F3; }

</style>

</head>

<body>

<h2>📦 物品分类 - 拖到正确的分类框</h2>

<div id="score">得分: 0 / 6</div>

<div class="items" id="itemsArea"></div>

<div class="bins" id="binsArea"></div>

<button onclick="resetGame()">重新开始</button>



<script>

const categories = {

  "水果": ["🍎","🍌","🍇"],

  "动物": ["🐶","🐱","🐰"],

};

let score = 0;

let total = Object.values(categories).flat().length;



function render() {

  // 渲染物品

  let itemsDiv = document.getElementById("itemsArea");

  itemsDiv.innerHTML = "";

  Object.entries(categories).forEach(([cat, emojis]) => {

    emojis.forEach(emoji => {

      let div = document.createElement("div");

      div.className = "item";

      div.textContent = emoji;

      div.draggable = true;

      div.dataset.category = cat;

      div.addEventListener("dragstart", e => {

        e.dataTransfer.setData("text/plain",

          JSON.stringify({emoji, category: cat}));

      });

      itemsDiv.appendChild(div);

    });

  });



  // 渲染分类框

  let binsDiv = document.getElementById("binsArea");

  binsDiv.innerHTML = "";

  Object.keys(categories).forEach(cat => {

    let bin = document.createElement("div");

    bin.className = "bin";

    bin.innerHTML = `<h3>${cat}</h3>`;

    bin.addEventListener("dragover", e => {

      e.preventDefault();

      bin.classList.add("over");

    });

    bin.addEventListener("dragleave", () => bin.classList.remove("over"));

    bin.addEventListener("drop", e => {

      e.preventDefault();

      bin.classList.remove("over");

      let data = JSON.parse(e.dataTransfer.getData("text/plain"));

      let dragItem = e.target.closest(".item");



      if (data.category === cat && dragItem) {

        dragItem.remove();

        score++;

        document.getElementById("score").textContent = `得分: ${score} / ${total}`;

        bin.classList.add("correct");

        setTimeout(() => bin.classList.remove("correct"), 500);



        if (score >= total) {

          setTimeout(() => alert("🎉 全部分类正确！"), 100);

        }

      } else {

        alert("❌ 不对哦，再试试！");

      }

    });

    binsDiv.appendChild(bin);

  });

}



function resetGame() {

  score = 0;

  document.getElementById("score").textContent = `得分: 0 / ${total}`;

  render();

}



render();

</script>

</body>

</html>

```

