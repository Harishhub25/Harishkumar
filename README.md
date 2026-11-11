<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>HK – Cab Booking</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link href="https://fonts.googleapis.com/css2?family=Bricolage+Grotesque:wght@500;700;800&family=Figtree:wght@400;500;600&display=swap" rel="stylesheet">
<style>
:root{--bg:#f4f3ee;--card:#fff;--ink:#1b2340;--mute:#6a7088;--line:#e2e1da;--y:#ffb300;--yt:#1b2340;--ok:#1f9d63;--bad:#d64545}
@media(prefers-color-scheme:dark){:root:not([data-t=light]){--bg:#10142a;--card:#1a2040;--ink:#eef0fa;--mute:#9aa1c4;--line:#2b3260}}
:root[data-t=dark]{--bg:#10142a;--card:#1a2040;--ink:#eef0fa;--mute:#9aa1c4;--line:#2b3260}
*{box-sizing:border-box;margin:0}
body{font:15px/1.45 Figtree,system-ui,sans-serif;background:var(--bg);color:var(--ink)}
h1,h2,h3,.big{font-family:'Bricolage Grotesque',system-ui,sans-serif;letter-spacing:-.02em}
header{display:flex;align-items:center;gap:12px;padding:12px 16px;border-bottom:1px solid var(--line);position:sticky;top:0;background:var(--bg);z-index:5;flex-wrap:wrap}
.logo{font:800 22px 'Bricolage Grotesque';background:var(--y);color:var(--yt);padding:2px 10px;border-radius:6px;transform:skew(-6deg)}
nav{display:flex;gap:4px;margin-left:auto}
button,select,input{font:inherit;color:inherit}
nav button,.chip{border:1px solid var(--line);background:var(--card);padding:7px 14px;border-radius:99px;cursor:pointer}
nav button[aria-pressed=true],.chip.on{background:var(--ink);color:var(--bg);border-color:var(--ink)}
button:focus-visible,select:focus-visible,input:focus-visible{outline:3px solid var(--y);outline-offset:2px}
main{max-width:1000px;margin:0 auto;padding:16px;display:grid;gap:16px}
.rider{display:grid;gap:16px;grid-template-columns:minmax(0,420px) 1fr;align-items:start}
@media(max-width:760px){.rider{display:grid;gap:16px;grid-template-columns:1fr}}
.card{background:var(--card);border:1px solid var(--line);border-radius:14px;padding:16px}
label{display:block;font-size:13px;color:var(--mute);margin:10px 0 4px}
select,input{width:100%;padding:11px;border:1px solid var(--line);border-radius:10px;background:var(--bg)}
.row{display:flex;gap:8px;flex-wrap:wrap;margin:10px 0}
.cab{display:flex;align-items:center;gap:12px;padding:12px;border:2px solid var(--line);border-radius:12px;margin:8px 0;cursor:pointer;width:100%;background:none;text-align:left}
.cab[aria-checked=true]{border-color:var(--y);background:color-mix(in srgb,var(--y) 14%,transparent)}
.cab .ic{font-size:28px}.cab .p{margin-left:auto;font:700 18px 'Bricolage Grotesque'}
.cab small{color:var(--mute);display:block}
.cta{width:100%;background:var(--y);color:var(--yt);border:0;border-radius:12px;padding:14px;font:700 16px 'Bricolage Grotesque';cursor:pointer;margin-top:12px}
.cta.alt{background:var(--card);color:var(--ink);border:1px solid var(--line)}
.fare td{padding:3px 0}.fare td:last-child{text-align:right}.fare tr:last-child{font-weight:700;border-top:1px solid var(--line)}
table{width:100%;border-collapse:collapse}
.tbl{overflow-x:auto}.tbl td,.tbl th{padding:8px;border-bottom:1px solid var(--line);text-align:left;white-space:nowrap}
.map{width:100%;border-radius:14px;border:1px solid var(--line);background:#dfe8dd}
@media(prefers-color-scheme:dark){:root:not([data-t=light]) .map{background:#1d2a3a}}
:root[data-t=dark] .map{background:#1d2a3a}
.kpis{display:grid;grid-template-columns:repeat(auto-fit,minmax(150px,1fr));gap:10px}
.kpi b{display:block;font:800 26px 'Bricolage Grotesque'}
.bars{display:flex;align-items:flex-end;gap:6px;height:120px}
.bars i{flex:1;background:var(--y);border-radius:4px 4px 0 0;position:relative;min-height:4px}
.bars i span{position:absolute;bottom:-18px;left:0;right:0;text-align:center;font-size:10px;color:var(--mute);font-style:normal}
.tag{display:inline-block;padding:2px 9px;border-radius:99px;font-size:12px;background:var(--line)}
.ok{background:var(--ok);color:#fff}.bad{background:var(--bad);color:#fff}
.stars button{font-size:28px;background:none;border:0;cursor:pointer;opacity:.35}.stars button.on{opacity:1}
.sos{background:var(--bad);color:#fff;border:0;border-radius:99px;padding:8px 14px;cursor:pointer}
.note{font-size:12px;color:var(--mute)}
[hidden]{display:none!important}
</style>
</head>
<body>
<header>
  <span class="logo">Chalo</span>
  <nav aria-label="Switch app">
    <button data-v="rider" aria-pressed="true">Rider</button>
    <button data-v="driver" aria-pressed="false">Driver</button>
    <button data-v="admin" aria-pressed="false">Admin</button>
    <button id="theme" aria-label="Toggle dark mode">◐</button>
  </nav>
</header>
<main id="app"></main>

<script>
/* ===== Data (MOCK: replace with API calls to your backend) ===== */
const PLACES={"Anna Nagar":[13.085,80.21],"T. Nagar":[13.041,80.234],"Adyar":[13.001,80.257],"Velachery":[12.978,80.221],"Chennai Airport (MAA)":[12.994,80.17],"Central Station":[13.082,80.275],"Egmore":[13.078,80.26],"Guindy":[13.007,80.22],"Sholinganallur (OMR)":[12.9,80.228],"Marina Beach":[13.05,80.283],"Pondicherry (outstation)":[11.934,79.83]};
const CATS=[
 {id:"auto",n:"Auto",ic:"🛺",cap:3,lug:"1 bag",base:30,km:11,min:1},
 {id:"mini",n:"Mini",ic:"🚗",cap:4,lug:"2 bags",base:50,km:12,min:1.2},
 {id:"sedan",n:"Sedan",ic:"🚙",cap:4,lug:"3 bags",base:70,km:15,min:1.5},
 {id:"suv",n:"SUV",ic:"🚐",cap:6,lug:"4 bags",base:100,km:20,min:2},
 {id:"prem",n:"Premium",ic:"🏎️",cap:4,lug:"3 bags",base:150,km:28,min:3}];
const TYPES={city:"City ride",airport:"Airport",rental:"Rental (4 hr)",out:"Outstation",round:"Round trip"};
const DRIVERS=[{n:"Murugan S.",car:"Maruti Dzire · TN 09 BX 4412",r:4.8},{n:"Kavitha R.",car:"Hyundai Xcent · TN 10 CD 2290",r:4.9},{n:"Imran K.",car:"Toyota Innova · TN 07 AK 9081",r:4.7}];
const PAY=["UPI","Credit/Debit card","Net banking","Wallet","Cash"];
const ld=(k,d)=>{try{return JSON.parse(localStorage.getItem(k))??d}catch{return d}};
const sv=(k,v)=>{try{localStorage.setItem(k,JSON.stringify(v))}catch{}};
let cfg=ld("cfg",{comm:20,surge:true,night:true}),hist=ld("hist",seed()),req=ld("req",null);
function seed(){const h=[];const a=Object.keys(PLACES);for(let i=0;i<24;i++)h.push({id:"B"+(1000+i),from:a[i%7],to:a[(i*3+2)%a.length],cat:CATS[i%5].n,fare:120+((i*97)%600),st:i%9==0?"Cancelled":"Completed",hr:[8,9,18,19,10,17,20,13][i%8],mock:1});return h}

/* ===== Fare engine ===== */
const hav=(a,b)=>{const R=6371,t=x=>x*Math.PI/180,dl=t(b[0]-a[0]),dn=t(b[1]-a[1]);const x=Math.sin(dl/2)**2+Math.cos(t(a[0]))*Math.cos(t(b[0]))*Math.sin(dn/2)**2;return 2*R*Math.asin(Math.sqrt(x))*1.35};
function fare(c,km,type,hr){
 const min=Math.round(km*2.6+3);let dist=km,b=c.base,extra=[];
 if(type=="round")dist=km*2*.9;
 if(type=="out"){dist=Math.max(km,80);extra.push(["Driver allowance",300])}
 if(type=="airport")extra.push(["Airport charge",100]);
 let rows=[["Base fare",b],["Distance ("+dist.toFixed(1)+" km × ₹"+c.km+")",dist*c.km]];
 if(type=="rental")rows=[["Rental 4 hr package",c.base*4+c.km*10*4]];
 else rows.push(["Time ("+min+" min)",min*c.min]);
 rows=rows.concat(extra);
 let sub=rows.reduce((s,r)=>s+r[1],0);
 if(cfg.night&&(hr>=22||hr<5)){rows.push(["Night charge (25%)",sub*.25]);sub*=1.25}
 const pk=(hr>=8&&hr<10)||(hr>=17&&hr<20);
 if(cfg.surge&&pk&&type!="rental"){rows.push(["Peak-hour demand (1.2×)",sub*.2]);sub*=1.2}
 rows.push(["Platform fee",12]);sub+=12;
 rows.push(["GST (5%)",sub*.05]);sub*=1.05;
 return{rows:rows.map(r=>[r[0],Math.round(r[1])]),total:Math.round(sub),min,dist}}

/* ===== State ===== */
let view="rider",B={from:"Anna Nagar",to:"Chennai Airport (MAA)",type:"city",cat:"sedan",pay:"UPI",when:"now",stage:"plan",rate:0};
const $=s=>document.querySelector(s),app=$("#app");
const inr=n=>"₹"+Math.round(n).toLocaleString("en-IN");
const opts=(o,v)=>Object.keys(o).map(k=>`<option ${k==v?"selected":""}>${k}</option>`).join("");
function mapSvg(a,b,p){
 const all=Object.values(PLACES).filter(x=>x[0]>12.85),la=all.map(x=>x[0]),lo=all.map(x=>x[1]);
 const mn=[Math.min(...la),Math.min(...lo)],mx=[Math.max(...la),Math.max(...lo)];
 const P=c=>{const x=Math.max(20,Math.min(380,20+(c[1]-mn[1])/(mx[1]-mn[1])*360)),y=Math.max(20,Math.min(280,280-(c[0]-mn[0])/(mx[0]-mn[0])*260));return[x,y]};
 const A=P(PLACES[a]),Z=P(PLACES[b]),M=[(A[0]+Z[0])/2+30,(A[1]+Z[1])/2-30];
 const car=p==null?"":(()=>{const t=p,x=(1-t)**2*A[0]+2*(1-t)*t*M[0]+t*t*Z[0],y=(1-t)**2*A[1]+2*(1-t)*t*M[1]+t*t*Z[1];return`<text x="${x}" y="${y}" font-size="22" text-anchor="middle">🚖</text>`})();
 return`<svg class="map" viewBox="0 0 400 300" role="img" aria-label="Route map from ${a} to ${b}"><path d="M0 230 Q100 200 200 240 T400 210" stroke="#9ec3e6" stroke-width="22" fill="none" opacity=".5"/><path d="M${A} Q${M} ${Z}" stroke="#1b2340" stroke-width="4" fill="none" stroke-dasharray="2 7" stroke-linecap="round"/><circle cx="${A[0]}" cy="${A[1]}" r="8" fill="#1f9d63"/><rect x="${Z[0]-8}" y="${Z[1]-8}" width="16" height="16" fill="#d64545"/>${car}</svg>`}

/* ===== Rider ===== */
function rider(){
 const km=hav(PLACES[B.from],PLACES[B.to]),hr=new Date().getHours();
 const list=CATS.map(c=>({c,f:fare(c,km,B.type,hr)}));
 const sel=list.find(x=>x.c.id==B.cat)||list[2];
 let left="";
 if(B.stage=="plan")left=`
 <h1 style="font-size:28px">Where would you like to go?</h1>
 <label for="f">Pickup</label><select id="f">${opts(PLACES,B.from)}</select>
 <label for="t">Drop</label><select id="t">${opts(PLACES,B.to)}</select>
 <div class="row" role="group" aria-label="Trip type">${Object.entries(TYPES).map(([k,v])=>`<button class="chip ${k==B.type?"on":""}" data-type="${k}">${v}</button>`).join("")}</div>
 <p class="note">${km.toFixed(1)} km · about ${sel.f.min} min · ${B.from==B.to?"Pick a different drop":"Fares update live"}</p>
 <div role="radiogroup" aria-label="Cab category">${list.map(({c,f})=>`<button class="cab" role="radio" aria-checked="${c.id==B.cat}" data-cat="${c.id}"><span class="ic">${c.ic}</span><span><b>${c.n}</b><small>${c.cap} seats · ${c.lug} · ${Math.max(2,Math.round(c.base/25))} min away</small><small>₹${c.km}/km</small></span><span class="p">${inr(f.total)}</span></button>`).join("")}</div>
 <label for="w">When</label><select id="w"><option value="now">Now</option><option value="later" ${B.when=="later"?"selected":""}>Schedule for later</option></select>
 ${B.when=="later"?`<input type="datetime-local" aria-label="Pickup time">`:""}
 <label for="pay">Payment</label><select id="pay">${PAY.map(p=>`<option ${p==B.pay?"selected":""}>${p}</option>`).join("")}</select>
 <details class="card" style="margin-top:12px" open><summary><b>Fare breakdown · ${inr(sel.f.total)}</b></summary><table class="fare">${sel.f.rows.map(r=>`<tr><td>${r[0]}</td><td>${inr(r[1])}</td></tr>`).join("")}<tr><td>Total</td><td>${inr(sel.f.total)}</td></tr></table></details>
 <p class="note">Free cancellation within 2 minutes of booking. Fare is final unless you add stops or tolls.</p>
 <button class="cta" id="go" ${B.from==B.to?"disabled":""}>Book ${sel.c.n} · ${inr(sel.f.total)}</button>
 ${hist.filter(h=>!h.mock).slice(0,2).map(h=>`<button class="cta alt" data-rep="${h.id}">Repeat: ${h.from} → ${h.to}</button>`).join("")}`;
 if(B.stage=="search")left=`<h2>Finding your driver…</h2><p class="note">Sending your request to nearby drivers. Open the Driver tab (or another browser tab) to accept it yourself.</p><button class="cta alt" id="cancel">Cancel request</button>`;
 if(B.stage=="track"||B.stage=="trip"){const d=B.drv;left=`<span class="tag ok">${B.stage=="track"?"Driver arriving":"On trip"}</span><h2 style="margin:8px 0">${d.n} · ★ ${d.r}</h2><p>${d.car}</p><p class="note">OTP for pickup: <b>${B.otp}</b> · ETA ${B.stage=="track"?Math.max(1,Math.round((1-B.p)*5)):Math.max(1,Math.round((1-B.p)*sel.f.min))} min</p><div class="row"><button class="chip">📞 Call</button><button class="chip" id="share">Share trip</button><button class="sos" id="sos">SOS</button></div><button class="cta alt" id="cancel">Cancel ride</button>`}
 if(B.stage=="done")left=`<h2>Trip complete</h2><p>You paid <b>${inr(B.paid)}</b> via ${B.pay}. Receipt ${B.id} is saved in Admin → Bookings.</p><p>Rate ${B.drv.n}</p><div class="stars">${[1,2,3,4,5].map(i=>`<button class="${i<=B.rate?"on":""}" data-star="${i}" aria-label="${i} stars">★</button>`).join("")}</div><button class="cta" id="again">Book another ride</button>`;
 return`<section class="rider"><div class="card">${left}</div><div>${mapSvg(B.from,B.to,B.stage=="track"?B.p*.3:B.stage=="trip"?.3+B.p*.7:null)}<div class="card" style="margin-top:12px"><b>Recent rides</b><div class="tbl"><table>${hist.slice(0,4).map(h=>`<tr><td>${h.from} → ${h.to}</td><td>${h.cat}</td><td>${inr(h.fare)}</td></tr>`).join("")}</table></div></div></div></section>`}

function bindRider(){
 const g=(s,f)=>{const e=$(s);e&&(e.onchange=e.onclick=f)};
 $("#f")&&($("#f").onchange=e=>{B.from=e.target.value;render()});
 $("#t")&&($("#t").onchange=e=>{B.to=e.target.value;if(/outstation/.test(B.to))B.type="out";render()});
 $("#w")&&($("#w").onchange=e=>{B.when=e.target.value;render()});
 $("#pay")&&($("#pay").onchange=e=>{B.pay=e.target.value});
 document.querySelectorAll("[data-type]").forEach(b=>b.onclick=()=>{B.type=b.dataset.type;if(B.type=="airport")B.to="Chennai Airport (MAA)";render()});
 document.querySelectorAll("[data-cat]").forEach(b=>b.onclick=()=>{B.cat=b.dataset.cat;render()});
 document.querySelectorAll("[data-rep]").forEach(b=>b.onclick=()=>{const h=hist.find(x=>x.id==b.dataset.rep);B.from=h.from;B.to=h.to;render()});
 document.querySelectorAll("[data-star]").forEach(b=>b.onclick=()=>{B.rate=+b.dataset.star;render()});
 $("#go")&&($("#go").onclick=book);
 $("#cancel")&&($("#cancel").onclick=()=>{clearInterval(B.t);req=null;sv("req",null);B.stage="plan";render()});
 $("#again")&&($("#again").onclick=()=>{B.stage="plan";render()});
 $("#sos")&&($("#sos").onclick=()=>alert("SOS sent (mock): live location shared with your emergency contacts and Chalo safety team."));
 $("#share")&&($("#share").onclick=()=>alert("Trip link copied (mock): chalo.example/t/"+B.id));
}
function book(){
 const km=hav(PLACES[B.from],PLACES[B.to]),c=CATS.find(x=>x.id==B.cat),f=fare(c,km,B.type,new Date().getHours());
 B.id="B"+Date.now().toString().slice(-6);B.paid=f.total;B.catN=c.n;B.otp=String(1000+Math.floor(Math.random()*9000));B.stage="search";
 req={id:B.id,from:B.from,to:B.to,cat:c.n,fare:f.total,km:+km.toFixed(1),accepted:null};sv("req",req);render();
 setTimeout(()=>{ if(B.stage=="search"&&!(ld("req",{})||{}).accepted)accept(DRIVERS[Math.floor(Math.random()*3)],true)},4500)}
function accept(d,auto){ if(B.stage!="search")return;B.drv=d;B.stage="track";B.p=0;req=null;sv("req",null);render();
 B.t=setInterval(()=>{B.p+=.04;if(B.p>=1){B.p=0;if(B.stage=="track")B.stage="trip";else{clearInterval(B.t);B.stage="done";hist.unshift({id:B.id,from:B.from,to:B.to,cat:B.catN,fare:B.paid,st:"Completed",hr:new Date().getHours()});sv("hist",hist)}}render()},700)}

/* ===== Driver ===== */
let online=true,earn=ld("earn",1840);
function driver(){
 const r=ld("req",null);
 return`<div class="card"><div class="row" style="justify-content:space-between;align-items:center"><h2>Driver · Murugan S.</h2><button class="chip ${online?"on":""}" id="on">${online?"Online":"Offline"}</button></div>
 <div class="kpis"><div class="card kpi"><b>${inr(earn)}</b>Today</div><div class="card kpi"><b>${inr(earn*6.2)}</b>This week</div><div class="card kpi"><b>★ 4.8</b>Rating</div><div class="card kpi"><b>${inr(500)}</b>Bonus: 3 more trips</div></div></div>
 <div class="card"><h3>Ride request</h3>${online&&r?`<p><b>${r.from} → ${r.to}</b></p><p>${r.cat} · ${r.km} km · you earn ${inr(r.fare*(1-cfg.comm/100))} (after ${cfg.comm}% commission)</p><div class="row"><button class="cta" id="acc" style="flex:1">Accept</button><button class="cta alt" id="rej" style="flex:1">Reject</button></div>`:`<p class="note">${online?"No requests nearby. Book a ride in the Rider tab, or in another browser tab, and it will appear here.":"Go online to receive requests."}</p>`}</div>
 <div class="card"><h3>Documents</h3><p>Driving licence <span class="tag ok">Verified</span> RC book <span class="tag ok">Verified</span> Insurance <span class="tag">Expires in 41 days</span></p><button class="sos" onclick="alert('Emergency assistance requested (mock).')">Emergency help</button></div>`}
function bindDriver(){
 $("#on").onclick=()=>{online=!online;render()};
 $("#acc")&&($("#acc").onclick=()=>{const r=ld("req",null);if(!r)return;r.accepted=1;sv("req",r);earn+=Math.round(r.fare*(1-cfg.comm/100));sv("earn",earn);
  if(B.stage=="search")accept(DRIVERS[0]);else{sv("req",null);}render()});
 $("#rej")&&($("#rej").onclick=()=>{render()})}

/* ===== Admin ===== */
function admin(){
 const done=hist.filter(h=>h.st=="Completed"),rev=done.reduce((s,h)=>s+h.fare,0),canc=Math.round((hist.length-done.length)/hist.length*100);
 const hrs=[...Array(24).keys()].slice(6),cnt=hrs.map(h=>hist.filter(x=>x.hr==h).length),mx=Math.max(1,...cnt);
 const routes={};done.forEach(h=>{const k=h.from+" → "+h.to;routes[k]=(routes[k]||0)+1});
 return`<div class="kpis"><div class="card kpi"><b>${hist.length}</b>Total bookings</div><div class="card kpi"><b>${inr(rev)}</b>Revenue</div><div class="card kpi"><b>${inr(rev*cfg.comm/100)}</b>Commission</div><div class="card kpi"><b>${canc}%</b>Cancelled</div><div class="card kpi"><b>${inr(rev/Math.max(1,done.length))}</b>Avg booking</div></div>
 <div class="card"><h3>Peak booking hours</h3><div class="bars" style="margin-bottom:24px">${cnt.map((c,i)=>`<i style="height:${c/mx*100}%" title="${hrs[i]}:00 · ${c}"><span>${hrs[i]}</span></i>`).join("")}</div></div>
 <div class="card"><h3>Fare configuration (applies to Rider fares instantly)</h3><div class="row"><label>Commission % <input id="comm" type="number" min="0" max="40" value="${cfg.comm}" style="width:90px"></label><label><input type="checkbox" id="sg" ${cfg.surge?"checked":""} style="width:auto"> Peak surge</label><label><input type="checkbox" id="ng" ${cfg.night?"checked":""} style="width:auto"> Night charge</label></div></div>
 <div class="card tbl"><div class="row" style="justify-content:space-between"><h3>Bookings</h3><button class="chip" id="csv">Download CSV</button></div><table><tr><th>ID<th>Route<th>Cab<th>Fare<th>Status</tr>${hist.slice(0,12).map(h=>`<tr><td>${h.id}<td>${h.from} → ${h.to}<td>${h.cat}<td>${inr(h.fare)}<td><span class="tag ${h.st=="Completed"?"ok":"bad"}">${h.st}</span></tr>`).join("")}</table></div>
 <div class="card"><h3>Popular routes</h3>${Object.entries(routes).sort((a,b)=>b[1]-a[1]).slice(0,3).map(r=>`<p>${r[0]} · ${r[1]} trips</p>`).join("")}</div>`}
function bindAdmin(){
 $("#comm").onchange=e=>{cfg.comm=+e.target.value;sv("cfg",cfg);render()};
 $("#sg").onchange=e=>{cfg.surge=e.target.checked;sv("cfg",cfg)};
 $("#ng").onchange=e=>{cfg.night=e.target.checked;sv("cfg",cfg)};
 $("#csv").onclick=()=>{const c="id,from,to,cat,fare,status\n"+hist.map(h=>[h.id,h.from,h.to,h.cat,h.fare,h.st].join(",")).join("\n");const a=document.createElement("a");a.href=URL.createObjectURL(new Blob([c],{type:"text/csv"}));a.download="bookings.csv";a.click()}}

/* ===== Shell ===== */
function render(){
 const fx=document.activeElement&&document.activeElement.id;
 app.innerHTML=view=="rider"?rider():view=="driver"?driver():admin();
 (view=="rider"?bindRider:view=="driver"?bindDriver:bindAdmin)();
 fx&&$("#"+fx)&&$("#"+fx).focus()}
document.querySelectorAll("nav button[data-v]").forEach(b=>b.onclick=()=>{view=b.dataset.v;document.querySelectorAll("nav button[data-v]").forEach(x=>x.setAttribute("aria-pressed",x==b));render()});
$("#theme").onclick=()=>{const d=document.documentElement;d.dataset.t=(d.dataset.t=="dark"||(!d.dataset.t&&matchMedia("(prefers-color-scheme:dark)").matches))?"light":"dark"};
addEventListener("storage",()=>{hist=ld("hist",hist);req=ld("req",null);if(view=="driver"||view=="admin")render()});
render();
</script>
</body>
</html>
