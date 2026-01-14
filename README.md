<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>Aizaz App Store</title>
<meta name="viewport" content="width=device-width, initial-scale=1.0">

<style>
body{
  margin:0;
  font-family:monospace;
  background:black;
  color:#00eaff;
  overflow-x:hidden;
}
canvas{
  position:fixed;
  top:0;left:0;
  z-index:-1;
}
h1{
  text-align:center;
  margin:20px;
  font-size:28px;
}
.store{
  display:grid;
  grid-template-columns:repeat(auto-fit,minmax(150px,1fr));
  gap:15px;
  padding:15px;
}
.card{
  background:#001b2e;
  border:1px solid #00eaff;
  border-radius:10px;
  padding:10px;
  text-align:center;
  box-shadow:0 0 15px #00eaff55;
}
.card img{
  width:80px;
  height:80px;
  border-radius:15px;
}
button{
  margin-top:10px;
  padding:8px 15px;
  background:#00eaff;
  border:none;
  border-radius:5px;
  font-weight:bold;
  cursor:pointer;
}
button:hover{background:#00bcd4;}

#adminPanel{
  display:none;
  padding:20px;
  background:#000;
  border-top:2px solid #00eaff;
}
input{
  width:100%;
  padding:8px;
  margin:5px 0;
}
</style>
</head>

<body>

<canvas id="rain"></canvas>

<h1 id="title">✌️ Aizaz App Store 😈</h1>

<div class="store" id="store"></div>

<!-- ADMIN PANEL -->
<div id="adminPanel">
  <h2>Admin Panel</h2>
  <input id="name" placeholder="App Name">
  <input id="icon" placeholder="Icon Image URL">
  <input id="link" placeholder="MediaFire Download Link">
  <button onclick="addApp()">Add App</button>
</div>

<audio id="sound">
  <source src="https://www.soundjay.com/buttons/sounds/button-3.mp3">
</audio>

<script>
/* RAIN */
const c=document.getElementById("rain");
const ctx=c.getContext("2d");
c.width=innerWidth;c.height=innerHeight;
let drops=Array(300).fill(0);
function rain(){
  ctx.fillStyle="rgba(0,0,0,0.1)";
  ctx.fillRect(0,0,c.width,c.height);
  ctx.fillStyle="#00eaff";
  drops.forEach((y,i)=>{
    ctx.fillRect(i*5,y,2,10);
    drops[i]+=10;
    if(drops[i]>c.height)drops[i]=0;
  });
}
setInterval(rain,30);

/* STORE DATA */
let apps = JSON.parse(localStorage.getItem("apps")) || [
  {
    name:"Demo App",
    icon:"https://i.ibb.co/ZxY4QpJ/app.png",
    link:"https://www.mediafire.com"
  }
];

function render(){
  store.innerHTML="";
  apps.forEach(a=>{
    store.innerHTML+=`
      <div class="card">
        <img src="${a.icon}">
        <h3>${a.name}</h3>
        <button onclick="download('${a.link}')">Download</button>
      </div>`;
  });
}
render();

function download(link){
  document.getElementById("sound").play();
  window.open(link,"_blank");
}

/* ADMIN SECRET */
let taps=0;
document.getElementById("title").onclick=()=>{
  taps++;
  if(taps===5){
    let pass=prompt("Admin Password:");
    if(pass==="aizaz@admin"){
      adminPanel.style.display="block";
      alert("Admin Unlocked");
    }
    taps=0;
  }
};

function addApp(){
  apps.push({
    name:name.value,
    icon:icon.value,
    link:link.value
  });
  localStorage.setItem("apps",JSON.stringify(apps));
  render();
  alert("App Added");
}
</script>

</body>
</html>
