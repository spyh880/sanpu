<!DOCTYPE html>
<html lang="zh">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>志愿者等级系统</title>

<style>
    *{
        margin:0;
        padding:0;
        box-sizing:border-box;
        font-family:"Microsoft YaHei";
    }

    body{
        background:#f3f6fb;
        padding:30px;
    }

    .container{
        max-width:1000px;
        margin:auto;
    }

    h1{
        text-align:center;
        margin-bottom:30px;
        color:#222;
    }

    .card{
        background:white;
        padding:20px;
        border-radius:16px;
        margin-bottom:20px;
        box-shadow:0 5px 15px rgba(0,0,0,0.08);
    }

    .form-group{
        margin-bottom:15px;
    }

    label{
        display:block;
        margin-bottom:8px;
        font-weight:bold;
    }

    input{
        width:100%;
        padding:12px;
        border-radius:10px;
        border:1px solid #ccc;
        font-size:16px;
    }

    button{
        padding:12px 20px;
        border:none;
        border-radius:10px;
        background:#4f7cff;
        color:white;
        cursor:pointer;
        font-size:16px;
        transition:0.3s;
    }

    button:hover{
        background:#315fe0;
    }

    table{
        width:100%;
        border-collapse:collapse;
    }

    th,td{
        padding:14px;
        text-align:center;
        border-bottom:1px solid #eee;
    }

    th{
        background:#4f7cff;
        color:white;
    }

    .badge{
        padding:6px 12px;
        border-radius:20px;
        color:white;
        font-size:14px;
    }

    .bronze{
        background:#cd7f32;
    }

    .silver{
        background:#999;
    }

    .gold{
        background:#f0b400;
    }

    .diamond{
        background:#00bfff;
    }

    .progress{
        width:100%;
        height:12px;
        background:#eee;
        border-radius:20px;
        overflow:hidden;
        margin-top:8px;
    }

    .progress-bar{
        height:100%;
        background:#4f7cff;
    }

</style>
</head>

<body>

<div class="container">

    <h1>志愿者等级系统</h1>

    <div class="card">

        <div class="form-group">
            <label>志愿者姓名</label>
            <input type="text" id="name">
        </div>

        <div class="form-group">
            <label>增加服务场次</label>
            <input type="number" id="count" value="1">
        </div>

        <button onclick="addVolunteer()">提交记录</button>

    </div>

    <div class="card">

        <h2 style="margin-bottom:20px;">志愿者排行榜</h2>

        <table>
            <thead>
                <tr>
                    <th>排名</th>
                    <th>姓名</th>
                    <th>服务场次</th>
                    <th>等级</th>
                    <th>成长进度</th>
                </tr>
            </thead>

            <tbody id="volunteerTable">

            </tbody>
        </table>

    </div>

</div>

<script>

let volunteers = JSON.parse(localStorage.getItem("volunteers")) || [];

function getLevel(count){

    if(count >= 10){
        return {
            name:"钻石",
            class:"diamond",
            next:10
        };
    }

    if(count >= 5){
        return {
            name:"黄金",
            class:"gold",
            next:10
        };
    }

    if(count >= 3){
        return {
            name:"白银",
            class:"silver",
            next:5
        };
    }

    return {
        name:"青铜",
        class:"bronze",
        next:3
    };
}

function addVolunteer(){

    const name = document.getElementById("name").value.trim();
    const count = parseInt(document.getElementById("count").value);

    if(!name){
        alert("请输入姓名");
        return;
    }

    let existing = volunteers.find(v => v.name === name);

    if(existing){
        existing.count += count;
    }else{
        volunteers.push({
            name:name,
            count:count
        });
    }

    saveData();
    renderTable();

    document.getElementById("name").value = "";
    document.getElementById("count").value = 1;
}

function saveData(){
    localStorage.setItem("volunteers", JSON.stringify(volunteers));
}

function renderTable(){

    volunteers.sort((a,b)=>b.count-a.count);

    const table = document.getElementById("volunteerTable");

    table.innerHTML = "";

    volunteers.forEach((v,index)=>{

        const level = getLevel(v.count);

        let progress = (v.count / level.next) * 100;

        if(progress > 100){
            progress = 100;
        }

        table.innerHTML += `
            <tr>
                <td>${index+1}</td>
                <td>${v.name}</td>
                <td>${v.count}</td>

                <td>
                    <span class="badge ${level.class}">
                        ${level.name}
                    </span>
                </td>

                <td>
                    <div>${v.count}/${level.next}</div>

                    <div class="progress">
                        <div class="progress-bar"
                       # sanpu
