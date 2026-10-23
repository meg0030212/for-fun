# for-fun
<!DOCTYPE html>
<html lang="zh-Hant">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>測試你的 SM 屬性｜趣味心理測驗</title>
<meta name="description" content="20 題二選一，測出你的 S 指數與 M 指數，免費看結果。">
<style>
:root{--bg:#faf9f5;--card:#fff;--text:#1f1e1d;--sub:#6b6a66;--muted:#9a9893;--border:#e5e3dc;--track:#f1efe8;--accent:#185fa5;--accent-bg:#e6f1fb;--s:#D85A30;--m:#7F77DD}
@media (prefers-color-scheme:dark){:root{--bg:#1c1c1b;--card:#262624;--text:#f2f1ec;--sub:#b4b2a9;--muted:#888780;--border:#3a3a37;--track:#333331;--accent:#85b7eb;--accent-bg:#0c447c}}
*{box-sizing:border-box}
body{margin:0;background:var(--bg);color:var(--text);font-family:-apple-system,BlinkMacSystemFont,"PingFang TC","Noto Sans TC","Microsoft JhengHei",sans-serif;line-height:1.7}
.wrap{max-width:520px;margin:0 auto;padding:24px 16px 40px}
.card{background:var(--card);border:1px solid var(--border);border-radius:16px;padding:24px 20px}
h1{font-size:24px;font-weight:600;margin:0 0 8px}
.sub{color:var(--sub);margin:0 0 24px}
button{font:inherit;cursor:pointer;-webkit-tap-highlight-color:transparent}
.primary{width:100%;padding:14px;border:none;border-radius:10px;background:var(--text);color:var(--card);font-size:16px}
.top{display:flex;justify-content:space-between;font-size:13px;color:var(--sub);margin-bottom:8px}
.bar{height:8px;border-radius:4px;background:var(--track);overflow:hidden;display:flex}
.fill{background:var(--accent);transition:width .25s}
.q{font-size:19px;font-weight:600;margin:24px 0 18px;line-height:1.5}
.opt{display:block;width:100%;text-align:left;padding:14px 16px;margin:0 0 10px;font-size:16px;border:1px solid var(--border);border-radius:10px;background:var(--card);color:var(--text);transition:background .15s}
.opt:hover{background:var(--track)}
.opt.sel{border-color:var(--accent);background:var(--accent-bg);color:var(--accent)}
.ghost{background:none;border:none;color:var(--sub);padding:8px 0;font-size:14px}
.label{font-size:13px;color:var(--sub);margin:0}
.type{font-size:26px;font-weight:600;margin:0 0 16px}
.stats{display:grid;grid-template-columns:1fr 1fr;gap:12px;margin-bottom:12px}
.stat{background:var(--track);border-radius:10px;padding:14px}
.stat small{display:block;font-size:13px;color:var(--sub)}
.stat b{font-size:28px;font-weight:600}
.desc{margin:20px 0 0;font-size:16px}
.row{display:flex;gap:10px;margin-top:24px}
.row button{flex:1;padding:12px;border-radius:10px;border:1px solid var(--border);background:var(--card);color:var(--text);font-size:15px}
.foot{text-align:center;font-size:12px;color:var(--muted);margin-top:16px}
</style>
</head>
<body>
<div class="wrap">
<div class="card">
<section id="start">
<h1>測試你的 SM 屬性</h1>
<p class="sub">你是喜歡掌控全場的 S，還是被安排就安心的 M？共 20 題二選一，憑第一直覺作答，大約兩分鐘就能完成，結果免費查看。</p>
<button class="primary" id="go">開始測驗</button>
</section>
<section id="quiz" hidden>
<div class="top"><span>測試你的 SM 屬性</span><span id="prog"></span></div>
<div class="bar"><div class="fill" id="pbar"></div></div>
<p class="q" id="qt"></p>
<div id="opts"></div>
<button class="ghost" id="prev">← 上一題</button>
</section>
<section id="result" hidden></section>
</div>
<p class="foot">本測驗僅供娛樂，不具專業診斷效力</p>
</div>
<script>
const Q=[
["朋友聚餐決定吃什麼的時候，你通常會？",[["直接拍板，大家跟著走","s"],["看大家意見，吃什麼都可以","m"]]],
["被朋友開玩笑捉弄時，你的反應是？",[["笑一笑，其實還滿有趣的","m"],["想辦法捉弄回去","s"]]],
["有人對你下指令時，你的感覺是？",[["有點不爽，想自己決定","s"],["有明確方向反而安心","m"]]],
["看到別人慌張失措的樣子，你會？",[["跟著一起緊張","m"],["偷偷覺得有點好玩","s"]]],
["談戀愛時，你比較常是？",[["主動追的那一方","s"],["被追的那一方","m"]]],
["遇到很嚴格的老師或主管，你會？",[["越嚴格越想把事做好","m"],["心裡默默反抗","s"]]],
["你比較享受哪一種感覺？",[["掌控全局","s"],["被照顧、被安排好","m"]]],
["吵架的時候，你通常？",[["先退一步，不想鬧僵","m"],["堅持到對方認輸","s"]]],
["玩角色扮演遊戲時，你偏好選？",[["反派或大魔王","s"],["主角身邊的小跟班","m"]]],
["你比較喜歡？",[["被別人吐槽","m"],["吐槽別人","s"]]],
["排隊時被人插隊，你會？",[["當場指出來","s"],["算了，不想起衝突","m"]]],
["分組報告時，你通常是？",[["被分配工作的人","m"],["負責分配工作的人","s"]]],
["和朋友一起看恐怖片，你？",[["看朋友被嚇比自己被嚇好玩","s"],["一邊害怕一邊停不下來","m"]]],
["傳訊息的時候，你比較常？",[["秒回，還會等對方回","m"],["已讀先晾對方一下","s"]]],
["有人對你撒嬌時，你會？",[["故意不理，逗逗他","s"],["馬上心軟","m"]]],
["哪個比較吸引你？",[["被保護","m"],["被需要","s"]]],
["做重大決定時，你？",[["一個人說了算","s"],["希望有人幫忙拿主意","m"]]],
["比賽或遊戲輸了，你會？",[["輸了也沒關係，開心就好","m"],["不甘心，下次一定要贏回來","s"]]],
["朋友通常怎麼形容你？",[["腹黑、強勢","s"],["好說話、軟萌","m"]]],
["在團體裡，你比較常扮演？",[["默默配合的人","m"],["帶頭出主意的人","s"]]]
];
const T=[
[80,"鬼畜系抖 S","你天生是掌控全場的那個人，看別人手足無措會偷偷覺得有趣。主導欲強、不輕易服輸，身邊的人常被你牽著走，卻也因此很依賴你。"],
[60,"腹黑系小 S","表面好相處，骨子裡卻喜歡握有主導權。你懂得挑時機捉弄人，嘴上不饒人，但對真正在乎的人會默默護著。"],
[41,"S/M 雙屬性","你的屬性會看對象切換：遇到強勢的人就退一步，遇到軟的人就忍不住想逗。彈性很大，在什麼關係裡都能找到舒服的位置。"],
[21,"溫柔系小 M","你習慣配合別人、不愛起衝突，被照顧、被安排會讓你安心。偶爾被吐槽，反而覺得是被在乎的證明。"],
[0,"天然系抖 M","別人的要求你幾乎照單全收，越被管越有動力。容易心軟又好說話，記得付出的同時也替自己留點空間。"]
];
const $=id=>document.getElementById(id);
let i=0,ans=[],lock=false;
function show(id){["start","quiz","result"].forEach(s=>$(s).hidden=s!==id);window.scrollTo(0,0)}
function render(){
$("prog").textContent=`第 ${i+1} / ${Q.length} 題`;
$("pbar").style.width=Math.round((i+1)/Q.length*100)+"%";
$("qt").textContent=Q[i][0];
const box=$("opts");box.innerHTML="";
Q[i][1].forEach(o=>{
const b=document.createElement("button");
b.className="opt"+(ans[i]===o[1]?" sel":"");
b.textContent=o[0];
b.onclick=()=>pick(o[1],b);
box.appendChild(b);
});
$("prev").style.visibility=i>0?"visible":"hidden";
}
function pick(k,b){
if(lock)return;lock=true;ans[i]=k;b.classList.add("sel");
setTimeout(()=>{lock=false;if(i<Q.length-1){i++;render()}else result()},220);
}
function result(){
const s=ans.filter(x=>x==="s").length;
const sp=Math.round(s/Q.length*100),mp=100-sp;
const t=T.find(x=>sp>=x[0]);
$("result").innerHTML=`
<p class="label">你的屬性是</p>
<p class="type">${t[1]}</p>
<div class="stats">
<div class="stat"><small>S 指數</small><b style="color:var(--s)">${sp}%</b></div>
<div class="stat"><small>M 指數</small><b style="color:var(--m)">${mp}%</b></div>
</div>
<div class="bar"><div style="width:${sp}%;background:var(--s)"></div><div style="width:${mp}%;background:var(--m)"></div></div>
<p class="desc">${t[2]}</p>
<div class="row"><button id="again">重新測驗</button><button id="share">分享結果</button></div>`;
$("again").onclick=()=>{ans=[];i=0;render();show("quiz")};
$("share").onclick=()=>{
const txt=`我的 SM 屬性是「${t[1]}」，S 指數 ${sp}%、M 指數 ${mp}%！你也來測：${location.href}`;
if(navigator.share){navigator.share({text:txt}).catch(()=>{})}
else if(navigator.clipboard){navigator.clipboard.writeText(txt).then(()=>{$("share").textContent="已複製"})}
};
show("result");
}
$("go").onclick=()=>{render();show("quiz")};
$("prev").onclick=()=>{if(i>0){i--;render()}};
</script>
</body>
</html>
