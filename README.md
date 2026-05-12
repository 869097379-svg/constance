<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <title>5月16日陈楚生南村万博轰趴床位选择</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: "Microsoft YaHei", sans-serif;
        }
        body {
            min-height: 100vh;
            padding: 20px;
            background: #f5f7fa;
        }
        .box {
            background: #fff;
            max-width: 650px;
            margin: 40px auto;
            padding: 30px;
            border-radius: 12px;
            box-shadow: 0 2px 12px rgba(0,0,0,0.08);
        }
        h2 {
            text-align: center;
            margin-bottom: 25px;
            color: #333;
        }
        .room-item {
            padding: 10px 15px;
            background: #f0f4f9;
            border-radius: 6px;
            margin: 6px 0;
        }
        .inp-group {
            margin: 20px 0;
        }
        input {
            width: 100%;
            padding: 12px;
            margin: 8px 0;
            border: 1px solid #ddd;
            border-radius: 6px;
            font-size: 16px;
        }
        button {
            width: 100%;
            padding: 13px;
            border: none;
            border-radius: 6px;
            font-size: 16px;
            cursor: pointer;
            margin: 6px 0;
        }
        .btn-assign {
            background: #409eff;
            color: #fff;
        }
        .btn-reset {
            background: #eee;
            color: #333;
        }
        .result {
            padding: 15px;
            border-radius: 6px;
            margin: 15px 0;
            font-weight: bold;
            text-align: center;
        }
        .success {
            background: #e6f7ff;
            color: #0066cc;
        }
        .error {
            background: #fff2f0;
            color: #cf1322;
        }
        .history {
            margin-top: 20px;
        }
        .history h4 {
            margin-bottom: 10px;
        }
        .history-item {
            padding: 8px;
            background: #f8f9fa;
            border-radius: 4px;
            margin: 4px 0;
        }
    </style>
</head>
<body>
    <div class="box">
        <h2>5月16日陈楚生南村万博轰趴床位选择</h2>

        <div id="roomView">
            <div class="room-item">A房｜总3床，剩余：<span id="rA">3</span></div>
            <div class="room-item">B房｜总2床，剩余：<span id="rB">2</span></div>
            <div class="room-item">C房｜总2床，剩余：<span id="rC">2</span></div>
            <div class="room-item">D房｜总4床，剩余：<span id="rD">4</span></div>
        </div>

        <div class="inp-group">
            <input type="text" id="userName" placeholder="请输入姓名">
            <input type="number" id="needBed" placeholder="只能输入 1 或 2 个床位" min="1" max="2">
        </div>

        <button class="btn-assign" onclick="doAssign()">立即公平分配</button>
        <button class="btn-reset" onclick="resetAll()">一键重置所有人 & 床位</button>

        <div id="msg" class="result"></div>

        <div class="history">
            <h4>已分配记录（每人仅限一次）</h4>
            <div id="list"></div>
        </div>
    </div>

    <script>
        let room = {A:3, B:2, C:2, D:4};
        let assignedUsers = [];
        let recordList = [];

        function renderRoom(){
            document.getElementById('rA').innerText = room.A;
            document.getElementById('rB').innerText = room.B;
            document.getElementById('rC').innerText = room.C;
            document.getElementById('rD').innerText = room.D;
        }

        function renderRecord(){
            let html = '';
            recordList.forEach(item=>{
                html += `<div class="history-item">${item.name} → ${item.room}房，占用${item.bed}个床位</div>`;
            });
            document.getElementById('list').innerHTML = html;
        }

        function doAssign(){
            let name = document.getElementById('userName').value.trim();
            let need = parseInt(document.getElementById('needBed').value);
            let msgBox = document.getElementById('msg');

            if(!name){
                msgBox.className = "result error";
                msgBox.innerText = "请输入姓名";
                return;
            }

            if(need !== 1 && need !== 2){
                msgBox.className = "result error";
                msgBox.innerText = "❌ 只能输入 1 或 2 个床位！";
                return;
            }

            if(assignedUsers.includes(name)){
                msgBox.className = "result error";
                msgBox.innerText = "❌ 该人员已分配过，每人只有一次机会";
                return;
            }

            let canUse = [];
            if(room.A >= need) canUse.push('A');
            if(room.B >= need) canUse.push('B');
            if(room.C >= need) canUse.push('C');
            if(room.D >= need) canUse.push('D');

            if(canUse.length === 0){
                msgBox.className = "result error";
                msgBox.innerText = "暂无足够空余床位";
                return;
            }

            let randRoom = canUse[Math.floor(Math.random() * canUse.length)];
            room[randRoom] -= need;
            assignedUsers.push(name);
            recordList.push({name, room:randRoom, bed:need});

            renderRoom();
            renderRecord();

            msgBox.className = "result success";
            msgBox.innerText = `✅ 分配成功：${name} → ${randRoom}房`;

            document.getElementById('userName').value = '';
            document.getElementById('needBed').value = '';
        }

        function resetAll(){
            room = {A:3, B:2, C:2, D:4};
            assignedUsers = [];
            recordList = [];
            renderRoom();
            renderRecord();
            document.getElementById('msg').className = "result";
            document.getElementById('msg').innerText = "已重置所有数据";
        }

        renderRoom();
    </script>
</body>
</html>
