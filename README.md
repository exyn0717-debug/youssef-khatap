[calculator eng youssef.html](https://github.com/user-attachments/files/33035733/calculator.eng.youssef.html)
<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>آلة حاسبة</title>
<style>
  :root{--bg1:#0f172a;--bg2:#1e1b4b;--card:#1e293b;--line:#334155;--txt:#e2e8f0;--mut:#94a3b8;
        --num:#334155;--fn:#475569;--op:#4f46e5;--eqa:#818cf8;--eqb:#22d3ee;--err:#f87171}
  *{box-sizing:border-box;margin:0;padding:0;-webkit-tap-highlight-color:transparent}
  body{font-family:"Segoe UI",Tahoma,Arial,sans-serif;background:linear-gradient(160deg,var(--bg1),var(--bg2));
       color:var(--txt);min-height:100vh;display:flex;justify-content:center;align-items:flex-start;padding:22px 14px}
  .app{display:flex;gap:18px;flex-wrap:wrap;justify-content:center;width:100%;max-width:840px}
  .calc{flex:1 1 340px;max-width:420px;background:var(--card);border:1px solid var(--line);border-radius:22px;padding:16px}
  .side{flex:1 1 240px;max-width:300px;display:flex;flex-direction:column;gap:10px}
  .display{background:#0b1226;border:1px solid var(--line);border-radius:16px;padding:14px 16px;margin-bottom:14px;
           min-height:118px;display:flex;flex-direction:column;justify-content:flex-end;gap:6px;overflow:hidden}
  .display.shake{animation:shake .35s}
  @keyframes shake{0%,100%{transform:translateX(0)}25%{transform:translateX(-7px)}75%{transform:translateX(7px)}}
  .preview{font-size:15px;color:var(--mut);min-height:20px;direction:ltr;text-align:right;white-space:nowrap;
           overflow:hidden;text-overflow:ellipsis}
  .preview.err{color:var(--err);font-weight:700;direction:rtl;text-align:right}
  .expr{font-size:2.6rem;font-weight:700;direction:ltr;text-align:right;white-space:nowrap;overflow:hidden;
        text-overflow:ellipsis;letter-spacing:.5px;min-height:52px}
  .keys{display:grid;grid-template-columns:repeat(4,1fr);gap:10px;direction:ltr}
  button{border:0;border-radius:16px;font-family:inherit;font-size:1.35rem;font-weight:600;color:var(--txt);
         padding:16px 0;cursor:pointer;background:var(--num);transition:transform .07s,filter .12s;user-select:none}
  button:hover{filter:brightness(1.18)}
  button.active,button:active{transform:scale(.94);filter:brightness(1.3)}
  .fn{background:var(--fn);font-size:1.2rem}
  .op{background:var(--op);font-size:1.5rem}
  .eq{grid-column:1/-1;background:linear-gradient(135deg,var(--eqa),var(--eqb));color:#0b1020;font-size:1.5rem;padding:14px 0}
  .hist-head{display:flex;justify-content:space-between;align-items:center;font-size:14px;color:var(--mut)}
  .hist{background:var(--card);border:1px solid var(--line);border-radius:16px;padding:8px;min-height:80px;
        max-height:300px;overflow:auto;display:flex;flex-direction:column;gap:6px}
  .hitem{background:#0b1226;border:1px solid #263248;border-radius:10px;padding:8px 10px;cursor:pointer;
         display:flex;justify-content:space-between;gap:8px;font-size:13.5px;direction:ltr;transition:border-color .15s}
  .hitem:hover{border-color:var(--eqa)}
  .he{color:var(--mut);overflow:hidden;text-overflow:ellipsis;white-space:nowrap}
  .hr{color:#34d399;font-weight:700;white-space:nowrap}
  .empty{color:#64748b;font-size:13px;text-align:center;padding:14px 0}
  .ghost{background:transparent;border:1px solid var(--line);color:var(--mut);font-size:13px;font-weight:500;
         padding:7px 12px;border-radius:10px}
  .wide{width:100%;padding:11px}
  #toast{position:fixed;bottom:18px;right:50%;transform:translateX(50%) translateY(80px);background:#0b1226;
         border:1px solid var(--eqa);color:var(--txt);padding:11px 18px;border-radius:12px;font-size:14px;
         transition:transform .25s;pointer-events:none;z-index:9;direction:ltr}
  #toast.show{transform:translateX(50%) translateY(0)}
  .tip{color:#64748b;font-size:12.5px;text-align:center;margin-top:12px;line-height:1.8}
</style>
</head>
<body>
<div class="app">
  <div class="calc">
    <div class="display" id="display">
      <div class="preview" id="preview"></div>
      <div class="expr" id="expr">0</div>
    </div>
    <div class="keys" id="keys">
      <button class="fn" data-k="C">C</button>
      <button class="fn" data-k="back">⌫</button>
      <button class="fn" data-k="(">(</button>
      <button class="fn" data-k=")">)</button>
      <button data-k="7">7</button><button data-k="8">8</button><button data-k="9">9</button>
      <button class="op" data-k="/">÷</button>
      <button data-k="4">4</button><button data-k="5">5</button><button data-k="6">6</button>
      <button class="op" data-k="*">×</button>
      <button data-k="1">1</button><button data-k="2">2</button><button data-k="3">3</button>
      <button class="op" data-k="-">−</button>
      <button class="fn" data-k="±">±</button><button data-k="0">0</button><button data-k=".">.</button>
      <button class="op" data-k="+">+</button>
      <button class="fn" data-k="%">%</button>
      <button class="eq" data-k="=">=</button>
    </div>
    <div class="tip">تقدر تستخدم الكيبورد كمان: الأرقام · العمليات · Enter = يساوي · Backspace = مسح خانة · Esc = مسح الكل</div>
  </div>

  <div class="side">
    <div class="hist-head"><span>السجل الأخير</span><button class="ghost" id="clearHist">مسح السجل</button></div>
    <div class="hist" id="hist"><div class="empty">لسه مفيش حسابات…</div></div>
    <button class="ghost wide" id="copy">📋 نسخ الناتج الظاهر</button>
  </div>
</div>
<div id="toast"></div>

<script>
/* ==== ENGINE :START ==== */
function tokenize(s){
  const T=[], src=s
    .replace(/[٠-٩]/g,d=>"٠١٢٣٤٥٦٧٨٩".indexOf(d))
    .replace(/[۰-۹]/g,d=>"۰۱۲۳۴۵۶۷۸۹".indexOf(d))
    .replace(/\u066B/g,".").replace(/\u066A/g,"%")
    .replace(/×/g,"*").replace(/÷/g,"/").replace(/−/g,"-");
  let i=0, prev=null;
  const digit=c=>c>="0"&&c<="9";
  while(i<src.length){
    const c=src[i];
    if(c===" "){i++;continue;}
    if(digit(c)||c==="."){
      let j=i; while(j<src.length&&(digit(src[j])||src[j]===".")) j++;
      const raw=src.slice(i,j);
      if((raw.match(/\./g)||[]).length>1) throw "رقم فيه أكتر من نقطة";
      T.push({t:"num",v:parseFloat(raw)}); prev="num"; i=j; continue;
    }
    if("+-*/".includes(c)){
      const unary=(prev===null||prev==="op"||prev==="lp");
      if(c==="-"&&unary){
        let j=i+1; while(j<src.length&&(digit(src[j])||src[j]===".")) j++;
        if(j>i+1){ T.push({t:"num",v:parseFloat(src.slice(i,j))}); prev="num"; i=j; continue; }
        T.push({t:"neg"}); prev="op"; i++; continue;   // سالب قبل قوس أو في الأول
      }
      if(c==="+"&&unary){i++;continue;}
      if(prev!=="num"&&prev!=="rp"&&prev!=="pct") throw "الصيغة ناقصة";
      T.push({t:"op",v:c}); prev="op"; i++; continue;
    }
    if(c==="("){T.push({t:"lp"});prev="lp";i++;continue;}
    if(c===")"){T.push({t:"rp"});prev="rp";i++;continue;}
    if(c==="%"){if(prev!=="num"&&prev!=="rp") throw "علامة % في مكان غلط";T.push({t:"pct"});prev="pct";i++;continue;}
    throw "رمز غير معروف: "+c;
  }
  return T;
}
function toRPN(T){
  const prec={"+":1,"-":1,"*":2,"/":2}, out=[], ops=[];
  const tp=o=>o.t==="neg"?4:prec[o.v];
  for(const tk of T){
    if(tk.t==="num"||tk.t==="pct") out.push(tk);
    else if(tk.t==="lp") ops.push(tk);
    else if(tk.t==="rp"){ while(ops.length&&ops[ops.length-1].t!=="lp") out.push(ops.pop()); if(ops.length) ops.pop(); }
    else if(tk.t==="neg"){ while(ops.length&&ops[ops.length-1].t!=="lp"&&tp(ops[ops.length-1])>=4) out.push(ops.pop()); ops.push(tk); }
    else { while(ops.length&&ops[ops.length-1].t!=="lp"&&tp(ops[ops.length-1])>=prec[tk.v]) out.push(ops.pop()); ops.push(tk); }
  }
  while(ops.length){ const o=ops.pop(); if(o.t!=="lp") out.push(o); }
  return out;
}
function evalRPN(rpn){
  const st=[];
  for(const tk of rpn){
    if(tk.t==="num") st.push(tk.v);
    else if(tk.t==="pct"){ if(!st.length) throw "الصيغة ناقصة"; st.push(st.pop()/100); }
    else if(tk.t==="neg"){ if(!st.length) throw "الصيغة ناقصة"; st.push(-st.pop()); }
    else {
      if(st.length<2) throw "الصيغة ناقصة";
      const b=st.pop(), a=st.pop();
      if(tk.v==="/"&&b===0) throw "مش ممكن القسمة على صفر";
      st.push(tk.v==="+"?a+b:tk.v==="-"?a-b:tk.v==="*"?a*b:a/b);
    }
  }
  if(st.length!==1) throw "الصيغة ناقصة";
  const r=st[0];
  if(!isFinite(r)||isNaN(r)) throw "الناتج مش رقم صالح";
  return r;
}
function calc(expr){ return evalRPN(toRPN(tokenize(expr))); }
function fmt(n){ const r=Math.abs(n)>=1e12?n:parseFloat(n.toPrecision(12)); return String(r); }
/* ==== ENGINE :END ==== */

/* ==== UI :START ==== */
const $=id=>document.getElementById(id);
const exprEl=$("expr"), previewEl=$("preview"), histEl=$("hist"), toastEl=$("toast"), dispEl=$("display");
const PRETTY=s=>s.replace(/\*/g,"×").replace(/\//g,"÷").replace(/-/g,"−");
const esc=s=>s.replace(/&/g,"&amp;").replace(/</g,"&lt;").replace(/>/g,"&gt;");
let expr="", justDone=false, history=[];

function render(){
  exprEl.textContent = expr ? PRETTY(expr) : "0";
  const L=exprEl.textContent.length;
  exprEl.style.fontSize = L>20?"1.5rem":L>13?"2rem":"2.6rem";
  previewEl.className="preview";
  if(expr && !justDone && /[+\-*/%(]/.test(expr.slice(1))){
    try{ previewEl.textContent="= "+fmt(calc(expr)); }catch(e){ previewEl.textContent=""; }
  } else previewEl.textContent="";
}
function renderHist(){
  histEl.innerHTML = history.length
    ? history.map((h,i)=>`<div class="hitem" data-i="${i}"><span class="he">${esc(h.e)}</span><span class="hr">= ${esc(h.r)}</span></div>`).join("")
    : '<div class="empty">لسه مفيش حسابات…</div>';
}
function flash(msg){
  previewEl.className="preview err"; previewEl.textContent="⚠ "+msg;
  dispEl.classList.remove("shake"); void dispEl.offsetWidth; dispEl.classList.add("shake");
}
function toast(msg){
  toastEl.textContent=msg; toastEl.classList.add("show");
  clearTimeout(toast._t); toast._t=setTimeout(()=>toastEl.classList.remove("show"),2200);
}
function press(k){
  if(k==="C"){ expr=""; justDone=false; }
  else if(k==="back"){ expr=expr.slice(0,-1); justDone=false; }
  else if(k==="="){
    if(!expr) return;
    try{
      const shown=fmt(calc(expr));
      history.unshift({e:PRETTY(expr), r:shown}); history=history.slice(0,6); renderHist();
      expr=shown; justDone=true;
    }catch(e){ flash(e); return; }
  }
  else if(k==="±"){
    const m=expr.match(/\(-(\d+\.?\d*)\)$/);
    if(m) expr=expr.slice(0,m.index)+m[1];
    else if(/(\d+\.?\d*)$/.test(expr)) expr=expr.replace(/(\d+\.?\d*)$/,"(-$1)");
    justDone=false;
  }
  else {
    if(expr.length>=42) return;
    if(justDone && /[0-9.(]/.test(k)) expr="";
    justDone=false;
    if("+-*/".includes(k)){
      const lastc=expr.slice(-1);
      if(!expr){ if(k==="-") expr="-"; }
      else if("+-*/".includes(lastc)) expr=expr.slice(0,-1)+k;
      else if(lastc==="("){ if(k==="-") expr+=k; }
      else expr+=k;
    }
    else if(k==="."){
      const tail=expr.split(/[+\-*/()%]/).pop();
      if(!tail.includes(".")) expr += tail ? "." : "0.";
    }
    else if(k===")"){ 
      const open=(expr.match(/\(/g)||[]).length, close=(expr.match(/\)/g)||[]).length;
      if(open>close && /[\d)%]$/.test(expr)) expr+=")";
    }
    else if(k==="%"){ if(/[\d)]$/.test(expr)) expr+="%"; }
    else if(k==="("){ expr += /[\d)]$/.test(expr) ? "×(" : "("; }
    else { const lastc=expr.slice(-1);
      if(lastc===")"||lastc==="%") expr+="×";
      expr+=k;
    }
  }
  render();
}
document.querySelectorAll("[data-k]").forEach(b=>b.addEventListener("click",()=>press(b.dataset.k)));
document.addEventListener("keydown",e=>{
  let k=null;
  if(/^[0-9]$/.test(e.key)) k=e.key;
  else if("+-*/%().".includes(e.key)&&e.key.length===1) k=e.key;
  else if(e.key==="Enter"||e.key==="=") k="=";
  else if(e.key==="Backspace") k="back";
  else if(e.key==="Escape"||e.key==="Delete") k="C";
  else if(/[٠-٩]/.test(e.key)) k=String("٠١٢٣٤٥٦٧٨٩".indexOf(e.key));
  if(k){ e.preventDefault(); press(k);
    const btn=[...document.querySelectorAll("[data-k]")].find(b=>b.dataset.k===k);
    if(btn){ btn.classList.add("active"); setTimeout(()=>btn.classList.remove("active"),120); } }
});
histEl.addEventListener("click",e=>{
  const it=e.target.closest(".hitem"); if(!it) return;
  expr=history[+it.dataset.i].e.replace(/×/g,"*").replace(/÷/g,"/").replace(/−/g,"-");
  justDone=false; render();
});
$("clearHist").addEventListener("click",()=>{ history=[]; renderHist(); });
$("copy").addEventListener("click",async()=>{
  const v=exprEl.textContent;
  try{ await navigator.clipboard.writeText(v); toast("📋 اتنسخ: "+v); }
  catch(e){ toast("مقدرتش أنسخ أوتوماتيك — الناتج: "+v); }
});
render(); renderHist();
/* ==== UI :END ==== */
</script>
</body>
</html>
