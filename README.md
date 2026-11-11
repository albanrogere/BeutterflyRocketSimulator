<!DOCTYPE html>
<html lang="fr">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1, viewport-fit=cover">
<title>Beutterfly Rocket Simulator</title>
<style>
:root{--bg:#080A0E;--bg2:#10141B;--panel:#151A23;--bd:#252D38;--tx:#F2F4F8;--tx2:#929BA9;--cy:#00D9FF;--vi:#806BFF;--wa:#FFB547;--er:#FF5364;--ok:#39D98A;color-scheme:dark;box-sizing:border-box;padding-top:env(safe-area-inset-top,0px);padding-bottom:env(safe-area-inset-bottom,0px)}
*{box-sizing:border-box}
html,body{height:100%;margin:0;background:var(--bg);color:var(--tx);font:13px/1.45 "Segoe UI",system-ui,-apple-system,Roboto,sans-serif}
#app{display:grid;grid-template-columns:52px 1fr 310px;height:100%}
aside{background:var(--bg2);border-right:1px solid var(--bd);display:flex;flex-direction:column;align-items:center;padding-top:12px}
main{display:grid;grid-template-rows:1fr 300px;min-width:0;min-height:0}
#vp{position:relative;min-height:0;background:radial-gradient(ellipse at 50% 30%,#10141B,#080A0E)}
#cv{width:100%;height:100%;display:block;touch-action:none;cursor:grab}
.lbl{position:absolute;font-size:11px;color:var(--cy);background:#080A0Ecc;border:1px solid var(--bd);padding:1px 5px;border-radius:3px;pointer-events:none;transform:translate(8px,-50%);white-space:nowrap}
#tools{position:absolute;top:10px;left:10px;display:flex;gap:6px;flex-wrap:wrap;max-width:calc(100% - 20px)}
button,select{background:var(--panel);color:var(--tx);border:1px solid var(--bd);border-radius:5px;padding:5px 10px;font:inherit;cursor:pointer}
button:hover{border-color:var(--cy)}button:focus-visible,select:focus-visible,input:focus-visible{outline:2px solid var(--cy);outline-offset:1px}
button.on{border-color:var(--cy);color:var(--cy)}
#title{position:absolute;left:12px;bottom:10px;color:var(--tx2);font-size:12px;pointer-events:none}
#sim{background:var(--bg2);border-top:1px solid var(--bd);display:grid;grid-template-rows:auto 1fr;min-height:0}
#ctl{display:flex;gap:8px;align-items:center;padding:8px 12px;flex-wrap:wrap;border-bottom:1px solid var(--bd)}
#ctl input[type=range]{flex:1;min-width:120px}
#ro{color:var(--tx2);font-variant-numeric:tabular-nums}
#ro b{color:var(--tx);font-weight:600}
#chart{min-height:0}
#side{background:var(--bg2);border-left:1px solid var(--bd);overflow:auto;padding:14px}
h2{font-size:12px;font-weight:600;color:var(--tx2);margin:16px 0 8px}h2:first-child{margin-top:0}
.row{margin-bottom:9px}.row label{display:flex;justify-content:space-between}.row span{color:var(--cy);font-variant-numeric:tabular-nums}
input[type=range]{width:100%;accent-color:var(--cy)}
.msg{font-size:12px;padding:5px 8px;border-radius:4px;margin-top:4px;border:1px solid var(--bd)}
.msg.w{color:var(--wa);border-color:#FFB54766}.msg.e{color:var(--er);border-color:#FF536466}.msg.ok{color:var(--ok);border-color:#39D98A55}
#info{background:var(--panel);border:1px solid var(--bd);border-radius:6px;padding:10px;font-size:12px}
#info dt{color:var(--tx2)}#info dd{margin:0 0 5px}
.note{color:var(--tx2);font-size:11px;margin-top:14px}
@media(max-width:900px){#app{grid-template-columns:1fr;grid-template-rows:auto;height:auto}aside{display:none}main{grid-template-rows:60vh 320px}#side{border-left:0}}
details{border:1px solid var(--bd);border-radius:6px;margin:10px 0;padding:6px 10px;background:var(--panel)}summary{cursor:pointer;font-weight:600;color:var(--cy)}textarea,input[type=number],input[type=text]{background:#0c0f14;color:var(--tx);border:1px solid var(--bd);border-radius:5px;padding:5px;font:inherit}#lp button{padding:3px 7px;font-size:12px}[hidden]{display:none!important}#detail{position:absolute;top:52px;right:10px;bottom:10px;width:min(740px,calc(100% - 20px));background:#10141Bf2;border:1px solid var(--bd);border-radius:8px;overflow:auto;padding:12px;z-index:5}#detail header{display:flex;justify-content:space-between;align-items:center;margin-bottom:10px}.dg{display:grid;grid-template-columns:1fr 1fr;gap:12px;margin-bottom:12px}#dc{width:100%;height:230px;display:block;border:1px solid var(--bd);border-radius:6px;touch-action:none;cursor:grab}.cap{color:var(--tx2);font-size:11px;margin-top:4px}#dfiche dt{color:var(--tx2)}#dfiche dd{margin:0 0 6px}@media(max-width:700px){.dg{grid-template-columns:1fr}}
</style>
</head>
<body>
<div id="app">
<aside><svg width="30" height="30" viewBox="0 0 32 32" fill="none" stroke="#00D9FF" stroke-width="1.6" aria-label="Beutterfly"><path d="M16 5 L16 27"/><path d="M16 14 C10 4 3 8 5 15 C6 19 12 19 16 16 Z"/><path d="M16 14 C22 4 29 8 27 15 C26 19 20 19 16 16 Z"/><ellipse cx="16" cy="17" rx="13" ry="5" stroke="#806BFF" stroke-width="1" transform="rotate(-20 16 17)"/></svg></aside>
<main>
<div id="vp"><canvas id="cv"></canvas>
<div id="tools"><button data-v="p">Perspective</button><button data-v="f">Face</button><button data-v="s">Profil</button><button data-v="t">Dessus</button><button data-v="b">Dessous</button><button data-v="r">Arrière</button><button data-v="i">Isométrique</button><button id="xr" class="on">Corps transparent</button><button id="wf">Fil de fer</button><button id="ex">Vue éclatée</button></div>
<div id='detail' hidden><header><b id='dtitle'></b><button id='dclose'>Fermer</button></header><div class='dg'><div id='dfiche'></div><div><canvas id='dc'></canvas><div class='cap'>Rendu 3D isolé · faites glisser pour tourner</div></div></div><div id='dplan'></div></div><div id="title">BEUTTERFLY ROCKET SIMULATOR · modèle éducatif</div></div>
<section id="sim"><div id="ctl"><button id="run">Lancer</button><button id="pause">Pause</button><button id="rst">Réinitialiser</button>
<select id="spd" aria-label="Vitesse"><option value="1">×1</option><option value="5" selected>×5</option><option value="20">×20</option></select>
<input type="range" id="tl" min="0" max="1000" value="1000" aria-label="Chronologie"><span id="ro"></span>
<button id="png">PNG</button><button id="csv">CSV</button></div><div id="chart"></div></section>
</main>
<div id="side">
<h2>Paramètres géométriques</h2>
<div class="row"><label>Unité de longueur <select id="unit"><option value="1">m</option><option value="100" selected>cm</option><option value="1000">mm</option></select></label></div>
<div id="params"></div><div id="val"></div>
<h2>Matériau (catégorie)</h2>
<select id="mat" style="width:100%"><option value="alu">Alliage d’aluminium</option><option value="comp">Composite carbone</option><option value="steel">Acier inoxydable</option></select>
<h2>Vitesse d’animation de la vue éclatée</h2><input type="range" id="exs" min="1" max="6" value="3">
<h2>Composants et position (depuis la base)</h2><div id="list"></div><h2>Composant sélectionné</h2>
<div id="info">Cliquez sur une partie de la fusée.</div>
<details open><summary>Générateur de design</summary><textarea id='gt' rows='3' style='width:100%;margin:6px 0' placeholder='Ex : fusée noire et argent de 1,5 m, corps transparent, 4 ailerons'></textarea><div><button id='gg'>Générer</button> <button id='gu'>Annuler</button></div><div id='gx' class='note'></div><h2>Style visuel</h2><select id='sty' style='width:100%'><option value=''>Choisir un style…</option></select><h2>Longueur : préréglages <span id='lc' style='color:var(--cy)'></span></h2><div id='lp' style='display:flex;flex-wrap:wrap;gap:4px'></div><div class='row' style='margin-top:6px'>Longueur exacte (cm) <input type='number' id='ln' min='10' max='500' step='0.5' style='width:80px'></div></details>
<details open><summary>Physique du modèle (calculée)</summary><div id='ph'></div></details>
<details open><summary>Incertitudes (zone grisée)</summary><div class='row'><label><span><input type='checkbox' id='ub' checked> Afficher les zones</span></label></div><div class='row'><label>Traînée (Cd) <span id='ucv'></span></label><input type='range' id='uc' min='0' max='60' value='25'></div><div class='row'><label>Masse des pièces <span id='umv'></span></label><input type='range' id='um' min='0' max='40' value='10'></div><div class='row'><label>Vitesse initiale <span id='uvv'></span></label><input type='range' id='uv' min='0' max='30' value='5'></div><div class='row'><label>Densité de l’air <span id='urv'></span></label><input type='range' id='ur' min='0' max='30' value='5'></div><div class='row'><label>Portance des ailerons et de l’ogive <span id='unv'></span></label><input type='range' id='un' min='0' max='50' value='15'></div><div class='note'>Hypothèses que vous fixez, pas des mesures : ± x % ≈ intervalle à 95 % (loi normale). 300 tirages aléatoires reproductibles. Zone grise = 5–95 % des tirages ; billes et bandes 3D = plage du centre de gravité (cyan) et du centre de pression (ambre).</div></details>
<details><summary>Matériaux et surface</summary><div class='note' id='mn'></div><select id='ms' style='width:100%;margin-bottom:6px'></select><div class='row'><label>Couleur <input type='color' id='mc'></label></div><div class='row'><label>Rugosité <span id='mrv'></span></label><input type='range' id='mr' min='0' max='1' step='.01'></div><div class='row'><label>Effet métallique <span id='mmv'></span></label><input type='range' id='mm' min='0' max='1' step='.01'></div><div class='row'><label>Opacité <span id='mov'></span></label><input type='range' id='mo' min='.1' max='1' step='.01'></div><div class='note' id='mi'></div></details>
<details><summary>Affichage et coupe</summary><select id='dm' style='width:100%'><option value='full'>Assemblage complet</option><option value='int'>Vue interne (corps transparent)</option><option value='struct'>Structure seule</option><option value='elec'>Électronique seule</option><option value='prop'>Représentation propulsion seule</option></select><div class='row' style='margin-top:8px'><label><span><input type='checkbox' id='ce'> Plan de coupe</span><select id='ca'><option value='x'>Axe X</option><option value='y' selected>Axe Y</option><option value='z'>Axe Z</option></select></label></div><input type='range' id='cp' min='0' max='100' value='60'></details>
<details><summary>Attitude (tangage, roulis, lacet)</summary><div class='row'><label>Tangage (axe X) <span id='apv'>0°</span></label><input type='range' id='ap' min='-180' max='180' value='0'></div><div class='row'><label>Roulis (axe long Y) <span id='arv'>0°</span></label><input type='range' id='ar' min='-180' max='180' value='0'></div><div class='row'><label>Lacet (axe Z) <span id='ayv'>0°</span></label><input type='range' id='ay' min='-180' max='180' value='0'></div><div class='note' id='av'></div><button id='arst'>Réinitialiser</button></details>
<details><summary>Allumage virtuel (démonstration)</summary><div class='msg e' style='font-weight:600'>SIMULATION UNIQUEMENT — AUCUN ACTIONNEMENT PHYSIQUE</div><div id='ig' style='margin:6px 0'></div><div id='is' class='note'></div><button id='in'>État suivant</button> <button id='ir'>Réinitialiser</button><div style='margin-top:6px'><select id='if'></select> <button id='ifb'>Simuler le défaut</button></div><div id='il' style='max-height:120px;overflow:auto;font-size:11px;color:var(--tx2);margin-top:6px'></div></details>
<details><summary>Configurations</summary><input type='text' id='cn' placeholder='Nom' style='width:55%'> <button id='cs'>Enregistrer</button><div id='cl'></div><button id='cx' style='margin-top:6px'>Exporter en JSON</button></details>
<p class="note">Modèle éducatif : montée balistique à vitesse initiale donnée, atmosphère standard (ISA), traînée simplifiée (Cd calculé à partir de la forme, voir « Physique du modèle »). Aucune propulsion n’est modélisée. Les résultats sont simulés, pas validés expérimentalement. Les masses viennent d’un calcul surface × épaisseur × densité indicative.</p>
</div>
</div>
<script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/echarts/5.4.3/echarts.min.js"></script>
<script>
const $=i=>document.getElementById(i);
const P={L:3,D:.2,N:.6,fins:4,finH:.15,plm:.5,wall:.002,v0:150,mat:'alu'};
const ROWS=[['L','Longueur totale (10 cm à 5 m)',.1,5,.01,'len'],['D','Diamètre maximal',.01,.5,.005,'len'],['N','Longueur de l’ogive',.02,2,.01,'len'],['fins','Nombre d’ailerons',3,8,1,''],['finH','Envergure des ailerons',.005,.5,.005,'len'],['plm','Masse de charge utile (kg)',0,50,.01,''],['wall','Épaisseur de paroi',.0005,.02,.0005,'mm'],['v0','Vitesse initiale (m/s)',20,600,5,'']];
const U={get:()=>+$('unit').value,name:()=>$('unit').selectedOptions[0].text};
const inputs={};
function fmt(k,t){const r=ROWS.find(x=>x[0]===k);return r[5]==='len'?(P[k]*U.get()).toFixed(U.get()===1?3:U.get()===100?1:0)+' '+U.name():r[5]==='mm'?(P[k]*1000).toFixed(2)+' mm':(+P[k]).toString()}
ROWS.forEach(([k,n,mn,mx,st])=>{const d=document.createElement('div');d.className='row';d.innerHTML=`<label>${n}<span id="v_${k}"></span></label><input type="range" min="${mn}" max="${mx}" step="${st}" value="${P[k]}">`;$('params').appendChild(d);const i=d.querySelector('input');inputs[k]=i;i.addEventListener('input',()=>{P[k]=+i.value;if(k==='L')fit();change()})});
$('unit').onchange=()=>{labels();listC();showInfo()};$('mat').onchange=()=>{P.mat=$('mat').value;build();sim();setChart(S.t.length-1)};
function labels(){ROWS.forEach(r=>$('v_'+r[0]).textContent=fmt(r[0]));$('ln').value=(P.L*100).toFixed(1);$('lc').textContent=P.L<.3?'miniature':P.L<1?'petit modèle':P.L<2.5?'modèle moyen':'grand modèle'}

/* ---- 3D ---- */
const vp=$('vp'),R=new THREE.WebGLRenderer({canvas:$('cv'),antialias:true,preserveDrawingBuffer:true});
R.setPixelRatio(Math.min(devicePixelRatio,2));
const sc=new THREE.Scene();sc.background=new THREE.Color(0x080A0E);
const cam=new THREE.PerspectiveCamera(40,1,.002,100);
sc.add(new THREE.HemisphereLight(0xbfe9ff,0x10141B,.9));
const dl=new THREE.DirectionalLight(0xffffff,1.1);dl.position.set(8,14,10);sc.add(dl);
const dl2=new THREE.DirectionalLight(0x806BFF,.5);dl2.position.set(-10,4,-8);sc.add(dl2);
const rg=new THREE.Group();sc.add(rg);
let grid=new THREE.GridHelper(10,20,0x252D38,0x151A23);sc.add(grid);
const cs={th:.7,ph:1.25,d:22,ty:6};
function updCam(){cam.position.set(cs.d*Math.sin(cs.ph)*Math.sin(cs.th),cs.ty+cs.d*Math.cos(cs.ph),cs.d*Math.sin(cs.ph)*Math.cos(cs.th));cam.lookAt(0,cs.ty,0)}
function fit(){cs.d=Math.max(P.L*2.1,.3);cs.ty=P.L*.5}
let C=[],sel=null,wire=false,exT=0,ex=0,xr=true;
const SHELL=['engine','tank','instr','pay','nose'];
function ghost(){C.forEach(c=>{const o=MO[c.id],op=o?o.o:1,tr=xr&&SHELL.includes(c.id);c.obj.traverse(x=>{if(x.material){x.material.transparent=tr||op<1;x.material.opacity=tr?.16:op;x.material.depthWrite=!tr;x.material.needsUpdate=true}})})}
function listC(){const f=U.get(),n=U.name();$('list').innerHTML='';[...C].sort((a,b)=>a.y0-b.y0).forEach(c=>{const b=document.createElement('button');b.style.cssText='display:block;width:100%;text-align:left;margin-bottom:4px;font-size:12px';b.textContent=c.name+' — '+(c.y0*f).toFixed(1)+' à '+(c.y1*f).toFixed(1)+' '+n;b.onclick=()=>{sel=c.id;hl();showInfo()};$('list').appendChild(b)})}
const MAT={alu:[.85,.3],comp:[.2,.55],steel:[.95,.2]};
const SECT=[['engine','Section moteur (générique)','Propulsion (représentation visuelle)',.15,.06,0x9aa3b0],['tank','Section réservoir (générique)','Structure',.4,.06,0xc7ced8],['instr','Baie d’instrumentation','Avionique',.15,.04,0x4fd6f0],['pay','Compartiment charge utile','Charge utile',.30,.05,0x9b8cff]];
function mk(geo,col){const [m,r]=MAT[P.mat];return new THREE.Mesh(geo,new THREE.MeshStandardMaterial({color:col,metalness:m,roughness:r,wireframe:wire}))}
function reg(id,name,cat,obj,len,dia,mf,y0,y1,idx,sh){obj.traverse(o=>o.userData.cid=id);rg.add(obj);C.push({id,name,cat,obj,len,dia,mass:0,y0,y1,base:obj.position.clone(),idx,sh})}
function build(){
  C.forEach(c=>c.obj.traverse(o=>{if(o.geometry)o.geometry.dispose();if(o.material)o.material.dispose()}));
  while(rg.children.length)rg.remove(rg.children[0]);C=[];
  document.querySelectorAll('.lbl').forEach(e=>e.remove());
  const {L,D,N,fins,finH}=P,r=D/2,B=L-N;let y=0;const SP={};
  const nzH=L*.07;
  const nz=mk(new THREE.CylinderGeometry(r*.35,r*.62,nzH,40,1,true),0x6b7380);nz.position.y=-nzH/2;nz.material.side=THREE.DoubleSide;
  reg('nozzle','Tuyère (forme générique)','Propulsion (visuel uniquement)',nz,nzH,r*1.24,.04,-nzH,0,0,{t:'cone',h:nzH,d1:r*.7,d2:r*1.24});
  SECT.forEach(([id,n,cat,f,mf,col],i)=>{const h=B*f,m=mk(new THREE.CylinderGeometry(r,r,h,48),col);m.position.y=y+h/2;reg(id,n,cat,m,h,D,mf,y,y+h,i+1,{t:'cyl',h:h,d:D});SP[id]=[y,y+h];y+=h});
  const pts=[];for(let i=0;i<=24;i++){const t=i/24;pts.push(new THREE.Vector2(Math.max(r*Math.pow(1-t*t,.6),0),t*N))}
  const no=mk(new THREE.LatheGeometry(pts,48),0xe8edf3);no.position.y=B;reg('nose','Ogive','Aérodynamique',no,N,D,.06,B,L,5,{t:'ogive',h:N,d:D});
  const fg=new THREE.Group(),root=Math.min(B*.3,finH*1.8),th=Math.max(D*.04,.0005);
  const sh=new THREE.Shape();sh.moveTo(0,root);sh.lineTo(0,0);sh.lineTo(finH,-root*.45);sh.lineTo(finH,root*.15);sh.closePath();
  const fgeo=new THREE.ExtrudeGeometry(sh,{depth:th,bevelEnabled:false});fgeo.translate(0,0,-th/2);
  for(let i=0;i<fins;i++){const pv=new THREE.Object3D();pv.rotation.y=i*Math.PI*2/fins;const f=mk(fgeo,0x806BFF);f.position.x=r;f.position.y=0;pv.add(f);fg.add(pv)}
  fg.position.y=root*.5;reg('fins','Ailerons stabilisateurs','Stabilisation',fg,root,finH,.13,0,root,6,{t:'fins',n:fins,span:finH,root:root,th:th,d:D});
  const I=(id,name,cat,geo,col,x,yc,z,len,dia,mf,par,y0,y1,sh)=>{const m=mk(geo,col);m.position.set(x,yc,z);reg(id,name,cat,m,len,dia,mf,y0,y1,par,sh)};
  {const [a,b]=SP.engine,h=b-a;I('motor','Moteur (représentation générique)','Propulsion (visuel générique)',new THREE.CylinderGeometry(r*.55,r*.55,h*.8,32),0x59616d,0,(a+b)/2,0,h*.8,r*1.1,.14,1,a+h*.1,b-h*.1,{t:'cyl',h:h*.8,d:r*1.1})}
  {const [a,b]=SP.tank,h=b-a;I('rsv','Réservoir (représentation générique)','Réservoir (visuel générique)',new THREE.CylinderGeometry(r*.78,r*.78,h*.88,32),0x2f6fb0,0,(a+b)/2,0,h*.88,r*1.56,.16,2,a+h*.06,b-h*.06,{t:'cyl',h:h*.88,d:r*1.56})}
  {const [a,b]=SP.instr,h=b-a,yc=(a+b)/2;
   I('pcb','Carte électronique principale','Avionique',new THREE.BoxGeometry(r*1.3,h*.8,Math.max(r*.06,.0004)),0x1f8a5b,0,yc,-r*.25,h*.8,r*1.3,.03,3,a+h*.1,b-h*.1,{t:'box',w:r*1.3,h:h*.8,d:Math.max(r*.06,.0004)});
   I('mcu','Microcontrôleur (générique)','Avionique',new THREE.BoxGeometry(r*.35,r*.35,r*.08),0x15191f,0,yc+h*.2,-r*.25+r*.07,r*.35,r*.35,.02,3,yc+h*.2-r*.17,yc+h*.2+r*.17,{t:'box',w:r*.35,h:r*.35,d:r*.08});
   I('batt','Batterie (représentation)','Alimentation',new THREE.BoxGeometry(r*.5,h*.6,r*.35),0xFFB547,0,yc,r*.35,h*.6,r*.5,.09,3,a+h*.2,b-h*.2,{t:'box',w:r*.5,h:h*.6,d:r*.35})}
  {const [a,b]=SP.pay,h=b-a;I('pld','Charge utile (bloc générique)','Charge utile',new THREE.CylinderGeometry(r*.6,r*.6,h*.6,32),0xd05cff,0,(a+b)/2,0,h*.6,r*1.2,.12,4,a+h*.2,b-h*.2,{t:'cyl',h:h*.6,d:r*1.2})}
  sc.remove(grid);grid.geometry.dispose();grid=new THREE.GridHelper(Math.max(P.L*4,.6),20,0x252D38,0x151A23);sc.add(grid);grid.position.y=-nzH-.0001;
  C.forEach(c=>{const l=document.createElement('div');l.className='lbl';l.id='l_'+c.id;l.style.display='none';l.textContent=c.id;vp.appendChild(l)});
  phys();applyMat();vis();cut();ghost();hl();apply(ex);showInfo();listC();
}
function apply(e){const g=P.L*.1;C.forEach(c=>{const o=c.obj;if(c.id==='fins'){o.position.y=c.base.y-e*g*.8;o.children.forEach(p=>p.children[0].position.x=P.D/2+e*P.D*.7)}else if(c.id==='nozzle')o.position.y=c.base.y-e*g;else o.position.y=c.base.y+e*g*(c.idx-1)*.9})}
function hl(){C.forEach(c=>c.obj.traverse(o=>{if(o.material&&o.material.emissive){o.material.emissive.setHex(sel===c.id?0x00D9FF:0);o.material.emissiveIntensity=sel===c.id?.45:0}}));led()}
function showInfo(){const c=C.find(x=>x.id===sel);if(!c){$('info').textContent='Cliquez sur une partie de la fusée.';closeDetail();return}
  const f=U.get(),n=U.name(),mt=$('mat').selectedOptions[0].text;
  $('info').innerHTML=$('dfiche').innerHTML=`<dl style="margin:0"><dt>Nom</dt><dd><b>${c.name}</b> (${c.id})</dd><dt>Catégorie visuelle</dt><dd>${c.cat}</dd><dt>Dimensions</dt><dd>longueur ${(c.len*f).toFixed(2)} ${n} · diamètre ${(c.dia*f).toFixed(2)} ${n}</dd><dt>Matériau</dt><dd>${mt}</dd><dt>Masse calculée (surface × épaisseur × densité indicative)</dt><dd>${c.mass.toFixed(3)} kg</dd><dt>Position dans l’assemblage</dt><dd>de ${(c.y0*f).toFixed(2)} à ${(c.y1*f).toFixed(2)} ${n} depuis la base</dd></dl>`;openDetail(c);syncMat(c)}
function validate(){let h='';const {L,D,N,finH}=P;if(P.wall>D/4){P.wall=D/4;inputs.wall.value=P.wall;h+=`<div class='msg w'>Épaisseur de paroi limitée à D/4 : la paroi ne peut pas remplir la section.</div>`}
  if(N>L*.5){P.N=L*.5;inputs.N.value=P.N;h+='<div class="msg w">Ogive limitée à 50 % de la longueur totale.</div>'}
  if(L/D<5)h+='<div class="msg w">Élancement L/D inférieur à 5 : forme inhabituelle pour une fusée.</div>';
  if(finH>D*2)h+='<div class="msg w">Ailerons très grands par rapport au diamètre.</div>';
  if(!h)h='<div class="msg ok">Géométrie valide (valide géométriquement ≠ conception d’ingénierie validée).</div>';
  $('val').innerHTML=h}
function change(){validate();labels();build();sim();hold=false;setChart(S.t.length-1);$('tl').value=1000}
/* camera + interaction */
let drag=null,moved=false;const cv=$('cv');
cv.addEventListener('pointerdown',e=>{drag={x:e.clientX,y:e.clientY};moved=false;cv.setPointerCapture(e.pointerId)});
cv.addEventListener('pointermove',e=>{if(!drag)return;const dx=e.clientX-drag.x,dy=e.clientY-drag.y;if(Math.abs(dx)+Math.abs(dy)>3)moved=true;cs.th-=dx*.008;cs.ph=Math.min(Math.max(cs.ph-dy*.008,.05),3.09);drag={x:e.clientX,y:e.clientY}});
cv.addEventListener('pointerup',e=>{drag=null;if(!moved)pick(e)});
cv.addEventListener('wheel',e=>{e.preventDefault();cs.d=Math.min(Math.max(cs.d*(1+Math.sign(e.deltaY)*.1),.08),40)},{passive:false});
function pick(e){const b=cv.getBoundingClientRect(),m=new THREE.Vector2((e.clientX-b.left)/b.width*2-1,-((e.clientY-b.top)/b.height*2-1));const rc=new THREE.Raycaster();rc.setFromCamera(m,cam);const hs=rc.intersectObjects(rg.children,true).filter(i=>i.object.userData.cid&&C.find(x=>x.id===i.object.userData.cid).obj.visible);const h=hs.find(i=>!xr||!SHELL.includes(i.object.userData.cid))||hs[0];sel=h?h.object.userData.cid:null;hl();showInfo()}
document.querySelectorAll('[data-v]').forEach(b=>b.onclick=()=>{const v=b.dataset.v;if(v==='f'){cs.th=0;cs.ph=Math.PI/2}else if(v==='s'){cs.th=Math.PI/2;cs.ph=Math.PI/2}else if(v==='t'){cs.th=0;cs.ph=.05}else if(v==='b'){cs.th=0;cs.ph=3.09}else if(v==='r'){cs.th=Math.PI;cs.ph=Math.PI/2}else if(v==='i'){cs.th=Math.PI/4;cs.ph=.96}else{cs.th=.7;cs.ph=1.25}fit()});
$('wf').onclick=e=>{wire=!wire;e.target.classList.toggle('on',wire);C.forEach(c=>c.obj.traverse(o=>{if(o.material)o.material.wireframe=wire}))};
$('xr').onclick=e=>{xr=!xr;e.target.classList.toggle('on',xr);ghost()};
$('ex').onclick=e=>{exT=exT?0:1;e.target.classList.toggle('on',!!exT)};
function resize(){const w=vp.clientWidth,h=vp.clientHeight;if(!w||!h)return;R.setSize(w,h,false);cam.aspect=w/h;cam.updateProjectionMatrix();chart.resize()}
new ResizeObserver(resize).observe(vp);addEventListener('resize',resize);
/* ---- fiche détaillée : rendu 3D isolé + plan ---- */
const DR=new THREE.WebGLRenderer({canvas:$('dc'),antialias:true});DR.setPixelRatio(Math.min(devicePixelRatio,2));
const dsc=new THREE.Scene();dsc.background=new THREE.Color(0x080A0E);dsc.add(new THREE.HemisphereLight(0xbfe9ff,0x10141B,.9));
const ddl=new THREE.DirectionalLight(0xffffff,1.2);ddl.position.set(3,5,4);dsc.add(ddl);
const dcam=new THREE.PerspectiveCamera(35,1,.001,100),dcv=$('dc');let dh=null,dd=null,dw=0,dht=0;
function dResize(){const w=dcv.clientWidth,h=dcv.clientHeight;if(!w||!h||(w===dw&&h===dht))return;dw=w;dht=h;DR.setSize(w,h,false);dcam.aspect=w/h;dcam.updateProjectionMatrix()}
dcv.addEventListener('pointerdown',e=>{dd=e.clientX;dcv.setPointerCapture(e.pointerId)});
dcv.addEventListener('pointermove',e=>{if(dd===null||!dh)return;dh.rotation.y+=(e.clientX-dd)*.01;dd=e.clientX});
dcv.addEventListener('pointerup',()=>{dd=null});
$('dclose').onclick=()=>{sel=null;hl();showInfo()};
function closeDetail(){$('detail').hidden=true;syncMat(null)}
function openDetail(c){$('detail').hidden=false;$('dtitle').textContent=c.name;$('dplan').innerHTML=planSVG(c);mk3(c);dw=0}
function mk3(c){if(dh){dsc.remove(dh);dh.traverse(o=>{if(o.material)o.material.dispose()});dh=null}
 const cl=c.obj.clone(true);
 cl.traverse(o=>{if(o.material){const m=o.material=o.material.clone();m.transparent=false;m.opacity=1;m.depthWrite=true;m.wireframe=false;m.side=THREE.DoubleSide;if(m.emissive)m.emissive.setHex(0)}});
 cl.position.copy(c.base);if(c.id==='fins')cl.children.forEach(p=>p.children[0].position.x=P.D/2);
 dh=new THREE.Group();dh.add(cl);dh.updateMatrixWorld(true);
 const bx=new THREE.Box3().setFromObject(dh),ct=bx.getCenter(new THREE.Vector3()),sz=bx.getSize(new THREE.Vector3());
 cl.position.sub(ct);const m=Math.max(sz.x,sz.y,sz.z,1e-4);
 dcam.near=m*.01;dcam.far=m*30;dcam.position.set(0,m*.35,m*2.3);dcam.lookAt(0,0,0);dcam.updateProjectionMatrix();dh.rotation.x=.25;dsc.add(dh)}
function planSVG(c){
 const sh=c.sh||{t:'cyl',h:c.len,d:c.dia},f=U.get(),un=U.name(),dg=f===1?3:1,V=m=>(m*f).toFixed(dg)+' '+un,mt=$('mat').selectedOptions[0].text;
 let pts;
 if(sh.t==='box')pts=[[-sh.w/2,0],[sh.w/2,0],[sh.w/2,sh.h],[-sh.w/2,sh.h]];
 else if(sh.t==='cone')pts=[[-sh.d2/2,0],[sh.d2/2,0],[sh.d1/2,sh.h],[-sh.d1/2,sh.h]];
 else if(sh.t==='ogive'){const A=[],B2=[];for(let i=0;i<=24;i++){const t=i/24,x=sh.d/2*Math.pow(Math.max(1-t*t,0),.6);A.push([-x,t*sh.h]);B2.push([x,t*sh.h])}pts=A.concat(B2.reverse())}
 else if(sh.t==='fins')pts=[[0,sh.root],[0,0],[sh.span,-sh.root*.45],[sh.span,sh.root*.15]];
 else pts=[[-sh.d/2,0],[sh.d/2,0],[sh.d/2,sh.h],[-sh.d/2,sh.h]];
 const xs=pts.map(p=>p[0]),ys=pts.map(p=>p[1]),x0=Math.min(...xs),x1=Math.max(...xs),y0=Math.min(...ys),y1=Math.max(...ys),bw=x1-x0,bh=y1-y0;
 const s=Math.min(330/Math.max(bw,1e-9),200/Math.max(bh,1e-9)),X=x=>210+(x-(x0+x1)/2)*s,Y=y=>165-(y-(y0+y1)/2)*s;
 const T=(x,y,t,a,at)=>`<text x='${x}' y='${y}' text-anchor='${a}' ${at}>${t}</text>`;
 const dH=(xa,xb,y,t,yr)=>`<line x1='${xa}' x2='${xa}' y1='${yr}' y2='${y+4}' class='ex'/><line x1='${xb}' x2='${xb}' y1='${yr}' y2='${y+4}' class='ex'/><line x1='${xa}' x2='${xb}' y1='${y}' y2='${y}' class='dm'/><path d='M${xa} ${y}l6 -3v6zM${xb} ${y}l-6 -3v6z' class='ar'/>`+T((xa+xb)/2,y-5,t,'middle',"class='dt'");
 const dV=(x,ya,yb,t,xr)=>`<line x1='${xr}' x2='${x+4}' y1='${ya}' y2='${ya}' class='ex'/><line x1='${xr}' x2='${x+4}' y1='${yb}' y2='${yb}' class='ex'/><line x1='${x}' x2='${x}' y1='${ya}' y2='${yb}' class='dm'/><path d='M${x} ${ya}l-3 6h6zM${x} ${yb}l-3 -6h6z' class='ar'/><text transform='translate(${x+14},${(ya+yb)/2}) rotate(-90)' text-anchor='middle' class='dt'>${t}</text>`;
 let g=T(30,28,'Vue de face','start',"class='tx'")+T(430,28,'Vue de dessus','start',"class='tx'")+`<polygon points='${pts.map(p=>X(p[0]).toFixed(1)+','+Y(p[1]).toFixed(1)).join(' ')}' fill='#151A23' stroke='#F2F4F8' stroke-width='1.4'/>`;
 if(sh.t!=='fins')g+=`<line x1='${X(0)}' x2='${X(0)}' y1='${Y(y1)-14}' y2='${Y(y0)+14}' stroke='#806BFF' stroke-dasharray='9 3 2 3' stroke-width='.8'/>`;
 g+=dH(X(x0),X(x1),Y(y0)+26,sh.t==='box'?V(bw):sh.t==='fins'?V(bw)+' (envergure)':'Ø '+V(bw),Y(y0)+2)+dV(X(x1)+30,Y(y1),Y(y0),V(bh),X(x1)+2);
 if(sh.t==='cone')g+=dH(X(-sh.d1/2),X(sh.d1/2),Y(y1)-20,'Ø '+V(sh.d1),Y(y1)-2);
 const cx=505,cy=150;let ext=sh.t==='box'?Math.max(sh.w,sh.d):sh.t==='fins'?sh.d+2*sh.span:sh.t==='cone'?sh.d2:sh.d;
 const st=150/Math.max(ext,1e-9),St="fill='none' stroke='#F2F4F8' stroke-width='1.3'";
 if(sh.t==='box'){g+=`<rect x='${cx-sh.w*st/2}' y='${cy-Math.max(sh.d*st,1.5)/2}' width='${sh.w*st}' height='${Math.max(sh.d*st,1.5)}' ${St}/>`}
 else{g+=`<circle cx='${cx}' cy='${cy}' r='${(sh.d||sh.d2)/2*st}' ${St}/>`;
  if(sh.t==='cone')g+=`<circle cx='${cx}' cy='${cy}' r='${sh.d1/2*st}' ${St} stroke-dasharray='4 3'/>`;
  if(sh.t==='fins')for(let i=0;i<sh.n;i++)g+=`<rect x='${cx+sh.d/2*st}' y='${cy-Math.max(sh.th*st,1.5)/2}' width='${sh.span*st}' height='${Math.max(sh.th*st,1.5)}' fill='#806BFF55' stroke='#806BFF' transform='rotate(${i*360/sh.n} ${cx} ${cy})'/>`;
  g+=`<line x1='${cx-ext*st/2-10}' x2='${cx+ext*st/2+10}' y1='${cy}' y2='${cy}' stroke='#806BFF' stroke-dasharray='9 3 2 3' stroke-width='.8'/><line y1='${cy-ext*st/2-10}' y2='${cy+ext*st/2+10}' x1='${cx}' x2='${cx}' stroke='#806BFF' stroke-dasharray='9 3 2 3' stroke-width='.8'/>`}
 g+=dH(cx-ext*st/2,cx+ext*st/2,cy+ext*st/2+26,sh.t==='box'?V(sh.w):'Ø '+V(ext)+(sh.t==='fins'?' (avec ailerons)':''),cy+ext*st/2+2);
 if(sh.t==='box')g+=dV(cx+ext*st/2+24,cy-Math.max(sh.d*st,1.5)/2,cy+Math.max(sh.d*st,1.5)/2,V(sh.d),cx+sh.w*st/2+2);
 const raw=70/s,pw=Math.pow(10,Math.floor(Math.log10(raw))),nn=raw/pw,nice=(nn<1.5?1:nn<3.5?2:nn<7.5?5:10)*pw;
 g+=`<line x1='30' x2='${30+nice*s}' y1='306' y2='306' stroke='#F2F4F8' stroke-width='2'/>`+T(30+nice*s+8,310,V(nice)+' (échelle, vue de face)','start',"class='tx'");
 g+=`<rect x='4' y='322' width='612' height='54' fill='none' stroke='#252D38'/>`+T(14,343,c.name,'start',"style='fill:#F2F4F8;font-size:14px;font-weight:600'")+T(14,362,`${c.cat} · matériau : ${mt} · masse ≈ ${c.mass.toFixed(3)} kg (hypothèse illustrative)`,'start',"class='tx'")+T(606,370,'Cotes en '+un+' · plan généré depuis les paramètres du projet','end',"class='tx'");
 return `<svg viewBox='0 0 620 380' width='100%' style='background:#0c0f14;border:1px solid #252D38;border-radius:6px' font-family='Segoe UI,system-ui,sans-serif'><style>.ex{stroke:#555e6b;stroke-width:.6}.dm{stroke:#00D9FF;stroke-width:1}.ar{fill:#00D9FF}.dt{fill:#00D9FF;font-size:11px}.tx{fill:#929BA9;font-size:11px}</style><rect x='4' y='4' width='612' height='372' fill='none' stroke='#252D38'/>${g}</svg>`}

/* ---- matériaux, styles, affichage, attitude, allumage virtuel ---- */
const MATS={};[['pla','PLA','Plastique',1240,'#e6e8ea',0,.5,1,/(^|[^a-z])pla([^a-z]|$)/],['petg','PETG','Plastique',1270,'#cfe6ee',0,.3,1,/petg/],['abs','ABS','Plastique',1050,'#2b2d31',0,.45,1,/(^|[^a-z])abs([^a-z]|$)/],['asa','ASA','Plastique',1070,'#dcdde0',0,.5,1,/(^|[^a-z])asa([^a-z]|$)/],['nyl','Nylon','Plastique',1140,'#e9e4d4',0,.6,1,/nylon/],['pc','Polycarbonate transparent','Plastique',1200,'#bfe9ff',0,.1,.35,/polycarbonate/],['alu','Aluminium brossé','Métal',2700,'#b8c0cc',.9,.38,1,/alumin|(^|[^a-z])alu([^a-z]|$)/],['alup','Aluminium poli','Métal',2700,'#dfe5ec',1,.08,1,/poli/],['inox','Acier inoxydable','Métal',8000,'#9aa2ab',.95,.25,1,/inox|acier/],['ti','Titane','Métal',4500,'#8a8f98',.9,.35,1,/titane/],['cu','Cuivre','Métal',8960,'#b87333',1,.25,1,/cuivre/],['cfrp','Composite fibre de carbone','Composite',1600,'#15171b',.3,.4,1,/carbone/],['gfrp','Composite fibre de verre','Composite',1850,'#d9d3b8',.1,.5,1,/fibre de verre/],['rub','Caoutchouc','Autre',1100,'#16171a',0,.9,1,/caoutchouc/],['sil','Silicone','Autre',1150,'#d8d8e0',0,.6,.85,/silicone/],['cer','Céramique','Autre',3900,'#f2efe6',0,.35,1,/c[ée]ramique/],['peinN','Peinture noire mate','Revêtement',0,'#0b0c0f',0,.8,1,null],['peinB','Peinture blanche satinée','Revêtement',0,'#f3f5f8',0,.35,1,null]].forEach(a=>MATS[a[0]]={n:a[1],cat:a[2],rho:a[3],c:a[4],m:a[5],r:a[6],o:a[7],kw:a[8]});
const BODY=['engine','tank','instr','pay'],STRUCT=['engine','tank','instr','pay','nose','fins'],ELEC=['pcb','mcu','batt'],PROP=['nozzle','motor','rsv'],MO={},hist=[];
const STY={real:{n:'Aérospatial réaliste (blanc et argent)',b:'peinB',nn:'alup',f:'alu',kw:/r[ée]aliste/},fut:{n:'Industriel futuriste (noir et argent)',b:'peinN',nn:'alup',f:'alu',kw:/futuriste|industriel/},mini:{n:'Scientifique minimaliste',b:'peinB',nn:'peinB',f:'peinB',kw:/minimalist|scientifique/},age:{n:'Ère spatiale classique',b:'peinB',nn:'peinN',f:'peinN',kw:/classique|ann[ée]es 60/},black:{n:'Noir mat',b:'peinN',nn:'peinN',f:'peinN',kw:/noir mat/},wm:{n:'Blanc et métallique',b:'peinB',nn:'inox',f:'inox',kw:/blanc et (m[ée]tal|argent)/},edu:{n:'Éducatif transparent',b:'pc',nn:'pc',f:'abs',x:true,kw:/[ée]ducatif/},vin:{n:'Aérospatial vintage',b:'cu',nn:'cu',f:'inox',kw:/vintage/},exp:{n:'Recherche expérimentale',b:'cfrp',nn:'cfrp',f:'ti',kw:/exp[ée]rimental/}};
$('sty').innerHTML+=Object.keys(STY).map(k=>`<option value='${k}'>${STY[k].n}</option>`).join('');
$('ms').innerHTML=`<option value=''>(valeurs par défaut)</option>`+Object.keys(MATS).map(k=>`<option value='${k}'>${MATS[k].n}</option>`).join('');
function setMat(ids,k){const a=MATS[k];ids.forEach(i=>MO[i]={k,c:a.c,m:a.m,r:a.r,o:a.o||1})}
function applyStyle(k){const s=STY[k];setMat(BODY,s.b);setMat(['nose'],s.nn);setMat(['fins'],s.f);if(s.x!==undefined)xr=s.x}
function applyMat(){C.forEach(c=>{const o=MO[c.id];if(!o)return;c.obj.traverse(x=>{if(x.material){x.material.color.set(o.c);x.material.metalness=o.m;x.material.roughness=o.r}})})}
function syncMat(c){['ms','mc','mr','mm','mo'].forEach(i=>$(i).disabled=!c);
 if(!c){$('mn').textContent='Sélectionnez une pièce (vue 3D ou liste).';$('mi').textContent='';return}
 let q=null;c.obj.traverse(x=>{if(x.material&&!q)q=x.material});const o=MO[c.id];
 $('mn').textContent='Pièce : '+c.name;$('ms').value=o?o.k:'';$('mc').value=o?o.c:'#'+q.color.getHexString();$('mr').value=o?o.r:q.roughness;$('mm').value=o?o.m:q.metalness;$('mo').value=o?o.o:1;
 $('mrv').textContent=(+$('mr').value).toFixed(2);$('mmv').textContent=(+$('mm').value).toFixed(2);$('mov').textContent=(+$('mo').value).toFixed(2);
 const a=o&&MATS[o.k];$('mi').textContent=a?`${a.cat} · densité ≈ ${a.rho?a.rho+' kg/m³':'non applicable (revêtement)'} · valeurs génériques indicatives, pas des données certifiées de fabricant.`:'Matériau par défaut du projet.'}
function setF(f,v){if(!sel)return;const o=MO[sel]||(MO[sel]={k:'',c:$('mc').value,m:+$('mm').value,r:+$('mr').value,o:+$('mo').value});o[f]=v;applyMat();ghost();syncMat(C.find(x=>x.id===sel))}
$('mc').addEventListener('input',e=>setF('c',e.target.value));$('mr').addEventListener('input',e=>setF('r',+e.target.value));$('mm').addEventListener('input',e=>setF('m',+e.target.value));$('mo').addEventListener('input',e=>setF('o',+e.target.value));
$('ms').onchange=()=>{if(!sel)return;const k=$('ms').value;if(!k){delete MO[sel];build();return}const a=MATS[k];MO[sel]={k,c:a.c,m:a.m,r:a.r,o:a.o||1};applyMat();ghost();physChange();syncMat(C.find(x=>x.id===sel))};
function syncUI(){ROWS.forEach(r=>inputs[r[0]].value=P[r[0]]);$('mat').value=P.mat;$('xr').classList.toggle('on',xr);$('ex').classList.toggle('on',!!exT)}
function snap(){hist.push(JSON.stringify({P,MO,xr,exT}))}
function restore(s){Object.assign(P,s.P);Object.keys(MO).forEach(k=>delete MO[k]);Object.assign(MO,s.MO);xr=s.xr;exT=s.exT;syncUI();fit();change()}
$('gu').onclick=()=>{const s=hist.pop();if(!s){$('gx').textContent='Rien à annuler.';return}restore(JSON.parse(s))};
$('sty').onchange=()=>{if(!$('sty').value)return;snap();applyStyle($('sty').value);syncUI();change()};
function gen(){const t=$('gt').value.toLowerCase();if(!t.trim()){$('gx').textContent='Décrivez le modèle voulu.';return}
 snap();const d=[];let m=t.match(/(\d+(?:[.,]\d+)?)\s*(cm|mm|m)(?![a-z])/);
 if(m){const v=Math.min(Math.max(parseFloat(m[1].replace(',','.'))*{cm:.01,mm:.001,m:1}[m[2]],.1),5);P.L=v;d.push('longueur '+(v*100).toFixed(0)+' cm')}
 m=t.match(/(\d)\s*ailerons/);if(m){P.fins=Math.min(Math.max(+m[1],3),8);d.push(P.fins+' ailerons')}
 if(/plus long|allong/.test(t)){P.L=Math.min(P.L*1.25,5);d.push('corps plus long')}
 if(/plus court|raccourci/.test(t)){P.L=Math.max(P.L*.8,.1);d.push('corps plus court')}
 if(/plus large|plus gros/.test(t)){P.D=Math.min(P.D*1.25,.5);d.push('corps plus large')}
 let st=null;for(const k in STY)if(STY[k].kw.test(t)){st=k;break}
 if(st){applyStyle(st);d.push('style : '+STY[st].n)}
 else if(/noir/.test(t)&&/argent|métal|metal/.test(t)){applyStyle('fut');d.push('noir et argent')}
 else if(/noir/.test(t)){applyStyle('black');d.push('noir mat')}
 else if(/blanc/.test(t)&&/argent|métal|metal/.test(t)){applyStyle('wm');d.push('blanc et métal')}
 else if(/blanc/.test(t)){applyStyle('mini');d.push('blanc')}
 for(const k in MATS)if(MATS[k].kw&&MATS[k].kw.test(t)){setMat(BODY,k);d.push('corps : '+MATS[k].n);break}
 if(/transparent|interne|int[ée]rieur|inspection|coupe/.test(t)){xr=true;d.push('corps transparent')}
 if(/coupe/.test(t)){$('ce').checked=true;d.push('plan de coupe actif')}
 if(/[ée]clat|s[ée]par/.test(t)){exT=1;d.push('vue éclatée')}
 syncUI();fit();change();
 $('gx').textContent=d.length?'Appliqué : '+d.join(' · ')+'. (Interprétation par règles locales, pas un modèle de langage.)':'Aucune instruction reconnue. Essayez : longueur (1,5 m), couleurs, matériau (PLA, polycarbonate…), style, nombre d’ailerons, transparent, coupe, éclaté.'}
$('gg').onclick=gen;
function setL(m){P.L=Math.min(Math.max(m,.1),5);inputs.L.value=P.L;fit();change()}
[10,25,50,75,100,150,200,300,400,500].forEach(cm=>{const b=document.createElement('button');b.textContent=cm>=100?cm/100+' m':cm+' cm';b.onclick=()=>setL(cm/100);$('lp').appendChild(b)});
$('ln').onchange=()=>setL(+$('ln').value/100);
function vis(){const m=$('dm').value,Sx={struct:STRUCT,elec:ELEC,prop:PROP};C.forEach(c=>{c.obj.visible=Sx[m]?Sx[m].includes(c.id):true})}
$('dm').onchange=()=>{const m=$('dm').value;if(m==='full')xr=false;if(m==='int')xr=true;$('xr').classList.toggle('on',xr);vis();ghost()};
R.localClippingEnabled=true;const plane=new THREE.Plane(new THREE.Vector3(0,-1,0),0);
function cut(){const on=$('ce').checked,ax=$('ca').value,f=$('cp').value/100,w=P.D/2+P.finH;let lo=-w,hi=w;if(ax==='y'){lo=-P.L*.07;hi=P.L}
 plane.normal.set(ax==='x'?-1:0,ax==='y'?-1:0,ax==='z'?-1:0);plane.constant=lo+(hi-lo)*f;
 C.forEach(c=>c.obj.traverse(o=>{if(o.material){o.material.clippingPlanes=on?[plane]:[];o.material.side=on?THREE.DoubleSide:THREE.FrontSide;o.material.needsUpdate=true}}))}
['ce','ca','cp'].forEach(i=>{$(i).addEventListener('input',cut);$(i).addEventListener('change',cut)});
function att(){const d=Math.PI/180,p=+$('ap').value,r=+$('ar').value,y=+$('ay').value;rg.rotation.set(p*d,r*d,y*d);const v=new THREE.Vector3(0,1,0).applyEuler(rg.rotation);$('apv').textContent=p+'°';$('arv').textContent=r+'°';$('ayv').textContent=y+'°';$('av').textContent='Axe longitudinal dans le repère monde : ('+v.x.toFixed(2)+', '+v.y.toFixed(2)+', '+v.z.toFixed(2)+'). Repère monde fixe : X rouge, Y vert, Z bleu.'}
['ap','ar','ay'].forEach(i=>$(i).addEventListener('input',att));$('arst').onclick=()=>{['ap','ar','ay'].forEach(i=>$(i).value=0);att()};sc.add(new THREE.AxesHelper(.4));att();
{const pm=new THREE.PMREMGenerator(R),es=new THREE.Scene();es.background=new THREE.Color(0x151a23);[[0,8,0,8,.2,8,0xffffff],[9,2,3,.2,6,6,0xbfe9ff],[-9,2,-3,.2,6,6,0x9b8cff]].forEach(a=>{const b=new THREE.Mesh(new THREE.BoxGeometry(a[3],a[4],a[5]),new THREE.MeshBasicMaterial({color:a[6]}));b.position.set(a[0],a[1],a[2]);es.add(b)});sc.environment=pm.fromScene(es,.02).texture}
const ST=[['SYSTÈME ÉTEINT','Aucun module n’est alimenté dans la simulation.'],['INITIALISATION','Les modules virtuels démarrent.'],['AUTO-TEST','Vérification de la présence des composants virtuels.'],['SIMULATION PRÊTE','Les contrôles virtuels sont passés.'],['COMMANDE VIRTUELLE REÇUE','Une commande fictive est enregistrée.'],['ÉVÉNEMENT SIMULÉ','Animation symbolique, sans effet physique.'],['ÉVÉNEMENT TERMINÉ','Les indicateurs reviennent à la normale.'],['SYSTÈME VERROUILLÉ','Le système virtuel reste verrouillé jusqu’à réinitialisation.']];
const FL=[['Composant virtuel manquant','Un module attendu est absent de la configuration : l’auto-test ne peut pas conclure.'],['Communication indisponible','Un nœud du modèle ne répond pas : l’état reste bloqué.'],['Configuration invalide','Des paramètres incohérents empêchent d’atteindre l’état prêt.'],['Données capteur indisponibles','Aucune mesure simulée n’arrive au module de surveillance.'],['Interruption simulée','Un arrêt fictif force le verrouillage.']];
let si=0,sp=-1,fault=false;const lg=[];
$('if').innerHTML=FL.map((f,i)=>`<option value='${i}'>${f[0]}</option>`).join('');
function stLog(t){lg.unshift(new Date().toLocaleTimeString('fr-FR')+' — '+t)}
function ign(){const col=i=>i!==si?'#252D38':fault?'#FF5364':si===7?'#FFB547':'#00D9FF';$('ig').innerHTML=ST.map((s,i)=>`<span style='display:inline-block;margin:2px;padding:2px 6px;border-radius:3px;border:1px solid ${col(i)};color:${i===si?col(i):'#929BA9'};font-size:11px'>${s[0]}</span>`).join('');
 $('is').innerHTML='État : <b>'+ST[si][0]+'</b>'+(sp>=0?' · précédent : '+ST[sp][0]:'')+'<br>'+ST[si][1];$('il').innerHTML=lg.slice(0,30).map(x=>`<div>${x}</div>`).join('');led()}
function led(){const c=C.find(x=>x.id==='mcu');if(!c||sel==='mcu')return;const k=fault?0xFF5364:si===7?0xFFB547:si===0?0:0x00D9FF;c.obj.traverse(o=>{if(o.material&&o.material.emissive){o.material.emissive.setHex(k);o.material.emissiveIntensity=k?.9:0}})}
$('in').onclick=()=>{if(fault){stLog('Réinitialisation requise après un défaut.');ign();return}sp=si;si=Math.min(si+1,7);stLog('→ '+ST[si][0]);ign()};
$('ir').onclick=()=>{si=0;sp=-1;fault=false;stLog('Réinitialisation (simulation)');ign()};
$('ifb').onclick=()=>{const f=FL[+$('if').value];sp=si;si=7;fault=true;stLog('DÉFAUT SIMULÉ : '+f[0]+' — '+f[1]);$('is').innerHTML='<span style="color:#FF5364">Défaut simulé : <b>'+f[0]+'</b></span><br>'+f[1];$('il').innerHTML=lg.slice(0,30).map(x=>`<div>${x}</div>`).join('');$('ig').innerHTML=ST.map((s,i)=>`<span style='display:inline-block;margin:2px;padding:2px 6px;border-radius:3px;border:1px solid ${i===7?'#FF5364':'#252D38'};color:${i===7?'#FF5364':'#929BA9'};font-size:11px'>${s[0]}</span>`).join('');led()};
stLog('Système éteint (simulation)');ign();
const KEY='bf_cfg_v1';
function cfgs(){try{return JSON.parse(localStorage.getItem(KEY)||'[]')}catch(e){return[]}}
function putCfgs(a){try{localStorage.setItem(KEY,JSON.stringify(a))}catch(e){$('cl').textContent='Stockage indisponible dans ce contexte.'}}
function cfgList(){const a=cfgs();$('cl').innerHTML='';a.forEach((c,i)=>{const d=document.createElement('div');d.style.cssText='display:flex;gap:4px;margin-top:4px;align-items:center';const n=document.createElement('span');n.style.flex='1';n.textContent=c.n;const l=document.createElement('button');l.textContent='Ouvrir';l.onclick=()=>{snap();restore(c.s)};const x=document.createElement('button');x.textContent='Supprimer';x.onclick=()=>{a.splice(i,1);putCfgs(a);cfgList()};d.append(n,l,x);$('cl').appendChild(d)})}
$('cs').onclick=()=>{const a=cfgs();a.push({n:$('cn').value||'Configuration '+(a.length+1),s:JSON.parse(JSON.stringify({P,MO,xr,exT}))});putCfgs(a);cfgList()};
$('cx').onclick=()=>{const b=new Blob([JSON.stringify({format:'beutterfly-config',version:1,created:new Date().toISOString(),P,MO,note:'Modèle éducatif, valeurs indicatives'},null,2)],{type:'application/json'});const a=document.createElement('a');a.href=URL.createObjectURL(b);a.download='beutterfly_config.json';a.click()};
cfgList();
/* ---- physique du modèle : masse, centre de gravité, traînée, stabilité ---- */
const BASE={alu:2700,comp:1600,steel:8000},PH={};
function rhoOf(id){const o=MO[id],a=o&&MATS[o.k];return (a&&a.rho)||BASE[P.mat]}
function cdM(M){return M<.8?1:M<1.2?1+(M-.8)/.4:Math.max(2-(M-1.2)*.5,1.4)}
function phys(){
 const {L,D,N,fins,finH}=P,r=D/2,t=Math.min(P.wall,D/4),B=L-N;
 let An=0,yn=0,px=0,pr=r;for(let i=1;i<=48;i++){const u=i/48,x=u*N,q=r*Math.pow(Math.max(1-u*u,0),.6),ds=Math.hypot(x-px,q-pr),a=Math.PI*(q+pr)*ds;An+=a;yn+=a*(px+x)/2;px=x;pr=q}yn/=(An||1);
 let finA=0,root=0,Fy=0;
 C.forEach(c=>{const s=c.sh||{},rho=rhoOf(c.id);let m=0,yc=c.base.y;
  if(['engine','tank','instr','pay','motor','rsv'].includes(c.id))m=Math.PI*s.d*s.h*t*rho;
  else if(c.id==='nose'){m=An*t*rho;yc=c.base.y+yn}
  else if(c.id==='nozzle')m=Math.PI*(s.d1+s.d2)/2*Math.hypot(s.h,(s.d2-s.d1)/2)*t*rho;
  else if(c.id==='fins'){root=s.root;const q=[[0,s.root],[0,0],[s.span,-s.root*.45],[s.span,s.root*.15]];let a2=0,cy=0;for(let i=0;i<4;i++){const [x1,y1]=q[i],[x2,y2]=q[(i+1)%4],k=x1*y2-x2*y1;a2+=k;cy+=(y1+y2)*k}finA=Math.abs(a2)/2;Fy=cy/(3*a2);m=finA*s.th*s.n*rho;yc=c.base.y+Fy}
  else if(c.id==='pcb')m=s.w*s.h*s.d*1850;else if(c.id==='mcu')m=s.w*s.h*s.d*2000;else if(c.id==='batt')m=s.w*s.h*s.d*2500;else if(c.id==='pld')m=P.plm;
  c.mass=m;c.yc=yc});
 let M=0,Mc=0;C.forEach(c=>{M+=c.mass;Mc+=c.mass*c.yc});const ycg=Mc/M,xcg=L-ycg;
 const fc=C.find(x=>x.id==='fins'),A=Math.PI*r*r,Awet=Math.PI*D*B+An+2*fins*finA,fN=Math.max(N/D,.5);
 const cf=.005*Awet/A,cb=.12,cn=Math.min(.5/(fN*fN),.8),cfin=Math.min(.4*fins*fc.sh.th*finH/A,.3);
 const Cr=root,Ct=.6*root,Lm=.65*root,Xr=.85*root,Lf=Math.hypot(finH,Lm);
 const CNf=(1+r/(finH+r))*(4*fins*Math.pow(finH/D,2))/(1+Math.sqrt(1+Math.pow(2*Lf/(Cr+Ct),2)));
 const Xf=(Xr/3)*(Cr+2*Ct)/(Cr+Ct)+((Cr+Ct)-Cr*Ct/(Cr+Ct))/6,xfin=L-(fc.base.y+root)+Xf;
 const xcp=(2*.466*N+CNf*xfin)/(2+CNf);
 Object.assign(PH,{M,ycg,xcg,xcp,xfin,mar:(xcp-xcg)/D,Cd0:cf+cb+cn+cfin,cf,cb,cn,cfin,A,beta:M/((cf+cb+cn+cfin)*A),fN,CNf});
 [[ycg,0x00D9FF],[L-xcp,0xFFB547]].forEach(a=>{const s=new THREE.Mesh(new THREE.SphereGeometry(Math.max(D*.18,.004),16,12),new THREE.MeshBasicMaterial({color:a[1],depthTest:false}));s.position.y=a[0];s.renderOrder=9;rg.add(s)})}
function physChange(){phys();sim();setChart(S.t.length-1);$('tl').value=1000;showInfo()}
function showPhys(){if(!PH.M)return;const g=9.80665,ap=Math.max(...S.h),ia=S.h.indexOf(ap),Ma=P.v0/Math.sqrt(1.4*287.05*288.15),gm=Math.max(...S.a.map(Math.abs))/g,mar=PH.mar;
 const st=mar<0?['Instable : le centre de pression est devant le centre de gravité. La trajectoire 1D n’est alors pas représentative.','#FF5364']:mar<1?['Marge faible (repère usuel : 1 à 2 calibres)','#FFB547']:mar<=3?['Stable (repère usuel : 1 à 2 calibres)','#39D98A']:['Très stable : sensible au vent','#FFB547'];
 const f=(n,d)=>n.toFixed(d),u=U.get(),row=(a,b)=>`<div style='display:flex;justify-content:space-between;gap:8px;font-size:12px'><span style='color:var(--tx2)'>${a}</span><b>${b}</b></div>`;
 $('ph').innerHTML=row('Masse totale',f(PH.M,3)+' kg')+row('Poids (M·g)',f(PH.M*g,2)+' N')+row('Centre de gravité (<span style="color:#00D9FF">●</span>, depuis le nez)',f(PH.xcg*u,1)+' '+U.name())+row('Centre de pression (<span style="color:#FFB547">●</span>, depuis le nez)',f(PH.xcp*u,1)+' '+U.name())+row('Marge statique',f(mar,2)+' calibres'+(MC.mar?' (5–95 % : '+f(MC.mar[0],2)+' à '+f(MC.mar[2],2)+')':''))+`<div class='msg' style='color:${st[1]};border-color:${st[1]}66'>${st[0]}</div>`+row('Aire de référence',f(PH.A*1e4,2)+' cm²')+row('Finesse L/D · ogive N/D',f(P.L/P.D,1)+' · '+f(PH.fN,1))+row('Cd estimé',f(PH.Cd0,3))+row('dont frottement · culot · ogive · ailerons',f(PH.cf,2)+' · '+f(PH.cb,2)+' · '+f(PH.cn,2)+' · '+f(PH.cfin,2))+row('Coefficient balistique m/(Cd·A)',f(PH.beta,0)+' kg/m²')+row('Apogée simulé (déterministe)',f(ap,1)+' m (t = '+f(S.t[ia],1)+' s)')+mcRows(row)+row('Mach initial',f(Ma,2))+row('Décélération max',f(gm,1)+' g')+(Ma>.8?`<div class='msg w'>Régime transsonique ou supersonique : la traînée est corrigée par un modèle éducatif très simplifié.</div>`:'')+`<div class='note'>Modèle éducatif non validé : masses = surface × épaisseur × densité indicative ; Cd = somme de termes approchés ; stabilité par la méthode de Barrowman simplifiée (subsonique, petits angles) ; montée verticale 1D à vitesse initiale donnée, aucune poussée modélisée.</div>`}
/* ---- incertitudes : Monte-Carlo ---- */
const MC={Hb:[],Vb:[],ap:[],tap:[],gm:[],mar:null,p0:0,p1:0,hmax:0,vmin:0,vmax:0,tmax:0,dt:.1};let MCZ=[];
function rnd(a){return function(){a|=0;a=a+0x6D2B79F5|0;let t=Math.imul(a^a>>>15,1|a);t=t+Math.imul(t^t>>>7,61|t)^t;return((t^t>>>14)>>>0)/4294967296}}
function mcRun(){
 MCZ.forEach(z=>rg.remove(z));MCZ=[];
 const nr=300,rn=rnd(12345),gs=()=>Math.sqrt(-2*Math.log(1-rn()))*Math.cos(2*Math.PI*rn()),dt=MC.dt,sg=k=>(+$(k).value/100)/2,sc=sg('uc'),sm=sg('um'),sv=sg('uv'),sr=sg('ur'),sn=sg('un');
 const runs=[],ap=[],tp=[],gx=[],mars=[],xg=[],xp=[],ms0=C.map(c=>c.mass),ys=C.map(c=>c.yc),{L,N,D}=P;let Lm=0;
 for(let i=0;i<nr;i++){
  const fm=Math.max(.2,1+sm*gs()),fc=Math.max(.2,1+sc*gs()),fv=Math.max(.2,1+sv*gs()),fr=Math.max(.2,1+sr*gs()),m=PH.M*fm;
  let h=0,v=P.v0*fv,t=0,apx=0,tap=0,gmx=0;const H=[],V=[];
  while(t<900){const g=9.80665*Math.pow(6371000/(6371000+h),2),at=isa(h),Cd=PH.Cd0*fc*cdM(Math.abs(v)/Math.sqrt(1.4*287.05*at.T)),a=-g-Math.sign(v)*.5*at.rho*fr*Cd*PH.A*v*v/m;H.push(h);V.push(v);if(h>apx){apx=h;tap=t}gmx=Math.max(gmx,Math.abs(a)/9.80665);v+=a*dt;h+=v*dt;t+=dt;if(h<0&&t>dt)break}
  H.push(0);V.push(0);runs.push({H,V});ap.push(apx);tp.push(tap);gx.push(gmx);Lm=Math.max(Lm,H.length);
  let sw=0,sy=0;ms0.forEach((q,k)=>{const w=q*Math.max(.2,1+sm*gs());sw+=w;sy+=w*ys[k]});const xcg=L-sy/sw,cn=PH.CNf*Math.max(.2,1+sn*gs()),xcp=(2*.466*N+cn*PH.xfin)/(2+cn);
  xg.push(xcg);xp.push(xcp);mars.push((xcp-xcg)/D)}
 const q=(a,p)=>{const s=[...a].sort((x,y)=>x-y);return s[Math.round(p*(s.length-1))]},tri=a=>[q(a,.05),q(a,.5),q(a,.95)];
 MC.ap=tri(ap);MC.tap=tri(tp);MC.gm=tri(gx);MC.mar=tri(mars);MC.p0=mars.filter(x=>x<0).length/nr;MC.p1=mars.filter(x=>x<1).length/nr;
 MC.Hb=[];MC.Vb=[];for(let j=0;j<Lm;j++){const hs=runs.map(r=>r.H[j]??0).sort((x,y)=>x-y),vs=runs.map(r=>r.V[j]??0).sort((x,y)=>x-y),lo=Math.round(.05*(nr-1)),hi=Math.round(.95*(nr-1));MC.Hb.push([j*dt,hs[lo],hs[hi]]);MC.Vb.push([j*dt,vs[lo],vs[hi]])}
 MC.hmax=Math.max(...MC.Hb.map(x=>x[2]));MC.vmin=Math.min(...MC.Vb.map(x=>x[1]));MC.vmax=Math.max(...MC.Vb.map(x=>x[2]));MC.tmax=(Lm-1)*dt;
 if($('ub').checked){const zone=(a,col)=>{const y0=L-q(a,.95),y1=L-q(a,.05),hh=Math.max(y1-y0,.004),z=new THREE.Mesh(new THREE.CylinderGeometry(D*.56,D*.56,hh,32),new THREE.MeshBasicMaterial({color:col,transparent:true,opacity:.28,depthTest:false}));z.position.y=(y0+y1)/2;z.renderOrder=8;rg.add(z);MCZ.push(z)};zone(xg,0x00D9FF);zone(xp,0xFFB547)}}
function mcRows(row){if(!MC.ap.length)return'';const f=(n,d)=>n.toFixed(d);return row('Apogée (5 · 50 · 95 %)',f(MC.ap[0],0)+' · '+f(MC.ap[1],0)+' · '+f(MC.ap[2],0)+' m')+row('Temps d’apogée (5–95 %)',f(MC.tap[0],1)+' à '+f(MC.tap[2],1)+' s')+row('Décélération max (95 %)',f(MC.gm[2],1)+' g')+row('Probabilité marge < 0 · < 1 cal',f(MC.p0*100,0)+' % · '+f(MC.p1*100,0)+' %')}
function bands(n){const on=$('ub').checked&&MC.Hb.length>1,j=on?Math.min(MC.Hb.length-1,Math.floor(S.t[Math.min(n,S.t.length-1)]/MC.dt)):0;
 const mk=(name,key,idx,col)=>({name,type:'custom',yAxisIndex:idx,z:1,silent:true,itemStyle:{color:col},dimensions:['x','y'],encode:{x:0,y:1},data:[[0,0]],renderItem:()=>{if(!on||j<1)return null;const pts=[];for(let i=0;i<=j;i++)pts.push(chart.convertToPixel({yAxisIndex:idx},MC[key][i][2])!==undefined?[chart.convertToPixel({xAxisIndex:0},MC[key][i][0]),chart.convertToPixel({yAxisIndex:idx},MC[key][i][2])]:[0,0]);for(let i=j;i>=0;i--)pts.push([chart.convertToPixel({xAxisIndex:0},MC[key][i][0]),chart.convertToPixel({yAxisIndex:idx},MC[key][i][1])]);return{type:'polygon',shape:{points:pts},style:{fill:col}}}});
 return[mk('Zone d’incertitude altitude (5–95 %)','Hb',0,'rgba(0,217,255,.2)'),mk('Zone d’incertitude vitesse (5–95 %)','Vb',1,'rgba(128,107,255,.2)')]}
function refreshMC(){mcRun();showPhys();setChart(Math.round($('tl').value/1000*(S.t.length-1)))}
['uc','um','uv','ur','un'].forEach(k=>{const u=()=>$(k+'v').textContent='± '+$(k).value+' %';u();$(k).addEventListener('input',()=>{u();refreshMC()})});$('ub').addEventListener('change',refreshMC);
/* ---- Simulation éducative : montée balistique, ISA ---- */
let S={t:[],h:[],v:[],a:[],rho:[],T:[]};
function isa(h){let T,p;if(h<11000){T=288.15-.0065*h;p=101325*Math.pow(T/288.15,5.2559)}else{T=216.65;p=22632*Math.exp(-(h-11000)/6341.6)}return{T,rho:p/(287.05*T)}}
function sim(){const r=P.D/2,A=Math.PI*r*r,m=PH.M,dt=.05;let t=0,h=0,v=P.v0;S={t:[],h:[],v:[],a:[],rho:[],T:[]};
  while(t<900){const g=9.80665*Math.pow(6371000/(6371000+h),2),at=isa(h),Cd=PH.Cd0*cdM(Math.abs(v)/Math.sqrt(1.4*287.05*at.T)),a=-g-Math.sign(v)*.5*at.rho*Cd*A*v*v/m;
    S.t.push(t);S.h.push(h);S.v.push(v);S.a.push(a);S.rho.push(at.rho);S.T.push(at.T);
    v+=a*dt;h+=v*dt;t+=dt;if(h<0&&t>dt){break}}mcRun();showPhys()}
const chart=echarts.init($('chart'));
function setChart(n){const sl=k=>S.t.slice(0,n+1).map((t,i)=>[+t.toFixed(2),+S[k][i].toFixed(3)]);
  const tmax=Math.ceil(Math.max(S.t[S.t.length-1],MC.tmax||0)),hm=Math.max(...S.h,MC.hmax||0)*1.05,vmin=Math.min(...S.v,...S.a,MC.vmin||0),vmax=Math.max(...S.v,...S.a,MC.vmax||0);
  chart.setOption({animation:false,backgroundColor:'transparent',textStyle:{color:'#929BA9'},grid:{left:62,right:62,top:34,bottom:42},legend:{top:4,textStyle:{color:'#F2F4F8'}},tooltip:{trigger:'axis'},
  dataZoom:[{type:'inside'}],
  xAxis:{type:'value',name:'t (s)',min:0,max:tmax,splitLine:{lineStyle:{color:'#252D38'}}},
  yAxis:[{type:'value',name:'altitude (m)',min:0,max:hm,splitLine:{lineStyle:{color:'#252D38'}}},{type:'value',name:'v (m/s), a (m/s²)',min:vmin*1.05,max:vmax*1.05,splitLine:{show:false}}],
  series:[...bands(n),{name:'Altitude (m)',type:'line',showSymbol:false,data:sl('h'),itemStyle:{color:'#00D9FF'}},{name:'Vitesse (m/s)',type:'line',yAxisIndex:1,showSymbol:false,data:sl('v'),itemStyle:{color:'#806BFF'}},{name:'Accélération (m/s²)',type:'line',yAxisIndex:1,showSymbol:false,data:sl('a'),itemStyle:{color:'#FFB547'}}]});
  const i=Math.min(n,S.t.length-1);
  $('ro').innerHTML=`t <b>${S.t[i].toFixed(1)} s</b> · alt <b>${S.h[i].toFixed(0)} m</b> · v <b>${S.v[i].toFixed(1)}</b> · a <b>${S.a[i].toFixed(2)}</b> · ρ <b>${S.rho[i].toFixed(3)} kg/m³</b> · T <b>${S.T[i].toFixed(1)} K</b> · m <b>${PH.M.toFixed(3)} kg</b>`}
let playing=false,simT=0,hold=false,fc=0;
$('run').onclick=()=>{simT=0;playing=true};$('pause').onclick=()=>{playing=!playing};
$('rst').onclick=()=>{playing=false;simT=0;setChart(0);$('tl').value=0};
$('tl').oninput=e=>{playing=false;const n=Math.round(e.target.value/1000*(S.t.length-1));simT=S.t[n];setChart(n)};
$('png').onclick=()=>{const a=document.createElement('a');a.href=chart.getDataURL({backgroundColor:'#10141B',pixelRatio:2});a.download='beutterfly_graphique.png';a.click()};
$('csv').onclick=()=>{let s='t_s,altitude_m,vitesse_m_s,acceleration_m_s2,densite_kg_m3,temperature_K\n';S.t.forEach((t,i)=>s+=[t.toFixed(2),S.h[i].toFixed(3),S.v[i].toFixed(3),S.a[i].toFixed(4),S.rho[i].toFixed(5),S.T[i].toFixed(2)].join(',')+'\n');const a=document.createElement('a');a.href=URL.createObjectURL(new Blob([s],{type:'text/csv'}));a.download='beutterfly_simulation.csv';a.click()};

/* ---- boucle ---- */
let last=0,V=new THREE.Vector3();
function frame(ts){requestAnimationFrame(frame);const dt=Math.min((ts-last)/1000,.1);last=ts;
  ex+=(exT-ex)*Math.min(1,dt*(+$('exs').value));if(Math.abs(exT-ex)>.001||exT)apply(ex);
  if(playing){simT+=dt*(+$('spd').value);const n=Math.min(Math.round(simT/.05),S.t.length-1);if(++fc%3===0||n===S.t.length-1)setChart(n);$('tl').value=n/(S.t.length-1)*1000;if(n>=S.t.length-1)playing=false}
  updCam();R.render(sc,cam);if(!$('detail').hidden&&dh){if(dd===null)dh.rotation.y+=dt*.7;dResize();DR.render(dsc,dcam)}
  const show=ex>.04;C.forEach(c=>{const l=$('l_'+c.id);if(!l)return;const sv=show&&c.obj.visible;l.style.display=sv?'block':'none';if(sv){V.setFromMatrixPosition(c.obj.matrixWorld);if(c.id!=='fins')V.y+=c.len*.0;V.project(cam);l.style.left=(V.x*.5+.5)*vp.clientWidth+'px';l.style.top=(-V.y*.5+.5)*vp.clientHeight+'px'}})}
validate();labels();fit();build();sim();resize();setChart(S.t.length-1);requestAnimationFrame(frame);
</script>
</body>
</html>
