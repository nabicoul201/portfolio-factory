<!DOCTYPE html>
<html lang="fr">
<head>
<meta charset="UTF-8"/>
<meta name="viewport" content="width=device-width,initial-scale=1"/>
<title>Portfolio Factory v3.0</title>
<script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
<style>
*{box-sizing:border-box;margin:0;padding:0}
:root{
  --bg:#0c0c10;--bg2:#13131a;--bg3:#1a1a24;--bg4:#22222e;--bg5:#2a2a38;
  --border:#28283a;--border2:#36364a;--border3:#44445a;
  --text:#eeeef5;--text2:#8888a8;--text3:#4a4a68;
  --purple:#7c6af7;--purple2:#a89bff;--purple3:#2d2560;--purple4:#1a1540;
  --teal:#1cb88a;--teal2:#0d7a5c;--teal3:#083d2e;
  --amber:#e8a020;--amber2:#7a5010;
  --red:#e05050;--red2:#7a2020;
  --blue:#4a90e8;--blue2:#1a4080;
  --pink:#e060a0;--green:#50c878;--green2:#1a5a30;
  --radius:12px;--radius2:7px;
  --font:'Segoe UI',system-ui,sans-serif;
}
body{font-family:var(--font);background:var(--bg);color:var(--text);height:100vh;overflow:hidden}
.app{display:grid;grid-template-columns:240px 1fr;height:100vh}
/* SIDEBAR */
.sidebar{background:var(--bg2);border-right:1px solid var(--border);display:flex;flex-direction:column;overflow-y:auto}
.logo{padding:16px;display:flex;align-items:center;gap:10px;border-bottom:1px solid var(--border)}
.logo-icon{width:34px;height:34px;border-radius:9px;background:linear-gradient(135deg,var(--purple3),var(--purple));display:flex;align-items:center;justify-content:center;flex-shrink:0}
.logo-text{font-size:14px;font-weight:700}.logo-text span{color:var(--purple2)}
.vbadge{font-size:9px;background:var(--purple3);color:var(--purple2);padding:2px 6px;border-radius:10px;margin-left:4px}
.nav-sec{font-size:9px;font-weight:700;color:var(--text3);padding:12px 16px 4px;letter-spacing:.12em;text-transform:uppercase}
.nav-item{display:flex;align-items:center;gap:9px;padding:8px 14px;font-size:12.5px;cursor:pointer;color:var(--text2);border-left:2px solid transparent;transition:all .12s;user-select:none}
.nav-item:hover{color:var(--text);background:var(--bg3)}
.nav-item.active{color:var(--purple2);border-left-color:var(--purple);background:rgba(124,106,247,.1);font-weight:500}
.nav-item svg{width:14px;height:14px;flex-shrink:0;opacity:.65}
.nav-item.active svg{opacity:1}
.nbadge{margin-left:auto;background:var(--purple3);color:var(--purple2);font-size:10px;padding:1px 6px;border-radius:10px}
.sidebar-bot{margin-top:auto;padding:12px 16px;border-top:1px solid var(--border)}
.ai-dots{display:flex;gap:5px;margin-bottom:6px}
.dot{width:7px;height:7px;border-radius:50%;background:var(--border2)}
.dot.on{background:var(--teal)}.dot.warn{background:var(--amber)}
/* MAIN */
.main{display:flex;flex-direction:column;overflow:hidden}
.topbar{padding:12px 20px;border-bottom:1px solid var(--border);display:flex;align-items:center;justify-content:space-between;background:var(--bg2);flex-shrink:0}
.topbar h2{font-size:15px;font-weight:600;display:flex;align-items:center;gap:8px}
.tba{display:flex;gap:7px;align-items:center}
.content{flex:1;overflow-y:auto;padding:20px}
.panel{display:none}.panel.active{display:block}
/* BUTTONS */
.btn{padding:7px 13px;border-radius:var(--radius2);border:1px solid var(--border2);background:transparent;font-size:12.5px;cursor:pointer;color:var(--text);font-family:var(--font);transition:all .12s}
.btn:hover{background:var(--bg4)}.btn:active{transform:scale(.98)}
.btn-primary{background:var(--purple);color:#fff;border-color:var(--purple)}
.btn-primary:hover{background:var(--purple2);border-color:var(--purple2)}
.btn-teal{background:var(--teal2);color:#fff;border-color:var(--teal2)}
.btn-red{background:var(--red2);color:#fff;border-color:var(--red2)}
.btn-amber{background:var(--amber2);color:var(--amber);border-color:var(--amber2)}
.btn-sm{padding:5px 10px;font-size:12px}.btn-xs{padding:3px 8px;font-size:11px}
/* GRID */
.g2{display:grid;grid-template-columns:1fr 1fr;gap:14px}
.g3{display:grid;grid-template-columns:1fr 1fr 1fr;gap:12px}
.g4{display:grid;grid-template-columns:repeat(4,1fr);gap:10px}
/* CARDS */
.card{background:var(--bg3);border:1px solid var(--border);border-radius:var(--radius);padding:14px 16px}
.card2{background:var(--bg2);border:1px solid var(--border);border-radius:var(--radius);padding:14px 16px}
.stat{background:var(--bg3);border:1px solid var(--border);border-radius:var(--radius);padding:12px 14px}
.stat-lbl{font-size:10px;color:var(--text3);text-transform:uppercase;letter-spacing:.05em;margin-bottom:4px}
.stat-num{font-size:24px;font-weight:700}
.stat-sub{font-size:10px;color:var(--teal);margin-top:2px}
/* FORM */
.fl{font-size:12px;color:var(--text2);margin-bottom:4px;font-weight:500}
input,select,textarea{width:100%;padding:7px 10px;border:1px solid var(--border2);border-radius:var(--radius2);font-size:12.5px;background:var(--bg4);color:var(--text);font-family:var(--font);margin-bottom:10px;outline:none;transition:border .15s}
input:focus,select:focus,textarea:focus{border-color:var(--purple);box-shadow:0 0 0 2px rgba(124,106,247,.1)}
textarea{resize:vertical;min-height:68px}
select option{background:var(--bg3)}
input[type=color]{height:34px;padding:3px;cursor:pointer}
/* BADGES */
.badge{display:inline-flex;align-items:center;gap:3px;padding:2px 8px;border-radius:20px;font-size:11px;font-weight:600}
.b-purple{background:rgba(124,106,247,.2);color:var(--purple2)}
.b-teal{background:rgba(28,184,138,.15);color:var(--teal)}
.b-amber{background:rgba(232,160,32,.15);color:var(--amber)}
.b-red{background:rgba(224,80,80,.15);color:var(--red)}
.b-blue{background:rgba(74,144,232,.15);color:var(--blue)}
.b-green{background:rgba(80,200,120,.15);color:var(--green)}
/* SECTION TITLE */
.st{font-size:13px;font-weight:600;margin-bottom:10px;margin-top:16px;color:var(--text);display:flex;align-items:center;gap:7px}
.st:first-child{margin-top:0}
.stl{flex:1;height:1px;background:var(--border)}
/* PROGRESS */
.pb{height:5px;background:var(--bg5);border-radius:3px;margin-top:4px;overflow:hidden}
.pf{height:100%;border-radius:3px;background:linear-gradient(90deg,var(--purple),var(--purple2));transition:width .6s}
/* CODE BOX */
.cbox{background:var(--bg);border:1px solid var(--border);border-radius:var(--radius2);padding:12px;font-size:11.5px;font-family:'Courier New',monospace;color:#a8d8a8;line-height:1.7;white-space:pre-wrap;max-height:200px;overflow-y:auto}
/* AI OUTPUT */
.ai-box{background:var(--bg);border:1px solid var(--border2);border-radius:var(--radius);padding:14px;font-size:13px;line-height:1.85;color:var(--text);min-height:90px;max-height:360px;overflow-y:auto;white-space:pre-wrap}
.ai-box.loading{border-color:var(--purple3);animation:bp 1.5s ease infinite}
@keyframes bp{0%,100%{border-color:var(--purple3)}50%{border-color:var(--purple)}}
.dots span{animation:dot 1.2s ease infinite;display:inline-block}
.dots span:nth-child(2){animation-delay:.2s}.dots span:nth-child(3){animation-delay:.4s}
@keyframes dot{0%,80%,100%{opacity:.2}40%{opacity:1}}
/* AI TABS */
.ai-tabs{display:flex;gap:5px;margin-bottom:12px;flex-wrap:wrap}
.ai-tab{padding:5px 12px;border-radius:var(--radius2);border:1px solid var(--border2);font-size:12px;cursor:pointer;color:var(--text2);background:transparent;font-family:var(--font);transition:all .12s;display:flex;align-items:center;gap:5px}
.ai-tab:hover{background:var(--bg4)}
.ai-tab.active{background:var(--purple4);color:var(--purple2);border-color:var(--purple3)}
.sdot{width:6px;height:6px;border-radius:50%;background:var(--border2);flex-shrink:0}
.sdot.on{background:var(--teal)}.sdot.warn{background:var(--amber)}
/* TABS */
.tab-bar{display:flex;border-bottom:1px solid var(--border);margin-bottom:14px;gap:2px;overflow-x:auto}
.tab-btn{padding:7px 14px;font-size:12px;cursor:pointer;color:var(--text2);border-bottom:2px solid transparent;background:transparent;border-top:none;border-left:none;border-right:none;font-family:var(--font);transition:all .15s;white-space:nowrap}
.tab-btn.active{color:var(--purple2);border-bottom-color:var(--purple)}
.tab-content{display:none}.tab-content.active{display:block}
/* CLIENT ROW */
.clirow{display:flex;align-items:center;justify-content:space-between;padding:10px 0;border-bottom:1px solid var(--border)}
.clirow:last-child{border:none}
.avatar{width:32px;height:32px;border-radius:50%;background:var(--purple3);display:flex;align-items:center;justify-content:center;font-size:11px;font-weight:700;color:var(--purple2);flex-shrink:0}
/* TEMPLATE CARDS */
.tpl-card{border:1px solid var(--border);border-radius:var(--radius);overflow:hidden;cursor:pointer;transition:all .2s}
.tpl-card:hover{border-color:var(--purple);transform:translateY(-2px)}
.tpl-card.sel{border-color:var(--purple);box-shadow:0 0 0 2px rgba(124,106,247,.2)}
.tpl-prev{height:80px;display:flex;align-items:center;justify-content:center;font-size:13px;font-weight:600}
.tpl-info{padding:8px 12px;background:var(--bg3);border-top:1px solid var(--border)}
.tpl-name{font-size:12px;font-weight:500}
.tpl-desc{font-size:10px;color:var(--text2);margin-top:2px}
/* CV TEMPLATES */
.cv-tpl{border:1px solid var(--border);border-radius:var(--radius);overflow:hidden;cursor:pointer;transition:all .2s}
.cv-tpl:hover{border-color:var(--purple);transform:translateY(-2px)}
.cv-tpl.sel{border-color:var(--purple);box-shadow:0 0 0 2px rgba(124,106,247,.2)}
.cv-preview{height:160px;overflow:hidden;background:#fff;position:relative}
.cv-tpl-label{padding:8px 12px;background:var(--bg3);border-top:1px solid var(--border);font-size:12px;font-weight:500}
/* URL BOX */
.url-row{display:flex;align-items:center;gap:8px;background:var(--bg3);border:1px solid var(--border);border-radius:var(--radius2);padding:9px 13px;margin-top:7px}
.url-text{font-size:12px;color:var(--purple2);flex:1;font-family:monospace;word-break:break-all}
/* MODAL */
.modal-ov{position:fixed;inset:0;background:rgba(0,0,0,.75);z-index:1000;display:none;align-items:center;justify-content:center}
.modal-ov.open{display:flex}
.modal{background:var(--bg2);border:1px solid var(--border2);border-radius:var(--radius);padding:22px;width:min(600px,92vw);max-height:88vh;overflow-y:auto}
.modal-title{font-size:14px;font-weight:600;margin-bottom:14px;display:flex;justify-content:space-between;align-items:center}
/* NOTIF */
.notif{position:fixed;bottom:18px;right:18px;background:var(--bg3);border:1px solid var(--teal2);border-radius:var(--radius);padding:11px 16px;font-size:13px;color:var(--teal);display:none;z-index:9999;max-width:300px}
.notif.err{border-color:var(--red2);color:var(--red)}.notif.warn{border-color:var(--amber2);color:var(--amber)}
/* DIV */
.div{height:1px;background:var(--border);margin:14px 0}
/* PHOTO UPLOAD */
.photo-zone{border:2px dashed var(--border2);border-radius:var(--radius);padding:20px;text-align:center;cursor:pointer;transition:border-color .2s;margin-bottom:10px}
.photo-zone:hover{border-color:var(--purple)}
.photo-zone img{max-width:80px;max-height:80px;border-radius:50%;margin-bottom:8px;object-fit:cover}
/* CLIENT PORTAL PREVIEW */
.portal-preview{background:var(--bg3);border:1px solid var(--border);border-radius:var(--radius);overflow:hidden}
.portal-header{background:var(--purple);padding:12px 16px;display:flex;align-items:center;gap:10px}
.portal-content{padding:16px}
.portal-field{background:var(--bg4);border:1px solid var(--border2);border-radius:var(--radius2);padding:9px 12px;margin-bottom:8px;font-size:13px}
.portal-field-label{font-size:10px;color:var(--text3);text-transform:uppercase;letter-spacing:.06em;margin-bottom:3px}
/* SCROLLBAR */
::-webkit-scrollbar{width:4px}::-webkit-scrollbar-track{background:transparent}::-webkit-scrollbar-thumb{background:var(--border2);border-radius:2px}
</style>
</head>
<body>
<div class="app">

<!-- ══════════════ SIDEBAR ══════════════ -->
<div class="sidebar">
  <div class="logo">
    <div class="logo-icon"><svg width="18" height="18" viewBox="0 0 18 18" fill="none"><rect x="1" y="1" width="7" height="7" rx="2" fill="#a89bff"/><rect x="10" y="1" width="7" height="7" rx="2" fill="#7c6af7" opacity=".7"/><rect x="1" y="10" width="7" height="7" rx="2" fill="#7c6af7" opacity=".5"/><rect x="10" y="10" width="7" height="7" rx="2" fill="#a89bff" opacity=".4"/></svg></div>
    <div class="logo-text">Portfolio<span>Factory</span><span class="vbadge">v3.0</span></div>
  </div>

  <div class="nav-sec">Créer</div>
  <div class="nav-item active" onclick="nav(this,'create')"><svg viewBox="0 0 16 16" fill="none"><rect x="1" y="1" width="6" height="6" rx="1.5" fill="currentColor"/><rect x="9" y="1" width="6" height="6" rx="1.5" fill="currentColor"/><rect x="1" y="9" width="6" height="6" rx="1.5" fill="currentColor"/><rect x="9" y="9" width="6" height="6" rx="1.5" fill="currentColor"/></svg>Nouveau portfolio</div>
  <div class="nav-item" onclick="nav(this,'ai')"><svg viewBox="0 0 16 16" fill="none"><circle cx="8" cy="8" r="6" stroke="currentColor" stroke-width="1.5"/><circle cx="8" cy="8" r="2" fill="currentColor"/></svg>Générer avec IA<span class="nbadge">4 IA</span></div>
  <div class="nav-item" onclick="nav(this,'editor')"><svg viewBox="0 0 16 16" fill="none"><path d="M11 2L14 5L5 14H2V11L11 2Z" stroke="currentColor" stroke-width="1.5" fill="none"/></svg>Modifier</div>

  <div class="nav-sec">CV & Export</div>
  <div class="nav-item" onclick="nav(this,'cv')"><svg viewBox="0 0 16 16" fill="none"><rect x="2" y="1" width="9" height="14" rx="1.5" stroke="currentColor" stroke-width="1.5" fill="none"/><path d="M5 5h5M5 8h5M5 11h3" stroke="currentColor" stroke-width="1.5"/><path d="M11 1v4h3" stroke="currentColor" stroke-width="1.5"/></svg>CV Canva-style<span class="nbadge">5 modèles</span></div>
  <div class="nav-item" onclick="nav(this,'deploy')"><svg viewBox="0 0 16 16" fill="none"><path d="M8 2v8M5 7l3 3 3-3" stroke="currentColor" stroke-width="1.5"/><path d="M2 12h12" stroke="currentColor" stroke-width="1.5"/></svg>Publier GitHub</div>

  <div class="nav-sec">Business</div>
  <div class="nav-item" onclick="nav(this,'clients')"><svg viewBox="0 0 16 16" fill="none"><circle cx="6" cy="5" r="3" stroke="currentColor" stroke-width="1.5" fill="none"/><path d="M1 14c0-3 2.2-5 5-5s5 2 5 5" stroke="currentColor" stroke-width="1.5" fill="none"/></svg>Mes clients<span class="nbadge" id="nb-cli">4</span></div>
  <div class="nav-item" onclick="nav(this,'portal')"><svg viewBox="0 0 16 16" fill="none"><rect x="1" y="3" width="14" height="10" rx="2" stroke="currentColor" stroke-width="1.5" fill="none"/><path d="M5 3V2M11 3V2M1 7h14" stroke="currentColor" stroke-width="1.5"/></svg>Portail client<span class="nbadge">NOUVEAU</span></div>
  <div class="nav-item" onclick="nav(this,'templates')"><svg viewBox="0 0 16 16" fill="none"><rect x="1" y="1" width="14" height="9" rx="1.5" stroke="currentColor" stroke-width="1.5" fill="none"/><path d="M4 14h8M8 10v4" stroke="currentColor" stroke-width="1.5"/></svg>Templates<span class="nbadge">30+</span></div>

  <div class="nav-sec">Config</div>
  <div class="nav-item" onclick="nav(this,'settings')"><svg viewBox="0 0 16 16" fill="none"><circle cx="8" cy="8" r="2.5" stroke="currentColor" stroke-width="1.5" fill="none"/><path d="M8 1v2M8 13v2M1 8h2M13 8h2M3.2 3.2l1.4 1.4M11.4 11.4l1.4 1.4M11.4 4.6l-1.4 1.4M4.6 11.4l-1.4 1.4" stroke="currentColor" stroke-width="1.5"/></svg>Paramètres IA</div>

  <div class="sidebar-bot">
    <div class="ai-dots">
      <div class="dot" id="dot-claude" title="Claude"></div>
      <div class="dot warn" id="dot-manus" title="Manus"></div>
      <div class="dot" id="dot-openai" title="OpenAI"></div>
      <div class="dot" id="dot-custom" title="Custom"></div>
    </div>
    <div style="font-size:10px;color:var(--text3);line-height:1.5">Portfolio Factory v3.0<br>Multi-IA · CV Canva · Portail client</div>
  </div>
</div>

<!-- ══════════════ MAIN ══════════════ -->
<div class="main">

<!-- ══ CREATE ══ -->
<div class="panel active" id="panel-create">
  <div class="topbar"><h2>Nouveau portfolio</h2><div class="tba"><button class="btn btn-sm" onclick="nav(document.querySelectorAll('.nav-item')[1],'ai')">IA →</button><button class="btn btn-primary btn-sm" onclick="buildPortfolio()">Créer portfolio</button></div></div>
  <div class="content">
    <div class="g2">
      <div>
        <div class="st">Identité <span class="stl"></span></div>
        <div class="fl">Nom complet</div><input id="c-name" placeholder="Sara Benali"/>
        <div class="fl">Titre / Métier</div><input id="c-title" placeholder="Ingénieure Génie Industriel"/>
        <div class="fl">Bio professionnelle</div><textarea id="c-bio" placeholder="Ton parcours, tes passions, tes objectifs..."></textarea>
        <div class="fl">Email</div><input id="c-email" placeholder="sara@email.com"/>
        <div class="fl">Téléphone</div><input id="c-phone" placeholder="+212 6 XX XX XX XX"/>
        <div class="fl">GitHub</div><input id="c-github" placeholder="https://github.com/..."/>
        <div class="fl">LinkedIn</div><input id="c-linkedin" placeholder="https://linkedin.com/in/..."/>
        <div class="fl">Ville / Pays</div><input id="c-city" placeholder="Rabat, Maroc"/>
      </div>
      <div>
        <div class="st">Domaine & Design <span class="stl"></span></div>
        <div class="fl">Domaine professionnel</div>
        <select id="c-sector">
          <optgroup label="🔧 Ingénierie">
            <option>Génie industriel</option><option>Génie civil / BTP</option><option>Génie mécanique</option><option>Génie électrique</option><option>Génie électronique</option><option>Génie des procédés</option><option>Génie chimique</option><option>Génie environnemental</option><option>Génie aéronautique</option><option>Génie naval</option><option>Génie nucléaire</option><option>Génie pétrolier</option><option>Génie minier</option><option>Génie biomédical</option><option>Génie agricole</option>
          </optgroup>
          <optgroup label="💻 Tech & Digital">
            <option>Développeur / Ingénieur logiciel</option><option>Data Science / IA / ML</option><option>Cybersécurité</option><option>Cloud / DevOps</option><option>Blockchain / Web3</option><option>Développeur mobile</option><option>Administrateur systèmes</option><option>Architecte logiciel</option><option>QA / Test</option>
          </optgroup>
          <optgroup label="🎨 Créatif">
            <option>Designer UX/UI</option><option>Designer graphique</option><option>Directeur artistique</option><option>Motion designer</option><option>Photographe</option><option>Vidéaste</option><option>Architecte d'intérieur</option><option>Architecte</option>
          </optgroup>
          <optgroup label="💼 Business">
            <option>Marketing / Communication</option><option>Finance / Comptabilité</option><option>Audit / Contrôle de gestion</option><option>Ressources humaines</option><option>Consultant / Conseil</option><option>Chef de projet</option><option>Supply chain / Logistique</option><option>Commerce / Ventes</option><option>Entrepreneur / Startup</option>
          </optgroup>
          <optgroup label="🏥 Santé">
            <option>Médecin généraliste</option><option>Médecin spécialiste</option><option>Chirurgien</option><option>Pharmacien</option><option>Infirmier / IDE</option><option>Kinésithérapeute</option><option>Dentiste</option><option>Psychologue</option><option>Chercheur en santé</option>
          </optgroup>
          <optgroup label="⚖️ Droit & Juridique">
            <option>Avocat</option><option>Notaire</option><option>Juriste d'entreprise</option><option>Magistrat</option><option>Huissier</option>
          </optgroup>
          <optgroup label="🎓 Éducation">
            <option>Enseignant / Formateur</option><option>Chercheur / Doctorant</option><option>Étudiant / Jeune diplômé</option><option>Coach / Mentor</option>
          </optgroup>
          <optgroup label="🌍 Autres">
            <option>Journaliste / Rédacteur</option><option>Traducteur</option><option>Urbaniste</option><option>Géographe / Géomètre</option><option>Sociologue</option><option>Économiste</option><option>Géologue</option><option>Biologiste</option><option>Physicien</option><option>Mathématicien</option><option>Freelance généraliste</option><option>Autre domaine</option>
          </optgroup>
        </select>
        <div class="fl">Couleur principale</div>
        <select id="c-color"><option value="purple">Violet professionnel</option><option value="blue">Bleu corporate</option><option value="teal">Teal moderne</option><option value="orange">Orange dynamique</option><option value="red">Rouge impactant</option><option value="dark">Sombre élégant</option><option value="white">Blanc minimaliste</option></select>
        <div class="fl">Projets (virgule)</div><input id="c-projects" placeholder="Optimisation ligne prod., Audit qualité ISO, GMAO..."/>
        <div class="fl">Compétences (virgule)</div><input id="c-skills" placeholder="Lean Manufacturing, AutoCAD, Six Sigma, ERP..."/>
        <div class="fl">Expériences (virgule)</div><input id="c-exp" placeholder="Stage OCP 2023, Ingénieur ONA 2024..."/>
        <div class="fl">Certifications</div><input id="c-certs" placeholder="PMP, Lean Six Sigma, ISO 9001..."/>
        <div class="fl">Mode</div>
        <select id="c-mode"><option>Personnel</option><option>Client (portail activé)</option></select>
      </div>
    </div>
    <div id="create-output" style="display:none;margin-top:14px">
      <div class="div"></div>
      <div style="display:flex;gap:7px;flex-wrap:wrap;margin-bottom:10px">
        <button class="btn btn-teal btn-sm" onclick="downloadHTML()">Télécharger index.html</button>
        <button class="btn btn-sm" onclick="downloadJSON()">Télécharger data.json</button>
        <button class="btn btn-sm btn-primary" onclick="nav(document.querySelectorAll('.nav-item')[4],'cv')">Créer CV PDF →</button>
        <button class="btn btn-sm" onclick="nav(document.querySelectorAll('.nav-item')[6],'portal')">Générer portail client →</button>
      </div>
      <div class="cbox" id="create-code"></div>
    </div>
  </div>
</div>

<!-- ══ AI ══ -->
<div class="panel" id="panel-ai">
  <div class="topbar"><h2>Générer avec IA</h2><div class="tba"><button class="btn btn-sm" onclick="testConn()">Tester connexion</button></div></div>
  <div class="content">
    <div class="ai-tabs">
      <button class="ai-tab active" onclick="selAI(this,'claude')"><span class="sdot" id="sdt-claude"></span>Claude</button>
      <button class="ai-tab" onclick="selAI(this,'manus')"><span class="sdot warn" id="sdt-manus"></span>Manus AI</button>
      <button class="ai-tab" onclick="selAI(this,'openai')"><span class="sdot" id="sdt-openai"></span>OpenAI/GPT</button>
      <button class="ai-tab" onclick="selAI(this,'custom')"><span class="sdot" id="sdt-custom"></span>Custom IA</button>
    </div>
    <div class="g2" style="margin-bottom:12px">
      <div>
        <div class="fl">Type</div>
        <select id="ai-type">
          <option>Portfolio complet HTML + data.json</option><option>Bio professionnelle SEO</option><option>Description projet</option><option>Compétences par domaine</option><option>Texte À propos</option><option>Lettre de motivation</option><option>Résumé CV</option><option>Stratégie personal branding</option><option>Optimisation SEO</option>
        </select>
        <div class="fl">Domaine</div>
        <select id="ai-sector"><option>Génie industriel</option><option>Génie civil</option><option>Data Science</option><option>Développeur fullstack</option><option>Designer UX</option><option>Marketing</option><option>Médecin</option><option>Finance</option></select>
        <div class="fl">Langue</div>
        <select id="ai-lang"><option>Français</option><option>Anglais</option><option>Arabe</option></select>
      </div>
      <div>
        <div class="fl">Contexte & infos</div>
        <textarea id="ai-prompt" placeholder="Nom, expérience, projets, compétences, style souhaité..." style="min-height:110px"></textarea>
        <div class="fl">Tonalité</div>
        <select id="ai-tone"><option>Professionnel & formel</option><option>Moderne & dynamique</option><option>Créatif</option><option>Académique</option></select>
      </div>
    </div>
    <div style="display:flex;gap:7px;flex-wrap:wrap;margin-bottom:12px">
      <button class="btn btn-primary" onclick="callAI()">Générer →</button>
      <button class="btn btn-sm" onclick="clearOut()">Effacer</button>
    </div>
    <div class="fl" id="ai-model-lbl">Modèle : claude-sonnet-4-20250514</div>
    <div class="ai-box" id="ai-output">Le résultat apparaîtra ici...</div>
    <div style="display:flex;gap:7px;margin-top:9px;flex-wrap:wrap">
      <button class="btn btn-sm" onclick="copyOut()">Copier</button>
      <button class="btn btn-sm btn-teal" onclick="injectEditor()">→ Éditeur</button>
      <button class="btn btn-sm" onclick="injectCreate()">→ Créer</button>
    </div>
  </div>
</div>

<!-- ══ EDITOR ══ -->
<div class="panel" id="panel-editor">
  <div class="topbar"><h2>Modifier portfolio</h2><div class="tba"><button class="btn btn-primary btn-sm" onclick="saveEd()">Sauvegarder</button></div></div>
  <div class="content">
    <div class="tab-bar">
      <button class="tab-btn active" onclick="stab(this,'te-id')">Identité</button>
      <button class="tab-btn" onclick="stab(this,'te-sk')">Compétences</button>
      <button class="tab-btn" onclick="stab(this,'te-pr')">Projets</button>
      <button class="tab-btn" onclick="stab(this,'te-ex')">Expériences</button>
      <button class="tab-btn" onclick="stab(this,'te-de')">Design</button>
    </div>
    <div class="tab-content active" id="te-id">
      <div class="g2">
        <div><div class="fl">Nom</div><input id="e-name" value="Sara Benali"/><div class="fl">Titre</div><input id="e-title" value="Ingénieure Génie Industriel"/><div class="fl">Email</div><input id="e-email" value="sara@email.com"/><div class="fl">Ville</div><input id="e-city" value="Rabat, Maroc"/><div class="fl">Domaine</div>
        <select id="e-sector"><option>Génie industriel</option><option>Génie civil</option><option>Développeur</option><option>Data Science</option><option>Designer</option><option>Marketing</option><option>Médecin</option><option>Finance</option><option>Enseignant</option><option>Génie mécanique</option><option>Génie électrique</option><option>Supply chain</option></select></div>
        <div><div class="fl">Bio</div><textarea id="e-bio" style="min-height:120px">Ingénieure passionnée par l'optimisation industrielle et la qualité. Expérimentée en lean manufacturing, gestion de production et amélioration continue.</textarea><div class="fl">GitHub / Portfolio</div><input id="e-gh" value="https://github.com/sara-benali"/></div>
      </div>
    </div>
    <div class="tab-content" id="te-sk">
      <div id="skills-ed"></div>
      <div class="div"></div>
      <div style="display:flex;gap:7px;align-items:flex-end">
        <div style="flex:1"><div class="fl">Compétence</div><input id="nsk-n" placeholder="Lean Manufacturing" style="margin:0"/></div>
        <div style="width:70px"><div class="fl">%</div><input id="nsk-l" type="number" min="1" max="100" value="80" style="margin:0"/></div>
        <button class="btn btn-primary btn-sm" style="margin-bottom:0" onclick="addSkill()">+</button>
      </div>
    </div>
    <div class="tab-content" id="te-pr">
      <div id="proj-ed"></div>
      <div class="div"></div>
      <div class="card"><div class="st" style="margin-top:0">Nouveau projet</div>
        <div class="g2"><div><div class="fl">Titre</div><input id="np-t" placeholder="Optimisation ligne de prod."/><div class="fl">Technologies / Méthodes</div><input id="np-tech" placeholder="Lean, Six Sigma, AutoCAD"/></div><div><div class="fl">Description</div><textarea id="np-d" placeholder="Ce projet a permis de..."></textarea><button class="btn btn-primary btn-sm" onclick="addProj()">+ Ajouter</button></div></div>
      </div>
    </div>
    <div class="tab-content" id="te-ex">
      <div id="exp-ed"></div>
      <div class="div"></div>
      <div class="card"><div class="st" style="margin-top:0">Nouvelle expérience</div>
        <div class="g2"><div><div class="fl">Poste</div><input id="ne-r" placeholder="Ingénieur méthodes"/><div class="fl">Entreprise</div><input id="ne-c" placeholder="OCP Group"/></div><div><div class="fl">Période</div><input id="ne-p" placeholder="2023 – présent"/><div class="fl">Description</div><textarea id="ne-d" placeholder="Missions et réalisations..."></textarea><button class="btn btn-primary btn-sm" onclick="addExp()">+ Ajouter</button></div></div>
      </div>
    </div>
    <div class="tab-content" id="te-de">
      <div class="g2">
        <div><div class="fl">Couleur accent</div><input type="color" id="d-color" value="#7c6af7"/><div class="fl">Police</div><select><option>Segoe UI</option><option>Inter</option><option>Roboto</option><option>Poppins</option></select><div class="fl">Animation</div><select><option>Légère</option><option>Aucune</option><option>Dynamique</option></select></div>
        <div><div class="fl">Mode</div><select id="d-mode"><option>Sombre</option><option>Clair</option></select><div class="fl">Sections actives</div>
          <div style="display:grid;gap:6px;font-size:12.5px">
            <label style="display:flex;align-items:center;gap:7px;cursor:pointer"><input type="checkbox" checked style="width:auto;margin:0"> À propos & compétences</label>
            <label style="display:flex;align-items:center;gap:7px;cursor:pointer"><input type="checkbox" checked style="width:auto;margin:0"> Projets</label>
            <label style="display:flex;align-items:center;gap:7px;cursor:pointer"><input type="checkbox" checked style="width:auto;margin:0"> Expériences</label>
            <label style="display:flex;align-items:center;gap:7px;cursor:pointer"><input type="checkbox" style="width:auto;margin:0"> Certifications</label>
            <label style="display:flex;align-items:center;gap:7px;cursor:pointer"><input type="checkbox" style="width:auto;margin:0"> Témoignages</label>
          </div>
        </div>
      </div>
    </div>
  </div>
</div>

<!-- ══ CV CANVA-STYLE ══ -->
<div class="panel" id="panel-cv">
  <div class="topbar"><h2>CV Canva-style — 5 modèles</h2><div class="tba"><button class="btn btn-primary btn-sm" onclick="exportCV()">Télécharger PDF</button></div></div>
  <div class="content">
    <div class="tab-bar">
      <button class="tab-btn active" onclick="stab(this,'cv-choose')">Choisir modèle</button>
      <button class="tab-btn" onclick="stab(this,'cv-data')">Remplir infos</button>
      <button class="tab-btn" onclick="stab(this,'cv-preview')">Aperçu</button>
    </div>

    <!-- CHOOSE -->
    <div class="tab-content active" id="cv-choose">
      <div style="display:grid;grid-template-columns:repeat(5,1fr);gap:12px">
        <!-- TEMPLATE 1: Modern Purple -->
        <div class="cv-tpl sel" id="cvt-1" onclick="selCV(1)">
          <div class="cv-preview" style="padding:8px">
            <div style="background:#5b4af7;height:35px;display:flex;align-items:center;padding:0 8px;margin-bottom:6px"><div style="width:18px;height:18px;border-radius:50%;background:#fff;margin-right:6px"></div><div><div style="background:#fff;height:5px;width:40px;border-radius:2px;margin-bottom:2px"></div><div style="background:rgba(255,255,255,.5);height:3px;width:28px;border-radius:2px"></div></div></div>
            <div style="padding:0 6px"><div style="background:#5b4af7;height:7px;width:30px;border-radius:2px;margin-bottom:4px"></div><div style="background:#f0f0f0;height:3px;width:95%;margin-bottom:2px;border-radius:1px"></div><div style="background:#f0f0f0;height:3px;width:80%;margin-bottom:6px;border-radius:1px"></div><div style="background:#5b4af7;height:5px;width:25px;border-radius:2px;margin-bottom:3px"></div><div style="display:flex;gap:3px"><div style="flex:1;background:#f0f0f0;height:3px;border-radius:1px"></div><div style="background:#5b4af7;height:3px;width:60%;border-radius:1px"></div></div></div>
          </div>
          <div class="cv-tpl-label">Modern Purple</div>
        </div>
        <!-- TEMPLATE 2: Classic Corporate -->
        <div class="cv-tpl" id="cvt-2" onclick="selCV(2)">
          <div class="cv-preview" style="padding:8px;background:#f8f8f8">
            <div style="border-bottom:3px solid #1a4080;padding-bottom:6px;margin-bottom:6px"><div style="font-size:9px;font-weight:700;color:#1a4080">NOM PRÉNOM</div><div style="font-size:7px;color:#666">Titre professionnel</div></div>
            <div style="display:grid;grid-template-columns:1fr 2fr;gap:6px">
              <div style="background:#1a4080;padding:4px;border-radius:2px"><div style="background:rgba(255,255,255,.3);height:3px;margin-bottom:2px;border-radius:1px"></div><div style="background:rgba(255,255,255,.3);height:3px;margin-bottom:2px;border-radius:1px"></div><div style="background:rgba(255,255,255,.3);height:3px;border-radius:1px"></div></div>
              <div><div style="background:#e0e0e0;height:3px;margin-bottom:2px;border-radius:1px"></div><div style="background:#e0e0e0;height:3px;margin-bottom:2px;border-radius:1px"></div><div style="background:#e0e0e0;height:3px;border-radius:1px"></div></div>
            </div>
          </div>
          <div class="cv-tpl-label">Classic Corporate</div>
        </div>
        <!-- TEMPLATE 3: Minimal Clean -->
        <div class="cv-tpl" id="cvt-3" onclick="selCV(3)">
          <div class="cv-preview" style="padding:10px;background:#fff">
            <div style="margin-bottom:8px"><div style="font-size:10px;font-weight:700;color:#222;margin-bottom:1px">Nom Prénom</div><div style="font-size:7px;color:#888">Titre · email · ville</div><div style="height:1px;background:#222;margin-top:4px"></div></div>
            <div style="font-size:7px;color:#888;margin-bottom:2px;text-transform:uppercase;letter-spacing:.05em">Expérience</div>
            <div style="background:#f5f5f5;height:3px;margin-bottom:2px;border-radius:1px"></div>
            <div style="background:#f5f5f5;height:3px;margin-bottom:6px;border-radius:1px"></div>
            <div style="font-size:7px;color:#888;margin-bottom:2px;text-transform:uppercase;letter-spacing:.05em">Compétences</div>
            <div style="display:flex;gap:2px"><div style="background:#222;height:3px;width:70%;border-radius:1px"></div></div>
          </div>
          <div class="cv-tpl-label">Minimal Clean</div>
        </div>
        <!-- TEMPLATE 4: Creative Bold -->
        <div class="cv-tpl" id="cvt-4" onclick="selCV(4)">
          <div class="cv-preview" style="padding:0;background:#1a1a1a">
            <div style="background:linear-gradient(135deg,#e8a020,#e05050);height:40px;display:flex;align-items:center;padding:0 8px"><div style="font-size:10px;font-weight:700;color:#fff">NOM</div></div>
            <div style="padding:8px"><div style="background:#333;height:3px;margin-bottom:3px;border-radius:1px"></div><div style="background:#333;height:3px;margin-bottom:6px;border-radius:1px"></div><div style="display:flex;gap:2px"><div style="background:#e8a020;height:3px;width:75%;border-radius:1px"></div></div></div>
          </div>
          <div class="cv-tpl-label">Creative Bold</div>
        </div>
        <!-- TEMPLATE 5: Academic -->
        <div class="cv-tpl" id="cvt-5" onclick="selCV(5)">
          <div class="cv-preview" style="padding:8px;background:#fff">
            <div style="text-align:center;margin-bottom:6px"><div style="font-size:9px;font-weight:700;color:#333">NOM PRÉNOM</div><div style="font-size:6px;color:#666">Doctorant · Chercheur</div><div style="display:flex;justify-content:center;gap:4px;margin-top:3px"><div style="width:20px;height:1px;background:#333"></div><div style="width:4px;height:4px;background:#333;border-radius:50%;margin-top:-1.5px"></div><div style="width:20px;height:1px;background:#333"></div></div></div>
            <div style="font-size:7px;color:#333;font-weight:600;text-align:center;margin-bottom:3px;border-bottom:1px solid #333">PUBLICATIONS</div>
            <div style="background:#f0f0f0;height:2px;margin-bottom:2px;border-radius:1px"></div>
            <div style="background:#f0f0f0;height:2px;margin-bottom:2px;border-radius:1px"></div>
          </div>
          <div class="cv-tpl-label">Academic</div>
        </div>
      </div>
      <p style="font-size:12px;color:var(--text2);margin-top:12px">Sélectionne un modèle, puis va dans "Remplir infos" pour personnaliser.</p>
    </div>

    <!-- DATA -->
    <div class="tab-content" id="cv-data">
      <div class="g2">
        <div>
          <div class="st" style="margin-top:0">Informations personnelles</div>
          <div class="photo-zone" onclick="triggerPhoto()" id="photo-zone">
            <img id="cv-photo-img" src="" style="display:none;width:70px;height:70px;border-radius:50%;object-fit:cover;margin:0 auto 8px"/>
            <div id="photo-placeholder">
              <div style="font-size:24px;margin-bottom:6px">📷</div>
              <div style="font-size:12px;color:var(--text2)">Cliquez pour ajouter une photo</div>
              <div style="font-size:11px;color:var(--text3);margin-top:2px">JPG, PNG — 2MB max</div>
            </div>
            <input type="file" id="cv-photo-input" accept="image/*" style="display:none" onchange="loadPhoto(this)"/>
          </div>
          <div class="fl">Nom complet</div><input id="cv-name" placeholder="Sara Benali"/>
          <div class="fl">Titre</div><input id="cv-title" placeholder="Ingénieure Génie Industriel"/>
          <div class="fl">Email</div><input id="cv-email" placeholder="sara@email.com"/>
          <div class="fl">Téléphone</div><input id="cv-phone" placeholder="+212 6 XX XX XX"/>
          <div class="fl">Ville</div><input id="cv-city" placeholder="Rabat, Maroc"/>
          <div class="fl">LinkedIn</div><input id="cv-linkedin" placeholder="linkedin.com/in/..."/>
        </div>
        <div>
          <div class="st" style="margin-top:0">Contenu CV</div>
          <div class="fl">Résumé / Profil</div><textarea id="cv-summary" placeholder="Ingénieure avec 3 ans d'expérience en optimisation industrielle..." style="min-height:80px"></textarea>
          <div class="fl">Compétences (virgule)</div><input id="cv-skills" placeholder="Lean Manufacturing, Six Sigma, AutoCAD, ERP SAP"/>
          <div class="fl">Langues</div><input id="cv-langs" placeholder="Arabe (natif), Français (C1), Anglais (B2)"/>
          <div class="fl">Expériences</div><textarea id="cv-exp" placeholder="Ingénieure méthodes OCP — 2023-présent : Optimisation lignes&#10;Stage Renault — 2022 : Analyse AMDEC"></textarea>
          <div class="fl">Formation</div><textarea id="cv-edu" placeholder="Ingénieur — EMI Rabat — 2020-2023&#10;Bac Science Math — Lycée Hassan II — 2020"></textarea>
          <div class="fl">Certifications / Projets</div><input id="cv-certs2" placeholder="PMP, Lean Six Sigma Green Belt, ISO 9001"/>
          <div class="fl">Couleur accent PDF</div><input type="color" id="cv-accent" value="#7c6af7"/>
        </div>
      </div>
    </div>

    <!-- PREVIEW -->
    <div class="tab-content" id="cv-preview">
      <div style="display:flex;gap:10px;margin-bottom:12px">
        <button class="btn btn-primary" onclick="exportCV()">Télécharger PDF</button>
        <button class="btn btn-sm" onclick="previewCV()">Actualiser aperçu</button>
      </div>
      <div id="cv-preview-box" style="background:#fff;border-radius:var(--radius);overflow:hidden;min-height:300px;padding:0"></div>
    </div>
  </div>
</div>

<!-- ══ DEPLOY ══ -->
<div class="panel" id="panel-deploy">
  <div class="topbar"><h2>Publier sur GitHub Pages</h2></div>
  <div class="content">
    <div class="g2">
      <div>
        <div class="st" style="margin-top:0">Configuration</div>
        <div class="fl">Portfolio</div><select id="d-portfolio"><option>Sara Benali — Personal</option><option>Yassine Berrada — Client</option></select>
        <div class="fl">Nom du repo</div><input id="d-repo" value="portfolio-sara-benali"/>
        <div class="fl">Username GitHub</div><input id="d-user" placeholder="mon-username"/>
        <div class="fl">Token GitHub</div><input id="d-token" type="password" placeholder="ghp_xxxxxxxxxxxxxxxxxxxx"/>
        <a href="https://github.com/settings/tokens/new" target="_blank" style="font-size:11px;color:var(--purple2)">→ Créer un token GitHub</a>
        <div style="margin-top:12px"><button class="btn btn-primary" onclick="simDeploy()">Générer script & URLs</button></div>
      </div>
      <div>
        <div class="st" style="margin-top:0">Script PowerShell</div>
        <div class="cbox" id="deploy-script">Remplis les champs puis clique "Générer".</div>
        <button class="btn btn-sm" style="margin-top:7px" onclick="copyScript()">Copier script</button>
        <div id="deploy-result" style="display:none;margin-top:12px">
          <div class="div"></div>
          <div style="font-size:13px;font-weight:600;color:var(--teal);margin-bottom:7px">URLs générées ✓</div>
          <div class="url-row"><span class="url-text" id="pub-url"></span><button class="btn btn-xs" onclick="copyEl('pub-url')">Copier</button></div>
          <div class="url-row"><span class="url-text" id="repo-url"></span><button class="btn btn-xs" onclick="copyEl('repo-url')">Copier</button></div>
        </div>
      </div>
    </div>
  </div>
</div>

<!-- ══ CLIENTS ══ -->
<div class="panel" id="panel-clients">
  <div class="topbar"><h2>Mes clients</h2><div class="tba"><button class="btn btn-primary btn-sm" onclick="openModal('modal-client')">+ Nouveau client</button></div></div>
  <div class="content">
    <div class="g4" style="margin-bottom:16px">
      <div class="stat"><div class="stat-lbl">Total</div><div class="stat-num" id="ct-tot">4</div></div>
      <div class="stat"><div class="stat-lbl">Publiés</div><div class="stat-num" style="color:var(--teal)" id="ct-pub">2</div></div>
      <div class="stat"><div class="stat-lbl">En cours</div><div class="stat-num" style="color:var(--amber)" id="ct-prog">2</div></div>
      <div class="stat"><div class="stat-lbl">Revenus</div><div class="stat-num" style="color:var(--purple2)" id="ct-rev">3 500</div><div class="stat-sub">MAD</div></div>
    </div>
    <div class="card2" id="cli-list">
      <div class="clirow"><div style="display:flex;align-items:center;gap:9px"><div class="avatar">YB</div><div><div style="font-size:13px;font-weight:500">Yassine Berrada</div><div style="font-size:11px;color:var(--text2)">Génie civil · template-engineer</div></div></div><div style="display:flex;align-items:center;gap:6px"><span class="badge b-teal">Publié</span><button class="btn btn-xs" onclick="nav(document.querySelectorAll('.nav-item')[7],'portal')">Portail</button><button class="btn btn-xs" onclick="nav(document.querySelectorAll('.nav-item')[4],'cv')">CV</button></div></div>
      <div class="clirow"><div style="display:flex;align-items:center;gap:9px"><div class="avatar" style="background:var(--teal3);color:var(--teal)">LM</div><div><div style="font-size:13px;font-weight:500">Leila Mansouri</div><div style="font-size:11px;color:var(--text2)">Designer UX · template-designer</div></div></div><div style="display:flex;align-items:center;gap:6px"><span class="badge b-teal">Publié</span><button class="btn btn-xs" onclick="nav(document.querySelectorAll('.nav-item')[7],'portal')">Portail</button><button class="btn btn-xs" onclick="nav(document.querySelectorAll('.nav-item')[4],'cv')">CV</button></div></div>
      <div class="clirow"><div style="display:flex;align-items:center;gap:9px"><div class="avatar" style="background:var(--amber2);color:var(--amber)">AK</div><div><div style="font-size:13px;font-weight:500">Amine Khalil</div><div style="font-size:11px;color:var(--text2)">Développeur fullstack · template-dev</div></div></div><div style="display:flex;align-items:center;gap:6px"><span class="badge b-amber">En cours</span><button class="btn btn-xs" onclick="nav(document.querySelectorAll('.nav-item')[7],'portal')">Portail</button><button class="btn btn-xs" onclick="nav(document.querySelectorAll('.nav-item')[4],'cv')">CV</button></div></div>
    </div>
  </div>
</div>

<!-- ══ PORTAIL CLIENT ══ -->
<div class="panel" id="panel-portal">
  <div class="topbar"><h2>Portail client — Le client modifie son portfolio lui-même</h2></div>
  <div class="content">
    <div class="g2">
      <div>
        <div class="st" style="margin-top:0">Configurer le portail</div>
        <div class="fl">Nom du client</div><input id="pt-name" value="Yassine Berrada"/>
        <div class="fl">Email du client</div><input id="pt-email" placeholder="client@email.com"/>
        <div class="fl">Mot de passe portail</div><input id="pt-pass" type="password" placeholder="Client créera son mot de passe"/>
        <div class="fl">Plan / Accès</div>
        <select id="pt-plan">
          <option value="basic">Basique — modifier textes uniquement</option>
          <option value="standard">Standard — textes + compétences + projets</option>
          <option value="premium">Premium — accès complet + CV PDF + IA</option>
        </select>
        <div class="fl">URL du portail hébergé chez toi</div>
        <div class="url-row" style="margin-top:0;margin-bottom:10px"><span class="url-text" id="portal-url">https://ton-hebergeur.com/client/yassine-berrada</span></div>
        <button class="btn btn-primary" onclick="genPortal()">Générer portail client</button>
        <button class="btn btn-sm" style="margin-left:8px" onclick="dlPortal()">Télécharger le fichier portail</button>
        <div style="margin-top:12px;font-size:12px;color:var(--text2);line-height:1.7">
          <strong style="color:var(--text)">Comment ça marche :</strong><br>
          1. Tu génères le fichier portail<br>
          2. Tu l'héberges sur ton serveur (GitHub Pages, Netlify, cPanel...)<br>
          3. Tu envoies le lien au client<br>
          4. Le client accède à son portail avec son nom<br>
          5. Il modifie bio, projets, compétences, couleur<br>
          6. Il télécharge son index.html mis à jour<br>
          7. Il re-publie sur GitHub (ou tu le fais pour lui)
        </div>
      </div>
      <div>
        <div class="st" style="margin-top:0">Aperçu du portail client</div>
        <div class="portal-preview">
          <div class="portal-header">
            <div style="width:32px;height:32px;border-radius:50%;background:rgba(255,255,255,.2);display:flex;align-items:center;justify-content:center;font-size:11px;font-weight:700;flex-shrink:0" id="pt-avatar">YB</div>
            <div style="margin-left:10px">
              <div style="font-size:13px;font-weight:600;color:#fff" id="pt-name-display">Yassine Berrada</div>
              <div style="font-size:11px;color:rgba(255,255,255,.7)">Mon espace portfolio</div>
            </div>
            <span class="badge" style="margin-left:auto;background:rgba(255,255,255,.2);color:#fff;font-size:10px" id="pt-plan-display">Standard</span>
          </div>
          <div class="portal-content">
            <div class="portal-field">
              <div class="portal-field-label">Nom affiché</div>
              <div>Yassine Berrada</div>
            </div>
            <div class="portal-field">
              <div class="portal-field-label">Titre professionnel</div>
              <div>Ingénieur Génie Civil</div>
            </div>
            <div class="portal-field">
              <div class="portal-field-label">Bio</div>
              <div style="font-size:12px;color:var(--text2)">Ingénieur passionné par les structures et la gestion de projets BTP...</div>
            </div>
            <div style="display:flex;gap:7px;margin-top:10px">
              <button class="btn btn-primary btn-sm">Modifier mon portfolio</button>
              <button class="btn btn-sm">Télécharger CV</button>
            </div>
          </div>
        </div>
        <div class="div"></div>
        <div class="st" style="margin-top:0">Fichier portail généré</div>
        <div class="cbox" id="portal-code">Clique sur "Générer portail client" pour voir le code...</div>
        <button class="btn btn-sm" style="margin-top:7px" onclick="copyPortalCode()">Copier code</button>
      </div>
    </div>
  </div>
</div>

<!-- ══ TEMPLATES ══ -->
<div class="panel" id="panel-templates">
  <div class="topbar"><h2>Templates — 30+ domaines</h2><div class="tba"><button class="btn btn-primary btn-sm" onclick="nav(document.querySelectorAll('.nav-item')[1],'ai')">Générer avec IA →</button></div></div>
  <div class="content">
    <div class="tab-bar">
      <button class="tab-btn active" onclick="stab(this,'tpl-eng')">Ingénierie</button>
      <button class="tab-btn" onclick="stab(this,'tpl-tech')">Tech</button>
      <button class="tab-btn" onclick="stab(this,'tpl-biz')">Business</button>
      <button class="tab-btn" onclick="stab(this,'tpl-sante')">Santé</button>
      <button class="tab-btn" onclick="stab(this,'tpl-autres')">Autres</button>
    </div>
    <div class="tab-content active" id="tpl-eng">
      <div style="display:grid;grid-template-columns:repeat(4,1fr);gap:10px">
        <div class="tpl-card sel" onclick="selTpl(this)"><div class="tpl-prev" style="background:rgba(232,160,32,.12)"><span style="color:var(--amber)">Génie Industriel</span></div><div class="tpl-info"><div class="tpl-name">template-industrial</div><div class="tpl-desc">Lean · Six Sigma · ERP · Qualité</div></div></div>
        <div class="tpl-card" onclick="selTpl(this)"><div class="tpl-prev" style="background:rgba(28,184,138,.12)"><span style="color:var(--teal)">Génie Civil</span></div><div class="tpl-info"><div class="tpl-name">template-civil</div><div class="tpl-desc">BTP · Structures · AutoCAD</div></div></div>
        <div class="tpl-card" onclick="selTpl(this)"><div class="tpl-prev" style="background:rgba(74,144,232,.12)"><span style="color:var(--blue)">Génie Méca</span></div><div class="tpl-info"><div class="tpl-name">template-meca</div><div class="tpl-desc">CAO · SOLIDWORKS · CATIA</div></div></div>
        <div class="tpl-card" onclick="selTpl(this)"><div class="tpl-prev" style="background:rgba(224,80,80,.12)"><span style="color:var(--red)">Génie Élec</span></div><div class="tpl-info"><div class="tpl-name">template-elec</div><div class="tpl-desc">Automatisme · SCADA · PLC</div></div></div>
        <div class="tpl-card" onclick="selTpl(this)"><div class="tpl-prev" style="background:rgba(124,106,247,.12)"><span style="color:var(--purple2)">Génie Chim</span></div><div class="tpl-info"><div class="tpl-name">template-chem</div><div class="tpl-desc">Procédés · HSE · HAZOP</div></div></div>
        <div class="tpl-card" onclick="selTpl(this)"><div class="tpl-prev" style="background:rgba(80,200,120,.12)"><span style="color:var(--green)">Génie Env</span></div><div class="tpl-info"><div class="tpl-name">template-env</div><div class="tpl-desc">Environnement · Énergie</div></div></div>
        <div class="tpl-card" onclick="selTpl(this)"><div class="tpl-prev" style="background:rgba(232,96,160,.12)"><span style="color:var(--pink)">Génie Biomédical</span></div><div class="tpl-info"><div class="tpl-name">template-biomed</div><div class="tpl-desc">Dispositifs médicaux</div></div></div>
        <div class="tpl-card" onclick="selTpl(this)"><div class="tpl-prev" style="background:rgba(100,100,200,.12)"><span style="color:#a0a0ff">Génie Pétr</span></div><div class="tpl-info"><div class="tpl-name">template-petro</div><div class="tpl-desc">Oil & Gas · Offshore</div></div></div>
      </div>
    </div>
    <div class="tab-content" id="tpl-tech">
      <div style="display:grid;grid-template-columns:repeat(4,1fr);gap:10px">
        <div class="tpl-card" onclick="selTpl(this)"><div class="tpl-prev" style="background:rgba(124,106,247,.12)"><span style="color:var(--purple2)">Développeur</span></div><div class="tpl-info"><div class="tpl-name">template-dev</div><div class="tpl-desc">React · Node · APIs</div></div></div>
        <div class="tpl-card" onclick="selTpl(this)"><div class="tpl-prev" style="background:rgba(28,184,138,.12)"><span style="color:var(--teal)">Data Science</span></div><div class="tpl-info"><div class="tpl-name">template-data</div><div class="tpl-desc">Python · ML · BI</div></div></div>
        <div class="tpl-card" onclick="selTpl(this)"><div class="tpl-prev" style="background:rgba(224,80,80,.12)"><span style="color:var(--red)">Cybersécurité</span></div><div class="tpl-info"><div class="tpl-name">template-cyber</div><div class="tpl-desc">Pentest · SOC · SIEM</div></div></div>
        <div class="tpl-card" onclick="selTpl(this)"><div class="tpl-prev" style="background:rgba(74,144,232,.12)"><span style="color:var(--blue)">Cloud/DevOps</span></div><div class="tpl-info"><div class="tpl-name">template-devops</div><div class="tpl-desc">AWS · Docker · CI/CD</div></div></div>
        <div class="tpl-card" onclick="selTpl(this)"><div class="tpl-prev" style="background:rgba(232,160,32,.12)"><span style="color:var(--amber)">Blockchain</span></div><div class="tpl-info"><div class="tpl-name">template-web3</div><div class="tpl-desc">Web3 · Solidity · DeFi</div></div></div>
        <div class="tpl-card" onclick="selTpl(this)"><div class="tpl-prev" style="background:rgba(80,200,120,.12)"><span style="color:var(--green)">Mobile</span></div><div class="tpl-info"><div class="tpl-name">template-mobile</div><div class="tpl-desc">Flutter · React Native</div></div></div>
      </div>
    </div>
    <div class="tab-content" id="tpl-biz">
      <div style="display:grid;grid-template-columns:repeat(4,1fr);gap:10px">
        <div class="tpl-card" onclick="selTpl(this)"><div class="tpl-prev" style="background:rgba(74,144,232,.12)"><span style="color:var(--blue)">Marketing</span></div><div class="tpl-info"><div class="tpl-name">template-marketing</div><div class="tpl-desc">Digital · SEO · Branding</div></div></div>
        <div class="tpl-card" onclick="selTpl(this)"><div class="tpl-prev" style="background:rgba(28,184,138,.12)"><span style="color:var(--teal)">Finance</span></div><div class="tpl-info"><div class="tpl-name">template-finance</div><div class="tpl-desc">Audit · Contrôle · Banque</div></div></div>
        <div class="tpl-card" onclick="selTpl(this)"><div class="tpl-prev" style="background:rgba(232,160,32,.12)"><span style="color:var(--amber)">Consultant</span></div><div class="tpl-info"><div class="tpl-name">template-consult</div><div class="tpl-desc">Stratégie · Management</div></div></div>
        <div class="tpl-card" onclick="selTpl(this)"><div class="tpl-prev" style="background:rgba(124,106,247,.12)"><span style="color:var(--purple2)">RH</span></div><div class="tpl-info"><div class="tpl-name">template-hr</div><div class="tpl-desc">Talent · Formation · SIRH</div></div></div>
        <div class="tpl-card" onclick="selTpl(this)"><div class="tpl-prev" style="background:rgba(80,200,120,.12)"><span style="color:var(--green)">Supply Chain</span></div><div class="tpl-info"><div class="tpl-name">template-supply</div><div class="tpl-desc">Logistique · Achats · ERP</div></div></div>
        <div class="tpl-card" onclick="selTpl(this)"><div class="tpl-prev" style="background:rgba(232,96,160,.12)"><span style="color:var(--pink)">Freelance</span></div><div class="tpl-info"><div class="tpl-name">template-freelance</div><div class="tpl-desc">Généraliste · Prestataire</div></div></div>
      </div>
    </div>
    <div class="tab-content" id="tpl-sante">
      <div style="display:grid;grid-template-columns:repeat(4,1fr);gap:10px">
        <div class="tpl-card" onclick="selTpl(this)"><div class="tpl-prev" style="background:rgba(28,184,138,.12)"><span style="color:var(--teal)">Médecin</span></div><div class="tpl-info"><div class="tpl-name">template-doctor</div><div class="tpl-desc">Clinique · Publications</div></div></div>
        <div class="tpl-card" onclick="selTpl(this)"><div class="tpl-prev" style="background:rgba(74,144,232,.12)"><span style="color:var(--blue)">Pharmacien</span></div><div class="tpl-info"><div class="tpl-name">template-pharma</div><div class="tpl-desc">Officine · Industrie</div></div></div>
        <div class="tpl-card" onclick="selTpl(this)"><div class="tpl-prev" style="background:rgba(232,96,160,.12)"><span style="color:var(--pink)">Infirmier</span></div><div class="tpl-info"><div class="tpl-name">template-nurse</div><div class="tpl-desc">Soins · Spécialités</div></div></div>
        <div class="tpl-card" onclick="selTpl(this)"><div class="tpl-prev" style="background:rgba(80,200,120,.12)"><span style="color:var(--green)">Kiné / Psy</span></div><div class="tpl-info"><div class="tpl-name">template-therapy</div><div class="tpl-desc">Rééducation · Thérapie</div></div></div>
      </div>
    </div>
    <div class="tab-content" id="tpl-autres">
      <div style="display:grid;grid-template-columns:repeat(4,1fr);gap:10px">
        <div class="tpl-card" onclick="selTpl(this)"><div class="tpl-prev" style="background:rgba(124,106,247,.12)"><span style="color:var(--purple2)">Enseignant</span></div><div class="tpl-info"><div class="tpl-name">template-teacher</div><div class="tpl-desc">Formation · Pédagogie</div></div></div>
        <div class="tpl-card" onclick="selTpl(this)"><div class="tpl-prev" style="background:rgba(74,144,232,.12)"><span style="color:var(--blue)">Chercheur</span></div><div class="tpl-info"><div class="tpl-name">template-academic</div><div class="tpl-desc">Publications · Thèse · Labo</div></div></div>
        <div class="tpl-card" onclick="selTpl(this)"><div class="tpl-prev" style="background:rgba(232,96,160,.12)"><span style="color:var(--pink)">Designer</span></div><div class="tpl-info"><div class="tpl-name">template-designer</div><div class="tpl-desc">Galerie · UX · Portfolio</div></div></div>
        <div class="tpl-card" onclick="selTpl(this)"><div class="tpl-prev" style="background:rgba(80,200,120,.12)"><span style="color:var(--green)">Architecte</span></div><div class="tpl-info"><div class="tpl-name">template-archi</div><div class="tpl-desc">Projets · Rendus · Plans</div></div></div>
        <div class="tpl-card" onclick="selTpl(this)"><div class="tpl-prev" style="background:rgba(232,160,32,.12)"><span style="color:var(--amber)">Avocat</span></div><div class="tpl-info"><div class="tpl-name">template-lawyer</div><div class="tpl-desc">Droit · Plaidoiries · Cabinet</div></div></div>
        <div class="tpl-card" onclick="selTpl(this)"><div class="tpl-prev" style="background:rgba(28,184,138,.12)"><span style="color:var(--teal)">Étudiant</span></div><div class="tpl-info"><div class="tpl-name">template-student</div><div class="tpl-desc">Stages · PFE · 1er emploi</div></div></div>
        <div class="tpl-card" onclick="selTpl(this)"><div class="tpl-prev" style="background:rgba(224,80,80,.12)"><span style="color:var(--red)">Journaliste</span></div><div class="tpl-info"><div class="tpl-name">template-journalist</div><div class="tpl-desc">Articles · Médias · Presse</div></div></div>
        <div class="tpl-card" onclick="selTpl(this)" style="border-style:dashed"><div class="tpl-prev"><span style="color:var(--text3)">+ Créer avec IA</span></div><div class="tpl-info"><div class="tpl-name" style="color:var(--text2)">custom</div><div class="tpl-desc">Ton domaine</div></div></div>
      </div>
    </div>
  </div>
</div>

<!-- ══ SETTINGS ══ -->
<div class="panel" id="panel-settings">
  <div class="topbar"><h2>Paramètres IA</h2><div class="tba"><button class="btn btn-teal btn-sm" onclick="testConn()">Tester connexion</button></div></div>
  <div class="content">
    <div class="g2">
      <div>
        <div class="st" style="margin-top:0">Claude (Anthropic) <span class="stl"></span></div>
        <div class="card2" style="margin-bottom:12px">
          <div class="fl">Clé API</div>
          <div style="display:flex;gap:6px;margin-bottom:8px"><input id="claude-key" type="password" placeholder="sk-ant-api03-..." style="margin:0;flex:1"/><button class="btn btn-sm btn-primary" onclick="saveKey('claude')">Sauver</button></div>
          <div class="fl">Modèle</div>
          <select id="claude-model" style="margin-bottom:6px"><option value="claude-sonnet-4-20250514">claude-sonnet-4 (recommandé)</option><option value="claude-opus-4-20250514">claude-opus-4</option><option value="claude-haiku-4-5-20251001">claude-haiku-4.5 (rapide)</option></select>
          <div style="display:flex;justify-content:space-between;font-size:11px"><a href="https://console.anthropic.com/" target="_blank" style="color:var(--purple2)">→ console.anthropic.com</a><span class="badge" id="s-claude">Non configuré</span></div>
        </div>
        <div class="st">Manus AI <span class="stl"></span></div>
        <div class="card2">
          <div class="fl">Clé API</div>
          <div style="display:flex;gap:6px;margin-bottom:8px"><input id="manus-key" type="password" placeholder="Clé Manus..." style="margin:0;flex:1"/><button class="btn btn-sm" onclick="saveKey('manus')">Sauver</button></div>
          <div class="fl">Endpoint</div><input id="manus-ep" placeholder="https://api.manus.ai/v1/chat/completions" style="margin-bottom:6px"/>
          <div class="fl">Modèle</div><input id="manus-model" value="manus-1" style="margin-bottom:6px"/>
          <div style="font-size:11px;color:var(--text3)">API Manus pas encore publique — prêt quand disponible sur <a href="https://manus.im" target="_blank" style="color:var(--purple2)">manus.im</a></div>
          <div style="display:flex;justify-content:flex-end;margin-top:6px"><span class="badge b-amber" id="s-manus">En attente</span></div>
        </div>
      </div>
      <div>
        <div class="st" style="margin-top:0">OpenAI / GPT <span class="stl"></span></div>
        <div class="card2" style="margin-bottom:12px">
          <div class="fl">Clé API</div>
          <div style="display:flex;gap:6px;margin-bottom:8px"><input id="openai-key" type="password" placeholder="sk-..." style="margin:0;flex:1"/><button class="btn btn-sm" onclick="saveKey('openai')">Sauver</button></div>
          <div class="fl">Modèle</div><select id="openai-model" style="margin-bottom:6px"><option>gpt-4o</option><option>gpt-4o-mini</option><option>gpt-4-turbo</option></select>
          <div style="display:flex;justify-content:flex-end"><span class="badge" id="s-openai">Non configuré</span></div>
        </div>
        <div class="st">IA personnalisée (OpenAI-compatible) <span class="stl"></span></div>
        <div class="card2">
          <div class="fl">Endpoint</div><input id="custom-ep" placeholder="https://api.mistral.ai/v1/chat/completions" style="margin-bottom:8px"/>
          <div class="fl">Clé API</div>
          <div style="display:flex;gap:6px;margin-bottom:8px"><input id="custom-key" type="password" placeholder="Clé..." style="margin:0;flex:1"/><button class="btn btn-sm" onclick="saveKey('custom')">Sauver</button></div>
          <div class="fl">Modèle</div><input id="custom-model" placeholder="mistral-large, llama3..." style="margin-bottom:6px"/>
          <div style="display:flex;justify-content:flex-end"><span class="badge" id="s-custom">Non configuré</span></div>
        </div>
      </div>
    </div>
  </div>
</div>

</div><!-- end main -->
</div><!-- end app -->

<!-- MODAL CLIENT -->
<div class="modal-ov" id="modal-client">
  <div class="modal">
    <div class="modal-title">Nouveau client <button class="btn btn-xs" onclick="closeModal('modal-client')">✕</button></div>
    <div class="g2">
      <div><div class="fl">Nom complet</div><input id="mc-n" placeholder="Yassine Berrada"/><div class="fl">Domaine</div>
        <select id="mc-d"><option>Génie industriel</option><option>Génie civil</option><option>Développeur</option><option>Data Science</option><option>Designer</option><option>Marketing</option><option>Finance</option><option>Médecin</option><option>Enseignant</option><option>Avocat</option><option>Autre</option></select>
        <div class="fl">Email</div><input id="mc-e" placeholder="client@email.com"/>
      </div>
      <div><div class="fl">Template</div>
        <select id="mc-t"><option>template-industrial</option><option>template-civil</option><option>template-dev</option><option>template-data</option><option>template-designer</option><option>template-marketing</option><option>template-finance</option><option>template-doctor</option><option>template-student</option></select>
        <div class="fl">Plan portail</div>
        <select id="mc-p"><option>Basique</option><option>Standard</option><option>Premium</option></select>
        <div class="fl">Budget (MAD)</div><input id="mc-b" type="number" placeholder="1500"/>
      </div>
    </div>
    <div style="display:flex;gap:8px;justify-content:flex-end;margin-top:10px">
      <button class="btn" onclick="closeModal('modal-client')">Annuler</button>
      <button class="btn btn-primary" onclick="saveClient()">Ajouter client</button>
    </div>
  </div>
</div>

<div class="notif" id="notif"></div>

<script>
// ══ STATE ══
const S={
  claudeKey:localStorage.getItem('pf_ck')||'',
  claudeModel:localStorage.getItem('pf_cm')||'claude-sonnet-4-20250514',
  manusKey:localStorage.getItem('pf_mk')||'',
  manusEp:localStorage.getItem('pf_me')||'',
  manusModel:localStorage.getItem('pf_mm')||'manus-1',
  openaiKey:localStorage.getItem('pf_ok')||'',
  openaiModel:localStorage.getItem('pf_om')||'gpt-4o',
  customKey:localStorage.getItem('pf_ck2')||'',
  customEp:localStorage.getItem('pf_ce')||'',
  customModel:localStorage.getItem('pf_cm2')||'',
  currentAI:'claude',
  clients:JSON.parse(localStorage.getItem('pf_clients')||'[]'),
  skills:[{n:'Lean Manufacturing',l:90},{n:'Six Sigma',l:80},{n:'AutoCAD / SolidWorks',l:75},{n:'Gestion de production',l:85},{n:'SAP ERP',l:70}],
  projects:[{t:'Optimisation ligne de production',d:'Réduction des temps de cycle de 25% via analyse VSM et chantiers 5S.',tech:'Lean, VSM, 5S'},{t:'Audit qualité ISO 9001',d:'Mise en place du SMQ et préparation à la certification ISO 9001:2015.',tech:'ISO 9001, PDCA, AMDEC'}],
  exps:[{r:'Ingénieure méthodes',c:'OCP Group',p:'2023–présent',d:'Optimisation des procédés de production phosphatière.'},{r:'Stagiaire amélioration continue',c:'Renault Tanger',p:'2022',d:'Analyse AMDEC et mise en place indicateurs OEE.'}],
  cvTpl:1,
  lastHTML:null,lastJSON:null,photoB64:null
};

window.onload=()=>{
  ['claude-key','manus-key','openai-key','custom-key'].forEach((id,i)=>{const vals=[S.claudeKey,S.manusKey,S.openaiKey,S.customKey];if(vals[i])document.getElementById(id).value=vals[i]});
  if(S.claudeModel)document.getElementById('claude-model').value=S.claudeModel;
  if(S.manusEp)document.getElementById('manus-ep').value=S.manusEp;
  if(S.openaiModel)document.getElementById('openai-model').value=S.openaiModel;
  if(S.customEp)document.getElementById('custom-ep').value=S.customEp;
  if(S.customModel)document.getElementById('custom-model').value=S.customModel;
  updateStatus();renderSkills();renderProjs();renderExps();renderClients();
  updatePortalPreview();
};

// ══ NAV ══
function nav(el,id){document.querySelectorAll('.nav-item').forEach(n=>n.classList.remove('active'));document.querySelectorAll('.panel').forEach(p=>p.classList.remove('active'));el.classList.add('active');document.getElementById('panel-'+id).classList.add('active')}
function stab(el,id){const p=el.closest('.panel');p.querySelectorAll('.tab-btn').forEach(b=>b.classList.remove('active'));p.querySelectorAll('.tab-content').forEach(c=>c.classList.remove('active'));el.classList.add('active');document.getElementById(id).classList.add('active')}

// ══ AI ══
function selAI(el,ai){document.querySelectorAll('.ai-tab').forEach(t=>t.classList.remove('active'));el.classList.add('active');S.currentAI=ai;const ms={claude:S.claudeModel,manus:S.manusModel,openai:S.openaiModel,custom:S.customModel||'?'};document.getElementById('ai-model-lbl').textContent='Modèle : '+ms[ai]}
async function callAI(){
  const type=document.getElementById('ai-type').value,sector=document.getElementById('ai-sector').value,extra=document.getElementById('ai-prompt').value,lang=document.getElementById('ai-lang').value,tone=document.getElementById('ai-tone').value;
  const out=document.getElementById('ai-output');
  const prompt=`Tu es expert portfolios. Langue: ${lang}. Ton: ${tone}. Domaine: ${sector}. Infos: ${extra||'non précisé'}.\n\n${buildPr(type)}`;
  out.innerHTML='<span style="color:var(--purple2)">Génération<span class="dots"><span>.</span><span>.</span><span>.</span></span></span>';
  out.classList.add('loading');
  try{
    if(S.currentAI==='claude'){if(!S.claudeKey){out.textContent='⚠ Clé Claude manquante → Paramètres IA';out.classList.remove('loading');return;}const r=await fetch('https://api.anthropic.com/v1/messages',{method:'POST',headers:{'Content-Type':'application/json','x-api-key':S.claudeKey,'anthropic-version':'2023-06-01','anthropic-dangerous-direct-browser-access':'true'},body:JSON.stringify({model:S.claudeModel,max_tokens:2000,messages:[{role:'user',content:prompt}]})});if(!r.ok){const e=await r.json();out.textContent='⚠ Erreur Claude: '+(e.error?.message||r.status);out.classList.remove('loading');return;}const d=await r.json();out.textContent=d.content?.[0]?.text||'Aucune réponse.';}
    else if(S.currentAI==='manus'){if(!S.manusEp||!S.manusKey){out.textContent='⚠ Manus non configuré.\n\nComment configurer Manus :\n1. Va dans Paramètres IA\n2. Entre ton endpoint Manus (ex: https://api.manus.ai/v1/chat/completions)\n3. Entre ta clé API Manus\n4. Clique Sauver\n\nNote: Manus n\'a pas encore d\'API publique. Reviens ici dès que disponible sur manus.im';out.classList.remove('loading');return;}await callOAI(prompt,out,S.manusEp,S.manusKey,S.manusModel);}
    else if(S.currentAI==='openai'){if(!S.openaiKey){out.textContent='⚠ Clé OpenAI manquante';out.classList.remove('loading');return;}await callOAI(prompt,out,'https://api.openai.com/v1/chat/completions',S.openaiKey,S.openaiModel);}
    else{if(!S.customKey||!S.customEp){out.textContent='⚠ IA custom non configurée';out.classList.remove('loading');return;}await callOAI(prompt,out,S.customEp,S.customKey,S.customModel);}
  }catch(e){out.textContent='⚠ '+e.message;}
  out.classList.remove('loading');
}
function buildPr(t){
  if(t.includes('Portfolio'))return'Génère un portfolio HTML complet, moderne, responsive, avec animations CSS légères et un fichier data.json.';
  if(t.includes('Bio'))return'Génère une bio professionnelle SEO en 4 phrases percutantes avec mots-clés métier.';
  if(t.includes('projet'))return'Génère une description de projet professionnelle : contexte, méthodes, résultats mesurables.';
  if(t.includes('Comp'))return'Liste 10 compétences clés avec niveau en % pour ce domaine. Format: Compétence — XX%';
  if(t.includes('propos'))return'Génère un texte "À propos" engageant de 150 mots pour portfolio.';
  if(t.includes('motivation'))return'Génère une lettre de motivation professionnelle complète.';
  if(t.includes('CV'))return'Génère un résumé CV une page structuré et impactant.';
  if(t.includes('branding'))return'Crée une stratégie personal branding complète pour ce profil.';
  if(t.includes('SEO'))return'Donne 10 conseils SEO précis pour optimiser ce portfolio dans Google.';
  return'Génère du contenu professionnel adapté pour ce portfolio.';
}
async function callOAI(prompt,outEl,ep,key,model){try{const r=await fetch(ep,{method:'POST',headers:{'Content-Type':'application/json','Authorization':'Bearer '+key},body:JSON.stringify({model:model||'gpt-4o',max_tokens:2000,messages:[{role:'user',content:prompt}]})});if(!r.ok){const e=await r.json();outEl.textContent='⚠ Erreur ('+r.status+'): '+(e.error?.message||JSON.stringify(e));return;}const d=await r.json();outEl.textContent=d.choices?.[0]?.message?.content||d.content?.[0]?.text||JSON.stringify(d);}catch(e){outEl.textContent='⚠ Erreur: '+e.message;}}
async function testConn(){const out=document.getElementById('ai-output');out.textContent='Test en cours...';await callAI();}
function clearOut(){document.getElementById('ai-output').textContent='Le résultat apparaîtra ici...'}
function copyOut(){navigator.clipboard?.writeText(document.getElementById('ai-output').textContent||'');notif('✓ Copié !')}
function injectEditor(){const t=document.getElementById('ai-output').textContent;if(t.length>10){document.getElementById('e-bio').value=t.substring(0,400);notif('✓ Injecté dans l\'éditeur !')}else notif('Génère d\'abord du contenu','warn')}
function injectCreate(){const t=document.getElementById('ai-output').textContent;if(t.length>10){document.getElementById('c-bio').value=t.substring(0,300);notif('✓ Injecté !')}else notif('Génère d\'abord','warn')}

// ══ SAVE KEYS ══
function saveKey(p){
  if(p==='claude'){S.claudeKey=document.getElementById('claude-key').value.trim();S.claudeModel=document.getElementById('claude-model').value;localStorage.setItem('pf_ck',S.claudeKey);localStorage.setItem('pf_cm',S.claudeModel);}
  else if(p==='manus'){S.manusKey=document.getElementById('manus-key').value.trim();S.manusEp=document.getElementById('manus-ep').value.trim();S.manusModel=document.getElementById('manus-model').value;localStorage.setItem('pf_mk',S.manusKey);localStorage.setItem('pf_me',S.manusEp);localStorage.setItem('pf_mm',S.manusModel);}
  else if(p==='openai'){S.openaiKey=document.getElementById('openai-key').value.trim();S.openaiModel=document.getElementById('openai-model').value;localStorage.setItem('pf_ok',S.openaiKey);localStorage.setItem('pf_om',S.openaiModel);}
  else{S.customKey=document.getElementById('custom-key').value.trim();S.customEp=document.getElementById('custom-ep').value.trim();S.customModel=document.getElementById('custom-model').value;localStorage.setItem('pf_ck2',S.customKey);localStorage.setItem('pf_ce',S.customEp);localStorage.setItem('pf_cm2',S.customModel);}
  updateStatus();notif('✓ Clé sauvegardée !');
}
function updateStatus(){
  const set=(id,ok,cls,msg)=>{const el=document.getElementById(id);if(!el)return;el.textContent=ok?'Connecté ✓':msg||'Non configuré';el.className='badge '+(ok?'b-teal':cls||'b-red')};
  set('s-claude',!!S.claudeKey,'b-red');
  set('s-manus',!!(S.manusKey&&S.manusEp),'b-amber','En attente');
  if(document.getElementById('s-manus')&&!S.manusKey){document.getElementById('s-manus').className='badge b-amber';document.getElementById('s-manus').textContent='En attente API'}
  set('s-openai',!!S.openaiKey,'b-red');
  set('s-custom',!!(S.customKey&&S.customEp),'b-red');
  ['claude','manus','openai','custom'].forEach((ai,i)=>{const d=document.getElementById('dot-'+ai);const vals=[!!S.claudeKey,!!(S.manusKey&&S.manusEp),!!S.openaiKey,!!(S.customKey&&S.customEp)];if(d)d.className='dot'+(vals[i]?' on':ai==='manus'?' warn':'')});
}

// ══ BUILD PORTFOLIO ══
function buildPortfolio(){
  const d={name:document.getElementById('c-name').value||'Mon Portfolio',title:document.getElementById('c-title').value||'Professionnel',bio:document.getElementById('c-bio').value||'',email:document.getElementById('c-email').value||'',phone:document.getElementById('c-phone').value||'',github:document.getElementById('c-github').value||'',linkedin:document.getElementById('c-linkedin').value||'',city:document.getElementById('c-city').value||'',sector:document.getElementById('c-sector').value||'',skills:(document.getElementById('c-skills').value||'Compétence 1').split(',').map(s=>({n:s.trim(),l:Math.floor(Math.random()*25)+70})),projects:(document.getElementById('c-projects').value||'Projet 1').split(',').map((p,i)=>({t:p.trim(),d:'Description du projet '+(i+1),tech:''})),exps:(document.getElementById('c-exp').value||'').split(',').map(e=>({r:e.trim(),c:'Entreprise',p:'2023–présent',d:'Description.'})),certs:(document.getElementById('c-certs').value||'').split(',').map(c=>c.trim()).filter(Boolean)};
  S.lastJSON=d;S.lastHTML=genHTML(d);
  document.getElementById('create-output').style.display='block';
  document.getElementById('create-code').textContent=S.lastHTML.substring(0,1200)+'\n\n... (télécharge pour le code complet)';
  notif('✓ Portfolio créé !');
}

function genHTML(d){
  const accentMap={purple:'#7c6af7',blue:'#1a4080',teal:'#0d7a5c',orange:'#d4730a',red:'#b02020',dark:'#444',white:'#6c5ce7'};
  const acc=accentMap[document.getElementById('c-color')?.value]||'#7c6af7';
  const ar=parseInt(acc.slice(1,3),16),ag=parseInt(acc.slice(3,5),16),ab=parseInt(acc.slice(5,7),16);
  const skbars=d.skills.map(s=>`<div class="sk"><div class="sk-t"><span>${s.n}</span><span>${s.l}%</span></div><div class="skb"><div class="skf" style="width:${s.l}%"></div></div></div>`).join('');
  const prCards=d.projects.map(p=>`<div class="proj"><h3>${p.t}</h3><p>${p.d}</p>${p.tech?`<div class="tags"><span class="tag">${p.tech.split(',').map(t=>`<span>${t.trim()}</span>`).join(' · ')}</span></div>`:''}</div>`).join('');
  const expHtml=d.exps.map(e=>`<div class="exp"><div class="exp-t"><strong>${e.r}</strong><span>${e.p}</span></div><div style="color:#9898b0;font-size:12px">${e.c}</div><p>${e.d}</p></div>`).join('');
  const certsHtml=d.certs.length?`<section><h2>Certifications</h2><div class="tags-c">${d.certs.map(c=>`<span class="cert">${c}</span>`).join('')}</div></section>`:'';
  return`<!DOCTYPE html><html lang="fr"><head><meta charset="UTF-8"><meta name="viewport" content="width=device-width,initial-scale=1"><meta name="description" content="${d.name} — ${d.title} — Portfolio"><title>${d.name} | ${d.title}</title><style>*{box-sizing:border-box;margin:0;padding:0}:root{--a:${acc};--bg:#0c0c10;--bg2:#13131a;--t:#eeeef5;--t2:#8888a8}body{font-family:'Segoe UI',sans-serif;background:var(--bg);color:var(--t);scroll-behavior:smooth}nav{position:fixed;top:0;left:0;right:0;padding:13px 5%;display:flex;justify-content:space-between;align-items:center;background:rgba(12,12,16,.92);backdrop-filter:blur(12px);z-index:100;border-bottom:1px solid rgba(255,255,255,.05)}.nl{font-size:15px;font-weight:700;color:var(--a)}.links{display:flex;gap:22px}.links a{font-size:13px;color:var(--t2);text-decoration:none;transition:color .2s}.links a:hover{color:var(--t)}header{min-height:100vh;display:flex;align-items:center;justify-content:center;text-align:center;padding:6rem 2rem 3rem;background:radial-gradient(ellipse at 50% 40%,rgba(${ar},${ag},${ab},.09) 0%,transparent 65%)}.hero-tag{background:rgba(${ar},${ag},${ab},.2);color:var(--a);padding:5px 16px;border-radius:20px;font-size:12px;display:inline-block;margin-bottom:1.5rem;animation:fu .6s ease both}h1{font-size:clamp(2rem,5vw,3.5rem);font-weight:800;line-height:1.15;margin-bottom:1rem;animation:fu .7s .1s ease both}h1 span{color:var(--a)}.sub{font-size:1rem;color:var(--t2);margin-bottom:.5rem;animation:fu .7s .15s ease both}.meta{font-size:12px;color:var(--t2);margin-bottom:1.8rem;animation:fu .7s .2s ease both}.cta{display:flex;gap:10px;justify-content:center;flex-wrap:wrap;animation:fu .7s .3s ease both}.bp{background:var(--a);color:#fff;padding:11px 26px;border-radius:8px;text-decoration:none;font-weight:500;font-size:13px;display:inline-block;transition:transform .2s,opacity .2s}.bp:hover{transform:translateY(-2px);opacity:.9}.bo{background:transparent;color:var(--t);padding:11px 26px;border-radius:8px;text-decoration:none;font-weight:500;font-size:13px;display:inline-block;border:1px solid rgba(255,255,255,.2);transition:background .2s}.bo:hover{background:rgba(255,255,255,.06)}section{padding:4.5rem 5%;max-width:1050px;margin:0 auto}h2{font-size:1.8rem;font-weight:700;margin-bottom:2rem;text-align:center}h2 span{color:var(--a)}.ag{display:grid;grid-template-columns:1fr 1fr;gap:2.5rem;align-items:start}@media(max-width:700px){.ag{grid-template-columns:1fr}.links{display:none}}.at{font-size:1rem;line-height:1.85;color:var(--t2)}.clinks{display:flex;gap:9px;flex-wrap:wrap;margin-top:1.2rem}.clinks a{padding:7px 14px;background:rgba(255,255,255,.05);color:var(--a);border-radius:6px;font-size:12px;text-decoration:none;border:1px solid rgba(255,255,255,.07);transition:background .2s}.clinks a:hover{background:rgba(255,255,255,.09)}.sk{margin-bottom:14px}.sk-t{display:flex;justify-content:space-between;font-size:12px;margin-bottom:5px;color:var(--t2)}.skb{height:5px;background:rgba(255,255,255,.06);border-radius:3px;overflow:hidden}.skf{height:100%;background:var(--a);border-radius:3px;transition:width 1.2s ease}.pg{display:grid;grid-template-columns:repeat(auto-fit,minmax(280px,1fr));gap:1.3rem}.proj{background:var(--bg2);border:1px solid rgba(255,255,255,.06);border-radius:12px;padding:1.3rem;transition:border-color .2s,transform .2s}.proj:hover{border-color:var(--a);transform:translateY(-3px)}.proj h3{margin-bottom:.5rem;font-size:1rem}.proj p{color:var(--t2);font-size:.88rem;margin-bottom:.8rem;line-height:1.65}.tags{display:flex;gap:5px;flex-wrap:wrap}.tag{padding:3px 9px;background:rgba(255,255,255,.06);color:var(--t2);border-radius:4px;font-size:11px}.el{display:grid;gap:.9rem}.exp{background:var(--bg2);border:1px solid rgba(255,255,255,.06);border-radius:11px;padding:1.1rem}.exp-t{display:flex;justify-content:space-between;align-items:center;margin-bottom:3px;flex-wrap:wrap;gap:4px}.exp-t strong{font-size:13px}.exp-t span{font-size:11px;color:var(--t2)}.exp p{font-size:12px;color:var(--t2);margin-top:5px;line-height:1.6}.tags-c{display:flex;gap:7px;flex-wrap:wrap;justify-content:center}.cert{padding:7px 16px;background:rgba(255,255,255,.05);border:1px solid rgba(255,255,255,.09);border-radius:7px;font-size:12px}footer{text-align:center;padding:2rem;color:var(--t2);font-size:12px;border-top:1px solid rgba(255,255,255,.04)}@keyframes fu{from{opacity:0;transform:translateY(18px)}to{opacity:1;transform:translateY(0)}}</style></head><body>
<nav><div class="nl">${d.name.split(' ')[0]}</div><div class="links"><a href="#about">À propos</a><a href="#projects">Projets</a><a href="#exp">Expériences</a><a href="#contact">Contact</a></div></nav>
<header><div><div class="hero-tag">${d.sector||'Portfolio Professionnel'} · 2026</div><h1>Bonjour, je suis<br><span>${d.name}</span></h1><p class="sub">${d.title}</p><p class="meta">${[d.city,d.email].filter(Boolean).join(' · ')}</p><div class="cta">${d.email?`<a href="mailto:${d.email}" class="bp">Me contacter</a>`:''} ${d.github?`<a href="${d.github}" class="bo" target="_blank">GitHub</a>`:''} ${d.linkedin?`<a href="${d.linkedin}" class="bo" target="_blank">LinkedIn</a>`:''}</div></div></header>
<section id="about"><h2>À <span>propos</span></h2><div class="ag"><div><p class="at">${d.bio}</p><div class="clinks">${d.linkedin?`<a href="${d.linkedin}" target="_blank">LinkedIn</a>`:''} ${d.email?`<a href="mailto:${d.email}">Email</a>`:''} ${d.phone?`<a href="tel:${d.phone}">Tél</a>`:''}</div></div><div>${skbars}</div></div></section>
<section id="projects"><h2>Mes <span>Projets</span></h2><div class="pg">${prCards}</div></section>
<section id="exp"><h2>Mes <span>Expériences</span></h2><div class="el">${expHtml}</div></section>
${certsHtml}
<section id="contact"><h2><span>Contact</span></h2><div style="text-align:center"><p style="color:var(--t2);margin-bottom:1.3rem">Disponible pour opportunités · Collaboration · Freelance</p><div class="cta">${d.email?`<a href="mailto:${d.email}" class="bp">Envoyer un email</a>`:''}</div></div></section>
<footer>Créé avec Portfolio Factory v3.0 · ${new Date().getFullYear()}</footer>
<script>const f=document.querySelectorAll('.skf'),ob=new IntersectionObserver(es=>{es.forEach(e=>{if(e.isIntersecting){const w=e.target.style.width;e.target.style.width='0';setTimeout(()=>e.target.style.width=w,50)}})},{threshold:.3});f.forEach(x=>ob.observe(x));<\/script></body></html>`;
}

// ══ DOWNLOAD ══
function downloadHTML(){if(!S.lastHTML)buildPortfolio();dl('index.html',S.lastHTML,'text/html')}
function downloadJSON(){if(!S.lastJSON)buildPortfolio();dl('data.json',JSON.stringify(S.lastJSON,null,2),'application/json')}
function dl(n,c,t){const a=document.createElement('a');a.href=URL.createObjectURL(new Blob([c],{type:t}));a.download=n;a.click();notif('✓ '+n+' téléchargé !')}

// ══ CV PDF ══
function selCV(n){document.querySelectorAll('.cv-tpl').forEach(c=>c.classList.remove('sel'));document.getElementById('cvt-'+n).classList.add('sel');S.cvTpl=n;}
function triggerPhoto(){document.getElementById('cv-photo-input').click()}
function loadPhoto(input){const f=input.files[0];if(!f)return;const r=new FileReader();r.onload=e=>{S.photoB64=e.target.result;const img=document.getElementById('cv-photo-img');img.src=S.photoB64;img.style.display='block';document.getElementById('photo-placeholder').style.display='none';notif('✓ Photo chargée !')};r.readAsDataURL(f);}

function exportCV(){
  const {jsPDF}=window.jspdf;
  if(!jsPDF){notif('Chargement PDF...','warn');setTimeout(exportCV,1200);return;}
  const doc=new jsPDF({orientation:'portrait',unit:'mm',format:'a4'});
  const name=document.getElementById('cv-name').value||document.getElementById('e-name')?.value||'Prénom Nom';
  const title=document.getElementById('cv-title').value||'Professionnel';
  const email=document.getElementById('cv-email').value||'';
  const phone=document.getElementById('cv-phone').value||'';
  const city=document.getElementById('cv-city').value||'';
  const linkedin=document.getElementById('cv-linkedin').value||'';
  const summary=document.getElementById('cv-summary').value||'';
  const skills=document.getElementById('cv-skills').value||'';
  const langs=document.getElementById('cv-langs').value||'';
  const exp=document.getElementById('cv-exp').value||'';
  const edu=document.getElementById('cv-edu').value||'';
  const certs=document.getElementById('cv-certs2').value||'';
  const acc=document.getElementById('cv-accent').value||'#7c6af7';
  const ar=parseInt(acc.slice(1,3),16),ag=parseInt(acc.slice(3,5),16),ab=parseInt(acc.slice(5,7),16);
  const pw=210,ml=15,mr=15,lw=pw-ml-mr;let y=0;

  const tpl=S.cvTpl;

  if(tpl===1){
    // MODERN PURPLE — barre latérale colorée
    doc.setFillColor(ar,ag,ab);doc.rect(0,0,60,297,'F');
    // Photo placeholder
    if(S.photoB64){try{doc.addImage(S.photoB64,'JPEG',10,10,40,40);doc.setDrawColor(255,255,255);doc.circle(30,30,20);}catch(e){}}
    else{doc.setFillColor(255,255,255,50);doc.circle(30,20,15,'F');}
    doc.setTextColor(255,255,255);doc.setFont('helvetica','bold');doc.setFontSize(9);y=60;
    doc.text('CONTACT',10,y);y+=6;
    doc.setFont('helvetica','normal');doc.setFontSize(8);
    [email,phone,city,linkedin].filter(Boolean).forEach(c=>{const ls=doc.splitTextToSize(c,42);doc.text(ls,10,y);y+=ls.length*4+1;});
    y+=4;doc.setFont('helvetica','bold');doc.setFontSize(9);doc.text('COMPÉTENCES',10,y);y+=6;
    doc.setFont('helvetica','normal');doc.setFontSize(8);
    skills.split(',').map(s=>s.trim()).filter(Boolean).forEach(s=>{doc.setFillColor(255,255,255,40);doc.roundedRect(10,y-3,42,5,1,1,'F');doc.text(s.substring(0,18),11,y);y+=7;if(y>260){doc.addPage();y=20;}});
    y+=4;doc.setFont('helvetica','bold');doc.setFontSize(9);doc.text('LANGUES',10,y);y+=6;
    doc.setFont('helvetica','normal');doc.setFontSize(8);langs.split(',').map(l=>l.trim()).filter(Boolean).forEach(l=>{doc.text(l.substring(0,18),10,y);y+=5;});
    // Right side
    let ry=15;doc.setTextColor(40,40,40);doc.setFont('helvetica','bold');doc.setFontSize(18);doc.text(name,65,ry);ry+=7;
    doc.setFont('helvetica','normal');doc.setFontSize(11);doc.setTextColor(ar,ag,ab);doc.text(title,65,ry);ry+=8;
    if(summary){doc.setTextColor(80,80,80);doc.setFontSize(9);const sl=doc.splitTextToSize(summary,135);doc.text(sl,65,ry);ry+=sl.length*4+6;}
    const sec=(t)=>{doc.setFont('helvetica','bold');doc.setFontSize(10);doc.setTextColor(ar,ag,ab);doc.text(t,65,ry);ry+=2;doc.setDrawColor(ar,ag,ab);doc.line(65,ry,200,ry);ry+=5;doc.setFont('helvetica','normal');doc.setFontSize(9);doc.setTextColor(50,50,50);};
    if(exp){sec('EXPÉRIENCES PROFESSIONNELLES');exp.split('\n').filter(Boolean).forEach(e=>{const ls=doc.splitTextToSize(e,133);doc.text(ls,65,ry);ry+=ls.length*4+4;if(ry>275){doc.addPage();ry=15;}});}
    if(edu){sec('FORMATION');edu.split('\n').filter(Boolean).forEach(e=>{const ls=doc.splitTextToSize(e,133);doc.text(ls,65,ry);ry+=ls.length*4+4;if(ry>275){doc.addPage();ry=15;}});}
    if(certs){sec('CERTIFICATIONS');const ls=doc.splitTextToSize(certs,133);doc.text(ls,65,ry);}
  }
  else if(tpl===2){
    // CLASSIC CORPORATE — 2 colonnes
    doc.setFillColor(ar,ag,ab);doc.rect(0,0,pw,30,'F');
    if(S.photoB64){try{doc.addImage(S.photoB64,'JPEG',175,2,26,26);}catch(e){}}
    doc.setTextColor(255,255,255);doc.setFont('helvetica','bold');doc.setFontSize(16);doc.text(name,ml,14);
    doc.setFont('helvetica','normal');doc.setFontSize(10);doc.text(title,ml,21);
    doc.setFontSize(8);doc.text([email,phone,city].filter(Boolean).join(' | '),ml,27);
    y=38;
    const sec2=(t)=>{doc.setFont('helvetica','bold');doc.setFontSize(10);doc.setTextColor(ar,ag,ab);doc.text(t.toUpperCase(),ml,y);y+=2;doc.setDrawColor(ar,ag,ab);doc.line(ml,y,pw-ml,y);y+=5;doc.setFont('helvetica','normal');doc.setFontSize(9);doc.setTextColor(40,40,40);};
    if(summary){sec2('Profil');const sl=doc.splitTextToSize(summary,lw);doc.text(sl,ml,y);y+=sl.length*4+6;}
    // 2 cols
    const lc=85,rc=115,rcw=lw-lc+ml;
    let ly=y,ry2=y;
    doc.setFont('helvetica','bold');doc.setFontSize(9);doc.setTextColor(ar,ag,ab);doc.text('EXPÉRIENCES',ml,ly);ly+=3;doc.line(ml,ly,lc-5,ly);ly+=4;
    doc.setFont('helvetica','normal');doc.setFontSize(8);doc.setTextColor(40,40,40);
    exp.split('\n').filter(Boolean).forEach(e=>{const ls=doc.splitTextToSize(e,lc-ml-5);doc.text(ls,ml,ly);ly+=ls.length*4+4;if(ly>270){doc.addPage();ly=20;}});
    doc.setFont('helvetica','bold');doc.setFontSize(9);doc.setTextColor(ar,ag,ab);doc.text('COMPÉTENCES',rc,ry2);ry2+=3;doc.line(rc,ry2,pw-ml,ry2);ry2+=4;
    doc.setFont('helvetica','normal');doc.setFontSize(8);doc.setTextColor(40,40,40);
    skills.split(',').map(s=>s.trim()).filter(Boolean).forEach(s=>{doc.text('• '+s,rc,ry2);ry2+=5;if(ry2>270){doc.addPage();ry2=20;}});
    ry2+=4;doc.setFont('helvetica','bold');doc.setFontSize(9);doc.setTextColor(ar,ag,ab);doc.text('FORMATION',rc,ry2);ry2+=3;doc.line(rc,ry2,pw-ml,ry2);ry2+=4;
    doc.setFont('helvetica','normal');doc.setFontSize(8);doc.setTextColor(40,40,40);
    edu.split('\n').filter(Boolean).forEach(e=>{const ls=doc.splitTextToSize(e,rcw);doc.text(ls,rc,ry2);ry2+=ls.length*4+4;});
  }
  else{
    // MINIMAL (tpl 3,4,5) — version propre
    if(tpl===4){doc.setFillColor(ar,ag,ab);doc.rect(0,0,pw,35,'F');doc.setTextColor(255,255,255);}
    else{doc.setTextColor(40,40,40);}
    if(S.photoB64&&tpl!==4){try{doc.addImage(S.photoB64,'JPEG',pw-ml-28,8,28,28);}catch(e){}}
    doc.setFont('helvetica','bold');doc.setFontSize(tpl===5?18:20);
    if(tpl===5){doc.setTextColor(40,40,40);y=18;doc.text(name,pw/2,y,'center');}
    else{y=16;doc.text(name,ml,y);}
    y+=7;doc.setFont('helvetica','normal');doc.setFontSize(10);
    if(tpl===4){doc.setTextColor(255,255,255,180);}else doc.setTextColor(ar,ag,ab);
    if(tpl===5){doc.text(title,pw/2,y,'center');y+=4;doc.text([email,phone,city].filter(Boolean).join(' | '),pw/2,y,'center');}
    else doc.text(title,ml,y);
    y+=4;if(tpl!==5){doc.setFontSize(8);doc.setTextColor(120,120,120);doc.text([email,phone,city].filter(Boolean).join(' · '),ml,y);}
    y+=5;doc.setTextColor(40,40,40);if(tpl===4)doc.setTextColor(200,200,200);
    doc.setDrawColor(ar,ag,ab);doc.line(ml,y,pw-ml,y);y+=5;
    const sec3=(t)=>{doc.setFont('helvetica','bold');doc.setFontSize(9);doc.setTextColor(tpl===4?255:ar,tpl===4?255:ag,tpl===4?255:ab);doc.text(t,ml,y);y+=3;doc.line(ml,y,pw-ml,y);y+=4;doc.setFont('helvetica','normal');doc.setFontSize(9);doc.setTextColor(tpl===4?210:50,tpl===4?210:50,tpl===4?210:50);};
    if(summary){sec3('PROFIL');const sl=doc.splitTextToSize(summary,lw);doc.text(sl,ml,y);y+=sl.length*4+5;}
    if(exp){sec3('EXPÉRIENCES');exp.split('\n').filter(Boolean).forEach(e=>{const ls=doc.splitTextToSize(e,lw);doc.text(ls,ml,y);y+=ls.length*4+4;if(y>275){doc.addPage();y=15;}});}
    if(edu){sec3('FORMATION');edu.split('\n').filter(Boolean).forEach(e=>{const ls=doc.splitTextToSize(e,lw);doc.text(ls,ml,y);y+=ls.length*4+4;if(y>275){doc.addPage();y=15;}});}
    if(skills){sec3('COMPÉTENCES');const ls=doc.splitTextToSize(skills,lw);doc.text(ls,ml,y);y+=ls.length*4+5;}
    if(langs){sec3('LANGUES');doc.text(langs,ml,y);}
  }
  doc.setFontSize(7);doc.setTextColor(180,180,180);doc.text('Portfolio Factory v3.0',pw/2,292,'center');
  const fname=name.replace(/\s+/g,'-');doc.save('CV-'+fname+'-2026.pdf');notif('✓ CV PDF téléchargé !');
}

function previewCV(){
  const name=document.getElementById('cv-name').value||'Prénom Nom';
  const title=document.getElementById('cv-title').value||'Professionnel';
  const acc=document.getElementById('cv-accent').value||'#7c6af7';
  const tpl=S.cvTpl;
  const box=document.getElementById('cv-preview-box');
  box.innerHTML=`<div style="background:${tpl===4?'#1a1a1a':tpl===2?'#f8f8f8':'#fff'};padding:20px;min-height:300px;font-family:Arial,sans-serif;font-size:12px">
    ${tpl===1?`<div style="position:relative;overflow:hidden"><div style="background:${acc};width:60px;float:left;min-height:300px;margin-right:16px;padding:10px;color:#fff;font-size:10px"><div style="width:40px;height:40px;border-radius:50%;background:rgba(255,255,255,.3);margin:0 auto 10px;overflow:hidden">${S.photoB64?`<img src="${S.photoB64}" style="width:100%;height:100%;object-fit:cover">`:'<div style="text-align:center;line-height:40px">📷</div>'}</div><div><strong>CONTACT</strong><br>${[document.getElementById('cv-email').value,document.getElementById('cv-city').value].filter(Boolean).join('<br>')}<br><br><strong>COMPÉTENCES</strong><br>${(document.getElementById('cv-skills').value||'').split(',').map(s=>`<div style="background:rgba(255,255,255,.2);padding:2px 4px;margin:2px 0;border-radius:2px">${s.trim()}</div>`).join('')}</div></div><div style="margin-left:70px"><h1 style="font-size:18px;color:#333;margin-bottom:4px">${name}</h1><p style="color:${acc};font-size:13px;margin-bottom:10px">${title}</p><p style="font-size:11px;color:#666">${document.getElementById('cv-summary').value||''}</p></div></div>`
    :tpl===2?`<div style="background:${acc};padding:14px;color:#fff;margin:-20px -20px 14px"><h1 style="font-size:18px;margin-bottom:2px">${name}</h1><p style="font-size:11px;opacity:.85">${title}</p></div><div style="display:grid;grid-template-columns:1fr 1fr;gap:14px"><div><div style="font-size:9px;text-transform:uppercase;color:${acc};font-weight:700;margin-bottom:4px">Expériences</div><p style="font-size:10px;color:#555">${document.getElementById('cv-exp').value||''}</p></div><div><div style="font-size:9px;text-transform:uppercase;color:${acc};font-weight:700;margin-bottom:4px">Compétences</div><p style="font-size:10px;color:#555">${document.getElementById('cv-skills').value||''}</p></div></div>`
    :`<div style="text-align:${tpl===5?'center':'left'}"><h1 style="font-size:20px;color:${tpl===4?'#fff':'#333'};margin-bottom:4px">${name}</h1><p style="color:${acc};font-size:12px;margin-bottom:10px">${title}</p><hr style="border-color:${acc};margin:8px 0"><p style="font-size:10px;color:${tpl===4?'#ccc':'#666'}">${document.getElementById('cv-summary').value||''}</p></div>`}
  </div>`;
  notif('✓ Aperçu mis à jour !');
}

// ══ PORTAL ══
function genPortal(){
  const name=document.getElementById('pt-name').value||'Client';
  const plan=document.getElementById('pt-plan').value;
  const slug=name.toLowerCase().replace(/\s+/g,'-').replace(/[^a-z0-9-]/g,'');
  document.getElementById('portal-url').textContent=`https://ton-hebergeur.com/clients/${slug}/index.html`;
  updatePortalPreview();
  const portalHTML=buildPortalHTML(name,plan);
  document.getElementById('portal-code').textContent=portalHTML.substring(0,800)+'...\n\n(Télécharge pour le fichier complet)';
  S.lastPortal={html:portalHTML,name,slug};
  notif('✓ Portail généré !');
}

function buildPortalHTML(clientName,plan){
  return`<!DOCTYPE html>
<html lang="fr">
<head>
<meta charset="UTF-8"><meta name="viewport" content="width=device-width,initial-scale=1">
<title>Mon Portfolio — ${clientName}</title>
<style>
*{box-sizing:border-box;margin:0;padding:0}
body{font-family:'Segoe UI',sans-serif;background:#0c0c10;color:#eeeef5;min-height:100vh}
.header{background:#7c6af7;padding:16px 20px;display:flex;align-items:center;gap:12px}
.avatar{width:40px;height:40px;border-radius:50%;background:rgba(255,255,255,.2);display:flex;align-items:center;justify-content:center;font-weight:700;font-size:14px;flex-shrink:0}
.header-info h1{font-size:15px;color:#fff;font-weight:600}
.header-info p{font-size:12px;color:rgba(255,255,255,.7)}
.plan-badge{margin-left:auto;background:rgba(255,255,255,.2);color:#fff;padding:3px 10px;border-radius:12px;font-size:11px;font-weight:600}
.container{max-width:700px;margin:0 auto;padding:24px 16px}
.section-title{font-size:13px;font-weight:600;color:#7c6af7;margin-bottom:12px;margin-top:20px;text-transform:uppercase;letter-spacing:.06em}
.field-card{background:#1a1a24;border:1px solid #28283a;border-radius:10px;padding:14px;margin-bottom:10px}
.field-label{font-size:11px;color:#4a4a68;text-transform:uppercase;letter-spacing:.06em;margin-bottom:5px}
textarea,input{width:100%;padding:8px 10px;border:1px solid #36364a;border-radius:7px;font-size:13px;background:#22222e;color:#eeeef5;font-family:'Segoe UI',sans-serif;outline:none;transition:border .15s}
textarea:focus,input:focus{border-color:#7c6af7}
textarea{resize:vertical;min-height:80px}
.btn{padding:9px 18px;border-radius:7px;border:1px solid #36364a;background:transparent;font-size:13px;cursor:pointer;color:#eeeef5;font-family:'Segoe UI',sans-serif;transition:all .12s;margin-top:4px}
.btn-primary{background:#7c6af7;color:#fff;border-color:#7c6af7}
.btn-primary:hover{background:#a89bff}
.btn-row{display:flex;gap:8px;margin-top:16px;flex-wrap:wrap}
.msg{display:none;background:#083d2e;border:1px solid #0d7a5c;border-radius:8px;padding:11px 14px;font-size:13px;color:#1cb88a;margin-top:12px}
.skill-row{display:flex;align-items:center;gap:8px;margin-bottom:8px}
.skill-row input[type=text]{flex:1;margin:0}
.skill-row input[type=range]{flex:0 0 100px;margin:0}
.skill-row span{min-width:32px;font-size:12px;color:#8888a8}
.add-btn{background:transparent;border:1px dashed #36364a;border-radius:7px;padding:6px 12px;font-size:12px;color:#8888a8;cursor:pointer;transition:all .15s}
.add-btn:hover{border-color:#7c6af7;color:#7c6af7}
footer{text-align:center;padding:20px;font-size:11px;color:#4a4a68;margin-top:24px}
</style>
</head>
<body>
<div class="header">
  <div class="avatar">${clientName.split(' ').map(w=>w[0]).join('').substring(0,2).toUpperCase()}</div>
  <div class="header-info"><h1>${clientName}</h1><p>Mon espace portfolio · Portfolio Factory</p></div>
  <span class="plan-badge">${plan==='basic'?'Basique':plan==='standard'?'Standard':'Premium'}</span>
</div>

<div class="container">
  <div class="section-title">Mes informations</div>
  <div class="field-card"><div class="field-label">Nom affiché sur le portfolio</div><input id="f-name" value="${clientName}"/></div>
  <div class="field-card"><div class="field-label">Titre professionnel</div><input id="f-title" placeholder="ex: Ingénieur Génie Industriel"/></div>
  <div class="field-card"><div class="field-label">Bio / À propos</div><textarea id="f-bio" placeholder="Décris ton parcours, tes compétences, tes passions..."></textarea></div>
  <div class="field-card"><div class="field-label">Email de contact</div><input id="f-email" type="email" placeholder="ton@email.com"/></div>
  <div class="field-card"><div class="field-label">LinkedIn</div><input id="f-linkedin" placeholder="https://linkedin.com/in/..."/></div>
  <div class="field-card"><div class="field-label">Ville</div><input id="f-city" placeholder="Rabat, Maroc"/></div>

  ${plan!=='basic'?`
  <div class="section-title">Mes compétences</div>
  <div class="field-card">
    <div id="skills-list">
      <div class="skill-row"><input type="text" placeholder="Compétence" value="Lean Manufacturing"/><input type="range" min="1" max="100" value="85" oninput="this.nextElementSibling.textContent=this.value+'%'"/><span>85%</span></div>
      <div class="skill-row"><input type="text" placeholder="Compétence" value="Six Sigma"/><input type="range" min="1" max="100" value="75" oninput="this.nextElementSibling.textContent=this.value+'%'"/><span>75%</span></div>
    </div>
    <button class="add-btn" onclick="addSkillRow()" style="margin-top:8px">+ Ajouter une compétence</button>
  </div>

  <div class="section-title">Mes projets</div>
  <div class="field-card"><div class="field-label">Projet 1 — Titre</div><input id="p1-title" placeholder="Optimisation ligne de production"/><div class="field-label" style="margin-top:6px">Description</div><textarea id="p1-desc" placeholder="Ce projet a permis de..."></textarea></div>
  <div class="field-card"><div class="field-label">Projet 2 — Titre</div><input id="p2-title" placeholder="Audit qualité ISO"/><div class="field-label" style="margin-top:6px">Description</div><textarea id="p2-desc" placeholder="Ce projet consistait à..."></textarea></div>`:''}

  ${plan==='premium'?`
  <div class="section-title">Design</div>
  <div class="field-card"><div class="field-label">Couleur principale</div><input type="color" id="f-color" value="#7c6af7" style="height:36px;padding:3px;cursor:pointer"/></div>`:''}

  <div class="btn-row">
    <button class="btn btn-primary" onclick="savePortfolio()">Sauvegarder et télécharger mon portfolio</button>
    <button class="btn" onclick="previewPortfolio()">Aperçu</button>
  </div>
  <div id="save-msg" class="msg">✓ Ton portfolio a été mis à jour ! Télécharge-le et envoie-le à ${clientName.split(' ')[0]} (ton prestataire) pour le mettre en ligne.</div>
</div>

<footer>Portail créé par Portfolio Factory · Géré par ton prestataire</footer>

<script>
function addSkillRow(){const d=document.createElement('div');d.className='skill-row';d.innerHTML='<input type="text" placeholder="Compétence"/><input type="range" min="1" max="100" value="70" oninput="this.nextElementSibling.textContent=this.value+\'%\'"/><span>70%</span>';document.getElementById('skills-list').appendChild(d);}
function savePortfolio(){
  const data={name:document.getElementById('f-name').value,title:document.getElementById('f-title').value,bio:document.getElementById('f-bio').value,email:document.getElementById('f-email').value,linkedin:document.getElementById('f-linkedin').value,city:document.getElementById('f-city').value};
  localStorage.setItem('portfolio_data',JSON.stringify(data));
  const msg=document.getElementById('save-msg');msg.style.display='block';
  const html=buildMyPortfolio(data);
  const a=document.createElement('a');a.href=URL.createObjectURL(new Blob([html],{type:'text/html'}));a.download='mon-portfolio.html';a.click();
  setTimeout(()=>msg.style.display='none',4000);
}
function buildMyPortfolio(d){
  return '<!DOCTYPE html><html lang="fr"><head><meta charset="UTF-8"><meta name="viewport" content="width=device-width,initial-scale=1"><title>'+d.name+'</title><style>body{font-family:Segoe UI,sans-serif;background:#0c0c10;color:#eee;margin:0}header{min-height:60vh;display:flex;align-items:center;justify-content:center;text-align:center;padding:4rem 2rem;background:radial-gradient(ellipse,rgba(124,106,247,.1),transparent)}h1{font-size:2.5rem;font-weight:700;margin-bottom:.5rem}h1 span{color:#7c6af7}.sub{color:#8888a8;font-size:1.1rem;margin-bottom:1.5rem}.btn{background:#7c6af7;color:#fff;padding:10px 24px;border-radius:8px;text-decoration:none;display:inline-block}section{padding:4rem 5%;max-width:900px;margin:0 auto}h2{font-size:1.6rem;font-weight:700;margin-bottom:1.5rem;text-align:center}h2 span{color:#7c6af7}p{color:#8888a8;line-height:1.8}</style></head><body><header><div><h1>Bonjour, je suis<br><span>'+d.name+'</span></h1><p class="sub">'+d.title+'</p>'+(d.email?'<a href="mailto:'+d.email+'" class="btn">Me contacter</a>':'')+'</div></header><section><h2>À <span>propos</span></h2><p>'+d.bio+'</p><div style="margin-top:1.5rem;display:flex;gap:10px;justify-content:center;flex-wrap:wrap">'+(d.linkedin?'<a href="'+d.linkedin+'" target="_blank" style="color:#7c6af7">LinkedIn</a>':'')+'</div></section></body></html>';
}
function previewPortfolio(){savePortfolio();}
const saved=JSON.parse(localStorage.getItem('portfolio_data')||'{}');
if(saved.name){['name','title','bio','email','linkedin','city'].forEach(k=>{const el=document.getElementById('f-'+k);if(el&&saved[k])el.value=saved[k];});}
<\/script>
</body></html>`;
}

function dlPortal(){if(!S.lastPortal){genPortal();}dl(S.lastPortal.slug+'-portal.html',S.lastPortal.html,'text/html')}
function copyPortalCode(){navigator.clipboard?.writeText(document.getElementById('portal-code').textContent||'');notif('✓ Code copié !')}
function updatePortalPreview(){
  const name=document.getElementById('pt-name')?.value||'Client';
  const plan=document.getElementById('pt-plan')?.value||'standard';
  const init=name.split(' ').map(w=>w[0]).join('').substring(0,2).toUpperCase();
  const el=document.getElementById('pt-avatar');if(el)el.textContent=init;
  const nd=document.getElementById('pt-name-display');if(nd)nd.textContent=name;
  const pd=document.getElementById('pt-plan-display');if(pd)pd.textContent={basic:'Basique',standard:'Standard',premium:'Premium'}[plan]||'Standard';
  const pu=document.getElementById('portal-url');
  if(pu){const slug=name.toLowerCase().replace(/\s+/g,'-').replace(/[^a-z0-9-]/g,'');pu.textContent=`https://ton-hebergeur.com/clients/${slug}/index.html`;}
}
document.getElementById('pt-name')?.addEventListener('input',updatePortalPreview);
document.getElementById('pt-plan')?.addEventListener('change',updatePortalPreview);

// ══ EDITOR ══
function renderSkills(){const el=document.getElementById('skills-ed');if(!el)return;el.innerHTML=S.skills.map((s,i)=>`<div style="display:flex;align-items:center;gap:7px;margin-bottom:9px"><input value="${s.n}" style="flex:1;margin:0" onchange="S.skills[${i}].n=this.value"/><input type="number" value="${s.l}" min="1" max="100" style="width:60px;margin:0" onchange="S.skills[${i}].l=+this.value"/><div class="pb" style="flex:1;margin-top:0"><div class="pf" style="width:${s.l}%"></div></div><button class="btn btn-xs btn-red" onclick="S.skills.splice(${i},1);renderSkills()">×</button></div>`).join('');}
function addSkill(){const n=document.getElementById('nsk-n').value.trim(),l=+document.getElementById('nsk-l').value||80;if(!n){notif('Entre un nom !','warn');return;}S.skills.push({n,l});renderSkills();document.getElementById('nsk-n').value='';notif('✓ Ajouté !')}
function renderProjs(){const el=document.getElementById('proj-ed');if(!el)return;el.innerHTML=S.projects.map((p,i)=>`<div class="card" style="margin-bottom:9px"><div style="display:flex;justify-content:space-between;align-items:center;margin-bottom:7px"><span style="font-size:13px;font-weight:500">${p.t}</span><button class="btn btn-xs btn-red" onclick="S.projects.splice(${i},1);renderProjs()">Supprimer</button></div><input value="${p.t}" style="margin-bottom:6px" onchange="S.projects[${i}].t=this.value"/><textarea style="min-height:50px" onchange="S.projects[${i}].d=this.value">${p.d}</textarea></div>`).join('');}
function addProj(){const t=document.getElementById('np-t').value.trim(),d=document.getElementById('np-d').value.trim(),tc=document.getElementById('np-tech').value;if(!t)return;S.projects.push({t,d,tech:tc});renderProjs();document.getElementById('np-t').value='';document.getElementById('np-d').value='';notif('✓ Projet ajouté !')}
function renderExps(){const el=document.getElementById('exp-ed');if(!el)return;el.innerHTML=S.exps.map((e,i)=>`<div class="card" style="margin-bottom:9px"><div style="display:flex;justify-content:space-between;align-items:center;margin-bottom:7px"><span style="font-size:13px;font-weight:500">${e.r} — ${e.c}</span><button class="btn btn-xs btn-red" onclick="S.exps.splice(${i},1);renderExps()">Supprimer</button></div><input value="${e.r}" style="margin-bottom:5px" onchange="S.exps[${i}].r=this.value"/><input value="${e.c}" style="margin-bottom:5px" onchange="S.exps[${i}].c=this.value"/><input value="${e.p}" style="margin-bottom:5px" onchange="S.exps[${i}].p=this.value"/></div>`).join('');}
function addExp(){const r=document.getElementById('ne-r').value.trim(),c=document.getElementById('ne-c').value.trim(),p=document.getElementById('ne-p').value.trim(),d=document.getElementById('ne-d').value.trim();if(!r)return;S.exps.push({r,c,p,d});renderExps();document.getElementById('ne-r').value='';document.getElementById('ne-c').value='';notif('✓ Expérience ajoutée !')}
function saveEd(){localStorage.setItem('pf_editor',JSON.stringify({name:document.getElementById('e-name').value,bio:document.getElementById('e-bio').value,skills:S.skills,projects:S.projects,exps:S.exps}));notif('✓ Portfolio sauvegardé !')}

// ══ CLIENTS ══
function saveClient(){const n=document.getElementById('mc-n').value.trim();if(!n){notif('Entre un nom !','warn');return;}S.clients.push({name:n,domain:document.getElementById('mc-d').value,email:document.getElementById('mc-e').value,tpl:document.getElementById('mc-t').value,plan:document.getElementById('mc-p').value,budget:document.getElementById('mc-b').value,status:'En cours'});localStorage.setItem('pf_clients',JSON.stringify(S.clients));closeModal('modal-client');renderClients();notif('✓ Client ajouté !');}
function renderClients(){
  const list=document.getElementById('cli-list');if(!list)return;
  const base=`<div class="clirow"><div style="display:flex;align-items:center;gap:9px"><div class="avatar">YB</div><div><div style="font-size:13px;font-weight:500">Yassine Berrada</div><div style="font-size:11px;color:var(--text2)">Génie civil · template-civil</div></div></div><div style="display:flex;align-items:center;gap:6px"><span class="badge b-teal">Publié</span><button class="btn btn-xs" onclick="nav(document.querySelectorAll('.nav-item')[7],'portal')">Portail</button><button class="btn btn-xs" onclick="nav(document.querySelectorAll('.nav-item')[4],'cv')">CV</button></div></div><div class="clirow"><div style="display:flex;align-items:center;gap:9px"><div class="avatar" style="background:var(--teal3);color:var(--teal)">LM</div><div><div style="font-size:13px;font-weight:500">Leila Mansouri</div><div style="font-size:11px;color:var(--text2)">Designer UX · template-designer</div></div></div><div style="display:flex;align-items:center;gap:6px"><span class="badge b-teal">Publié</span><button class="btn btn-xs" onclick="nav(document.querySelectorAll('.nav-item')[7],'portal')">Portail</button><button class="btn btn-xs" onclick="nav(document.querySelectorAll('.nav-item')[4],'cv')">CV</button></div></div><div class="clirow"><div style="display:flex;align-items:center;gap:9px"><div class="avatar" style="background:var(--amber2);color:var(--amber)">AK</div><div><div style="font-size:13px;font-weight:500">Amine Khalil</div><div style="font-size:11px;color:var(--text2)">Développeur fullstack · template-dev</div></div></div><div style="display:flex;align-items:center;gap:6px"><span class="badge b-amber">En cours</span><button class="btn btn-xs" onclick="nav(document.querySelectorAll('.nav-item')[7],'portal')">Portail</button><button class="btn btn-xs" onclick="nav(document.querySelectorAll('.nav-item')[4],'cv')">CV</button></div></div>`;
  const extras=S.clients.map(c=>{const init=c.name.split(' ').map(w=>w[0]).join('').substring(0,2).toUpperCase();return`<div class="clirow"><div style="display:flex;align-items:center;gap:9px"><div class="avatar">${init}</div><div><div style="font-size:13px;font-weight:500">${c.name}</div><div style="font-size:11px;color:var(--text2)">${c.domain} · ${c.tpl} · ${c.plan}</div></div></div><div style="display:flex;align-items:center;gap:6px"><span class="badge b-amber">${c.status}</span><button class="btn btn-xs" onclick="genPortalForClient('${c.name}','${c.plan}')">Portail</button><button class="btn btn-xs" onclick="nav(document.querySelectorAll('.nav-item')[4],'cv')">CV</button></div></div>`;}).join('');
  list.innerHTML=base+extras;
  const tot=3+S.clients.length;document.getElementById('ct-tot').textContent=tot;document.getElementById('nb-cli').textContent=tot;
}
function genPortalForClient(name,plan){document.getElementById('pt-name').value=name;document.getElementById('pt-plan').value=plan.toLowerCase();nav(document.querySelectorAll('.nav-item')[7],'portal');updatePortalPreview();genPortal();}

// ══ DEPLOY ══
function simDeploy(){const user=document.getElementById('d-user').value||'username',repo=document.getElementById('d-repo').value||'portfolio';document.getElementById('deploy-script').textContent=`# PowerShell — Exécute dans ton terminal Windows\n\ncd "E:\\projets IA\\Portfolio_Factory\\Users\\Personal\\${(document.getElementById('d-portfolio').value||'').split('—')[0].trim().toLowerCase().replace(/ /g,'-')}"\n\ngit init\ngit add .\ngit commit -m "Portfolio Factory v3.0"\ngit branch -M main\n\ngh repo create ${repo} --public --source=. --remote=origin\ngit push -u origin main\n\ngh api repos/${user}/${repo}/pages -X POST -f source='{"branch":"main","path":"/"}'\n\necho "✓ URL: https://${user}.github.io/${repo}/"`;document.getElementById('pub-url').textContent=`https://${user}.github.io/${repo}/`;document.getElementById('repo-url').textContent=`https://github.com/${user}/${repo}`;document.getElementById('deploy-result').style.display='block';notif('✓ Script généré !')}
function copyScript(){navigator.clipboard?.writeText(document.getElementById('deploy-script').textContent||'');notif('✓ Script copié !')}
function copyEl(id){navigator.clipboard?.writeText(document.getElementById(id).textContent||'');notif('✓ Copié !')}

// ══ MISC ══
function selTpl(el){document.querySelectorAll('.tpl-card').forEach(c=>c.classList.remove('sel'));el.classList.add('sel')}
function openModal(id){document.getElementById(id).classList.add('open')}
function closeModal(id){document.getElementById(id).classList.remove('open')}
let _nt;function notif(msg,type){const n=document.getElementById('notif');n.textContent=msg;n.className='notif'+(type?' '+type:'');n.style.display='block';clearTimeout(_nt);_nt=setTimeout(()=>n.style.display='none',3000);}
</script>
</body>
</html>
