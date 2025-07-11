<!DOCTYPE html>
<html lang="ja">
<head>
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>todotask</title>
<style>
/* ----------------------------
   フォントと色定義
---------------------------- */
    @font-face {
        font-family: 'komadori_mini';
        src: url('komadori-mini.otf') format('opentype');
    }

    :root {
        --background: #F3F3E6;
    }

/* ----------------------------
   全体のレイアウト（html / body） - PCデフォルト
---------------------------- */
    html {
        height: 100vh;
        margin: 0;
        padding: 0;
    }

    body {
        width: 100vw;
        height: 100vh;
        margin: 0;
        padding: 0;
        display: flex;
        align-items: center;
        justify-content: center;
        font-family: 'komadori_mini', sans-serif;
        background-color: var(--background);
        overflow-x: hidden;
        gap: 40px;
        box-sizing: border-box;
    }

/* ライト・ダークモード */
    body.light-mode {
        background-color: var(--background);
        color: #333333;
    }

    body.dark-mode {
        background-color: #67403b;
        color: #F3F3E6;
    }

/* ----------------------------
   アイコン
---------------------------- */
    img {
        width: 30px;
        height: auto;
        cursor: pointer;
        transition: transform 0.2s;
        position: absolute;
        top: 10px;
        right: 10px;
        z-index: 100;
    }

    img:hover {
        transform: scale(1.1);
    }

/* ----------------------------
   メインボックス（枠） - PCデフォルト
---------------------------- */
    .box {
        width: 600px;
        height: 400px;
        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: flex-start;
        border-radius: 10px;
        box-shadow: 0 2px 6px rgba(0, 0, 0, 0.1);
        position: relative;
        flex-shrink: 0;
        padding-top: 20px;
    }

    .box.dark-mode {
        background-color: #4a62b1;
        color: white;
    }

    .box.light-mode {
        background-color: rgb(253, 225, 225);
        color: #000;
    }

    /* 見出し */
    .h1 {
        font-size: 30px;
        color: #F7465D;
        margin: 0;
        font-weight: bold;
        position: absolute;
        top: 10%;
        left: 50%;
        transform: translateX(-50%);
        text-align: center;
    }

/* ----------------------------
   入力エリア - PCデフォルト
---------------------------- */
    .input-group {
        display: flex;
        align-items: center;
        gap: 10px;
        justify-content: center;
        position: absolute;
        top: 25%;
    }

    #task-input {
        width: 400px;
        height: 50px;
        font-size: 15px;
        border-radius: 10px;
        text-align: center;
        line-height: 40px;
        padding: 0;
        resize: none;
    }

    #task-input::placeholder {
        font-size: 15px;
    }

    #extra-button {
        width: 100px;
        height: 50px;
        background-color: var(--background);
        color: #F3F3E6;
        background-color: #874338;
        font-size: 15px;
        border-radius: 10px;
        border: none;
        display: flex;
        align-items: center;
        justify-content: center;
        cursor: pointer;
    }

/* ----------------------------
   タスクリスト表示 - PCデフォルト
---------------------------- */
    /*タスクスクロール*/
    .scroll-container {
        width: 100%;
        height: 200px;
        overflow-y: auto;
        margin-top: 30%;
    }

    .all-box {
        display: flex;
        flex-direction: column;
        align-items: center;
        margin-top: 0;
        gap: 10px;
    }

    .item-box {
        width: 480px;
        display: flex;
        flex-direction: row;
        align-items: center;
        justify-content: space-between;
        background-color: #FFF2B4;
        padding: 16px;
        border-radius: 10px;
    }

    .item-box.selected {
        background-color: #ffed92;
    }

    .item-content {
        font-size: 15px;
        color: #000;
        margin: 0;
        padding: 0;
        display: flex;
        align-items: center;
        flex-grow: 1;
    }

    .remove-button {
        background: none;
        color: #F7465D;
        font-size: 15px;
        border: none;
        cursor: pointer;
        display: flex;
        align-items: center;
        justify-content: center;
        flex-shrink: 0;
    }
    
    .check-button {
        width: 20px;
        height: 20px;
        border: 2px solid #333;
        border-radius: 4px; 
        cursor: pointer;
        margin-right: 10px; 
        display: inline-block; 
        flex-shrink: 0;
    }

    .check-button.checked {
        background-color: #333;
    }

    .item-content.completed {
        text-decoration: line-through;
        color: #aaa;
    }

/* ----------------------------
   詳細box (box2) - PCデフォルト
---------------------------- */
    .box2 {
        position: fixed; /* 常に画面に固定 */
        top: 50%;
        left: 50%;
        transform: translate(-50%, -50%) translateX(100vw); /* 画面右に隠す */
        width: 600px;
        height: 400px;
        transition: transform 0.3s ease;
        border-radius: 10px;
        box-shadow: 0 2px 6px rgba(0, 0, 0, 0.1);
        display: flex;
        flex-direction: column;
        align-items: center;
        padding-top: 20px;
        z-index: 99;
    }

    .box2.show-pc { /* PC用の表示クラス */
        transform: translate(-50%, -50%) translateX(0); /* 画面中央に表示 */
    }

    .box2.dark-mode {
        background-color: #3d8860;
        color: white;
    }

    .box2.light-mode {
        background-color: rgb(253, 225, 225);
        color: #000;
    }

    .box2 .h1 {
        top: 10%;
    }

    .box2 .input-group {
        position: absolute;
        top: 25%;
        margin: 0;
        justify-content: center;
    }

    /* box2内のメインタスク表示用 */
    #box2-main-task-display {
        width: 480px;
        background-color: #ffed92;
        padding: 16px;
        border-radius: 10px;
        font-size: 18px;
        color: #000;
        text-align: center;
        margin-bottom: 20px;
        margin-top: 30%;
    }

    #box2-task-input {
        width: 400px;
        height: 50px;
        font-size: 15px;
        border-radius: 10px;
        text-align: center;
        line-height: 40px;
        padding: 0;
        resize: none;
    }

    #box2-task-input::placeholder {
        font-size: 15px;
    }

    #box2-extra-button {
        width: 100px;
        height: 50px;
        background-color: var(--background);
        color: #F3F3E6;
        background-color: #874338;
        font-size: 15px;
        border-radius: 10px;
        border: none;
        display: flex;
        align-items: center;
        justify-content: center;
        cursor: pointer;
    }

    /* box2内のサブタスク用スクロールコンテナ */
    #box2-sub-tasks-container {
        width: 100%;
        height: 200px;
        overflow-y: auto;
        display: flex;
        flex-direction: column;
        align-items: center;
        padding: 0 20px;
        margin-top: 10px;
    }

    /* box2内のサブタスク用スタイル */
    .box2 .item-box {
        width: 480px;
        display: flex;
        flex-direction: row;
        align-items: center;
        justify-content: space-between;
        background-color: #b4ffb4;
        padding: 16px;
        border-radius: 10px;
        margin-bottom: 10px;
    }

    .box2 .item-content {
        font-size: 15px;
        color: #000;
        margin: 0;
        padding: 0;
        display: flex;
        align-items: center;
        flex-grow: 1;
    }

    .box2 .remove-button,
    .box2 .check-button {
        flex-shrink: 0;
    }

    .box2 .item-content.completed {
        text-decoration: line-through;
        color: #aaa;
    }

    .item-box.selected {
        background-color: #ffed92;
    }

    /* box2を閉じるボタンの追加 */
    #box2-close-button {
        position: absolute;
        top: 10px;
        left: 10px;
        background: none;
        border: none;
        font-size: 24px;
        color: #333;
        cursor: pointer;
        z-index: 10; /* 他の要素より前面に */
    }
    .box2.dark-mode #box2-close-button {
        color: white;
    }


/* ----------------------------
   メディアクエリ (スマートフォン最適化)
---------------------------- */
    @media (max-width: 600px) {
        body {
            flex-direction: column;
            padding: 10px;
            gap: 20px;
            justify-content: flex-start;
        }

        .box {
            width: 95%;
            height: 350px;
            box-sizing: border-box;
            position: static;
            transform: none;
            margin: 0 auto;
            box-shadow: 0 1px 4px rgba(0, 0, 0, 0.1);
            padding-top: 15px;
        }

        .h1 {
            font-size: 24px;
            position: static;
            transform: none;
            margin-bottom: 20px;
            top: unset;
        }

        .input-group {
            position: static;
            transform: none;
            flex-direction: column;
            align-items: center;
            top: unset;
            margin-bottom: 15px;
        }

        #task-input, #box2-task-input {
            width: 90%;
            height: 40px;
            font-size: 14px;
            margin-bottom: 10px;
        }

        #extra-button, #box2-extra-button {
            width: 150px;
            height: 40px;
            font-size: 14px;
        }

        .scroll-container {
            width: 95%;
            height: 150px;
            margin-top: 15px;
            padding: 0 10px;
        }
        
        .item-box, .box2 .item-box, #box2-main-task-display {
            width: 95%;
            padding: 12px;
            font-size: 14px;
        }

        .item-content {
            font-size: 14px;
        }

        .remove-button, .check-button {
            font-size: 14px;
        }

        /* box2のスマートフォン専用スタイル */
        .box2 {
            top: auto; /* topの指定を解除 */
            bottom: -100vh; /* 画面下部に隠す */
            left: 0;
            width: 100vw;
            height: 80vh; /* 画面の80%の高さで表示 */
            transform: translateX(0) translateY(0); /* transformをリセットしてbottomで制御 */
            transition: bottom 0.3s ease; /* bottomプロパティをアニメーション */
            border-bottom-left-radius: 0;
            border-bottom-right-radius: 0;
            padding-top: 50px;
        }

        .box2.show-mobile { /* スマホ用の表示クラス */
            bottom: 0; /* 画面下部に表示 */
        }
        
        /* スマートフォンでのアイコン位置調整 */
        img {
            top: 10px;
            right: 10px;
        }

        .box2 .h1 {
            position: static;
            transform: none;
            margin-bottom: 20px;
            top: unset;
        }

        .box2 .input-group {
            position: static;
            transform: none;
            margin-top: 20px;
            margin-bottom: 20px;
        }

        #box2-main-task-display {
            position: static;
            transform: none;
            margin-top: 0;
        }
        
        #box2-sub-tasks-container {
            position: static;
            transform: none;
            margin-top: 10px;
            height: 180px;
        }

        /* スマホ版の閉じるボタン位置調整 */
        #box2-close-button {
            top: 15px;
            left: 15px;
            font-size: 28px; /* スマホでは少し大きめに */
        }
    }
</style>
</head>

<body id ="mode-body">
    <div class="box light-mode" id="mode-box">
        <span class ="h1">本日のタスク</span>
    <div class="input-group">
        <input id="task-input" maxlength="10" placeholder="新しいタスクを入力...">
        <button id="extra-button">追加</button>
    </div>

    <div class="light-mode">
        <img id="mode-icon" src="途中.png" alt="モード切替アイコン" onclick="toggleMode()">
    </div>
    
    <div class="scroll-container">
        <div class="all-box" id="main-container"></div>
    </div>

    </div>

    <div class="box2 light-mode" id="mode-box2">
        <button id="box2-close-button">×</button> <span class="h1">やること</span>
        
        <div id="box2-main-task-display">メインタスクをせんたくしてね</div>
        
        <div id="box2-sub-tasks-container"></div>

        <div class="input-group">
           <input id="box2-task-input" maxlength="10" placeholder="具体的なタスクを入力...">
            <button id="box2-extra-button">追加</button>
        </div>
    </div>

<script>
// ===============================
//        モード切り替え機能
// ===============================
    let isLight = true;

    function toggleMode() {
        const modeBody = document.getElementById('mode-body');
        const modeIcon = document.getElementById('mode-icon');
        const modeBox = document.getElementById('mode-box');
        const modeBox2 = document.getElementById('mode-box2');

        if (isLight) {
            modeBody.classList.remove('light-mode');
            modeBody.classList.add('dark-mode');

            modeBox.classList.remove('light-mode');
            modeBox.classList.add('dark-mode');

            modeBox2.classList.remove('light-mode');
            modeBox2.classList.add('dark-mode');

            modeIcon.src = '終わり.png';
        } else {
            modeBody.classList.remove('dark-mode');
            modeBody.classList.add('light-mode');

            modeBox.classList.remove('dark-mode');
            modeBox.classList.add('light-mode');

            modeBox2.classList.remove('dark-mode');
            modeBox2.classList.add('light-mode');

            modeIcon.src = '途中.png';
        }

        isLight = !isLight;
    }

// ===============================
//        要素取得（共通）
// ===============================
    const mainContainer = document.getElementById('main-container'); // メインのタスクリスト
    const extraButton = document.getElementById('extra-button');
    const taskInput = document.getElementById('task-input');
    
    const box2MainTaskDisplay = document.getElementById('box2-main-task-display'); // box2のメインタスク表示エリア
    const box2SubTasksContainer = document.getElementById('box2-sub-tasks-container'); // box2のサブタスクリスト
    const box2Input = document.getElementById('box2-task-input');
    const box2Button = document.getElementById('box2-extra-button');
    const box2 = document.getElementById('mode-box2'); // box2要素を取得
    const box2CloseButton = document.getElementById('box2-close-button'); // 閉じるボタン

    let selectedMainTask = null; // 現在選択されているメインタスク

// ===============================
//    ロード時：保存済みタスクを表示
// ===============================
    window.onload = function () {
        const savedMainTasks = JSON.parse(localStorage.getItem("mainTasks")) || [];
        savedMainTasks.forEach(taskData => {
            createTaskBox(taskData.task, taskData.completed, taskData.subTasks);
        });

        // box2の初期表示をリセット
        box2MainTaskDisplay.textContent = 'メインタスクをせんたくしてね';
        box2SubTasksContainer.innerHTML = '';
        selectedMainTask = null; // 初期化
    };

// ===============================
//     タスク追加 (メイン)
// ===============================
    extraButton.addEventListener('click', () => {
        const task = taskInput.value.trim();
        if (task === "") return;

        createTaskBox(task, false, []); // 新規タスクは未完了、サブタスクなし
        saveTasks(); // 全タスクを保存
        taskInput.value = ""; 
    });

// ===============================
//   タスク追加 (box2 サブタスク)
// ===============================
    box2Button.addEventListener('click', () => {
        const subTask = box2Input.value.trim();
        if (subTask === "" || !selectedMainTask) return; // メインタスクが選択されていない場合は何もしない

        // 選択中のメインタスクのサブタスクリストに追加
        selectedMainTask.subTasks.push({ task: subTask, completed: false });
        saveTasks(); // 全タスクを保存

        // box2にサブタスクを表示
        createSubTaskBox(subTask, false, box2SubTasksContainer, selectedMainTask); 
        box2Input.value = "";
    });

// ===============================
//    タスク保存（localStorage）
// ===============================
    function saveTasks() {
        const allMainTasks = [];
        // mainContainer内のすべてのタスクデータを収集
        mainContainer.querySelectorAll('.item-box').forEach(box => {
            const taskText = box.querySelector('.item-content').textContent;
            const completed = box.querySelector('.check-button').classList.contains('checked');
            // サブタスクは選択されたメインタスクにのみ紐づいているので、直接保存しない
            // createTaskBox関数で渡されるsubTasksは、初回ロード時にのみ使用されることを想定
            // 実際はselectedMainTaskオブジェクト経由でサブタスクを管理する
            const savedSubTasks = JSON.parse(localStorage.getItem(`subTasks_${taskText}`)) || [];
            allMainTasks.push({
                task: taskText,
                completed: completed,
                subTasks: savedSubTasks // ここでサブタスクを保存
            });
        });
        localStorage.setItem("mainTasks", JSON.stringify(allMainTasks));

        // 選択中のメインタスクがある場合、そのサブタスクを保存
        if (selectedMainTask) {
             localStorage.setItem(`subTasks_${selectedMainTask.task}`, JSON.stringify(selectedMainTask.subTasks));
        }
    }


// ===============================
//     タスク削除 (メイン/サブ)
// ===============================
    function removeTask(taskText, containerElement, isSubTask = false, parentTask = null) {
        let saved;
        let updated;

        if (isSubTask && parentTask) {
            // サブタスクの場合
            parentTask.subTasks = parentTask.subTasks.filter(st => st.task !== taskText);
            localStorage.setItem(`subTasks_${parentTask.task}`, JSON.stringify(parentTask.subTasks));
            // box2SubTasksContainerから該当要素を削除
            const targetBox = Array.from(containerElement.children).find(child => 
                child.querySelector('.item-content') && child.querySelector('.item-content').textContent === taskText
            );
            if (targetBox) {
                containerElement.removeChild(targetBox);
            }

        } else {
            // メインタスクの場合
            saved = JSON.parse(localStorage.getItem("mainTasks")) || [];
            updated = saved.filter(t => t.task !== taskText);
            localStorage.setItem("mainTasks", JSON.stringify(updated));
            containerElement.innerHTML = ''; // 一度コンテナをクリア
            updated.forEach(taskData => { // 残りのタスクを再描画
                createTaskBox(taskData.task, taskData.completed, taskData.subTasks);
            });
            // 削除されたタスクが選択中のメインタスクだった場合、box2をリセット
            if (selectedMainTask && selectedMainTask.task === taskText) {
                box2MainTaskDisplay.textContent = 'メインタスクをせんたくしてね';
                box2SubTasksContainer.innerHTML = '';
                selectedMainTask = null;
                // box2を閉じる
                box2.classList.remove('show-pc');
                box2.classList.remove('show-mobile');
            }
            // 関連するサブタスクのlocalStorageも削除
            localStorage.removeItem(`subTasks_${taskText}`);
        }
    }

// ===============================
//   タスク表示ボックス作成 (メイン)
// ===============================
    function createTaskBox(task, completed, subTasksData) {
        const box = document.createElement('div');
        box.className = 'item-box';

        const checkBtn = document.createElement('div');
        checkBtn.className = 'check-button';
        if (completed) {
            checkBtn.classList.add('checked');
        }

        const content = document.createElement('div');
        content.className = 'item-content';
        content.textContent = task;
        if (completed) {
            content.classList.add('completed');
        }

        const removeBtn = document.createElement('button');
        removeBtn.className = 'remove-button';
        removeBtn.textContent = '削除';

        // イベントリスナーの追加
        checkBtn.addEventListener('click', (e) => {
            e.stopPropagation(); // イベント伝播を停止
            checkBtn.classList.toggle('checked');
            content.classList.toggle('completed');
            // localStorage の状態も更新
            const savedMainTasks = JSON.parse(localStorage.getItem("mainTasks")) || [];
            const taskIndex = savedMainTasks.findIndex(t => t.task === task);
            if (taskIndex !== -1) {
                savedMainTasks[taskIndex].completed = checkBtn.classList.contains('checked');
                localStorage.setItem("mainTasks", JSON.stringify(savedMainTasks));
            }
        });

        removeBtn.addEventListener('click', (e) => {
            e.stopPropagation(); // イベント伝播を停止
            removeTask(task, mainContainer, false);
        });

        box.addEventListener('click', () => {
            const allBoxes = document.querySelectorAll('#main-container .item-box');
            allBoxes.forEach(b => b.classList.remove('selected'));
            box.classList.add('selected');

            // 選択されたメインタスクをセット
            selectedMainTask = { task: task, completed: completed, subTasks: subTasksData || [] };
            box2MainTaskDisplay.textContent = task; // box2のメインタスク表示を更新
            
            // box2のサブタスクをクリアして再描画
            box2SubTasksContainer.innerHTML = '';
            // サブタスクを localStorage からロードして表示
            const savedSubTasks = JSON.parse(localStorage.getItem(`subTasks_${task}`)) || [];
            selectedMainTask.subTasks = savedSubTasks; // selectedMainTaskのサブタスクも更新
            savedSubTasks.forEach(subTaskData => {
                createSubTaskBox(subTaskData.task, subTaskData.completed, box2SubTasksContainer, selectedMainTask);
            });

            // box2を表示
            if (window.innerWidth > 600) { // PC画面の場合 (600pxより大きい場合)
                box2.classList.add('show-pc');
            } else { // スマートフォン画面の場合 (600px以下の場合)
                box2.classList.add('show-mobile');
            }
        });

        box.appendChild(checkBtn);
        box.appendChild(content);
        box.appendChild(removeBtn);

        mainContainer.appendChild(box);
    }

// ===============================
//  サブタスク表示ボックス作成 (box2用)
// ===============================
    function createSubTaskBox(subTask, completed, containerElement, parentTask) {
        const box = document.createElement('div');
        box.className = 'item-box'; // スタイルは共通

        const checkBtn = document.createElement('div');
        checkBtn.className = 'check-button';
        if (completed) {
            checkBtn.classList.add('checked');
        }

        const content = document.createElement('div');
        content.className = 'item-content';
        content.textContent = subTask;
        if (completed) {
            content.classList.add('completed');
        }

        const removeBtn = document.createElement('button');
        removeBtn.className = 'remove-button';
        removeBtn.textContent = '削除';

        // イベントリスナーの追加
        checkBtn.addEventListener('click', (e) => {
            e.stopPropagation(); // イベント伝播を停止
            checkBtn.classList.toggle('checked');
            content.classList.toggle('completed');
            // localStorage のサブタスクの状態も更新
            if (parentTask) {
                const subTaskIndex = parentTask.subTasks.findIndex(st => st.task === subTask);
                if (subTaskIndex !== -1) {
                    parentTask.subTasks[subTaskIndex].completed = checkBtn.classList.contains('checked');
                    localStorage.setItem(`subTasks_${parentTask.task}`, JSON.stringify(parentTask.subTasks));
                }
            }
        });

        removeBtn.addEventListener('click', (e) => {
            e.stopPropagation(); // イベント伝播を停止
            removeTask(subTask, containerElement, true, parentTask);
        });

        box.appendChild(checkBtn);
        box.appendChild(content);
        box.appendChild(removeBtn);

        containerElement.appendChild(box);
    }

    // box2を閉じるボタンのイベントリスナー
    box2CloseButton.addEventListener('click', () => {
        box2.classList.remove('show-pc');
        box2.classList.remove('show-mobile');
    });

</script>
</body>
</html>
