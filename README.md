<!doctype html>
<html lang="kk">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>INTK Delivery</title>
  <style>
    :root{--accent:#ffcc00;--bg:#f6f7fb;--card:#fff;--muted:#666}
    *{box-sizing:border-box;font-family:Inter,Segoe UI,Roboto,Arial,sans-serif}
    body{margin:0;background:var(--bg);padding:24px;display:flex;align-items:center;justify-content:center;min-height:100vh}
    .container{width:100%;max-width:860px;background:linear-gradient(180deg,#fff,#fbfbff);border-radius:12px;box-shadow:0 8px 30px rgba(30,30,80,0.08);padding:26px}
    h1{margin:0 0 8px;font-size:20px}
    p.lead{margin:0 0 18px;color:var(--muted)}
    .grid{display:grid;grid-template-columns:1fr 330px;gap:20px}
    .box{background:var(--card);padding:18px;border-radius:10px;border:1px solid rgba(20,20,60,0.04)}
    label{display:block;font-size:13px;margin-bottom:6px;color:#222}
    input,textarea,select{width:100%;padding:10px;border-radius:8px;border:1px solid #e5e7f0;background:white;font-size:14px}
    .btn{display:inline-block;padding:10px 14px;border-radius:8px;background:var(--accent);border:none;color:#111;font-weight:600;cursor:pointer}
    .muted{color:var(--muted);font-size:13px}
    .field-row{display:flex;gap:10px}
    .field-row > *{flex:1}
    .small{font-size:13px}
    .error{color:#b00020;font-size:13px;margin-top:6px}
    .success{color:green;font-size:14px;margin-top:8px}
    /* modal */
    .modal-backdrop{position:fixed;inset:0;background:rgba(0,0,0,0.45);display:flex;align-items:center;justify-content:center}
    .modal{width:100%;max-width:420px;background:white;padding:20px;border-radius:10px}
    .center{text-align:center}
    footer{margin-top:14px;font-size:13px;color:var(--muted);text-align:center}
    @media(max-width:820px){.grid{grid-template-columns:1fr;}.container{padding:18px}}
  </style>
</head>
<body>
  <div class="container">
    <h1>Yandex Delivery — Тапсырыс үлгісі</h1>
    <p class="lead">Алдымен тіркеліңіз, сосын жеткізу және төлем деректерін толтырыңыз.</p>

    <div class="grid">
      <div class="box" id="order-area">
        <h3>Тапсырыс формасы</h3>
        <p class="muted">Тіркелмесеңіз тапсырыс жібере алмайсыз.</p>

        <form id="order-form" onsubmit="return false;">
          <div>
            <label>Аты-жөні *</label>
            <input id="fullname" type="text" placeholder="Мысалы: Айдос Нұрғалиев" required />
            <div id="err-name" class="error" style="display:none">Аты-жөнін енгізіңіз</div>
          </div>

          <div style="margin-top:12px">
            <label>Жеткізу мекен-жайы *</label>
            <textarea id="address" rows="3" placeholder="Қала, көше, үй/пәтер" required></textarea>
            <div id="err-address" class="error" style="display:none">Мекен-жайды толтырыңыз</div>
          </div>

          <div style="margin-top:12px">
            <label>Жеткізу уақыты</label>
            <select id="delivery-time">
              <option>Бүгін — мүмкіндігінше жылдам</option>
              <option>Ертең таңертең</option>
              <option>Ертең түстен кейін</option>
            </select>
          </div>

          <hr style="margin:16px 0" />

          <h4>Төлем (демо)</h4>
          <div>
            <label>Карта нөмірі *</label>
            <input id="card-number" type="text" inputmode="numeric" maxlength="19" placeholder="4242 4242 4242 4242" />
            <div id="err-card" class="error" style="display:none"></div>
          </div>

          <div class="field-row" style="margin-top:10px">
            <div>
              <label>Жарамдылық (MM/YY) *</label>
              <input id="exp" type="text" placeholder="09/26" maxlength="5" />
              <div id="err-exp" class="error" style="display:none"></div>
            </div>
            <div>
              <label>CVC *</label>
              <input id="cvc" type="password" inputmode="numeric" maxlength="4" placeholder="123" />
              <div id="err-cvc" class="error" style="display:none"></div>
            </div>
          </div>

          <div style="margin-top:12px" class="muted small">
            Бұл демо форма — нақты төлем үшін карта деректерін қауіпсіз түрде өңдейтін төлем провайдерін (tokenization) қолданыңыз.
          </div>

          <div style="margin-top:14px;display:flex;gap:10px;align-items:center">
            <button id="submit-btn" class="btn">Тапсырыс жіберу</button>
            <div id="form-msg" class="small"></div>
          </div>

          <div id="result" style="margin-top:10px"></div>
        </form>
      </div>

      <div class="box">
        <h3>Пайдаланушы</h3>
        <div id="user-info">
          <p class="muted">Сіз тіркелмегенсіз</p>
          <button class="btn" onclick="openRegister()">Тіркелу / Кіру</button>
        </div>

        <hr style="margin:14px 0" />
        <h4>Жеткізу бағалары (мысал)</h4>
        <ul class="muted small">
          <li>Қалалық — 500 ₸</li>
          <li>Қашықтық — есептеледі</li>
          <li>Экспресс жеткізу — қосымша 1200 ₸</li>
        </ul>

        <footer>Ескерту: нақты интеграция үшін Yandex.Delivery API және төлем провайдерінің құжаттамасын пайдаланыңыз.</footer>
      </div>
    </div>
  </div>

  <!-- Registration modal -->
  <div id="modal" style="display:none" class="modal-backdrop">
    <div class="modal">
      <h3 class="center">Тіркелу / Кіру</h3>
      <p class="muted center">Тіркелу немесе қарапайым логин арқылы жалғастырыңыз</p>

      <div style="margin-top:8px">
        <label>E-mail</label>
        <input id="reg-email" type="email" placeholder="example@mail.com" />
        <div id="err-email" class="error" style="display:none"></div>
      </div>

      <div style="margin-top:8px" id="name-field">
        <label>Аты-жөні</label>
        <input id="reg-name" type="text" placeholder="Толық атыңыз" />
      </div>

      <div style="margin-top:8px">
        <label>Құпиясөз</label>
        <input id="reg-pass" type="password" placeholder="Құпиясөз" />
      </div>

      <div style="margin-top:12px;display:flex;gap:10px;justify-content:center">
        <button class="btn" onclick="doRegister()">Тіркелу</button>
        <button class="btn" style="background:#ddd" onclick="closeRegister()">Болдырмау</button>
      </div>
      <p style="margin-top:10px" class="muted small">Бұл үлгі. Тіркелгі деректерін қауіпсіз түрде серверде сақтаңыз және реальды жүктемелерде HTTPS пайдаланыңыз.</p>
    </div>
  </div>

  <script>
    // Қарапайым локальді "кіру" — демо үшін
    let currentUser = null;

    function openRegister(){
      document.getElementById('modal').style.display = 'flex';
    }
    function closeRegister(){
      document.getElementById('modal').style.display = 'none';
    }

    function doRegister(){
      const email = document.getElementById('reg-email').value.trim();
      const name = document.getElementById('reg-name').value.trim();
      const pass = document.getElementById('reg-pass').value;
      let ok = true;
      document.getElementById('err-email').style.display = 'none';

      if(!email || !email.includes('@')){ document.getElementById('err-email').innerText = 'Жарамды email енгізіңіз'; document.getElementById('err-email').style.display='block'; ok=false;}
      if(!ok) return;
      // Демода біз локальде ғана сақтаймыз — нақты жүйеде серверге жіберіңіз
      currentUser = {email,name};
      document.getElementById('user-info').innerHTML = `
        <p><strong>${name || email}</strong></p>
        <p class="muted small">${email}</p>
        <button class="btn" onclick="logout()">Шығу</button>
      `;
      closeRegister();
    }

    function logout(){
      currentUser = null;
      document.getElementById('user-info').innerHTML = `<p class="muted">Сіз тіркелмегенсіз</p><button class="btn" onclick="openRegister()">Тіркелу / Кіру</button>`;
    }

    // Luhn тексеру — карта нөмірінің валидтілігін бастапқы тексеру
    function luhnCheck(num){
      const s = num.replace(/\D/g,'');
      let sum = 0, alt=false;
      for(let i=s.length-1;i>=0;i--){
        let n = parseInt(s[i],10);
        if(alt){ n*=2; if(n>9) n-=9; }
        sum += n;
        alt = !alt;
      }
      return s.length>=12 && (sum % 10 === 0);
    }

    function validateExp(exp){
      if(!/^\d{2}\/\d{2}$/.test(exp)) return false;
      const [mm,yy] = exp.split('/').map(x=>parseInt(x,10));
      if(mm<1||mm>12) return false;
      const now = new Date();
      const thisYY = now.getFullYear() % 100;
      const thisMM = now.getMonth()+1;
      if(yy < thisYY) return false;
      if(yy === thisYY && mm < thisMM) return false;
      return true;
    }

    document.getElementById('submit-btn').addEventListener('click', ()=>{
      // Тіркелгенін тексеру
      if(!currentUser){
        alert('Тапсырыс жіберу үшін алдымен тіркеліңіз.');
        openRegister();
        return;
      }

      // Форманы тексеру
      const name = document.getElementById('fullname').value.trim();
      const address = document.getElementById('address').value.trim();
      const card = document.getElementById('card-number').value.trim();
      const exp = document.getElementById('exp').value.trim();
      const cvc = document.getElementById('cvc').value.trim();

      let ok = true;
      document.getElementById('err-name').style.display='none';
      document.getElementById('err-address').style.display='none';
      document.getElementById('err-card').style.display='none';
      document.getElementById('err-exp').style.display='none';
      document.getElementById('err-cvc').style.display='none';
      document.getElementById('form-msg').innerText='';

      if(!name){ document.getElementById('err-name').style.display='block'; ok=false;}
      if(!address){ document.getElementById('err-address').style.display='block'; ok=false;}
      if(!luhnCheck(card)){ document.getElementById('err-card').innerText='Карта нөмірі жарамсыз'; document.getElementById('err-card').style.display='block'; ok=false;}
      if(!validateExp(exp)){ document.getElementById('err-exp').innerText='Жарамдылық мерзімі жарамсыз'; document.getElementById('err-exp').style.display='block'; ok=false;}
      if(!/^\d{3,4}$/.test(cvc)){ document.getElementById('err-cvc').innerText='CVC 3 немесе 4 саннан тұруы керек'; document.getElementById('err-cvc').style.display='block'; ok=false;}

      if(!ok) return;

      // Демода: карта дерегін жібермейміз. Нақты жүйеде төмендегі әрекет орындалуы керек:
      // 1) Төлем провайдерінің JS SDK арқылы картадан токен (payment token) алу
      // 2) Токенді ғана өз серверіңізге жіберіп, сол серверден төлемді аяқтау
      // Міне — біз тек симуляция жасаймыз:
      document.getElementById('form-msg').innerText = 'Жіберілуде...';
      setTimeout(()=>{
        const fakeOrderId = 'ORD-' + Math.random().toString(36).slice(2,9).toUpperCase();
        document.getElementById('result').innerHTML = `<div class="success">Тапсырыс қабылданды — №${fakeOrderId}. Жеткізу туралы хабарлама сіздің email: <strong>${currentUser.email}</strong></div>`;
        document.getElementById('form-msg').innerText = '';
      },900);
    });
  </script>
</body>
</html>
