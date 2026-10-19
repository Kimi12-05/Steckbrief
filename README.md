<!DOCTYPE html>
<html lang="de">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width,initial-scale=1,viewport-fit=cover">
<title>Steckbriefe</title>
<meta name="apple-mobile-web-app-capable" content="yes">
<meta name="mobile-web-app-capable" content="yes">
<meta name="apple-mobile-web-app-status-bar-style" content="default">
<meta name="apple-mobile-web-app-title" content="Steckbriefe">
<meta name="theme-color" content="#f7f2ea">
<link rel="manifest" href="manifest.json">
<link rel="apple-touch-icon" href="icon.png">
<link rel="icon" href="icon.svg">
<link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600&family=Caveat:wght@500;600&family=Special+Elite&family=Courier+Prime:wght@400;700&display=swap" rel="stylesheet">
<style>
:root{--bg:#f7f2ea;--fg:#1b1917;--muted:#8a8074;--card:#fff;--line:rgba(0,0,0,.09);
      --accent:#d6336c;--r:5px;--th:132px;--fs:16px;--navh:56px;}
html{font-size:var(--fs);}
*{box-sizing:border-box;-webkit-tap-highlight-color:transparent;}
html,body{margin:0;padding:0;height:100%;}
body{background:#e9e8e4;font-family:Inter,system-ui,-apple-system,sans-serif;color:var(--fg);overscroll-behavior:none;}
.app{position:relative;max-width:480px;margin:0 auto;height:100vh;height:100dvh;background:var(--bg);overflow:hidden;}

/* ---------- Startseite ---------- */
.scroller{position:absolute;inset:0;overflow-y:auto;-webkit-overflow-scrolling:touch;
  padding:calc(env(safe-area-inset-top) + 18px) 16px calc(var(--navh) + env(safe-area-inset-bottom) + 26px);}
.head{display:flex;align-items:flex-start;justify-content:space-between;gap:12px;}
.ttl{font-family:'Special Elite',cursive;font-size:1.6rem;font-weight:400;letter-spacing:0;}
.sub{font-family:'Courier Prime',monospace;font-size:.82rem;color:var(--muted);margin:3px 0 14px;}
.lockbtn{width:38px;height:38px;border-radius:11px;border:1px solid var(--line);background:var(--card);
  display:flex;align-items:center;justify-content:center;color:var(--muted);flex:0 0 38px;}
.lockbtn svg{width:19px;height:19px;}
.search{display:flex;align-items:center;gap:9px;height:46px;border-radius:14px;background:var(--card);
  border:1px solid var(--line);padding:0 14px;color:var(--muted);margin-bottom:16px;}
.search svg{width:17px;height:17px;flex:0 0 17px;}
.search input{flex:1;border:0;background:transparent;outline:none;font:inherit;font-size:1rem;color:var(--fg);}
.grid{display:grid;grid-template-columns:1fr 1fr;gap:14px;}
.tile{position:relative;background:#fff;padding:8px 8px 30px;border:1px solid var(--line);border-radius:var(--r);
  box-shadow:0 4px 14px rgba(0,0,0,.07);transform:rotate(-1.3deg);cursor:pointer;}
.tile:nth-child(even){transform:rotate(1.1deg);}
.tile .ph{position:relative;height:var(--th);overflow:hidden;background:#eae4d9;border-radius:2px;}
.tile .ph img{width:100%;height:100%;object-fit:cover;display:block;}
.tile .nm{font-family:Caveat,cursive;font-size:1.3rem;font-weight:600;text-align:center;margin-top:5px;
  white-space:nowrap;overflow:hidden;text-overflow:ellipsis;color:#2a2622;}
.tile .upl{position:absolute;top:14px;right:14px;width:28px;height:28px;border-radius:9px;background:rgba(0,0,0,.45);
  display:flex;align-items:center;justify-content:center;z-index:2;}

/* ---------- Polaroid-Knöpfe ---------- */
.pbtn{position:relative;background:#fff;border:1px solid var(--line);border-radius:var(--r);
  box-shadow:0 4px 14px rgba(0,0,0,.07);transform:rotate(-1.3deg);cursor:pointer;
  display:flex;align-items:center;justify-content:center;text-align:center;line-height:1.2;
  min-height:calc(var(--th)/3 + 12px);font-family:'Courier Prime',monospace;font-size:.9rem;color:#2a2622;
  padding:6px;}
.pbtn:nth-child(even){transform:rotate(1.1deg);}

.reslist{display:none;flex-direction:column;}
.reslist.on{display:flex;}
.res{display:flex;justify-content:space-between;gap:10px;padding:13px 2px;border-bottom:1px solid var(--line);cursor:pointer;}
.res .rn{font-family:'Courier Prime',monospace;font-size:.98rem;}
.res .rh{font-size:.78rem;color:var(--muted);text-align:right;max-width:52%;}
.empty{font-size:.9rem;color:var(--muted);padding:14px 2px;}

/* ---------- Leiste ---------- */
.nav{position:absolute;left:0;right:0;bottom:0;z-index:45;height:calc(var(--navh) + env(safe-area-inset-bottom));
  padding-bottom:env(safe-area-inset-bottom);display:flex;background:rgba(247,242,234,.96);
  -webkit-backdrop-filter:blur(9px);backdrop-filter:blur(9px);border-top:1px solid var(--line);}
.nav.off{display:none;}
.nav button{flex:1;border:0;background:transparent;display:flex;align-items:center;justify-content:center;
  color:var(--muted);cursor:pointer;padding:0;}
.nav button.on{color:var(--accent);}
.nav button svg,.nav button img{width:24px;height:24px;display:block;}
.nav button .ic{width:24px;height:24px;display:flex;align-items:center;justify-content:center;}

/* ---------- KI-Blase ---------- */
.fab{position:absolute;right:16px;bottom:calc(var(--navh) + env(safe-area-inset-bottom) + 16px);z-index:46;
  width:52px;height:52px;border-radius:50%;border:0;background:var(--accent);color:#fff;cursor:pointer;
  box-shadow:0 8px 22px rgba(214,51,108,.36);display:flex;align-items:center;justify-content:center;}
.fab.off{display:none;}
.fab svg{width:25px;height:25px;}

/* ---------- Fenster ---------- */
.overlay{position:absolute;inset:0;z-index:30;background:var(--bg);display:flex;flex-direction:column;
  padding-bottom:calc(var(--navh) + env(safe-area-inset-bottom));
  opacity:0;pointer-events:none;transform:translateX(22px);transition:opacity .2s,transform .2s;}
.overlay.on{opacity:1;pointer-events:auto;transform:none;}
#ovSub{z-index:34;}
.phead{display:flex;align-items:center;gap:8px;padding:calc(env(safe-area-inset-top) + 10px) 14px 10px;}
.phead .grow{flex:1;}
.ibtn{border:0;background:var(--card);border:1px solid var(--line);color:var(--fg);
  font-family:'Courier Prime',monospace;font-size:.85rem;
  padding:9px 13px;border-radius:11px;cursor:pointer;}
.ibtn.on{background:var(--accent);border-color:var(--accent);color:#fff;}
.pbody{flex:1;overflow-y:auto;-webkit-overflow-scrolling:touch;padding:0 16px 26px;}
.hero{position:relative;height:340px;border-radius:var(--r);overflow:hidden;background:#eae4d9;margin-bottom:14px;}
.hero img{width:100%;height:100%;object-fit:cover;display:block;}
.hero .g2{position:absolute;inset:0;background:linear-gradient(to top,rgba(0,0,0,.72),rgba(0,0,0,0) 54%);}
.hero .hm{position:absolute;left:18px;right:18px;bottom:14px;color:#fff;}
.hero .hn{font-family:Caveat,cursive;font-size:2.1rem;font-weight:600;outline:none;}
.hero .hs{font-size:.8rem;opacity:.85;margin-top:2px;}
.hero .imgbtn{position:absolute;top:12px;right:12px;width:36px;height:36px;border-radius:50%;border:0;
  background:rgba(0,0,0,.45);color:#fff;display:flex;align-items:center;justify-content:center;cursor:pointer;}
.hero .imgbtn svg{width:17px;height:17px;}
.boxes{display:grid;grid-template-columns:1fr 1fr 1fr;gap:9px;margin-bottom:16px;}
.box{position:relative;background:var(--card);border:1px solid var(--line);border-radius:14px;padding:11px 5px;
  text-align:center;cursor:pointer;}
.box .pen{position:absolute;top:4px;right:6px;font-size:.68rem;color:var(--muted);}
.box .bl{font-family:'Courier Prime',monospace;font-size:.66rem;color:var(--muted);line-height:1.25;}
.box .bv{font-family:'Courier Prime',monospace;font-size:1.2rem;margin-top:3px;word-break:break-word;}

/* ---------- Kapitel-Knöpfe ---------- */
.catlist{display:flex;flex-direction:column;gap:10px;margin-bottom:16px;}
.catbtn{display:flex;align-items:center;gap:10px;background:var(--card);border:1px solid var(--line);
  border-radius:14px;padding:16px;cursor:pointer;box-shadow:0 3px 10px rgba(0,0,0,.05);
  font-family:'Courier Prime',monospace;font-size:.95rem;color:var(--fg);}
.catbtn .cnt{margin-left:auto;font-size:.75rem;color:var(--muted);}
.catbtn .chev{color:var(--muted);}
.catbtn .csub{font-size:.75rem;color:var(--muted);margin-top:3px;line-height:1.3;}

/* ---------- Aufklappbare Balken ---------- */
.bar{background:var(--card);border:1px solid var(--line);border-radius:14px;margin-bottom:10px;overflow:hidden;
  box-shadow:0 3px 10px rgba(0,0,0,.05);}
.barhead{display:flex;align-items:center;gap:10px;padding:16px;cursor:pointer;
  font-family:'Courier Prime',monospace;font-size:.95rem;}
.barhead .cnt{margin-left:auto;font-size:.75rem;color:var(--muted);}
.barhead .del{color:var(--accent);cursor:pointer;padding:0 4px;font-size:.8rem;}
.barhead .chev{color:var(--muted);transition:transform .22s;}
.bar.open .barhead .chev{transform:rotate(90deg);}
.barbody{max-height:0;overflow:hidden;transition:max-height .3s ease;}
.bar.open .barbody{max-height:9000px;}
.bar.flat .barbody{max-height:none;}
.bar.flat .barbody>.prow:first-child{border-top:0;}
.prow{border-top:1px solid var(--line);padding:12px 16px;}
.prow .pl{display:flex;justify-content:space-between;gap:10px;font-family:'Courier Prime',monospace;
  font-size:.72rem;color:var(--muted);margin-bottom:3px;}
.prow .pl .del{color:var(--accent);cursor:pointer;padding:0 3px;}
.prow .pv{font-size:.98rem;line-height:1.4;cursor:pointer;white-space:pre-wrap;word-break:break-word;}
.prow .pv.empty{color:var(--muted);}
.addrow{border-top:1px solid var(--line);padding:12px 16px;text-align:center;cursor:pointer;
  font-family:'Courier Prime',monospace;font-size:.82rem;color:var(--muted);}
.subhint{font-family:'Courier Prime',monospace;font-size:.75rem;color:var(--muted);padding:2px 2px 12px;}

.story{width:100%;height:46px;border-radius:14px;border:0;background:var(--accent);color:#fff;
  font-family:'Courier Prime',monospace;font-size:1rem;cursor:pointer;margin-bottom:14px;}
.delrow{display:flex;justify-content:center;}
.delrow button{border:0;background:transparent;color:var(--accent);
  font-family:'Courier Prime',monospace;font-size:.88rem;padding:10px;}

/* ---------- Einstellungen ---------- */
.setsec{padding:16px 2px 6px;font-family:'Courier Prime',monospace;font-size:.74rem;
  letter-spacing:.06em;color:var(--muted);}
.setrow{display:flex;justify-content:space-between;align-items:center;gap:12px;padding:14px 2px;
  border-bottom:1px solid var(--line);cursor:pointer;font-family:'Courier Prime',monospace;font-size:.92rem;}
.setrow .rv{color:var(--muted);font-size:.82rem;text-align:right;max-width:56%;overflow:hidden;
  text-overflow:ellipsis;white-space:nowrap;}
.setrow .rv.wrap{white-space:normal;line-height:1.35;}
.hint{font-size:.8rem;color:var(--muted);line-height:1.5;padding:10px 2px 0;}

/* ---------- Blätter ---------- */
.sheet{position:absolute;inset:0;z-index:60;display:none;align-items:flex-end;background:rgba(20,17,15,.34);}
.sheet.on{display:flex;}
.sheetin{width:100%;max-height:88%;background:var(--bg);border-radius:16px 16px 0 0;display:flex;
  flex-direction:column;padding-bottom:env(safe-area-inset-bottom);overflow:hidden;}
.sheetin.tall{height:88%;}
.shhead{display:flex;align-items:center;justify-content:space-between;gap:10px;padding:14px 16px;
  border-bottom:1px solid var(--line);}
.shtitle{font-family:'Special Elite',cursive;font-size:1.02rem;}
.shclose{border:0;background:transparent;font-size:1.2rem;color:var(--muted);cursor:pointer;padding:4px 8px;}
.shbody{overflow-y:auto;-webkit-overflow-scrolling:touch;padding:14px 16px;}
.opt{display:flex;align-items:center;justify-content:space-between;gap:10px;padding:14px 2px;
  border-bottom:1px solid var(--line);cursor:pointer;font-family:'Courier Prime',monospace;font-size:.92rem;}
.opt .sub2{color:var(--muted);font-size:.78rem;}
.shrow{display:flex;gap:10px;padding:12px 16px;border-top:1px solid var(--line);}
.shrow button{flex:1;height:44px;border-radius:12px;border:1px solid var(--line);background:var(--card);
  font-family:'Courier Prime',monospace;font-size:.92rem;color:var(--fg);cursor:pointer;}
.shrow button.pri{background:var(--accent);border-color:var(--accent);color:#fff;}
.shbody input,.shbody textarea{width:100%;border:1px solid var(--line);border-radius:11px;padding:12px;font:inherit;
  font-size:1rem;background:var(--card);outline:none;color:var(--fg);}
.shbody textarea{min-height:150px;line-height:1.5;resize:vertical;}
.qtext{font-size:.95rem;line-height:1.5;}
#shAsk{z-index:70;}

/* ---------- Chat ---------- */
.chatlog{flex:1;overflow-y:auto;-webkit-overflow-scrolling:touch;padding:14px;display:flex;
  flex-direction:column;gap:10px;}
.msg{max-width:84%;padding:10px 13px;border-radius:14px;font-size:.93rem;line-height:1.45;white-space:pre-wrap;
  word-break:break-word;}
.msg.me{align-self:flex-end;background:var(--accent);color:#fff;border-bottom-right-radius:5px;}
.msg.ai{align-self:flex-start;background:var(--card);border:1px solid var(--line);border-bottom-left-radius:5px;}
.msg.err{align-self:stretch;max-width:100%;background:rgba(214,51,108,.08);border:1px solid rgba(214,51,108,.3);
  color:#a3204f;font-size:.82rem;}
.msg img{max-width:100%;border-radius:9px;display:block;margin-top:6px;}
.chatrow{display:flex;align-items:flex-end;gap:8px;padding:10px 12px;border-top:1px solid var(--line);}
.chatrow textarea{flex:1;border:1px solid var(--line);border-radius:13px;background:var(--card);padding:10px 12px;
  font:inherit;font-size:.95rem;resize:none;max-height:110px;outline:none;color:var(--fg);}
.cbtn{width:42px;height:42px;flex:0 0 42px;border-radius:50%;border:0;background:var(--card);border:1px solid var(--line);
  display:flex;align-items:center;justify-content:center;color:var(--fg);cursor:pointer;}
.cbtn.send{background:var(--accent);border-color:var(--accent);color:#fff;}
.cbtn svg{width:19px;height:19px;}

/* ---------- Galerie / Papierkorb ---------- */
.galgrid{display:grid;grid-template-columns:1fr 1fr 1fr;gap:8px;}
.gcell{position:relative;aspect-ratio:1;border-radius:9px;overflow:hidden;background:#eae4d9;cursor:pointer;}
.gcell img{width:100%;height:100%;object-fit:cover;display:block;}
.gcell .x{position:absolute;top:4px;right:4px;width:22px;height:22px;border-radius:7px;background:rgba(0,0,0,.5);
  color:#fff;font-size:.75rem;display:flex;align-items:center;justify-content:center;}
.cropbox{position:relative;height:240px;border-radius:12px;overflow:hidden;background:#eae4d9;margin-bottom:12px;}
.cropbox img{width:100%;height:100%;object-fit:cover;display:block;}
.croprow{display:flex;align-items:center;gap:10px;margin-bottom:8px;font-size:.82rem;color:var(--muted);}
.croprow input[type=range]{flex:1;}

/* ---------- Buchseite ---------- */
.book{position:absolute;inset:0;z-index:100;background:#f1e9da;transition:opacity .52s ease;
  display:flex;align-items:center;justify-content:center;
  padding:calc(env(safe-area-inset-top) + 26px) 26px calc(env(safe-area-inset-bottom) + 26px);}
.book.hide{opacity:0;pointer-events:none;}
.book::before{content:'';position:absolute;inset:0;z-index:2;pointer-events:none;
  background:linear-gradient(to right,rgba(96,72,38,.12),rgba(0,0,0,0) 11%),
             radial-gradient(120% 85% at 50% 42%,rgba(0,0,0,0) 55%,rgba(96,72,38,.10) 100%);}
.book::after{content:'';position:absolute;inset:0;z-index:2;pointer-events:none;opacity:.28;mix-blend-mode:multiply;
  background-image:url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='170' height='170'%3E%3Cfilter id='n'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.85' numOctaves='4' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='170' height='170' filter='url(%23n)'/%3E%3C/svg%3E");}
.bpage{position:relative;z-index:1;width:100%;max-width:380px;color:#2b2620;
  font-family:Georgia,"Times New Roman",serif;}
.bpage p{font-size:1.02rem;line-height:1.75;text-align:justify;-webkit-hyphens:auto;hyphens:auto;margin:0 0 1em;}
.pwf{border:0;background:transparent;border-bottom:1px solid rgba(43,38,32,.45);font:inherit;font-size:1.02rem;
  color:#2b2620;outline:none;caret-color:transparent;text-align:left;padding:0 1px 1px;min-width:60px;}
.bpno{position:absolute;left:0;right:0;bottom:-4px;text-align:center;font-size:.8rem;color:rgba(43,38,32,.5);}
.bnote{position:absolute;right:-14px;top:44%;transform:rotate(90deg);transform-origin:right center;
  font-family:Caveat,cursive;font-size:.95rem;color:#a3204f;opacity:0;transition:opacity .25s;white-space:nowrap;}
.bnote.on{opacity:1;}

/* ---------- Toast ---------- */
.toast{position:absolute;left:50%;bottom:calc(var(--navh) + env(safe-area-inset-bottom) + 78px);z-index:120;
  transform:translateX(-50%) translateY(10px);background:rgba(27,25,23,.92);color:#fff;font-size:.85rem;
  padding:10px 15px;border-radius:11px;opacity:0;pointer-events:none;transition:opacity .2s,transform .2s;
  max-width:82%;text-align:center;}
.toast.on{opacity:1;transform:translateX(-50%) translateY(0);}
</style>
</head>
<body>
<div class="app" id="app">

  <!-- ============ Startseite ============ -->
  <div class="scroller" id="home">
    <div class="head">
      <div>
        <div class="ttl">Steckbriefe</div>
        <div class="sub" id="cnt"></div>
      </div>
      <button class="lockbtn" id="lockBtn" title="Ausloggen">
        <svg viewBox="0 0 24 24" fill="none"><path d="M12 3v9" stroke="currentColor" stroke-width="1.8" stroke-linecap="round"/><path d="M6.5 6.6a8 8 0 1 0 11 0" stroke="currentColor" stroke-width="1.8" stroke-linecap="round"/></svg>
      </button>
    </div>
    <div class="search">
      <svg viewBox="0 0 24 24" fill="none"><circle cx="11" cy="11" r="6.4" stroke="currentColor" stroke-width="1.8"/><path d="M16 16l4.4 4.4" stroke="currentColor" stroke-width="1.8" stroke-linecap="round"/></svg>
      <input id="q" placeholder="Suchen" autocapitalize="none" autocorrect="off" spellcheck="false">
    </div>
    <div class="grid" id="grid"></div>
    <div class="reslist" id="reslist"></div>
  </div>

  <!-- ============ Steckbrief ============ -->
  <div class="overlay" id="ovProf">
    <div class="phead">
      <button class="ibtn" id="pBack">Zurück</button>
      <div class="grow"></div>
      <button class="ibtn" id="pEdit">Bearbeiten</button>
    </div>
    <div class="pbody" id="pbody">
      <div class="hero">
        <img id="pHero" alt="">
        <div class="g2"></div>
        <button class="imgbtn" id="pImg">
          <svg viewBox="0 0 24 24" fill="none"><path d="M4 20h16" stroke="currentColor" stroke-width="1.8" stroke-linecap="round"/><path d="M12 15V4M12 4L7.5 8.5M12 4l4.5 4.5" stroke="currentColor" stroke-width="1.8" stroke-linecap="round"/></svg>
        </button>
        <div class="hm"><div class="hn" id="pName"></div><div class="hs" id="pSub"></div></div>
      </div>
      <div class="boxes" id="boxes"></div>
      <div class="catlist" id="catlist"></div>
      <button class="story" id="storyBtn">Storys</button>
      <div class="delrow"><button id="pDel">In den Papierkorb</button></div>
    </div>
  </div>

  <!-- ============ Kapitel ============ -->
  <div class="overlay" id="ovSub">
    <div class="phead">
      <button class="ibtn" id="uBack">Zurück</button>
      <div class="grow"></div>
      <button class="ibtn" id="uEdit">Bearbeiten</button>
    </div>
    <div class="pbody">
      <div class="shtitle" id="uTtl" style="padding:6px 2px 12px"></div>
      <div id="uBody"></div>
    </div>
  </div>

  <!-- ============ Einstellungen ============ -->
  <div class="overlay" id="ovSet">
    <div class="phead">
      <button class="ibtn" id="sBack">Zurück</button>
      <div class="grow"></div>
    </div>
    <div class="pbody"><div class="shtitle" style="padding:6px 2px 4px">Einstellungen</div><div id="sBody"></div></div>
  </div>

  <!-- ============ KI-Blase ============ -->
  <button class="fab" id="fab">
    <svg viewBox="0 0 24 24" fill="none"><path d="M12 3l1.9 5.1L19 10l-5.1 1.9L12 17l-1.9-5.1L5 10l5.1-1.9L12 3z" fill="currentColor"/></svg>
  </button>

  <!-- ============ Leiste ============ -->
  <nav class="nav" id="nav">
    <button id="nBack" title="Zurück">
      <span class="ic"><svg viewBox="0 0 24 24" fill="none"><path d="M15 5l-7 7 7 7" stroke="currentColor" stroke-width="1.9" stroke-linecap="round" stroke-linejoin="round"/></svg></span></button>
    <button id="nHome" class="on" title="Start">
      <span class="ic"><svg viewBox="0 0 24 24" fill="none"><path d="M4 11l8-6.5 8 6.5" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round"/><path d="M6.5 10.5V19h11v-8.5" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round"/></svg></span></button>
    <button id="nSet" title="Einstellungen">
      <span class="ic"><svg viewBox="0 0 24 24" fill="none"><circle cx="12" cy="12" r="3.1" stroke="currentColor" stroke-width="1.8"/><path d="M12 3.6v2.2M12 18.2v2.2M4.6 12h2.2M17.2 12h2.2M6.8 6.8l1.6 1.6M15.6 15.6l1.6 1.6M17.2 6.8l-1.6 1.6M8.4 15.6l-1.6 1.6" stroke="currentColor" stroke-width="1.6" stroke-linecap="round"/></svg></span></button>
  </nav>

  <!-- ============ Chat ============ -->
  <div class="sheet" id="shChat">
    <div class="sheetin tall">
      <div class="shhead"><div class="shtitle" id="chatTitle">KI</div>
        <div style="display:flex;gap:4px"><button class="shclose" id="chatSet">⚙</button><button class="shclose" id="chatClose">×</button></div>
      </div>
      <div class="chatlog" id="chatlog"></div>
      <div class="chatrow">
        <button class="cbtn" id="chatImg" title="Bild erzeugen">
          <svg viewBox="0 0 24 24" fill="none"><rect x="3.5" y="5" width="17" height="14" rx="3" stroke="currentColor" stroke-width="1.7"/><circle cx="9" cy="10.4" r="1.6" fill="currentColor"/><path d="M4.5 17l4.6-4.3 3.2 3 2.6-2.2 4.6 4" stroke="currentColor" stroke-width="1.7" stroke-linecap="round" stroke-linejoin="round"/></svg>
        </button>
        <textarea id="chatIn" rows="1" placeholder="Schreiben…"></textarea>
        <button class="cbtn send" id="chatSend">
          <svg viewBox="0 0 24 24" fill="none"><path d="M4 12l16-7-7 16-2.2-6.8L4 12z" stroke="currentColor" stroke-width="1.7" stroke-linejoin="round"/></svg>
        </button>
      </div>
    </div>
  </div>

  <!-- ============ Bildmenü ============ -->
  <div class="sheet" id="shImg">
    <div class="sheetin">
      <div class="shhead"><div class="shtitle">Bild</div><button class="shclose" id="imgClose">×</button></div>
      <div class="shbody" id="imgBody"></div>
    </div>
  </div>

  <!-- ============ Feld wählen ============ -->
  <div class="sheet" id="shPick">
    <div class="sheetin">
      <div class="shhead"><div class="shtitle">Feld wählen</div><button class="shclose" id="pickClose">×</button></div>
      <div class="shbody" id="pickBody"></div>
    </div>
  </div>

  <!-- ============ Eigenes Frage-Fenster ============ -->
  <div class="sheet" id="shAsk">
    <div class="sheetin">
      <div class="shhead"><div class="shtitle" id="askTitle"></div><button class="shclose" id="askClose">×</button></div>
      <div class="shbody" id="askBody"></div>
      <div class="shrow"><button id="askNo">Abbrechen</button><button class="pri" id="askYes">Übernehmen</button></div>
    </div>
  </div>

  <!-- ============ Zahlenrad ============ -->
  <div class="sheet" id="shWheel">
    <div class="sheetin">
      <div class="shhead"><div class="shtitle" id="wTtl"></div><button class="shclose" id="wClose">×</button></div>
      <div class="shbody" style="padding:0"><div id="wList" style="height:190px;overflow-y:scroll;scroll-snap-type:y mandatory;text-align:center"></div></div>
      <div class="shrow"><button id="wCancel">Abbrechen</button><button class="pri" id="wOk">Übernehmen</button></div>
    </div>
  </div>

  <!-- ============ Galerie ============ -->
  <div class="sheet" id="shGal">
    <div class="sheetin tall">
      <div class="shhead"><div class="shtitle" id="galTitle">Galerie</div><button class="shclose" id="galClose">×</button></div>
      <div class="shbody"><div class="galgrid" id="galGrid"></div>
        <div class="addrow"><button id="galAdd">Bild hinzufügen</button></div></div>
    </div>
  </div>

  <!-- ============ Papierkorb ============ -->
  <div class="sheet" id="shTrash">
    <div class="sheetin tall">
      <div class="shhead"><div class="shtitle">Papierkorb</div><button class="shclose" id="trashClose">×</button></div>
      <div class="shbody" id="trashBody"></div>
    </div>
  </div>

  <!-- ============ Zuschnitt ============ -->
  <div class="sheet" id="shCrop">
    <div class="sheetin">
      <div class="shhead"><div class="shtitle">Ausschnitt</div><button class="shclose" id="cropClose">×</button></div>
      <div class="shbody">
        <div class="cropbox"><img id="cropImg" alt=""></div>
        <div class="croprow"><span>links/rechts</span><input type="range" id="cropX" min="0" max="100" value="50"></div>
        <div class="croprow"><span>hoch/runter</span><input type="range" id="cropY" min="0" max="100" value="50"></div>
        <div class="croprow"><span>Grösse</span><input type="range" id="cropZ" min="100" max="260" value="100"></div>
      </div>
      <div class="shrow"><button id="cropCancel">Abbrechen</button><button class="pri" id="cropOk">Übernehmen</button></div>
    </div>
  </div>

  <!-- ============ Buchseite (Passwort) ============ -->
  <div class="book" id="book">
    <div class="bpage">
      <p>Der Nachlass lag seit Jahren im Regal, ohne dass jemand danach griff. Auf dem Umschlag stand kein Titel, nur eine Nummer.</p>
      <p>Darin lagen Karten, sauber mit einem Gummiband gebündelt. Auf jeder Karte eine Frau, ihr Name, ihr Alter, was sie mochte und was nicht.</p>
      <p>Nur das <input id="pw" class="pwf" type="password" autocomplete="off" autocapitalize="none" autocorrect="off" spellcheck="false"> fehlte. Es stand auf keinem Zettel und in keinem der Bücher daneben.</p>
      <p>Wer es kannte, durfte die Karten lesen.</p>
      <div class="bpno">13</div>
      <div class="bnote" id="bnote">nicht das rechte wort</div>
    </div>
  </div>

  <div class="toast" id="toast"></div>
  <input type="file" id="pick" accept="image/*" style="display:none">
  <input type="file" id="pickJson" accept=".json,application/json" style="display:none">
</div>

<script>
/* ===================== Konstanten ===================== */
var API='https://api.venice.ai/api/v1';
var PASS='Lennox84!';
var K_UNLOCK='sb_unlocked_v1';
var K_PEOPLE='sb_people_v1';
var K_IMG='sb_img_v1';
var K_SET='sb_set_v1';
var K_TRASH='sb_trash_v1';
var GRUND_VORGABE='Beschreibe Körper, Kleidung, sexuelle Erfahrungen, Vorlieben und Ähnliches — sowohl in Bezug auf die Steckbriefe als auch beim Verfassen der Storys — grundsätzlich in einer detaillierten, anschaulichen, lebhaften, grafischen, plastischen und expliziten Art und Weise.';
var PH='data:image/svg+xml;charset=utf-8,'+encodeURIComponent('<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 100 100"><rect width="100" height="100" fill="#eae4d9"/><circle cx="50" cy="36" r="14" fill="#b9b0a2"/><path d="M24 92 C24 70 35 60 50 60 C65 60 76 70 76 92 Z" fill="#b9b0a2"/></svg>');

/* ===================== Steckbrief-Aufbau =====================
   Kapitel -> Abschnitte -> Felder
   Feld: [key, Name, art, von, bis]   art: text | long | num
   rep  = Abschnitt mit beliebig vielen Einträgen (fortlaufend nummeriert)
   flat = Felder stehen direkt auf der Kapitelseite, ohne eigenen Balken
=============================================================== */
var CH=[
 {n:'Kapitel 1',u:'Person & Erscheinung',s:[
  {id:'s11',n:'1.1 Allgemeine Informationen',f:[
    ['name','Name','text'],['alter','Alter','num',14,80],['wohnort','Wohnort','text'],
    ['wohnsituation','Wohnsituation','long'],['beruf','Beruf','long'],
    ['geschwister','Geschwister','long'],['beziehungsstatus','Beziehungsstatus','long'],
    ['hobbys','Hobbys und Interessen','long'],['freundeskreis','Freundeskreis','long']]},
  {id:'s12',n:'1.2 Charaktereigenschaften',rep:'Charaktereigenschaft',f:[
    ['text','Charaktereigenschaft','long']]}
 ]},
 {n:'Kapitel 2',u:'Aussehen des nackten Körpers',s:[
  {id:'s21',flat:1,n:'Nackter Körper',f:[
    ['haare','Haare','text'],['koerperbehaarung','Körperbehaarung','text'],
    ['koerbchen','Körbchengröße','text'],['brueste','Beschreibung der Brüste','long'],
    ['arsch','Arsch und Arschloch','long'],['muschi','Muschi','long'],
    ['groesse','Größe','num',130,210],['figur','Figur','long'],
    ['tattoos','Tattoos','long'],['piercings','Piercings','long']]}
 ]},
 {n:'Kapitel 3',u:'Ihre Kleidungsstücke',s:[
  {id:'s31',flat:1,n:'Kleidungsstücke',f:[
    ['oberteil','Oberteil','long'],['beinbekleidung','Beinbekleidung','long'],
    ['unterhose','Unterhose','long'],['bh','BH','long'],['bikini','Bikini','long'],
    ['kleid','Kleid','long'],['schlafanzug','Schlafanzug','long']]}
 ]},
 {n:'Kapitel 4',u:'Sexuelle Erfahrungen',s:[
  {id:'s411',n:'4.1.1 Anzahl Männer, von denen sie gefickt wurde',f:[
    ['anzahl','Anzahl','num',0,500],
    ['juengster','Jüngster Mann, von dem sie gefickt wurde','text'],
    ['aeltester','Ältester Mann, von dem sie gefickt wurde','text'],
    ['groesster','Größter Schwanz, von dem sie bisher gefickt wurde','text'],
    ['dauer','Längste Dauer, die sie am Stück gefickt wurde','text'],
    ['mund','Maximal gleichzeitig in den Mund gefickt (Anzahl Schwänze)','num',1,10],
    ['arsch','Maximal gleichzeitig in den Arsch gefickt (Anzahl Schwänze)','num',1,10],
    ['muschi','Maximal gleichzeitig in die Muschi gefickt (Anzahl Schwänze)','num',1,10],
    ['max','Höchste Anzahl an Schwänzen gleichzeitig in ihr bzw. gleichzeitig befriedigt','num',1,10]]},
  {id:'s412',n:'4.1.2 Anzahl Männer, denen sie nur den Schwanz geblasen hat',f:[
    ['anzahl','Anzahl','num',0,500],
    ['juengster','Jüngster Mann, dem sie nur den Schwanz geblasen hat','text'],
    ['aeltester','Ältester Mann, dem sie nur den Schwanz geblasen hat','text'],
    ['groesster','Größter Schwanz, den sie nur geblasen hat','text'],
    ['dauer','Längste Dauer, die sie am Stück geblasen hat','text']]},
  {id:'s413',n:'4.1.3 Anzahl Männer, denen sie nur den Schwanz gewichst hat',f:[
    ['anzahl','Anzahl','num',0,500],
    ['juengster','Jüngster Mann, dem sie nur den Schwanz gewichst hat','text'],
    ['aeltester','Ältester Mann, dem sie nur den Schwanz gewichst hat','text'],
    ['groesster','Größter Schwanz, den sie nur gewichst hat','text'],
    ['dauer','Längste Dauer, die sie am Stück gewichst hat','text']]},
  {id:'s414',n:'4.1.4 Männer, die ihr Sperma ins Gesicht oder auf den nackten Körper gespritzt haben (ohne Sex, ohne Blasen, ohne Wichsen)',f:[
    ['anzahl','Anzahl','num',0,500],
    ['gleichzeitig','Von wie vielen Männern sie maximal gleichzeitig das Sperma ins Gesicht oder auf den nackten Körper gespritzt bekam','num',0,20],
    ['zuschauer','Maximale Anzahl an Personen, die dabei zugesehen haben','num',0,200],
    ['gegenwillen','Wie oft gegen ihren Willen bzw. ohne sich wehren zu können','long'],
    ['orte','Orte, an denen ihr Männer schon auf den nackten Körper oder ins Gesicht gespritzt haben','long'],
    ['beschreibung','Beschreibungen, wie es dazu kam','long']]},
  {id:'s42',n:'4.2 Feste Beziehungen',rep:'Feste Beziehung',f:[
    ['alter','Wie alt sie zu Beginn der Beziehung war','num',14,80],
    ['name','Name des Mannes','text'],
    ['malter','Alter des Mannes zu Beginn der Beziehung','num',14,90],
    ['mgroesse','Größe des Mannes','num',130,210],
    ['schwanz','Schwanzgröße des Mannes im erigierten Zustand','num',8,30],
    ['dauer','Wie lange die Beziehung gedauert hat','text'],
    ['wie','Wie es zu der Beziehung kam, wie sie zusammengekommen sind','long'],
    ['vorher','Ob sie sich vorher schon kannten bzw. woher sie sich vorher kannten','long'],
    ['vorbeziehung','In welcher Beziehung sie vorher zueinander standen','long'],
    ['erstersex','Wann sie das erste Mal Sex hatten','text'],
    ['erstesblasen','Wann sie ihm das erste Mal den Schwanz geblasen hat','text'],
    ['erstesnackt','Wann er sie das erste Mal nackt gesehen hat','text'],
    ['betrug','Ob er sie betrogen hat, wenn ja, wie oft, wie regelmäßig und mit wem','long'],
    ['ihrbetrug','Ob sie ihn betrogen hat, wenn ja, wie oft, wie regelmäßig und mit wem','long'],
    ['gruppensex','Ob es vor oder nach der Beziehung einen Dreier, Vierer oder Sex mit noch mehr Männern gab','long'],
    ['schluss','Wer Schluss gemacht hat und warum','long'],
    ['nachsex','Ob sie nach der Trennung noch Sex hatten','long'],
    ['haeufigkeit','Wie oft sie während der Beziehung Sex hatten','text'],
    ['qualitaet','Wie gut der Sex war, auch ob er sich über ihre Grenzen hinweggesetzt und sie gegen ihren Willen gefickt hat','long'],
    ['fremdficken','Ob er sie mal gegen ihren Willen bzw. ohne ihr Wissen von einem anderen Mann hat ficken lassen','long'],
    ['wollte','Wie oft er sie ficken wollte und wie oft er sie tatsächlich gefickt hat','long'],
    ['obwohl','Wie oft sie sich von ihm hat ficken lassen, obwohl sie eigentlich nicht wollte','long'],
    ['einfach','Wie oft er sie, obwohl sie nicht wollte, einfach gefickt hat, ohne dass sie sich entziehen konnte','long'],
    ['gezwungen','Wie oft er sie dazu gedrängt oder gezwungen hat, sich ficken zu lassen','long'],
    ['blasenorte','Wie oft, wie regelmäßig und an welchen Orten sie ihm den Schwanz geblasen hat','long'],
    ['blasenobwohl','Wie oft sie ihm den Schwanz geblasen hat, obwohl sie eigentlich nicht wollte','long'],
    ['mundficken','Wie oft er sie in den Mund gefickt hat, obwohl sie nicht wollte','long']]},
  {id:'s43',n:'4.3 Freundschaft-plus-Beziehungen',rep:'Freundschaft-plus',f:[
    ['alter','Ihr Alter zu Beginn','num',14,80],
    ['name','Sein Name','text'],
    ['malter','Sein Alter zu Beginn','num',14,90],
    ['mgroesse','Seine Größe','num',130,210],
    ['schwanz','Seine Schwanzgröße im erigierten Zustand','num',8,30],
    ['dauer','Wie lange die Freundschaft-plus-Beziehung gedauert hat','text'],
    ['woher','Woher die beiden sich kannten','long'],
    ['vorbeziehung','In welcher Beziehung sie vorher zueinander standen','long'],
    ['wie','Wie es zu der Freundschaft-plus-Beziehung gekommen ist','long'],
    ['haeufigkeit','Wie regelmäßig und wie oft die beiden Sex hatten','long'],
    ['blasen','Wie oft sie ihm dabei den Schwanz geblasen hat bzw. wie oft er sie in den Mund gefickt hat','long'],
    ['orte','Wo die beiden Sex hatten','long'],
    ['sex','Wie der Sex war, alles, was sie beim Sex gemacht haben','long'],
    ['schlagen','Ob er sie beim Sex gebürstet oder geschlagen hat','long'],
    ['gefilmt','Ob er sie beim Sex gefilmt hat','long'],
    ['weitere','Ob sie während der Freundschaft-plus-Beziehung mit einem oder mehreren weiteren Männern Sex hatte','long'],
    ['beziehung','Ob sie oder der Mann zu Beginn, während oder am Ende in einer Beziehung war','long'],
    ['art','Die Art der Freundschaft-plus-Beziehung','long'],
    ['offen','Wie offen sie mit der Freundschaft-plus-Beziehung umgegangen sind','long'],
    ['wissen','Wer davon Bescheid wusste und warum diese Personen bzw. warum niemand oder alle','long'],
    ['dreier','Ob es eine Dreier-Freundschaft-plus-Beziehung war','long']]},
  {id:'s441',n:'4.4.1 Beim ersten Date von einem Mann ficken lassen',rep:'Date',f:[
    ['name','Name','text'],['alter','Alter','num',14,80],['aussehen','Aussehen','long'],
    ['kennen','Wie und wo sie diesen Mann kennengelernt hat','long'],
    ['date','Wo und wie das erste Date ablief','long'],
    ['schnell','Wie schnell es beim Date zum Sex kam','text'],
    ['ort','Wo der Sex stattfand','text'],
    ['wie','Wie es dazu kam','long'],
    ['betrunken','Ob sie dabei betrunken war','text'],
    ['sex','Wie der Sex ablief, alles, was sie dabei gemacht haben','long'],
    ['geblasen','Ob sie ihm dabei den Schwanz geblasen hat','long'],
    ['gespritzt','Ob er in sie oder auf sie gespritzt hat','text'],
    ['wiedersehen','Ob es ein Wiedersehen gab','long'],
    ['folge','Ob daraus eine Beziehung oder Freundschaft-plus wurde','long'],
    ['gefuehl','Wie sie sich danach gefühlt hat','long'],
    ['bereut','Ob sie es bereut hat','text']]},
  {id:'s442',n:'4.4.2 Am Tag des Kennenlernens von einem Mann ficken lassen',rep:'Mann',f:[
    ['name','Name','text'],['alter','Alter','num',14,80],['aussehen','Aussehen','long'],
    ['kennen','Wie und wo sie diesen Mann kennengelernt hat','long'],
    ['zeit','Wie viel Zeit zwischen Kennenlernen und Sex lag','text'],
    ['ort','Wo der Sex stattfand','text'],
    ['wie','Wie es dazu kam','long'],
    ['betrunken','Ob sie dabei betrunken war','text'],
    ['sex','Wie der Sex ablief, alles, was sie dabei gemacht haben','long'],
    ['geblasen','Ob sie ihm dabei den Schwanz geblasen hat','long'],
    ['gespritzt','Ob er in sie oder auf sie gespritzt hat','text'],
    ['wiedersehen','Ob es ein Wiedersehen gab','long'],
    ['folge','Ob daraus eine Beziehung oder Freundschaft-plus wurde','long'],
    ['gefuehl','Wie sie sich danach gefühlt hat','long'],
    ['bereut','Ob sie es bereut hat','text']]},
  {id:'s451',n:'4.5.1 Beim ersten Date einem Mann den Schwanz geblasen',rep:'Date',f:[
    ['name','Name','text'],['alter','Alter','num',14,80],['aussehen','Aussehen','long'],
    ['kennen','Wie und wo sie diesen Mann kennengelernt hat','long'],
    ['date','Wo und wie das erste Date ablief','long'],
    ['schnell','Wie schnell es beim Date dazu kam','text'],
    ['ort','Wo es passierte','text'],
    ['wie','Wie es dazu kam','long'],
    ['betrunken','Ob sie dabei betrunken war','text'],
    ['blasen','Wie sie ihm den Schwanz geblasen hat, wie lange, wie tief','long'],
    ['mundficken','Ob er ihr dabei in den Mund gefickt hat','long'],
    ['gekommen','Ob er geschluckt hat, in ihren Mund oder auf ihr Gesicht gespritzt hat','long'],
    ['wiedersehen','Ob es ein Wiedersehen gab','long'],
    ['folge','Ob daraus eine Beziehung oder Freundschaft-plus wurde','long'],
    ['gefuehl','Wie sie sich danach gefühlt hat','long'],
    ['bereut','Ob sie es bereut hat','text']]},
  {id:'s452',n:'4.5.2 Am Tag des Kennenlernens einem Mann den Schwanz geblasen',rep:'Mann',f:[
    ['name','Name','text'],['alter','Alter','num',14,80],['aussehen','Aussehen','long'],
    ['kennen','Wie und wo sie diesen Mann kennengelernt hat','long'],
    ['zeit','Wie viel Zeit zwischen Kennenlernen und Blasen lag','text'],
    ['ort','Wo es passierte','text'],
    ['wie','Wie es dazu kam','long'],
    ['betrunken','Ob sie dabei betrunken war','text'],
    ['blasen','Wie sie ihm den Schwanz geblasen hat, wie lange, wie tief','long'],
    ['mundficken','Ob er ihr dabei in den Mund gefickt hat','long'],
    ['gekommen','Ob er geschluckt hat, in ihren Mund oder auf ihr Gesicht gespritzt hat','long'],
    ['wiedersehen','Ob es ein Wiedersehen gab','long'],
    ['folge','Ob daraus eine Beziehung oder Freundschaft-plus wurde','long'],
    ['gefuehl','Wie sie sich danach gefühlt hat','long'],
    ['bereut','Ob sie es bereut hat','text']]},
  {id:'s461',n:'4.6.1 Beim ersten Date einem Mann den Schwanz gewichst',rep:'Date',f:[
    ['name','Name','text'],['alter','Alter','num',14,80],['aussehen','Aussehen','long'],
    ['kennen','Wie und wo sie diesen Mann kennengelernt hat','long'],
    ['date','Wo und wie das erste Date ablief','long'],
    ['schnell','Wie schnell es beim Date dazu kam','text'],
    ['ort','Wo es passierte','text'],
    ['wie','Wie es dazu kam','long'],
    ['betrunken','Ob sie dabei betrunken war','text'],
    ['wichsen','Wie sie ihm den Schwanz gewichst hat, wie lange, wie fest','long'],
    ['gespritzt','Ob er auf sie gespritzt hat, wohin','long'],
    ['wiedersehen','Ob es ein Wiedersehen gab','long'],
    ['folge','Ob daraus eine Beziehung oder Freundschaft-plus wurde','long'],
    ['gefuehl','Wie sie sich danach gefühlt hat','long'],
    ['bereut','Ob sie es bereut hat','text']]},
  {id:'s462',n:'4.6.2 Am Tag des Kennenlernens einem Mann den Schwanz gewichst',rep:'Mann',f:[
    ['name','Name','text'],['alter','Alter','num',14,80],['aussehen','Aussehen','long'],
    ['kennen','Wie und wo sie diesen Mann kennengelernt hat','long'],
    ['zeit','Wie viel Zeit zwischen Kennenlernen und Wichsen lag','text'],
    ['ort','Wo es passierte','text'],
    ['wie','Wie es dazu kam','long'],
    ['betrunken','Ob sie dabei betrunken war','text'],
    ['wichsen','Wie sie ihm den Schwanz gewichst hat, wie lange, wie fest','long'],
    ['gespritzt','Ob er auf sie gespritzt hat, wohin','long'],
    ['wiedersehen','Ob es ein Wiedersehen gab','long'],
    ['folge','Ob daraus eine Beziehung oder Freundschaft-plus wurde','long'],
    ['gefuehl','Wie sie sich danach gefühlt hat','long'],
    ['bereut','Ob sie es bereut hat','text']]},
  {id:'s471',n:'4.7.1 Beim ersten Date von einem Mann nackt gesehen',rep:'Date',f:[
    ['name','Name','text'],['alter','Alter','num',14,80],['aussehen','Aussehen','long'],
    ['kennen','Wie und wo sie diesen Mann kennengelernt hat','long'],
    ['date','Wo und wie das erste Date ablief','long'],
    ['schnell','Wie schnell es beim Date dazu kam','text'],
    ['ort','Wo es passierte','text'],
    ['wie','Wie es dazu kam','long'],
    ['betrunken','Ob sie dabei betrunken war','text'],
    ['nackt','Ob sie ganz nackt oder nur teilweise nackt war','long'],
    ['angefasst','Ob er sie dabei angefasst oder gefickt hat','long'],
    ['wiedersehen','Ob es ein Wiedersehen gab','long'],
    ['folge','Ob daraus eine Beziehung oder Freundschaft-plus wurde','long'],
    ['gefuehl','Wie sie sich danach gefühlt hat','long'],
    ['bereut','Ob sie es bereut hat','text']]},
  {id:'s472',n:'4.7.2 Am Tag des Kennenlernens von einem Mann nackt gesehen',rep:'Mann',f:[
    ['name','Name','text'],['alter','Alter','num',14,80],['aussehen','Aussehen','long'],
    ['kennen','Wie und wo sie diesen Mann kennengelernt hat','long'],
    ['zeit','Wie viel Zeit zwischen Kennenlernen und Ausziehen lag','text'],
    ['ort','Wo es passierte','text'],
    ['wie','Wie es dazu kam','long'],
    ['betrunken','Ob sie dabei betrunken war','text'],
    ['nackt','Ob sie ganz nackt oder nur teilweise nackt war','long'],
    ['angefasst','Ob er sie dabei angefasst oder gefickt hat','long'],
    ['wiedersehen','Ob es ein Wiedersehen gab','long'],
    ['folge','Ob daraus eine Beziehung oder Freundschaft-plus wurde','long'],
    ['gefuehl','Wie sie sich danach gefühlt hat','long'],
    ['bereut','Ob sie es bereut hat','text']]},
  {id:'s48',n:'4.8 Mit mutwilligem Vorsatz über ihre Belastungsgrenze hinaus gefickt',rep:'Mann',f:[
    ['name','Name','text'],['alter','Alter','num',14,80],['aussehen','Aussehen','long'],
    ['bestueckt','Wie gut er bestückt war','long'],
    ['woher','Woher sie ihn kannte, in welcher Beziehung sie zueinander standen','long'],
    ['gelegenheit','Bei welcher Gelegenheit es dazu kam','long'],
    ['wie','Wie es dazu kam','long'],
    ['was','Was sie genau gemacht haben, dass es über ihre Belastungsgrenze ging','long'],
    ['grenze','Woran man merkte, dass sie nicht mehr konnte (schrie, weinte, flehte, zitterte)','long'],
    ['betteln','Ob sie ihn anbettelte aufzuhören','text'],
    ['weiter','Ob er trotzdem weitermachte','text'],
    ['tabu','Ob es ein Tabu war, das er absichtlich brach','long'],
    ['schmerz','Ob er es bewusst tat, um ihr Schmerzen zuzufügen','long'],
    ['dauer','Wie lange es über ihre Grenze hinaus ging','text'],
    ['kam','Ob sie dabei kam','text'],
    ['gespritzt','Ob er in sie oder auf sie gespritzt hat','text'],
    ['gefuehl','Wie sie sich danach fühlte','long'],
    ['bereut','Ob sie es bereute','text'],
    ['oefter','Ob es öfter mit demselben Mann passierte','long'],
    ['sehnsucht','Ob sie sich danach nach mehr davon sehnte','long']]},
  {id:'s491',n:'4.9.1 Erster Sex',f:[
    ['alter','Wie alt sie damals war','num',14,40],
    ['name','Name','text'],['malter','Sein damaliges Alter','num',14,90],
    ['groesse','Seine Größe','num',130,210],['schwanz','Seine Schwanzgröße','num',8,30],
    ['woher','Woher sie ihn kannte, in welcher Beziehung sie zueinander standen','long'],
    ['ort','Wo es passierte','text'],
    ['dauer','Wie lange es dauerte','text'],
    ['verlauf','Wie es dazu kam und wie der Sex verlief, inklusive Beschreibung dessen, was gemacht wurde und wie es war','long'],
    ['danach','Ob die beiden danach nochmal Sex hatten, wenn ja, wie oft und über welchen Zeitraum','long'],
    ['beziehung','Wie die Beziehung danach war und ob sie sich verändert hat','long'],
    ['wissen','Wer davon erfahren hat, wem sie oder er davon erzählt hat','long']]},
  {id:'s492',n:'4.9.2 Erstes Mal einem Mann den Schwanz geblasen',f:[
    ['alter','Wie alt sie damals war','num',14,40],
    ['name','Name','text'],['malter','Sein damaliges Alter','num',14,90],
    ['groesse','Seine Größe','num',130,210],['schwanz','Seine Schwanzgröße','num',8,30],
    ['woher','Woher sie ihn kannte, in welcher Beziehung sie zueinander standen','long'],
    ['ort','Wo es passierte','text'],
    ['dauer','Wie lange es dauerte','text'],
    ['verlauf','Wie es dazu kam und wie das Blasen verlief','long'],
    ['gekommen','Ob er gekommen ist und wohin, ob geschluckt, in den Mund, ins Gesicht oder auf den Körper','long'],
    ['danach','Ob sie danach nochmal mit ihm zu tun hatte, wenn ja, wie oft und über welchen Zeitraum','long'],
    ['beziehung','Wie die Beziehung danach war und ob sie sich verändert hat','long'],
    ['wissen','Wer davon erfahren hat, wem sie oder er davon erzählt hat','long']]},
  {id:'s493',n:'4.9.3 Erstes Mal einem Mann den Schwanz gewichst',f:[
    ['alter','Wie alt sie damals war','num',14,40],
    ['name','Name','text'],['malter','Sein damaliges Alter','num',14,90],
    ['groesse','Seine Größe','num',130,210],['schwanz','Seine Schwanzgröße','num',8,30],
    ['woher','Woher sie ihn kannte, in welcher Beziehung sie zueinander standen','long'],
    ['ort','Wo es passierte','text'],
    ['dauer','Wie lange es dauerte','text'],
    ['verlauf','Wie es dazu kam und wie das Wichsen verlief','long'],
    ['gekommen','Ob er gekommen ist und wohin, ob in ihre Hand, auf sie, ins Gesicht oder auf den Körper','long'],
    ['danach','Ob sie danach nochmal mit ihm zu tun hatte, wenn ja, wie oft und über welchen Zeitraum','long'],
    ['beziehung','Wie die Beziehung danach war und ob sie sich verändert hat','long'],
    ['wissen','Wer davon erfahren hat, wem sie oder er davon erzählt hat','long']]},
  {id:'s494',n:'4.9.4 Erstes Mal Sperma ins Gesicht oder auf den nackten Körper gespritzt bekommen',f:[
    ['alter','Wie alt sie damals war','num',14,40],
    ['name','Name(n)','text'],['malter','Damaliges Alter','num',14,90],
    ['groesse','Größe','num',130,210],['schwanz','Schwanzgröße','num',8,30],
    ['woher','Woher sie ihn oder sie kannte, in welcher Beziehung sie zueinander standen','long'],
    ['ort','Wo es passierte','text'],
    ['dauer','Wie lange es dauerte','text'],
    ['verlauf','Wie es dazu kam und wie es verlief','long'],
    ['position','Ob sie dabei nackt war und wie sie sich positioniert hat','long'],
    ['wohin','Wohin gespritzt wurde (Gesicht, Brüste, Bauch, sonstiger Körper)','long'],
    ['danach','Ob sie danach nochmal mit ihm oder ihnen zu tun hatte, wenn ja, wie oft und über welchen Zeitraum','long'],
    ['beziehung','Wie die Beziehung danach war und ob sie sich verändert hat','long'],
    ['wissen','Wer davon erfahren hat, wem sie oder er davon erzählt hat','long']]},
  {id:'s495',n:'4.9.5 Erster Dreier',f:[
    ['alter','Ihr Alter','num',14,80],
    ['malter','Alter der beiden Männer','text'],
    ['beziehung','Beziehung, in der sie zu diesen Männern jeweils stand','long']]},
  {id:'s496',n:'4.9.6 Erster Vierer',f:[
    ['alter','Ihr Alter','num',14,80],
    ['malter','Alter der drei Männer','text'],
    ['beziehung','Beziehung, in der sie zu diesen Männern jeweils stand','long']]},
  {id:'s497',n:'4.9.7 Erstes Mal Sex mit mehr als drei Männern',f:[
    ['alter','Ihr Alter','num',14,80],
    ['malter','Alter der Männer, die dabei waren','text'],
    ['beziehung','Beziehung, in der sie zu diesen Männern jeweils stand','long']]},
  {id:'s410',n:'4.10 Sexrekorde',f:[
    ['ficktag','Wie oft sie maximal an einem Tag gefickt wurde','text'],
    ['blastag','Wie oft sie maximal an einem Tag Schwänze geblasen hat','text'],
    ['maennerfick','Maximale Anzahl Männer, von denen sie an einem Tag gefickt wurde','num',0,50],
    ['maennerblas','Maximale Anzahl an Männern, denen sie an einem Tag den Schwanz geblasen hat','num',0,50],
    ['wochefick','Maximale Anzahl verschiedener Männer, von denen sie innerhalb einer Woche gefickt wurde','num',0,50],
    ['wocheblas','Maximale Anzahl an Männern, denen sie innerhalb einer Woche den Schwanz geblasen hat','num',0,50],
    ['gesehen','Größter harter Schwanz, den sie jemals nackt gesehen hat','text'],
    ['geblasen','Größter nackter Schwanz, den sie jemals geblasen hat','text'],
    ['gefickt','Größter nackter Schwanz, von dem sie jemals gefickt wurde','text']]},
  {id:'s4111',n:'4.11.1 Dreier — Anzahl und Konstellation',f:[
    ['konstellationen','Anzahl der verschiedenen Konstellationen, in denen sie Dreier hatte','num',0,30],
    ['anzahl','Anzahl an Dreiern — Konstellationen mit Anzahl, Beziehung der Männer zu ihr und zueinander und Zeitraum','long'],
    ['alter','Alter beim ersten Dreier','num',14,80],
    ['laengster','Längster Dreier','text']]},
  {id:'s4111b',n:'4.11.1 Dreier — Beschreibung einzelner Dreier',rep:'Dreier',f:[
    ['wann','Wann und wo','long'],
    ['alter','Wie alt sie war','num',14,80],
    ['mann1','Mann 1: Name, Alter, Größe, Schwanzgröße','long'],
    ['mann2','Mann 2: Name, Alter, Größe, Schwanzgröße','long'],
    ['bekannt','Ob sie die Männer kannte und ob die Männer sich kannten','long'],
    ['kombis','In welchen Kombinationen sie gefickt wurde und wie lange in jeder (Muschi und Arsch, Arsch und Mund, Muschi und Mund, beide zusammen in den Arsch)','long'],
    ['beschreibung','Beschreibung des Dreiers von der Idee bis zur Umsetzung — wer die Idee hatte, geplant oder spontan, ob sie davon wusste oder überrascht wurde, ob sie einverstanden war, und der Dreier an sich','long']]},
  {id:'s4112',n:'4.11.2 Vierer — Anzahl und Konstellation',f:[
    ['konstellationen','Anzahl der verschiedenen Konstellationen, in denen sie Vierer hatte','num',0,30],
    ['anzahl','Anzahl an Vierern — Konstellationen mit Anzahl, Beziehung der Männer zu ihr und zueinander und Zeitraum','long'],
    ['alter','Alter beim ersten Vierer','num',14,80],
    ['laengster','Längster Vierer','text']]},
  {id:'s4112b',n:'4.11.2 Vierer — Beschreibung einzelner Vierer',rep:'Vierer',f:[
    ['wann','Wann und wo','long'],
    ['alter','Wie alt sie war','num',14,80],
    ['mann1','Mann 1: Name, Alter, Größe, Schwanzgröße','long'],
    ['mann2','Mann 2: Name, Alter, Größe, Schwanzgröße','long'],
    ['mann3','Mann 3: Name, Alter, Größe, Schwanzgröße','long'],
    ['bekannt','Ob sie die Männer kannte und ob die Männer sich kannten','long'],
    ['kombis','In welchen Kombinationen sie gefickt wurde und wie lange in jeder','long'],
    ['beschreibung','Beschreibung des Vierers von der Planung bis zur Umsetzung','long']]},
  {id:'s4113',n:'4.11.3 Gangbang',rep:'Gangbang',f:[
    ['wann','Wann und wo','long'],
    ['alter','Wie alt sie war','num',14,80],
    ['anzahl','Anzahl der Männer','num',0,100],
    ['maenner','Jeder Mann: Name, Alter, Größe, Schwanzgröße','long'],
    ['bekannt','Ob sie die Männer kannte','long'],
    ['gleichzeitig','Maximale Anzahl derer, von denen sie gleichzeitig gefickt wurde','num',1,20],
    ['stellungen','In welchen Stellungen und in welcher Reihenfolge','long'],
    ['dauer','Wie lange es insgesamt ging','text'],
    ['beschreibung','Beschreibung des Gangbangs von der Planung und Idee bis zur Umsetzung','long']]},
  {id:'s412o',n:'4.12 Orte',f:[
    ['topfick','Top 10 Orte gefickt','long'],
    ['topblas','Top 10 Orte Schwanz geblasen','long']]}
 ]},
 {n:'Kapitel 5',u:'Beziehungen',s:[
  {id:'s51',flat:1,rep:'Person',n:'Beziehungen',f:[
    ['seit','Seit wann sie sich kennen','text'],
    ['woher','Woher sie sich kennen','long'],
    ['kontakt','Auf welche Art sie Kontakt hatten','long'],
    ['kontaktjetzt','Auf welche Art sie derzeit Kontakt haben','long'],
    ['jetzt','Wie ihre Beziehung aktuell ist','long'],
    ['zwischendurch','Wie ihre Beziehung zwischendurch war','long'],
    ['denkt','Was sie über ihn denkt','long'],
    ['denkter','Was er über sie denkt','long'],
    ['weiss','Was sie von ihm weiß','long'],
    ['weisster','Was er von ihr weiß','long'],
    ['geheimnis','Ein Geheimnis, das sie vor ihm hat','long'],
    ['geheimniser','Ein Geheimnis, das er vor ihr hat','long'],
    ['gesehen','Was sie von ihm gesehen hat','long'],
    ['gesehener','Was er von ihr gesehen hat','long'],
    ['erlebt','Was sie schon miteinander erlebt haben','long']]}
 ]}
];

var SEC={};
CH.forEach(function(c){c.s.forEach(function(s){SEC[s.id]=s;});});

/* Alte Feldnamen -> neue Stelle */
var MAP={
 age:'s11#0#alter', groesse:'s21#0#groesse', cup:'s21#0#koerbchen', figur:'s21#0#figur',
 geschwister:'s11#0#geschwister', beruf:'s11#0#beruf', wohnort:'s11#0#wohnort',
 wohnsituation:'s11#0#wohnsituation', status:'s11#0#beziehungsstatus',
 brust:'s21#0#brueste', arsch:'s21#0#arsch', muschi:'s21#0#muschi',
 oberteil:'s31#0#oberteil', unterteil:'s31#0#beinbekleidung', unterhose:'s31#0#unterhose', bh:'s31#0#bh',
 partner:'s411#0#anzahl', blasen:'s412#0#anzahl', erstesex:'s491#0#verlauf',
 dreier:'s4111#0#konstellationen', vierer:'s4112#0#konstellationen', rekord:'s410#0#maennerfick'
};
var MAPC={gewicht:'s21', gruppe:'s4113', sperma:'s414', doppel:'s4113', squirt:'s411',
          fessel:'s414', notiz:'s11'};
var MAPCL={gewicht:'Gewicht in kg', gruppe:'Anzahl Konstellationen Gruppensex',
           sperma:'Wo landet das Sperma', doppel:'Doppelpenetration — wie oft und mit wem',
           squirt:'Squirtet sie — und wie stark',
           fessel:'Fesselspiele und Spielzeug — und welche Rolle sie dabei hat', notiz:'Notiz'};
var OLDCAT=[['s11','s11'],['s21','s31'],['s411','s4111'],['s414','s410']];

var BOXFIELDS=[
 ['s11#0#alter','Alter'],['s21#0#koerbchen','Körbchengrösse'],['s411#0#anzahl','Anzahl Sexpartner'],
 ['s21#0#groesse','Grösse'],['s21#0#figur','Figur'],['s11#0#beruf','Beruf'],['s11#0#wohnort','Wohnort'],
 ['s11#0#beziehungsstatus','Beziehungsstatus'],['s410#0#maennerfick','Rekord (Männer an einem Tag)']
];
var BOXSHORT={'s411#0#anzahl':'Sexpartner','s21#0#groesse':'Grösse','s11#0#alter':'Alter',
              's21#0#koerbchen':'Körbchengrösse'};
var DEFBOX=['s11#0#alter','s21#0#koerbchen','s411#0#anzahl'];

function kf(sid,i,k){return sid+'#'+i+'#'+k;}
function kc(sid,cid){return 'c#'+sid+'#'+cid;}
function defOf(key,per){
  per=per||cur;
  var p=String(key).split('#');
  if(p[0]==='c'){
    var list=(per&&per.custom&&per.custom[p[1]])||[];
    for(var i=0;i<list.length;i++)if(list[i].id===p[2])return [p[2],list[i].label||'Eigener Punkt','long'];
    return null;
  }
  var s=SEC[p[0]];if(!s)return null;
  for(var j=0;j<s.f.length;j++)if(s.f[j][0]===p[2])return s.f[j];
  return null;
}
function labOf(key,per){var d=defOf(key,per);return d?d[1]:key;}
function itemCount(s){return (cur&&cur.cnt&&cur.cnt[s.id])||1;}

/* ===================== Zustand ===================== */
var people=[],IMGS={},set={},TRASH={imgs:{},people:{}},CUSTOM={};
var chats={},ctxKey='start',editing=false,cur=null,curCat=0,fileTarget=null;
var askCb=null,wheelCb=null,wFrom=0,stack=[],cropId=null,openBars={};

function $(i){return document.getElementById(i);}
function esc(s){return String(s==null?'':s).replace(/&/g,'&amp;').replace(/</g,'&lt;').replace(/>/g,'&gt;');}
function ls(k,d){try{var v=localStorage.getItem(k);return v?JSON.parse(v):d;}catch(e){return d;}}
function savePeople(){try{localStorage.setItem(K_PEOPLE,JSON.stringify(people));}catch(e){toast('Speicher voll');}}
function saveImgs(){try{localStorage.setItem(K_IMG,JSON.stringify(IMGS));}catch(e){toast('Speicher voll');}}
function saveSet(){try{localStorage.setItem(K_SET,JSON.stringify(set));}catch(e){}}
function saveTrash(){try{localStorage.setItem(K_TRASH,JSON.stringify(TRASH));}catch(e){}}
function newId(){var n=1;Object.keys(IMGS).forEach(function(k){if(+k>=n)n=+k+1;});return 'i'+n;}
var tmr;
function toast(t){var e=$('toast');e.textContent=t;e.classList.add('on');clearTimeout(tmr);
  tmr=setTimeout(function(){e.classList.remove('on');},2100);}

/* Alte Daten in die neue Ordnung bringen */
function migrate(p){
  if(p.mig)return;
  p.f=p.f||{};p.custom=p.custom||{};p.cnt=p.cnt||{};
  Object.keys(MAP).forEach(function(k){
    if(p.f[k]!==undefined){p.f[MAP[k]]=p.f[k];delete p.f[k];}
  });
  Object.keys(MAPC).forEach(function(k){
    if(p.f[k]===undefined)return;
    var sid=MAPC[k],id='m'+k;
    p.custom[sid]=p.custom[sid]||[];
    p.custom[sid].push({id:id,label:MAPCL[k]});
    p.f[kc(sid,id)]=p.f[k];delete p.f[k];
  });
  Object.keys(CUSTOM).forEach(function(k){
    var c=CUSTOM[k];if(!c||p.f[k]===undefined)return;
    var row=OLDCAT[c.cat];var sid=row?(row[c.sub]||row[0]):'s11';
    var id='o'+k;
    p.custom[sid]=p.custom[sid]||[];
    p.custom[sid].push({id:id,label:c.label||k});
    p.f[kc(sid,id)]=p.f[k];delete p.f[k];
  });
  p.box=(p.box||[]).map(function(k){return MAP[k]||k;});
  p.mig=1;
}

function init(){
  set=ls(K_SET,{});
  var d={key:'',base:API,model:'venice-uncensored-1-2',imgmodel:'',grund:GRUND_VORGABE,fs:16,
         imgsize:760,sort:'name',size:'768x1024',custom:{}};
  for(var k in d){if(set[k]===undefined||set[k]===null||set[k]==='')set[k]=d[k];}
  CUSTOM=set.custom||{};
  people=ls(K_PEOPLE,null);
  if(!people){
    people=[['Eva Elfie',57,26,'C',42],['Mia Khalifa',12,30,'D',96],['Lia Engel',27,24,'B',31],
            ['Diana Rider',41,29,'D',58],['Molly Little',63,23,'B',27],['Abella Danger',78,31,'D',74]]
      .map(function(s,i){return {id:i+1,n:s[0],ph:s[1],f:{age:s[2],cup:s[3],partner:s[4]},
        box:['age','cup','partner'],img:null};});
  }
  IMGS=ls(K_IMG,{});
  TRASH=ls(K_TRASH,{imgs:{},people:{}});
  if(!TRASH.imgs)TRASH.imgs={};if(!TRASH.people)TRASH.people={};
  document.documentElement.style.setProperty('--fs',(set.fs||16)+'px');
  if(!people.some(function(p){return p.leon==='student';})){
    people.push({id:1001,leon:'student',n:'Leon',ph:33,img:null,
      f:{age:'22',groesse:'184',figur:'sportlich, durchtrainiert, kurze braune Haare',status:'Student'},
      box:['age','groesse','figur']});
  }
  if(!people.some(function(p){return p.leon==='street';})){
    people.push({id:1002,leon:'street',n:'Leon',ph:44,img:null,
      f:{age:'30',groesse:'184',figur:'sportlich, kräftig, kurze braune Haare',status:'Streetworker'},
      box:['age','groesse','figur']});
  }
  people.forEach(migrate);
  savePeople();
}

/* ===================== Eigene Fenster ===================== */
function sheetAsk(title,ph,val,cb,multi){
  askCb=cb;$('askTitle').textContent=title;
  $('askBody').innerHTML=multi
    ? '<textarea id="askIn" placeholder="'+esc(ph||'')+'">'+esc(val||'')+'</textarea>'
    : '<input id="askIn" placeholder="'+esc(ph||'')+'" value="'+esc(val||'')+'">';
  $('askYes').textContent='Übernehmen';$('shAsk').classList.add('on');
  setTimeout(function(){var i=$('askIn');if(i)i.focus();},140);
}
function sheetConfirm(title,text,cb){
  askCb=cb;$('askTitle').textContent=title;
  $('askBody').innerHTML='<div class="qtext">'+esc(text)+'</div>';
  $('askYes').textContent='Ja';$('shAsk').classList.add('on');
}
function closeAsk(){askCb=null;$('shAsk').classList.remove('on');}
$('askNo').addEventListener('click',closeAsk);
$('askClose').addEventListener('click',closeAsk);
$('askYes').addEventListener('click',function(){
  var inp=$('askIn');var v=inp?inp.value:true;
  var cb=askCb;askCb=null;$('shAsk').classList.remove('on');
  if(cb)cb(v);
});
$('shAsk').addEventListener('click',function(e){if(e.target===this)closeAsk();});

/* ===================== Navigation ===================== */
function push(v){stack.push(v);}
function closeView(v){
  if(v==='profile')$('ovProf').classList.remove('on');
  else if(v==='sub')$('ovSub').classList.remove('on');
  else if(v==='settings')$('ovSet').classList.remove('on');
}
var SHEETS=['shChat','shImg','shPick','shWheel','shGal','shTrash','shCrop','shAsk'];
function back(){
  for(var i=0;i<SHEETS.length;i++){
    if($(SHEETS[i]).classList.contains('on')){$(SHEETS[i]).classList.remove('on');return;}
  }
  if(stack.length){closeView(stack.pop());markNav();}
}
function goHome(){
  while(stack.length){closeView(stack.pop());}
  SHEETS.forEach(function(s){$(s).classList.remove('on');});
  $('q').value='';search('');$('home').scrollTop=0;markNav();
}
function markNav(){
  $('nHome').classList.toggle('on',!stack.length);
  $('nSet').classList.toggle('on',stack[stack.length-1]==='settings');
}
$('nBack').addEventListener('click',back);
$('nHome').addEventListener('click',goHome);
$('nSet').addEventListener('click',function(){openSet();});

/* ===================== Startseite ===================== */
function imgOf(p){return (p&&p.img&&IMGS[p.img])?IMGS[p.img].u:null;}
function cropOf(p){return (p&&p.img&&IMGS[p.img])?IMGS[p.img].c:null;}
function ageOf(p){return +p.f['s11#0#alter']||0;}
function sorted(){
  var a=people.filter(function(p){return !p.leon;});
  if(set.sort==='age')a.sort(function(x,y){return ageOf(x)-ageOf(y);});
  else if(set.sort==='added')a.sort(function(x,y){return y.id-x.id;});
  else a.sort(function(x,y){return x.n.localeCompare(y.n,'de');});
  return a;
}
function tileHTML(p){
  var src=imgOf(p)||PH;
  return '<div class="ph"><img alt="" src="'+esc(src)+'"'+(cropOf(p)?' style="object-position:'+
    (cropOf(p).x*100)+'% '+(cropOf(p).y*100)+'%;transform:scale('+cropOf(p).z+')"':'')+'></div>'+
    '<div class="upl"><svg width="15" height="15" viewBox="0 0 16 16" fill="none">'+
    '<path d="M8 11V3M8 3L4.5 6.5M8 3l3.5 3.5" stroke="#fff" stroke-width="1.7" stroke-linecap="round"/>'+
    '<path d="M2.5 11.5v2h11v-2" stroke="#fff" stroke-width="1.7" stroke-linecap="round"/></svg></div>'+
    '<div class="nm">'+esc(p.n)+'</div>';
}
function renderHome(){
  var g=$('grid');g.innerHTML='';
  var list=sorted();
  $('cnt').textContent=list.length+(list.length===1?' Eintrag':' Einträge');
  var b1=document.createElement('div');b1.className='pbtn';b1.textContent='Alle Storys';
  b1.addEventListener('click',function(){toast('Storys kommen später');});
  g.appendChild(b1);
  var b2=document.createElement('div');b2.className='pbtn';b2.textContent='Neuer Steckbrief';
  b2.addEventListener('click',newPerson);
  g.appendChild(b2);
  list.forEach(function(p){
    var d=document.createElement('div');d.className='tile';d.innerHTML=tileHTML(p);
    d.addEventListener('click',function(){openProfile(p,d);});
    d.querySelector('.upl').addEventListener('click',function(ev){ev.stopPropagation();askImage(p);});
    g.appendChild(d);
  });
}
function newPerson(){
  var id=1;people.forEach(function(p){if(p.id>=id)id=p.id+1;});
  var p={id:id,n:'Neue Frau',ph:Math.floor(Math.random()*80)+1,f:{},custom:{},cnt:{},
    box:DEFBOX.slice(),img:null,mig:1};
  people.push(p);savePeople();renderHome();
  toast('Steckbrief angelegt');
  openProfile(p,null);
}
function search(q){
  q=(q||'').trim().toLowerCase();
  var g=$('grid'),r=$('reslist');
  if(!q){g.style.display='';r.classList.remove('on');return;}
  g.style.display='none';r.classList.add('on');r.innerHTML='';
  var hits=sorted().filter(function(p){
    if(p.n.toLowerCase().indexOf(q)>=0)return true;
    return Object.keys(p.f).some(function(k){return String(p.f[k]||'').toLowerCase().indexOf(q)>=0;});
  });
  if(!hits.length){r.innerHTML='<div class="empty">Nichts gefunden.</div>';return;}
  hits.forEach(function(p){
    var d=document.createElement('div');d.className='res';
    var hit='';
    Object.keys(p.f).forEach(function(k){
      if(!hit&&String(p.f[k]||'').toLowerCase().indexOf(q)>=0)hit=labOf(k,p)+': '+p.f[k];
    });
    d.innerHTML='<div class="rn">'+esc(p.n)+'</div><div class="rh">'+esc(hit)+'</div>';
    d.addEventListener('click',function(){openProfile(p,null);});
    r.appendChild(d);
  });
}
$('q').addEventListener('input',function(){search(this.value);});

/* ===================== Steckbrief ===================== */
function openProfile(p,tile){
  cur=p;editing=false;openBars={};
  cur.custom=cur.custom||{};cur.cnt=cur.cnt||{};
  $('pEdit').classList.remove('on');$('pEdit').textContent='Bearbeiten';
  $('pName').contentEditable='false';
  $('pHero').src=imgOf(p)||PH;
  $('pHero').style.objectPosition=cropOf(p)?(cropOf(p).x*100)+'% '+(cropOf(p).y*100)+'%':'';
  $('pHero').style.transform=cropOf(p)?'scale('+cropOf(p).z+')':'';
  $('pName').textContent=p.n;
  $('pSub').textContent=(p.f['s11#0#alter']?p.f['s11#0#alter']+' Jahre':'')+
    (p.f['s11#0#beziehungsstatus']?' · '+p.f['s11#0#beziehungsstatus']:'');
  renderBoxes();renderCats();
  $('ovProf').classList.add('on');
  if(!stack.length||stack[stack.length-1]!=='profile')push('profile');
  markNav();
}
function renderBoxes(){
  var b=$('boxes');b.innerHTML='';
  (cur.box||[]).forEach(function(k,i){
    var d=document.createElement('div');d.className='box';
    d.innerHTML='<div class="pen">✎</div><div class="bl">'+esc(BOXSHORT[k]||labOf(k,cur))+'</div>'+
      '<div class="bv">'+esc(cur.f[k]||'–')+'</div>';
    d.addEventListener('click',function(){
      if(editing){pickField(function(nk){cur.box[i]=nk;savePeople();renderBoxes();});}
      else if(k)editField(k);
    });
    b.appendChild(d);
  });
}
function pickField(cb){
  var b=$('pickBody');b.innerHTML='';
  BOXFIELDS.forEach(function(f){
    var d=document.createElement('div');d.className='opt';
    d.innerHTML='<span>'+esc(f[1])+'</span>';
    d.addEventListener('click',function(){$('shPick').classList.remove('on');cb(f[0]);});
    b.appendChild(d);
  });
  $('shPick').classList.add('on');
}
$('pickClose').addEventListener('click',function(){$('shPick').classList.remove('on');});
function afterEdit(){
  savePeople();renderBoxes();renderCats();renderSub();
  $('pSub').textContent=(cur.f['s11#0#alter']?cur.f['s11#0#alter']+' Jahre':'')+
    (cur.f['s11#0#beziehungsstatus']?' · '+cur.f['s11#0#beziehungsstatus']:'');
}
function editField(key){
  var d=defOf(key);if(!d)return;
  if(d[2]==='num'){
    wheel(d[1],d[3]===undefined?0:d[3],d[4]===undefined?99:d[4],cur.f[key],function(v){
      cur.f[key]=v;afterEdit();
    });
  }else{
    sheetAsk(d[1],'',cur.f[key]||'',function(v){
      if(v===null)return;
      if(v==='')delete cur.f[key];else cur.f[key]=v;
      afterEdit();
    },d[2]==='long');
  }
}
function chFilled(c){
  var n=0;
  c.s.forEach(function(s){n+=secFilled(s);});
  return n;
}
function renderCats(){
  var w=$('catlist');w.innerHTML='';
  CH.forEach(function(c,ci){
    var n=chFilled(c);
    var d=document.createElement('div');d.className='catbtn';
    d.innerHTML='<div style="flex:1"><div>'+esc(c.n)+'</div><div class="csub">'+esc(c.u)+'</div></div>'+
      '<span class="cnt">'+(n?n+' Punkte':'leer')+'</span><span class="chev">›</span>';
    d.addEventListener('click',function(){openSub(ci);});
    w.appendChild(d);
  });
}
$('pBack').addEventListener('click',back);
$('pEdit').addEventListener('click',function(){
  editing=!editing;this.classList.toggle('on',editing);
  this.textContent=editing?'Fertig':'Bearbeiten';
  $('pName').contentEditable=editing?'true':'false';
  renderBoxes();renderSub();
});
$('pName').addEventListener('blur',function(){
  if(editing){cur.n=this.textContent.trim()||'Ohne Namen';savePeople();renderHome();}
});
$('storyBtn').addEventListener('click',function(){toast('Storys kommen später');});
$('pDel').addEventListener('click',function(){
  var p=cur;
  sheetConfirm('Steckbrief löschen','«'+p.n+'» wandert in den Papierkorb.',function(){
    TRASH.people[p.id]=p;saveTrash();
    people=people.filter(function(x){return x.id!==p.id;});savePeople();
    cur=null;back();renderHome();toast('In den Papierkorb');
  });
});
$('pImg').addEventListener('click',function(){if(cur)askImage(cur);});

/* ===================== Kapitel & Punkte ===================== */
function secFilled(s){
  var n=0;
  if(s.rep){
    var cnt=itemCount(s);
    for(var i=0;i<cnt;i++)s.f.forEach(function(f){if(cur.f[kf(s.id,i,f[0])])n++;});
  }else{
    s.f.forEach(function(f){if(cur.f[kf(s.id,0,f[0])])n++;});
  }
  (cur.custom[s.id]||[]).forEach(function(c){if(cur.f[kc(s.id,c.id)])n++;});
  return n;
}
function secTotal(s){
  return (s.rep?itemCount(s)*s.f.length:s.f.length)+((cur.custom[s.id]||[]).length);
}
function openSub(ci){
  curCat=ci;editing=false;
  $('uEdit').classList.remove('on');$('uEdit').textContent='Bearbeiten';
  $('uTtl').textContent=CH[ci].n+' — '+CH[ci].u;
  renderSub();
  $('ovSub').classList.add('on');
  if(stack[stack.length-1]!=='sub')push('sub');
  markNav();
}
$('uBack').addEventListener('click',back);
$('uEdit').addEventListener('click',function(){
  editing=!editing;this.classList.toggle('on',editing);
  this.textContent=editing?'Fertig':'Bearbeiten';renderSub();
});
function prowEl(key){
  var d=document.createElement('div');d.className='prow';
  var v=cur.f[key];
  d.innerHTML='<div class="pl"><span>'+esc(labOf(key,cur))+'</span>'+
    (editing?'<span class="del" data-k="'+esc(key)+'">✕</span>':'')+'</div>'+
    '<div class="pv'+(v?'':' empty')+'">'+esc(v||'tippen')+'</div>';
  d.querySelector('.pv').addEventListener('click',function(){editField(key);});
  return d;
}
function customEls(sid,box){
  (cur.custom[sid]||[]).forEach(function(c){
    var key=kc(sid,c.id);
    var d=document.createElement('div');d.className='prow';
    var v=cur.f[key];
    d.innerHTML='<div class="pl"><span>'+esc(c.label||'Eigener Punkt')+'</span>'+
      (editing?'<span class="del" data-c="'+esc(sid)+'#'+esc(c.id)+'">✕</span>':'')+'</div>'+
      '<div class="pv'+(v?'':' empty')+'">'+esc(v||'tippen')+'</div>';
    d.querySelector('.pv').addEventListener('click',function(){editField(key);});
    box.appendChild(d);
  });
}
function addRow(sid,box,txt,fn){
  var a=document.createElement('div');a.className='addrow';a.textContent=txt;
  a.addEventListener('click',fn);box.appendChild(a);
}
function bodyFor(s,sid,index,box){
  s.f.forEach(function(f){box.appendChild(prowEl(kf(sid,index,f[0])));});
}
function itemBar(s,i){
  var bar=document.createElement('div');bar.className='bar';
  var oid=s.id+':'+i;
  if(openBars[oid])bar.classList.add('open');
  var filled=s.f.filter(function(f){return cur.f[kf(s.id,i,f[0])];}).length;
  bar.innerHTML='<div class="barhead"><span>'+esc(s.rep)+' '+(i+1)+'</span>'+
    '<span class="cnt">'+filled+' von '+s.f.length+'</span>'+
    (editing?'<span class="del" data-i="'+esc(s.id)+'#'+i+'">✕</span>':'')+
    '<span class="chev">›</span></div>';
  var body=document.createElement('div');body.className='barbody';
  bodyFor(s,s.id,i,body);
  bar.appendChild(body);
  bar.querySelector('.barhead').addEventListener('click',function(e){
    if(e.target.className==='del')return;
    bar.classList.toggle('open');openBars[oid]=bar.classList.contains('open');
  });
  return bar;
}
function renderSub(){
  var b=$('uBody');if(!b)return;
  b.innerHTML='';
  CH[curCat].s.forEach(function(s){
    if(s.rep){
      var cnt=itemCount(s);
      for(var i=0;i<cnt;i++)b.appendChild(itemBar(s,i));
      var card=document.createElement('div');card.className='bar open flat';
      var box=document.createElement('div');box.className='barbody';
      customEls(s.id,box);
      addRow(s.id,box,'+ Punkt hinzufügen',function(){addPoint(s.id);});
      addRow(s.id,box,'+ '+s.rep+' hinzufügen',function(){
        cur.cnt[s.id]=itemCount(s)+1;savePeople();renderSub();renderCats();
      });
      card.appendChild(box);b.appendChild(card);
    }else if(s.flat){
      var card2=document.createElement('div');card2.className='bar open flat';
      var box2=document.createElement('div');box2.className='barbody';
      bodyFor(s,s.id,0,box2);
      customEls(s.id,box2);
      addRow(s.id,box2,'+ Punkt hinzufügen',function(){addPoint(s.id);});
      card2.appendChild(box2);b.appendChild(card2);
    }else{
      var bar=document.createElement('div');bar.className='bar';
      if(openBars[s.id])bar.classList.add('open');
      bar.innerHTML='<div class="barhead"><span>'+esc(s.n)+'</span><span class="cnt">'+
        secFilled(s)+' von '+secTotal(s)+'</span><span class="chev">›</span></div>';
      var box3=document.createElement('div');box3.className='barbody';
      bodyFor(s,s.id,0,box3);
      customEls(s.id,box3);
      addRow(s.id,box3,'+ Punkt hinzufügen',function(){addPoint(s.id);});
      bar.appendChild(box3);
      bar.querySelector('.barhead').addEventListener('click',function(){
        bar.classList.toggle('open');openBars[s.id]=bar.classList.contains('open');
      });
      b.appendChild(bar);
    }
  });
  var hint=document.createElement('div');hint.className='subhint';
  hint.textContent='Tippe auf einen Punkt, um ihn zu schreiben. Über Bearbeiten kannst du Punkte löschen oder ergänzen.';
  b.appendChild(hint);

  b.querySelectorAll('.del').forEach(function(d){
    d.addEventListener('click',function(ev){
      ev.stopPropagation();
      if(this.dataset.c){
        var p=this.dataset.c.split('#');
        cur.custom[p[0]]=(cur.custom[p[0]]||[]).filter(function(x){return x.id!==p[1];});
        delete cur.f[kc(p[0],p[1])];
        savePeople();renderSub();renderCats();renderBoxes();
      }else if(this.dataset.i){
        var q=this.dataset.i.split('#');
        delItem(q[0],+q[1]);
      }else if(this.dataset.k){
        var k=this.dataset.k;
        sheetConfirm('Punkt leeren','«'+labOf(k,cur)+'» wirklich leeren?',function(){
          delete cur.f[k];savePeople();renderSub();renderCats();renderBoxes();
        });
      }
    });
  });
}
function delItem(sid,idx){
  var cnt=cur.cnt[sid]||1;
  if(cnt<=1){toast('Der letzte Eintrag bleibt');return;}
  var out={};
  Object.keys(cur.f).forEach(function(k){
    var p=k.split('#');
    if(p[0]===sid&&p.length===3&&+p[1]>idx){
      out[sid+'#'+(+p[1]-1)+'#'+p[2]]=cur.f[k];delete cur.f[k];
    }
  });
  for(var k2 in out)cur.f[k2]=out[k2];
  cur.cnt[sid]=cnt-1;
  savePeople();renderSub();renderCats();
}
function addPoint(sid){
  sheetAsk('Neuer Punkt','z.B. Halsband','',function(name){
    if(!name||!name.trim())return;
    name=name.trim();
    var id='x'+Date.now().toString(36);
    cur.custom[sid]=cur.custom[sid]||[];
    cur.custom[sid].push({id:id,label:name});
    savePeople();
    editing=true;$('uEdit').classList.add('on');$('uEdit').textContent='Fertig';
    renderSub();renderCats();
    toast('Punkt angelegt');
  });
}

/* ===================== Zahlenrad ===================== */
function wheel(title,from,to,val,cb){
  if(from===undefined||from===null)from=0;if(to===undefined)to=99;
  wFrom=from;wheelCb=cb;$('wTtl').textContent=title;
  var l=$('wList');l.innerHTML='';
  var pad=document.createElement('div');pad.style.height='76px';l.appendChild(pad);
  for(var i=from;i<=to;i++){
    var d=document.createElement('div');
    d.style.cssText='height:38px;line-height:38px;scroll-snap-align:center;font-size:1.05rem';
    d.textContent=i;d.dataset.v=i;l.appendChild(d);
  }
  var pad2=document.createElement('div');pad2.style.height='76px';l.appendChild(pad2);
  $('shWheel').classList.add('on');
  setTimeout(function(){
    l.scrollTop=(val!==undefined&&val!==''&&!isNaN(+val)?(+val-from):0)*38;
    var ch=function(){
      var i=Math.round(l.scrollTop/38);
      Array.prototype.forEach.call(l.children,function(c,j){
        c.style.color=(j-1===i)?'var(--fg)':'var(--muted)';
        c.style.fontWeight=(j-1===i)?'600':'400';
      });
    };
    l.onscroll=ch;ch();
  },40);
}
$('wOk').addEventListener('click',function(){
  var i=Math.round($('wList').scrollTop/38)+wFrom;
  if(wheelCb)wheelCb(i);
  $('shWheel').classList.remove('on');
});
$('wCancel').addEventListener('click',function(){$('shWheel').classList.remove('on');});
$('wClose').addEventListener('click',function(){$('shWheel').classList.remove('on');});

/* ===================== Bild ===================== */
function askImage(p){
  fileTarget=p;
  var b=$('imgBody');b.innerHTML='';
  var opts=[['Hochladen','Datei vom Gerät'],['Bild per Link','Adresse einfügen']];
  if(imgOf(p))opts.push(['Ausschnitt ändern','Ausschnitt neu setzen']);
  if(imgOf(p))opts.push(['Bild entfernen','zurück zur Silhouette']);
  opts.forEach(function(o){
    var d=document.createElement('div');d.className='opt';
    d.innerHTML='<span>'+esc(o[0])+'</span><span class="sub2">'+esc(o[1])+'</span>';
    d.addEventListener('click',function(){
      $('shImg').classList.remove('on');
      if(o[0]==='Hochladen'){$('pick').click();}
      else if(o[0]==='Bild per Link'){
        sheetAsk('Bildadresse','https://…','',function(u){
          if(!u||!u.trim())return;
          var id=newId();IMGS[id]={u:u.trim(),c:{x:.5,y:.5,z:1}};saveImgs();
          p.img=id;savePeople();refreshAll();
        });
      }
      else if(o[0]==='Ausschnitt ändern'){openCrop(p.img);}
      else{
        sheetConfirm('Bild entfernen','Das Bild wandert in den Papierkorb.',function(){
          var id=p.img;TRASH.imgs[id]=IMGS[id];delete IMGS[id];
          p.img=null;saveImgs();saveTrash();savePeople();refreshAll();
        });
      }
    });
    b.appendChild(d);
  });
  $('shImg').classList.add('on');
}
$('imgClose').addEventListener('click',function(){$('shImg').classList.remove('on');});
$('pick').addEventListener('change',function(){
  var f=this.files&&this.files[0],t=fileTarget;this.value='';
  if(!f||!t)return;
  shrinkTo(f,set.imgsize||760,0.82,function(data){
    var id=newId();IMGS[id]={u:data,c:{x:.5,y:.5,z:1}};saveImgs();
    t.img=id;savePeople();refreshAll();openCrop(id);
  });
});
function shrinkTo(file,max,wq,cb){
  var fr=new FileReader();
  fr.onload=function(){
    var im=new Image();
    im.onload=function(){
      var w=im.width,h=im.height;
      var s=Math.min(1,max/Math.max(w,h));
      w=Math.max(1,Math.round(w*s));h=Math.max(1,Math.round(h*s));
      var c=document.createElement('canvas');c.width=w;c.height=h;
      c.getContext('2d').drawImage(im,0,0,w,h);
      cb(c.toDataURL('image/jpeg',wq));
    };
    im.onerror=function(){toast('Bild nicht lesbar');};
    im.src=fr.result;
  };
  fr.readAsDataURL(file);
}
function refreshAll(){
  renderHome();
  if(cur){
    $('pHero').src=imgOf(cur)||PH;
    $('pHero').style.objectPosition=cropOf(cur)?(cropOf(cur).x*100)+'% '+(cropOf(cur).y*100)+'%':'';
    $('pHero').style.transform=cropOf(cur)?'scale('+cropOf(cur).z+')':'';
  }
  renderGal();renderTrash();
}

/* ===================== Zuschnitt ===================== */
function openCrop(id){
  if(!id||!IMGS[id])return;
  cropId=id;
  $('cropImg').src=IMGS[id].u;
  var c=IMGS[id].c||{x:.5,y:.5,z:1};
  $('cropX').value=c.x*100;$('cropY').value=c.y*100;$('cropZ').value=c.z*100;
  applyCrop();
  $('shCrop').classList.add('on');
}
function applyCrop(){
  var o={x:$('cropX').value/100,y:$('cropY').value/100,z:$('cropZ').value/100};
  $('cropImg').style.objectPosition=(o.x*100)+'% '+(o.y*100)+'%';
  $('cropImg').style.transform='scale('+o.z+')';
}
['cropX','cropY','cropZ'].forEach(function(i){$(i).addEventListener('input',applyCrop);});
$('cropOk').addEventListener('click',function(){
  if(cropId&&IMGS[cropId]){
    IMGS[cropId].c={x:$('cropX').value/100,y:$('cropY').value/100,z:$('cropZ').value/100};
    saveImgs();refreshAll();
  }
  $('shCrop').classList.remove('on');
});
$('cropCancel').addEventListener('click',function(){$('shCrop').classList.remove('on');});
$('cropClose').addEventListener('click',function(){$('shCrop').classList.remove('on');});

/* ===================== Galerie & Papierkorb ===================== */
function renderGal(){
  var g=$('galGrid');if(!g)return;
  g.innerHTML='';
  var ids=Object.keys(IMGS);
  if(!ids.length){g.innerHTML='<div class="empty" style="grid-column:1/-1">Noch keine Bilder.</div>';return;}
  ids.forEach(function(id){
    var d=document.createElement('div');d.className='gcell';
    d.innerHTML='<img src="'+esc(IMGS[id].u)+'" alt=""><div class="x">✕</div>';
    d.addEventListener('click',function(){openCrop(id);});
    d.querySelector('.x').addEventListener('click',function(ev){
      ev.stopPropagation();
      sheetConfirm('Bild löschen','Das Bild wandert in den Papierkorb.',function(){
        TRASH.imgs[id]=IMGS[id];delete IMGS[id];
        people.forEach(function(p){if(p.img===id)p.img=null;});
        saveImgs();saveTrash();savePeople();refreshAll();
      });
    });
    g.appendChild(d);
  });
}
$('galClose').addEventListener('click',function(){$('shGal').classList.remove('on');});
$('galAdd').addEventListener('click',function(){
  sheetAsk('Bildadresse','https://…','',function(u){
    if(!u||!u.trim())return;
    var id=newId();IMGS[id]={u:u.trim(),c:{x:.5,y:.5,z:1}};saveImgs();refreshAll();
  });
});
function renderTrash(){
  var b=$('trashBody');if(!b)return;
  b.innerHTML='';
  var ik=Object.keys(TRASH.imgs||{}),pk=Object.keys(TRASH.people||{});
  if(!ik.length&&!pk.length){b.innerHTML='<div class="empty">Der Papierkorb ist leer.</div>';return;}
  if(ik.length){
    var h=document.createElement('div');h.className='setsec';h.textContent='Bilder';b.appendChild(h);
    var g=document.createElement('div');g.className='galgrid';b.appendChild(g);
    ik.forEach(function(id){
      var d=document.createElement('div');d.className='gcell';
      d.innerHTML='<img src="'+esc(TRASH.imgs[id].u)+'" alt="">';
      d.addEventListener('click',function(){
        sheetConfirm('Wiederherstellen','Bild zurückholen?',function(){
          IMGS[id]=TRASH.imgs[id];delete TRASH.imgs[id];saveImgs();saveTrash();refreshAll();
        });
      });
      g.appendChild(d);
    });
  }
  if(pk.length){
    var h2=document.createElement('div');h2.className='setsec';h2.textContent='Steckbriefe';b.appendChild(h2);
    pk.forEach(function(id){
      var d=document.createElement('div');d.className='opt';
      d.innerHTML='<span>'+esc(TRASH.people[id].n)+'</span><span class="sub2">antippen zum Zurückholen</span>';
      d.addEventListener('click',function(){
        sheetConfirm('Wiederherstellen','«'+TRASH.people[id].n+'» zurückholen?',function(){
          people.push(TRASH.people[id]);delete TRASH.people[id];savePeople();saveTrash();refreshAll();
        });
      });
      b.appendChild(d);
    });
  }
  var e=document.createElement('div');e.className='addrow';
  e.innerHTML='<button>Papierkorb leeren</button>';
  e.querySelector('button').addEventListener('click',function(){
    sheetConfirm('Papierkorb leeren','Alles endgültig löschen?',function(){
      TRASH={imgs:{},people:{}};saveTrash();renderTrash();
    });
  });
  b.appendChild(e);
}
$('trashClose').addEventListener('click',function(){$('shTrash').classList.remove('on');});

/* ===================== Einstellungen ===================== */
function openSet(){
  if(stack[stack.length-1]==='settings'){return;}
  renderSet();$('ovSet').classList.add('on');push('settings');markNav();
}
$('sBack').addEventListener('click',back);
function renderSet(){
  var b=$('sBody');b.innerHTML='';
  function sec(t){var d=document.createElement('div');d.className='setsec';d.textContent=t;b.appendChild(d);}
  function row(lab,val,cb,wrap){
    var d=document.createElement('div');d.className='setrow';
    d.innerHTML='<span>'+esc(lab)+'</span><span class="rv'+(wrap?' wrap':'')+'">'+esc(val)+'</span>';
    if(cb)d.addEventListener('click',cb);else d.style.cursor='default';
    b.appendChild(d);return d;
  }
  function hint(t){var d=document.createElement('div');d.className='hint';d.textContent=t;b.appendChild(d);}

  sec('KI');
  row('Schlüssel',set.key?('…'+set.key.slice(-6)):'nicht gesetzt',function(){
    sheetAsk('Schlüssel','API-Schlüssel','',function(v){if(v!==null){set.key=v.trim();saveSet();renderSet();}});
  });
  row('Adresse',set.base||API,function(){
    sheetAsk('Adresse des Anbieters','https://…','',function(v){
      if(v!==null){set.base=v.trim()||API;saveSet();renderSet();}});
  });
  row('Textmodell',set.model,function(){
    sheetAsk('Textmodell','z.B. venice-uncensored-1-2','',function(v){
      if(v!==null){set.model=v.trim()||set.model;saveSet();renderSet();}});
  });
  row('Modelle laden','vom Server holen',function(){
    loadModels(function(list){
      $('pickBody').innerHTML='';
      list.forEach(function(id){
        var d=document.createElement('div');d.className='opt';
        d.innerHTML='<span>'+esc(id)+'</span>'+(id===set.model?'<span class="sub2">aktuell</span>':'');
        d.addEventListener('click',function(){$('shPick').classList.remove('on');set.model=id;saveSet();renderSet();});
        $('pickBody').appendChild(d);
      });
      $('shPick').classList.add('on');
    });
  });
  row('Bildmodell',set.imgmodel||'Standard',function(){
    sheetAsk('Bildmodell','leer lassen für Standard','',function(v){
      if(v!==null){set.imgmodel=v.trim();saveSet();renderSet();}});
  });
  row('Verbindung testen','jetzt prüfen',testConn);
  row('Grundanweisung','ändern',function(){
    sheetAsk('Grundanweisung','Text','',function(v){
      if(v!==null){set.grund=v;saveSet();renderSet();}},true);
  },true);
  hint('Grundanweisung: '+set.grund.slice(0,90)+(set.grund.length>90?'…':''));

  sec('Darstellung');
  row('Schriftgrösse',set.fs+' px',function(){
    wheel('Schriftgrösse',13,22,set.fs,function(v){
      set.fs=v;saveSet();document.documentElement.style.setProperty('--fs',v+'px');renderSet();});
  });
  row('Bildgrösse beim Hochladen',set.imgsize+' px',function(){
    wheel('Bildgrösse',400,1200,set.imgsize,function(v){set.imgsize=v;saveSet();renderSet();});
  });
  row('Sortierung',({name:'Name (A–Z)',age:'Alter',added:'Neueste zuerst'})[set.sort]||'Name',function(){
    $('pickBody').innerHTML='';
    [['name','Name (A–Z)'],['age','Alter'],['added','Neueste zuerst']].forEach(function(o){
      var d=document.createElement('div');d.className='opt';
      d.innerHTML='<span>'+o[1]+'</span>'+(set.sort===o[0]?'<span class="sub2">aktuell</span>':'');
      d.addEventListener('click',function(){$('shPick').classList.remove('on');set.sort=o[0];saveSet();renderHome();renderSet();});
      $('pickBody').appendChild(d);
    });
    $('shPick').classList.add('on');
  });
  row('Bildformat',set.size,function(){
    $('pickBody').innerHTML='';
    ['768x1024','832x1216','1024x1024','1024x1536'].forEach(function(s){
      var d=document.createElement('div');d.className='opt';
      d.innerHTML='<span>'+s+'</span>'+(set.size===s?'<span class="sub2">aktuell</span>':'');
      d.addEventListener('click',function(){$('shPick').classList.remove('on');set.size=s;saveSet();renderSet();});
      $('pickBody').appendChild(d);
    });
    $('shPick').classList.add('on');
  });

  sec('Sammlung');
  row('Galerie','öffnen',function(){$('shGal').classList.add('on');renderGal();});
  row('Papierkorb','öffnen',function(){$('shTrash').classList.add('on');renderTrash();});
  row('Leon (Student, 22)','Steckbrief öffnen',function(){
    openProfile(people.filter(function(p){return p.leon==='student';})[0],null);});
  row('Leon (Streetworker, 30)','Steckbrief öffnen',function(){
    openProfile(people.filter(function(p){return p.leon==='street';})[0],null);});

  sec('Sicherung');
  row('Exportieren','Datei teilen',doExport);
  row('Importieren','Datei wählen',function(){$('pickJson').click();});
  hint('Der Speicher liegt in jedem Browser einzeln. Safari, Chrome und der eingebettete Browser haben also je einen eigenen Stand. Über Exportieren und Importieren bringst du alles in einen anderen Browser.');

  sec('Zugang');
  row('Ausloggen','Passwortseite zeigen',function(){
    localStorage.removeItem(K_UNLOCK);showBook();
  });
}

/* ===================== Verbindung ===================== */
function api(path,opts){
  opts=opts||{};
  var url=(set.base||API)+path;
  return fetch(url,opts).then(function(r){
    return r.text().then(function(t){
      var j=null;try{j=JSON.parse(t);}catch(e){}
      if(!r.ok){
        var m='';
        if(j&&j.error){m=(typeof j.error==='string')?j.error:(j.error.message||JSON.stringify(j.error));}
        else if(j&&j.message){m=j.message;}
        else m=t.slice(0,200);
        throw new Error('HTTP '+r.status+' — '+m);
      }
      if(j===null)throw new Error('Antwort war kein JSON: '+t.slice(0,160));
      return j;
    });
  },function(e){
    throw new Error('Adresse nicht erreichbar ('+(e&&e.message?e.message:'Fehler')+'). Bei Neocities liegt es meist an der Sperre für fremde Adressen — auf GitHub Pages geht es.');
  });
}
function testConn(){
  if(!set.key){toast('Erst Schlüssel eintragen');return;}
  toast('Prüfe …');
  api('/models',{headers:{Authorization:'Bearer '+set.key}}).then(function(j){
    var n=(j&&j.data&&j.data.length)||0;
    toast('Verbindung in Ordnung. '+n+' Modelle gefunden.');
  }).catch(function(e){toast('Fehler: '+e.message);});
}
function typOf(m){
  var st=[m],seen=0;
  while(st.length&&seen<60){
    var o=st.pop();seen++;
    if(!o||typeof o!=='object')continue;
    for(var k in o){
      var v=o[k];
      if(k==='type'&&typeof v==='string')return v.toLowerCase();
      if(v&&typeof v==='object')st.push(v);
    }
  }
  return '';
}
function loadModels(cb){
  if(!set.key){toast('Erst Schlüssel eintragen');return;}
  api('/models',{headers:{Authorization:'Bearer '+set.key}}).then(function(j){
    var arr=Array.isArray(j.data)?j.data:[];
    var txt=arr.filter(function(m){return typOf(m).indexOf('text')>=0;});
    if(!txt.length)txt=arr;
    var ids=txt.map(function(m){return m.id;}).filter(Boolean).sort();
    if(!ids.length){toast('Keine Modelle gefunden');return;}
    cb(ids);
  }).catch(function(e){toast('Fehler: '+e.message);});
}

/* ===================== Chat ===================== */
function ctxTitle(){
  if(cur&&ctxKey==='p'+cur.id)return cur.n;
  return 'KI';
}
function ctxContext(){
  if(cur&&ctxKey==='p'+cur.id){
    var parts=['Steckbrief von '+cur.n];
    Object.keys(cur.f).forEach(function(k){if(cur.f[k])parts.push(labOf(k,cur)+': '+cur.f[k]);});
    return parts.join(', ');
  }
  return 'Startseite der Sammlung, '+people.filter(function(p){return !p.leon;}).length+' Steckbriefe.';
}
function sysPrompt(){
  return (set.grund?set.grund+'\n\n':'')+'Kontext: '+ctxContext()+'. Antworte auf Deutsch. '+
    'Bilder erzeugst du nicht selbst; dafür gibt es einen Bild-Knopf. Wenn ein Bild gewünscht ist, '+
    'liefere eine Zeile englischen Bildtext ohne Anführungszeichen.';
}
function openChat(){
  ctxKey=cur?('p'+cur.id):'start';
  $('chatTitle').textContent=ctxTitle();
  var log=$('chatlog');log.innerHTML='';
  var c=chats[ctxKey];
  if(!c||!c.length){chats[ctxKey]=[];greet();}
  else c.forEach(function(m){paint(m.role,m.text,m.img);});
  $('shChat').classList.add('on');
  setTimeout(function(){log.scrollTop=log.scrollHeight;},60);
}
function greet(){
  if(!set.key){addMsg('ai','Trag in den Einstellungen deinen Schlüssel ein, dann können wir loslegen.');return;}
  addMsg('ai','Bereit.');
}
function addMsg(role,text,img){
  var c=chats[ctxKey]||(chats[ctxKey]=[]);
  c.push({role:role,text:text,img:img});
  paint(role,text,img);
}
function paint(role,text,img){
  var log=$('chatlog');
  var d=document.createElement('div');d.className='msg '+(role==='user'?'me':(role==='err'?'err':'ai'));
  d.textContent=text||'';
  if(img){var im=document.createElement('img');im.src=img;d.appendChild(im);}
  log.appendChild(d);log.scrollTop=log.scrollHeight;
  return d;
}
$('chatClose').addEventListener('click',function(){$('shChat').classList.remove('on');});
$('chatSet').addEventListener('click',function(){$('shChat').classList.remove('on');openSet();});
$('fab').addEventListener('click',openChat);
$('chatIn').addEventListener('input',function(){this.style.height='auto';this.style.height=Math.min(this.scrollHeight,110)+'px';});
$('chatSend').addEventListener('click',function(){send(false);});
$('chatImg').addEventListener('click',function(){send(true);});
$('chatIn').addEventListener('keydown',function(e){
  if(e.key==='Enter'&&!e.shiftKey){e.preventDefault();send(false);}
});
function send(wantImage){
  var inp=$('chatIn'),text=inp.value.trim();
  if(!text&&!wantImage)return;
  if(!set.key){toast('Erst Schlüssel in den Einstellungen eintragen');return;}
  inp.value='';inp.style.height='auto';
  addMsg('user',text||'(Bild)');
  if(wantImage)makeImage(text);else askText(text);
}
function askText(text){
  var c=chats[ctxKey];
  var msgs=[{role:'system',content:sysPrompt()}];
  c.slice(-14).forEach(function(m){if(m.text)msgs.push({role:m.role==='user'?'user':'assistant',content:m.text});});
  var b=paint('ai','…');
  api('/chat/completions',{
    method:'POST',
    headers:{'Content-Type':'application/json',Authorization:'Bearer '+set.key},
    body:JSON.stringify({model:set.model,messages:msgs})
  }).then(function(j){
    var t=(j.choices&&j.choices[0]&&j.choices[0].message&&j.choices[0].message.content)||'(leer)';
    b.textContent=t;
    c.push({role:'assistant',text:t});
    $('chatlog').scrollTop=$('chatlog').scrollHeight;
  }).catch(function(e){
    b.className='msg err';b.textContent='Fehler: '+e.message;
    c.push({role:'err',text:'Fehler: '+e.message});
  });
}
function makeImage(text){
  var b=paint('ai','Bild wird erzeugt …');
  var body={model:set.imgmodel||set.model,prompt:text,size:set.size};
  api('/images/generations',{
    method:'POST',
    headers:{'Content-Type':'application/json',Authorization:'Bearer '+set.key},
    body:JSON.stringify(body)
  }).then(function(j){
    var src=pickImage(j);
    if(!src){b.className='msg err';b.textContent='Kein Bild in der Antwort: '+JSON.stringify(j).slice(0,180);return;}
    b.textContent='';var im=document.createElement('img');im.src=src;b.appendChild(im);
    chats[ctxKey].push({role:'assistant',text:'(Bild)',img:src});
  }).catch(function(e){
    b.className='msg err';b.textContent='Fehler beim Bild: '+e.message;
  });
}
function pickImage(j){
  if(!j)return null;
  var cand=[];
  if(Array.isArray(j.data))cand=cand.concat(j.data);
  if(Array.isArray(j.images))cand=cand.concat(j.images);
  if(j.image)cand.push(j.image);
  for(var i=0;i<cand.length;i++){
    var v=cand[i];
    if(!v)continue;
    if(typeof v==='string')return v.indexOf('data:')===0||v.indexOf('http')===0?v:'data:image/png;base64,'+v;
    if(v.b64_json)return 'data:image/png;base64,'+v.b64_json;
    if(v.base64)return 'data:image/png;base64,'+v.base64;
    if(v.url)return v.url;
  }
  return null;
}

/* ===================== Export / Import ===================== */
function doExport(){
  var data={v:2,people:people,imgs:IMGS,set:{model:set.model,imgmodel:set.imgmodel,
    grund:set.grund,fs:set.fs,imgsize:set.imgsize,sort:set.sort,size:set.size,base:set.base}};
  var json=JSON.stringify(data);
  try{
    var file=new File([json],'steckbriefe.json',{type:'application/json'});
    if(navigator.canShare&&navigator.canShare({files:[file]})){
      navigator.share({files:[file]});return;
    }
  }catch(e){}
  var a=document.createElement('a');
  a.href='data:application/json;charset=utf-8,'+encodeURIComponent(json);
  a.download='steckbriefe.json';a.click();
}
$('pickJson').addEventListener('change',function(){
  var f=this.files&&this.files[0];this.value='';
  if(!f)return;
  var fr=new FileReader();
  fr.onload=function(){
    var d;try{d=JSON.parse(fr.result);}catch(e){toast('Datei nicht lesbar');return;}
    if(!d||!d.people){toast('Keine Steckbriefe in der Datei');return;}
    sheetConfirm('Importieren','Die Steckbriefe aus der Datei werden hinzugefügt.',function(){
      var max=0;people.forEach(function(p){if(p.id>max)max=p.id;});
      d.people.forEach(function(p){max++;p.id=max;p.mig=0;migrate(p);people.push(p);});
      if(d.imgs)for(var k in d.imgs)IMGS[k]=d.imgs[k];
      savePeople();saveImgs();renderHome();refreshAll();toast('Importiert');
    });
  };
  fr.readAsText(f);
});

/* ===================== Buchseite ===================== */
var mtimer=null;
function showBook(){
  $('book').style.display='';$('nav').classList.add('off');$('fab').classList.add('off');
  requestAnimationFrame(function(){$('book').classList.remove('hide');});
  $('pw').value='';fit();
}
function fit(){
  var n=$('pw').value.length;
  var meas=fit._m||(fit._m=document.createElement('span'));
  meas.style.cssText='position:absolute;left:-9999px;top:0;white-space:pre;font-family:Georgia,serif;font-size:1.02rem;';
  if(!meas.parentNode)document.body.appendChild(meas);
  meas.textContent=n?new Array(n+1).join('\u2022'):'Passwort';
  $('pw').style.width=(Math.ceil(meas.getBoundingClientRect().width)+2)+'px';
}
$('pw').addEventListener('input',function(){fit();$('bnote').classList.remove('on');});
$('pw').addEventListener('keydown',function(e){if(e.key==='Enter'){e.preventDefault();pruefen();}});
$('book').addEventListener('click',function(e){if(e.target!==$('pw'))$('pw').focus();});
function pruefen(){
  if($('pw').value===PASS){
    localStorage.setItem(K_UNLOCK,'1');
    $('book').classList.add('hide');
    $('nav').classList.remove('off');$('fab').classList.remove('off');
    setTimeout(function(){$('book').style.display='none';},560);
  }else{
    $('bnote').classList.add('on');
    clearTimeout(mtimer);mtimer=setTimeout(function(){$('bnote').classList.remove('on');},2800);
    $('pw').value='';fit();
  }
}
$('lockBtn').addEventListener('click',function(){localStorage.removeItem(K_UNLOCK);showBook();});

/* ===================== Start ===================== */
init();
renderHome();
markNav();
if(localStorage.getItem(K_UNLOCK)==='1'){$('book').style.display='none';$('book').classList.add('hide');}
else{showBook();}
if('serviceWorker' in navigator){navigator.serviceWorker.register('Offline.js').catch(function(){});}
</script>
</body>
</html>
