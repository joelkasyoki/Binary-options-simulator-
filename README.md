# Binary-options-simulator-
I want a simulator for which I can enter market and exit , correct without making loses, winning probability to be above 97% and , should be able to read the market and give the correct entry,let it be working as low as 1 dollar account, and above,
<!DOCTYPE html>
<html lang="en"><head><meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1, viewport-fit=cover">
<title>Binary Options Simulator</title>
<style>
:root{--bg:#f6f7f9;--card:#fff;--tx:#14171c;--mu:#6b7380;--bd:#e1e4e9;--up:#0f9d58;--dn:#d93a3a;--ac:#2f6df6;box-sizing:border-box;padding-top:env(safe-area-inset-top,0px);padding-bottom:env(safe-area-inset-bottom,0px)}
@media (prefers-color-scheme:dark){:root:not([data-theme="light"]){--bg:#0f1216;--card:#181c22;--tx:#eceff3;--mu:#8b93a1;--bd:#2a303a;--up:#3ccf85;--dn:#ff6b6b;--ac:#6b96ff;color-scheme:dark}}
:root[data-theme="dark"]{--bg:#0f1216;--card:#181c22;--tx:#eceff3;--mu:#8b93a1;--bd:#2a303a;--up:#3ccf85;--dn:#ff6b6b;--ac:#6b96ff;color-scheme:dark}
*{box-sizing:border-box}html{scroll-padding-top:env(safe-area-inset-top,0px)}
body{margin:0;background:var(--bg);color:var(--tx);font:15px/1.4 system-ui,-apple-system,Segoe UI,Roboto,sans-serif}
.wrap{max-width:980px;margin:0 auto;padding:14px}
.banner{background:var(--card);border:1px solid var(--bd);border-left:4px solid var(--dn);border-radius:8px;padding:10px 12px;font-size:13px;color:var(--mu);margin-bottom:12px}
.top{display:flex;justify-content:space-between;align-items:center;gap:10px;flex-wrap:wrap;margin-bottom:12px}
h1{font-size:19px;margin:0}
.bal{font-size:22px;font-weight:700}
.grid{display:grid;grid-template-columns:1fr;gap:12px}
@media(min-width:800px){.grid{grid-template-columns:2fr 1fr}}
.card{background:var(--card);border:1px solid var(--bd);border-radius:12px;padding:12px}
.card h2{font-size:13px;text-transform:uppercase;letter-spacing:.05em;color:var(--mu);margin:0 0 8px}
canvas{width:100%;height:260px;display:block}
.price{font-size:20px;font-weight:700;margin-bottom:4px}
label{display:block;font-size:12px;color:var(--mu);margin:8px 0 4px}
input[type=number]{width:100%;padding:9px;border:1px solid var(--bd);border-radius:8px;background:var(--bg);color:var(--tx);font-size:15px}
.row{display:flex;gap:6px;flex-wrap:wrap}
.chip{flex:1;padding:8px;border:1px solid var(--bd);background:var(--bg);color:var(--tx);border-radius:8px;cursor:pointer;font-size:14px}
.chip.on{border-color:var(--ac);color:var(--ac);font-weight:700}
.big{flex:1;padding:14px;border:0;border-radius:10px;color:#fff;font-size:16px;font-weight:700;cursor:pointer;margin-top:10px}
.up{background:var(--up)}.dn{background:var(--dn)}
.mu{color:var(--mu);font-size:13px}
.stats{display:grid;grid-template-columns:repeat(2,1fr);gap:8px}
.stat b{display:block;font-size:18px}
.scroll{overflow-x:auto}
table{width:100%;border-collapse:collapse;font-size:13px}
th,td{text-align:left;padding:6px 4px;border-bottom:1px solid var(--bd);white-space:nowrap}
.w{color:var(--up)}.l{color:var(--dn)}
.reset{background:none;border:1px solid var(--bd);color:var(--mu);border-radius:8px;padding:6px 10px;cursor:pointer}
</style></head><body><div class="wrap">
<div class="banner"><b>Simulation only.</b> Virtual money, live Deriv market prices, no deposits or withdrawals. Real binary options are heavily restricted and most traders lose money. Use this to learn, not to predict real results.</div>
<div class="top"><h1>Binary Options Simulator</h1><div><span class="mu">Virtual balance</span><div class="bal" id="bal"></div></div><button class="reset" id="reset">Reset</button></div>
<div class="grid">
<div>
<div class="card"><h2>Live Deriv market <span id="st" class="mu"></span></h2><select id="sym" style="margin-bottom:6px;padding:6px;border-radius:8px;border:1px solid var(--bd);background:var(--bg);color:var(--tx)"><option value="1HZ100V">Volatility 100 (1s)</option><option value="1HZ75V">Volatility 75 (1s)</option><option value="1HZ50V">Volatility 50 (1s)</option><option value="R_100">Volatility 100</option><option value="R_50">Volatility 50</option></select><div class="price" id="price"></div><canvas id="cv"></canvas></div>
<div class="card" style="margin-top:12px"><h2>Open trades</h2><div class="scroll"><table><thead><tr><th>Side</th><th>Stake</th><th>Entry</th><th>Now</th><th>Left</th></tr></thead><tbody id="open"></tbody></table></div><div class="mu" id="noopen">No open trades.</div></div>
<div class="card" style="margin-top:12px"><h2>History</h2><div class="scroll"><table><thead><tr><th>Side</th><th>Stake</th><th>Entry</th><th>Exit</th><th>Result</th></tr></thead><tbody id="hist"></tbody></table></div></div>
</div>
<div>
<div class="card"><h2>Place a trade</h2>
<label>Stake (virtual $)</label><input type="number" id="stake" value="100" min="1">
<label>Expiry</label><div class="row" id="exp"></div>
<label>Payout on win (%)</label><input type="number" id="pay" value="85" min="1" max="100">
<div class="row"><button class="big up" id="up">▲ Up</button><button class="big dn" id="dn">▼ Down</button></div>
<div class="mu" style="margin-top:8px" id="msg">A win pays stake × payout %. A loss costs the full stake. If the exit price equals the entry price, you lose (as on Deriv).</div></div>
<div class="card" style="margin-top:12px"><h2>Your stats</h2><div class="stats">
<div class="stat"><span class="mu">Trades</span><b id="s1">0</b></div><div class="stat"><span class="mu">Win rate</span><b id="s2">–</b></div>
<div class="stat"><span class="mu">Net P&amp;L</span><b id="s3">$0</b></div><div class="stat"><span class="mu">Break-even win rate</span><b id="s4">–</b></div></div></div>
<div class="card" style="margin-top:12px"><h2>Partner dashboard (demo)</h2>
<label>Partner commission (% of platform net revenue)</label><input type="number" id="cm" value="40" min="0" max="100">
<div class="stats" style="margin-top:8px"><div class="stat"><span class="mu">Platform net</span><b id="p1">$0</b></div><div class="stat"><span class="mu">Partner earns</span><b id="p2">$0</b></div></div>
<div class="mu" style="margin-top:6px">Platform net is what you lose minus what you win. If you are winning, it is negative and the partner earns nothing.</div></div>
</div></div></div>
<script>
(function(){
var START=10000,EXPS=[30,60,120,300],exp=60,price=null,series=[],bal=START,open=[],hist=[],id=0;
var $=function(i){return document.getElementById(i)};
function load(){try{var s=JSON.parse(localStorage.getItem('binsim')||'null');if(s){bal=s.bal;hist=s.hist||[]}}catch(e){}}
function save(){try{localStorage.setItem('binsim',JSON.stringify({bal:bal,hist:hist.slice(0,100)}))}catch(e){}}
function money(n){return(n<0?'-':'')+'$'+Math.abs(n).toLocaleString(undefined,{maximumFractionDigits:2})}
var eb=$('exp');EXPS.forEach(function(s){var b=document.createElement('button');b.className='chip'+(s===exp?' on':'');b.textContent=s<60?s+'s':s/60+'m';b.onclick=function(){exp=s;[].forEach.call(eb.children,function(c){c.classList.remove('on')});b.classList.add('on')};eb.appendChild(b)});
var WS_URL='wss://api.derivws.com/trading/v1/options/ws/public',ws=null,subId=null,retry=0,sym='1HZ100V';
function setSt(t){$('st').textContent=t?'· '+t:''}
function connect(){
try{ws=new WebSocket(WS_URL)}catch(e){setSt('connection failed');return}
var mine=ws;setSt('connecting…');
ws.onopen=function(){retry=0;setSt('live');mine.send(JSON.stringify({ticks:sym,subscribe:1}))};
ws.onmessage=function(m){var d;try{d=JSON.parse(m.data)}catch(e){return}
if(mine!==ws)return;
if(d.error){setSt(d.error.message||'error');return}
if(d.tick&&d.tick.symbol===sym){subId=d.subscription&&d.subscription.id||subId;onTick(d.tick.quote)}};
ws.onclose=function(){if(mine!==ws)return;setSt('reconnecting…');setTimeout(connect,Math.min(10000,1000*Math.pow(2,retry++)))};
ws.onerror=function(){try{mine.close()}catch(e){}}}
function onTick(q){price=Number(q);series.push(price);if(series.length>240)series.shift();render()}
$('sym').onchange=function(){sym=this.value;series=[];price=null;open.forEach(function(t){bal+=t.stake});open=[];save();
if(ws&&ws.readyState===1){if(subId)ws.send(JSON.stringify({forget:subId}));subId=null;ws.send(JSON.stringify({ticks:sym,subscribe:1}))}render()};
function tick(){if(price===null)return;
var now=Date.now();open=open.filter(function(t){if(now<t.end)return true;settle(t);return false});render()}
function settle(t){var r,pl;
{var win=t.dir==='up'?price>t.entry:price<t.entry;if(win){pl=t.stake*t.pay/100;bal+=t.stake+pl;r='Win'}else{pl=-t.stake;r='Loss'}}
hist.unshift({dir:t.dir,stake:t.stake,entry:t.entry,exit:price,r:r,pl:pl,pay:t.pay});save()}
function trade(dir){if(price===null){$('msg').textContent='Waiting for live price…';return}
var st=parseFloat($('stake').value),py=parseFloat($('pay').value);
if(!(st>0)){$('msg').textContent='Enter a stake above 0.';return}
if(st>bal){$('msg').textContent='Not enough virtual balance.';return}
if(!(py>0&&py<=100)){$('msg').textContent='Payout must be 1–100%.';return}
bal-=st;open.push({id:++id,dir:dir,stake:st,pay:py,entry:price,end:Date.now()+exp*1000});$('msg').textContent='Trade placed: '+dir.toUpperCase()+' '+money(st)+'.';save();render()}
$('up').onclick=function(){trade('up')};$('dn').onclick=function(){trade('down')};
$('reset').onclick=function(){bal=START;hist=[];open=[];save();render()};
$('cm').oninput=render;$('pay').oninput=render;
function row(c){return'<tr>'+c.map(function(x){return'<td'+(x[1]?' class="'+x[1]+'"':'')+'>'+x[0]+'</td>'}).join('')+'</tr>'}
function render(){
$('bal').textContent=money(bal);$('price').textContent=price===null?'…':price.toFixed(price>1000?2:4);
var now=Date.now();
$('open').innerHTML=open.map(function(t){var w=price!==null&&(t.dir==='up'?price>t.entry:price<t.entry);return row([[t.dir==='up'?'▲ Up':'▼ Down',t.dir==='up'?'w':'l'],[money(t.stake)],[t.entry.toFixed(5)],[price.toFixed(5),w?'w':'l'],[Math.max(0,Math.ceil((t.end-now)/1000))+'s']])}).join('');
$('noopen').style.display=open.length?'none':'block';
$('hist').innerHTML=hist.slice(0,15).map(function(h){return row([[h.dir==='up'?'▲ Up':'▼ Down'],[money(h.stake)],[h.entry.toFixed(5)],[h.exit.toFixed(5)],[h.r+' '+(h.pl>=0?'+':'')+money(h.pl),h.r==='Win'?'w':h.r==='Loss'?'l':'']])}).join('');
var n=hist.length,wins=hist.filter(function(h){return h.r==='Win'}).length,dec=hist.filter(function(h){return h.r!=='Tie'}).length,net=hist.reduce(function(a,h){return a+h.pl},0);
$('s1').textContent=n;$('s2').textContent=dec?Math.round(wins/dec*100)+'%':'–';
$('s3').textContent=money(net);$('s3').className=net>=0?'w':'l';
var py=parseFloat($('pay').value)||0;$('s4').textContent=py>0?(100/(100+py)*100).toFixed(1)+'%':'–';
var house=-net,cm=parseFloat($('cm').value)||0;
$('p1').textContent=money(house);$('p2').textContent=money(Math.max(0,house)*cm/100);
draw()}
function draw(){var c=$('cv'),r=window.devicePixelRatio||1,w=c.clientWidth,h=c.clientHeight;
if(c.width!==w*r){c.width=w*r;c.height=h*r}
var g=c.getContext('2d');g.setTransform(r,0,0,r,0,0);g.clearRect(0,0,w,h);
var cs=getComputedStyle(document.documentElement),ac=cs.getPropertyValue('--ac').trim()||'#2f6df6',bd=cs.getPropertyValue('--bd').trim()||'#ccc';
if(series.length<2)return;var mn=Math.min.apply(null,series),mx=Math.max.apply(null,series),pad=(mx-mn)*.1||.0005;mn-=pad;mx+=pad;
function Y(v){return h-(v-mn)/(mx-mn)*h}
g.strokeStyle=bd;g.lineWidth=1;for(var i=1;i<5;i++){g.beginPath();g.moveTo(0,h*i/5);g.lineTo(w,h*i/5);g.stroke()}
g.strokeStyle=ac;g.lineWidth=2;g.beginPath();series.forEach(function(v,i){var x=i/(series.length-1)*w;i?g.lineTo(x,Y(v)):g.moveTo(x,Y(v))});g.stroke();
open.forEach(function(t){g.strokeStyle=t.dir==='up'?'#0f9d58':'#d93a3a';g.setLineDash([5,4]);g.beginPath();g.moveTo(0,Y(t.entry));g.lineTo(w,Y(t.entry));g.stroke();g.setLineDash([])})}
load();render();connect();setInterval(tick,500);window.addEventListener('resize',draw);
})();
</script></body></html>
