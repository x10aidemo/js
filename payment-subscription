/* payment-v2.js */
(function(){
  // --- CONFIG ---
  const CONFIG={
    url:'https://n8n.capchat.live/webhook/payment-slip',
    plans:{
      saver:{p:999,m:899,a:799},
      pro:{p:1999,m:1799,a:1599},
      premium:{p:4999,m:4500,a:3999}
    },
    // VERIFIED NEW BANK INFO
    bank:{
        n:'กสิกรไทย จำกัด(มหาชน)', 
        a:'206-2-74965-5', 
        o:'Euroform (Thailand)'
    }
  };

  // --- STYLES ---
  const css=`body{font-family:sans-serif;bg:#f7fafc;margin:0;padding:20px;color:#2d3748}.box{max-width:480px;margin:40px auto;bg:#fff;border-radius:16px;box-shadow:0 10px 30px #0000001a}.head{padding:24px;border-bottom:1px solid #edf2f7;text-align:center;font-weight:700;font-size:20px}.body{padding:32px}.row{margin-bottom:20px}label{display:block;font-size:14px;font-weight:600;margin-bottom:8px;color:#4a5568}.in{width:100%;padding:12px;border:1px solid #e2e8f0;border-radius:8px;font-size:16px;box-sizing:border-box}.sum{bg:#f0fff4;padding:20px;text-align:center;border-radius:12px;color:#276749;font-weight:800;font-size:36px;margin:24px 0;border:1px solid #c6f6d5}.btn{width:100%;padding:16px;bg:#2d3748;color:#fff;border:none;border-radius:10px;font-weight:600;cursor:pointer;font-size:16px;mt:20px}.btn:disabled{opacity:.6}.hide{display:none}.bank{bg:#ebf8ff;padding:20px;border-radius:12px;text-align:center;mb:24px;border:1px solid #bee3f8}.up{border:2px dashed #cbd5e0;padding:30px;text-align:center;border-radius:12px;cursor:pointer;mt:10px}.badge{text-align:center;color:#e53e3e;font-size:13px;mt:-15px;mb:20px;display:none;font-weight:500}a.back{text-decoration:none;font-size:14px;color:#718096;display:block;text-align:center;margin-top:24px}`;

  // --- HTML ---
  const html=`<div class="box"><div class="head">ชำระค่าสมาชิกรายเดือน</div><div class="body">
  <div id="s1"><div class="row"><label>แพ็กเกจ</label><select id="ip" class="in"><option value="saver">Saver</option><option value="pro">Pro</option><option value="premium">Premium</option></select></div>
  <div class="row"><label>ระยะเวลา</label><select id="im" class="in"><option value="1">1 เดือน</option><option value="6">6 เดือน</option><option value="12">12 เดือน</option></select></div>
  <div id="sum-box" class="sum"><span id="dp">0</span> ฿</div><div id="bdg" class="badge"></div><button id="bn" class="btn">ชำระเงิน</button><a href="/" class="back">← กลับสู่เว็บไซต์หลัก</a></div>
  <div id="s2" class="hide"><div class="bank"><b>${CONFIG.bank.n}</b><br>${CONFIG.bank.a}<br>${CONFIG.bank.o}</div>
  <div class="row"><label>ชื่อผู้ติดต่อ / ชื่อธุรกิจ</label><input type="text" id="nm" class="in" placeholder="ระบุชื่อ"></div>
  <div class="row"><label>เบอร์โทรศัพท์</label><input type="tel" id="ph" class="in" placeholder="08x-xxx-xxxx"></div>
  <div class="up" id="du">📸 แนบสลิป<div id="df" style="font-size:13px;color:#3182ce;mt:8px"></div></div><input type="file" id="if" hidden accept="image/*">
  <button id="bs" class="btn" style="background:#38a169" disabled>ยืนยัน</button><div id="bk" style="text-align:center;margin-top:16px;cursor:pointer;font-size:14px;color:#718096">ย้อนกลับ</div></div>
  <div id="s3" class="hide" style="text-align:center;padding:40px"><div style="font-size:60px;mb:16px">🎉</div><h2 style="margin:0">สำเร็จ!</h2><p style="color:#718096;margin:16px 0 32px">รอตรวจสอบ 15 นาที</p><button class="btn" onclick="location.href='/'">กลับหน้าหลัก</button></div></div></div>`;

  // --- LOGIC ---
  function init(){
    const s=document.createElement('style');s.textContent=css.replace(/bg:/g,'background:').replace(/mt:/g,'margin-top:').replace(/mb:/g,'margin-bottom:');document.head.appendChild(s);
    document.body.innerHTML=html;
    const g=id=>document.getElementById(id);
    const p=new URLSearchParams(location.search);if(p.get('plan')&&CONFIG.plans[p.get('plan')])g('ip').value=p.get('plan');
    
    const upd=()=>{
      const k=g('ip').value,pl=CONFIG.plans[k]||CONFIG.plans.saver,ops=g('im').options;
      ops[0].text=`1 เดือน (${pl.p.toLocaleString()}/ด)`;
      ops[1].text=`6 เดือน (${pl.m.toLocaleString()}/ด)`;
      ops[2].text=`12 เดือน (${pl.a.toLocaleString()}/ด)`;
    };
    const calc=()=>{
      const k=g('ip').value,pl=CONFIG.plans[k]||CONFIG.plans.saver,m=parseInt(g('im').value);
      const pr=(m==12?pl.a:(m==6?pl.m:pl.p))*m;
      g('dp').innerText=pr.toLocaleString();
      const sav=(pl.p*m)-pr,b=g('bdg');
      if(sav>0){b.innerText=`✨ ประหยัด ${sav.toLocaleString()} บาท`;b.style.display='block'}else{b.style.display='none'}
    };
    
    g('ip').addEventListener('change',()=>{upd();calc()});
    g('im').addEventListener('change',calc);
    upd();calc();

    g('bn').onclick=()=>{g('s1').classList.add('hide');g('s2').classList.remove('hide')};
    g('bk').onclick=()=>{g('s2').classList.add('hide');g('s1').classList.remove('hide')};
    g('du').onclick=()=>g('if').click();
    g('if').onchange=e=>{if(e.target.files[0]){g('df').innerText='✅ '+e.target.files[0].name;g('bs').disabled=0}};
    
    g('bs').onclick=async()=>{
      const n=g('nm').value.trim(),ph=g('ph').value.trim(),f=g('if').files[0];
      if(n.length<2||ph.length<9)return alert('กรุณากรอกข้อมูลให้ครบถ้วน');
      g('bs').innerText='กำลังส่ง...';g('bs').disabled=1;
      const d=new FormData();d.append('file',f);d.append('plan',g('ip').value);d.append('months',g('im').value);d.append('amount',g('dp').innerText);d.append('name',n);d.append('phone',ph);
      try{await fetch(CONFIG.url,{method:'POST',body:d});g('s2').classList.add('hide');g('s3').classList.remove('hide')}
      catch(e){alert('Error');g('bs').disabled=0;g('bs').innerText='ยืนยัน'}
    };
  }
  if(document.readyState==='loading')document.addEventListener('DOMContentLoaded',init);else init();
})();
