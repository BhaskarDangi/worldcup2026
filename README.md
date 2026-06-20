<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8"/>
<meta name="viewport" content="width=device-width,initial-scale=1"/>
<title>FIFA World Cup 2026 — Live Prediction Dashboard</title>
<link href="https://fonts.googleapis.com/css2?family=Bebas+Neue&family=Inter:wght@400;500;600;700&display=swap" rel="stylesheet"/>
<style>
*,*::before,*::after{box-sizing:border-box;margin:0;padding:0}
:root{
  --red:#CC0000;--red2:#E8000A;--blue:#0047AB;--blue2:#1A5FC8;
  --gold:#C9941A;--gold2:#F0B429;--goldf:#FFC833;
  --dark:#0A0D14;--dark2:#111828;--dark3:#1B2236;
  --surface:#1E2A42;--surface2:#263348;
  --border:rgba(255,255,255,.09);--border2:rgba(255,255,255,.18);
  --text:#EEF1FB;--text2:#8B96BE;--text3:#4A5478;
  --green:#1D9E75;--green2:#2DCC91;
  --r:10px;--rlg:14px;
}
body{font-family:'Inter',system-ui,sans-serif;background:var(--dark);color:var(--text);min-height:100vh;font-size:14px}

/* HERO */
.hero{position:relative;overflow:hidden;height:240px}
.hero-img{width:100%;height:100%;object-fit:cover;object-position:center 30%;display:block;filter:brightness(.48) saturate(1.2)}
.hero-overlay{position:absolute;inset:0;background:linear-gradient(105deg,rgba(0,71,171,.78) 0%,rgba(10,13,20,.3) 55%,rgba(204,0,0,.45) 100%)}
.hero-content{position:absolute;inset:0;display:flex;align-items:center;padding:0 36px;gap:24px}
.hero-logo{width:80px;height:80px;object-fit:contain;flex-shrink:0;filter:drop-shadow(0 2px 14px rgba(0,0,0,.7))}
.hero-eyebrow{font-size:10px;font-weight:700;letter-spacing:.16em;text-transform:uppercase;color:var(--goldf);margin-bottom:5px}
.hero-title{font-family:'Bebas Neue',sans-serif;font-size:48px;line-height:.95;color:#fff}
.hero-title span{color:var(--goldf)}
.hero-sub{font-size:12px;color:rgba(255,255,255,.68);margin-top:8px;display:flex;align-items:center;gap:8px;flex-wrap:wrap}
.live-pill{display:inline-flex;align-items:center;gap:4px;background:var(--red2);padding:3px 9px;border-radius:20px;font-size:10px;font-weight:700;letter-spacing:.07em;text-transform:uppercase}
.ldot{width:5px;height:5px;border-radius:50%;background:#fff;animation:blink 1.3s ease-in-out infinite}
@keyframes blink{0%,100%{opacity:1}50%{opacity:.2}}
.hero-trophy{position:absolute;right:32px;bottom:-6px;height:196px;opacity:.18;filter:brightness(1.6)}
.api-status{position:absolute;top:12px;right:16px;font-size:10px;font-weight:600;padding:3px 8px;border-radius:12px;letter-spacing:.05em}
.api-live{background:rgba(29,158,117,.2);border:1px solid var(--green);color:var(--green2)}
.api-offline{background:rgba(255,255,255,.08);border:1px solid var(--border2);color:var(--text3)}

/* NAV */
.nav{background:var(--dark2);border-bottom:1px solid var(--border);overflow-x:auto;scrollbar-width:none;position:sticky;top:0;z-index:100}
.nav::-webkit-scrollbar{display:none}
.nav-inner{max-width:1160px;margin:0 auto;padding:0 20px;display:flex;align-items:center;justify-content:space-between}
.nav-tabs{display:flex}
.tab{padding:13px 17px;font-size:10px;font-weight:700;letter-spacing:.06em;text-transform:uppercase;color:var(--text3);background:none;border:none;border-bottom:2px solid transparent;cursor:pointer;white-space:nowrap;transition:color .14s,border-color .14s;display:flex;align-items:center;gap:6px}
.tab svg{width:13px;height:13px;flex-shrink:0}
.tab:hover{color:var(--text2)}
.tab.on{color:var(--goldf);border-bottom-color:var(--goldf)}
.nav-right{display:flex;align-items:center;gap:8px;padding:0 4px}
.refresh-main{padding:6px 13px;font-size:10px;font-weight:700;letter-spacing:.06em;text-transform:uppercase;background:rgba(29,158,117,.15);border:1px solid rgba(29,158,117,.3);color:var(--green2);border-radius:var(--r);cursor:pointer;display:flex;align-items:center;gap:5px;transition:all .15s}
.refresh-main:hover{background:rgba(29,158,117,.25)}
.refresh-main svg{width:13px;height:13px}
.last-update{font-size:10px;color:var(--text3)}

/* SHELL */
.shell{max-width:1160px;margin:0 auto;padding:0 20px}

/* STATS */
.stats-row{display:grid;grid-template-columns:repeat(5,1fr);gap:10px;padding:18px 0 22px}
.sc{background:var(--surface);border:1px solid var(--border);border-radius:var(--r);padding:12px 14px;position:relative;overflow:hidden}
.sc::before{content:'';position:absolute;left:0;top:0;bottom:0;width:3px}
.sc.gold::before{background:var(--gold2)}
.sc.red::before{background:var(--red2)}
.sc.blue::before{background:var(--blue2)}
.sc.green::before{background:var(--green)}
.sc.purple::before{background:#8B5CF6}
.sc-val{font-family:'Bebas Neue',sans-serif;font-size:26px;line-height:1;padding-left:8px}
.sc.gold .sc-val{color:var(--goldf)}
.sc.red .sc-val{color:#FF6B6B}
.sc.blue .sc-val{color:#6AAFF5}
.sc.green .sc-val{color:var(--green2)}
.sc.purple .sc-val{color:#A78BFA}
.sc-lbl{font-size:9px;font-weight:700;letter-spacing:.08em;text-transform:uppercase;color:var(--text3);padding-left:8px;margin-top:3px}

/* SECTION */
.sec{display:none}.sec.on{display:block}

/* PRIZE */
.prize-row{display:grid;grid-template-columns:repeat(3,1fr);gap:12px;margin-bottom:20px}
.pz{background:var(--surface);border:1px solid var(--border);border-radius:var(--rlg);padding:14px 16px;text-align:center;position:relative;overflow:hidden;transition:border-color .2s}
.pz.p1{border-color:rgba(201,148,26,.5)}
.pz.p2{border-color:rgba(180,180,188,.3)}
.pz.p3{border-color:rgba(205,127,50,.3)}
.pz-icon{font-size:26px;margin-bottom:4px}
.pz-place{font-family:'Bebas Neue',sans-serif;font-size:14px;letter-spacing:.08em;color:var(--text2);margin-bottom:2px}
.pz-amt{font-family:'Bebas Neue',sans-serif;font-size:24px;letter-spacing:.03em}
.pz.p1 .pz-amt{color:var(--goldf)}
.pz.p2 .pz-amt{color:#D0D0D8}
.pz.p3 .pz-amt{color:#E8A060}
.pz-rule{font-size:9px;color:var(--text3);margin-bottom:8px}
.pz-winner{font-size:11px;font-weight:600;color:var(--text2);padding:6px 8px;background:rgba(255,255,255,.04);border-radius:6px;margin-top:6px;min-height:28px;display:flex;align-items:center;justify-content:center;flex-wrap:wrap;gap:3px}
.pz-winner.won{color:var(--goldf);background:rgba(201,148,26,.1);border:1px solid rgba(201,148,26,.2)}
.pz-winner.pending{color:var(--text3)}

/* PROBABILITY CARDS */
.prob-grid{display:grid;grid-template-columns:repeat(auto-fill,minmax(152px,1fr));gap:10px;margin-bottom:20px}
.prob-card{background:var(--surface);border:1px solid var(--border);border-radius:var(--r);padding:12px 13px;transition:border-color .15s,transform .15s}
.prob-card:hover{border-color:var(--border2);transform:translateY(-1px)}
.prob-card.top{border-color:rgba(240,180,41,.4);background:rgba(201,148,26,.06)}
.prob-flag{font-size:20px;margin-bottom:4px}
.prob-team{font-size:11px;font-weight:700;color:var(--text);margin-bottom:2px}
.prob-meta{font-size:9px;color:var(--text3);margin-bottom:7px}
.pbar{background:rgba(255,255,255,.07);border-radius:4px;height:5px;overflow:hidden;margin-bottom:5px}
.pbar-fill{height:100%;border-radius:4px;transition:width .8s ease}
.prob-pct{font-size:13px;font-weight:700;color:var(--goldf)}
.prob-card.top .prob-pct{color:var(--goldf)}
.prob-stage{font-size:9px;color:var(--green2);margin-top:2px;font-weight:600}
.prob-elim{font-size:9px;color:var(--red2);margin-top:2px;font-weight:600}

/* SECTION TITLE */
.stitle{font-size:10px;font-weight:700;letter-spacing:.1em;text-transform:uppercase;color:var(--text3);margin-bottom:12px;display:flex;align-items:center;gap:10px}
.stitle::after{content:'';flex:1;height:1px;background:var(--border)}

/* POINTS TABLE */
.tbl-wrap{overflow-x:auto;border:1px solid var(--border);border-radius:var(--rlg)}
table.pt{width:100%;border-collapse:collapse;min-width:820px}
table.pt thead tr{background:var(--surface2)}
table.pt th{padding:10px 12px;font-size:9px;font-weight:700;letter-spacing:.08em;text-transform:uppercase;color:var(--text3);text-align:left;border-bottom:1px solid var(--border);white-space:nowrap}
table.pt th.c{text-align:center}
table.pt td{padding:9px 12px;font-size:12px;color:var(--text);border-bottom:1px solid var(--border);vertical-align:middle}
table.pt td.c{text-align:center}
table.pt tbody tr:last-child td{border-bottom:none}
table.pt tbody tr:hover{background:var(--dark3)}
table.pt tbody tr.hi1{background:rgba(201,148,26,.08)!important}
table.pt tbody tr.hi2{background:rgba(180,180,188,.05)!important}
table.pt tbody tr.hi3{background:rgba(205,127,50,.05)!important}
table.pt tbody tr.elim td{opacity:.55}
.rbadge{display:inline-flex;align-items:center;justify-content:center;width:22px;height:22px;border-radius:50%;font-size:10px;font-weight:700}
.r1{background:rgba(201,148,26,.2);color:var(--goldf);border:1px solid var(--gold2)}
.r2{background:rgba(184,184,192,.15);color:#D0D0D8;border:1px solid #888}
.r3{background:rgba(205,127,50,.15);color:#E8A060;border:1px solid #cd7f32}
.rn{background:var(--surface2);color:var(--text2);border:1px solid var(--border)}
.team-cell{display:flex;align-items:center;gap:6px;white-space:nowrap}
.tf{font-size:14px}
.pts-val{font-family:'Bebas Neue',sans-serif;font-size:15px;letter-spacing:.04em;color:var(--goldf)}
.pts-val.z{color:var(--text3)}
.pts-val.active{color:var(--green2)}
.rnd-val{font-size:11px;font-weight:600;color:var(--green2)}
.rnd-val.z{color:var(--text3);font-weight:400}
.rnd-val.elim{color:var(--red2)}
.prob-val{font-size:11px;font-weight:700}
.prize-tag{font-size:10px;font-weight:700}
.g1{color:var(--goldf)}
.g2{color:#D0D0D8}
.g3{color:#E8A060}
.stage-tag{font-size:9px;padding:2px 6px;border-radius:4px;font-weight:600;white-space:nowrap}
.st-r32{background:rgba(26,95,200,.2);color:#6AAFF5}
.st-qf{background:rgba(240,180,41,.18);color:var(--goldf)}
.st-sf{background:rgba(29,158,117,.2);color:var(--green2)}
.st-fin{background:rgba(204,0,0,.2);color:#FF6B6B}
.st-champ{background:rgba(201,148,26,.3);color:var(--goldf);border:1px solid var(--gold2)}
.st-grp{background:rgba(255,255,255,.06);color:var(--text3)}
.st-elim{background:rgba(255,255,255,.04);color:var(--text3)}

/* FILTER ROW */
.frow{display:flex;gap:8px;align-items:center;flex-wrap:wrap;margin-bottom:14px}
.srch{position:relative;flex:1;min-width:160px;max-width:260px}
.srch svg{position:absolute;left:10px;top:50%;transform:translateY(-50%);width:13px;height:13px;color:var(--text3);pointer-events:none}
.srch input{width:100%;padding:7px 10px 7px 30px;background:var(--surface);border:1px solid var(--border2);border-radius:var(--r);color:var(--text);font-size:12px;font-family:inherit;outline:none;transition:border-color .14s}
.srch input::placeholder{color:var(--text3)}
.srch input:focus{border-color:var(--gold2)}
.chips{display:flex;gap:5px;flex-wrap:wrap}
.chip{padding:4px 10px;font-size:10px;font-weight:700;border:1px solid var(--border2);border-radius:20px;background:none;color:var(--text2);cursor:pointer;letter-spacing:.03em;transition:all .14s}
.chip:hover{border-color:var(--gold2);color:var(--goldf)}
.chip.on{background:rgba(201,148,26,.14);border-color:var(--gold2);color:var(--goldf)}

/* GROUPS */
.q-legend{display:flex;gap:14px;flex-wrap:wrap;margin-bottom:14px}
.ql{font-size:10px;color:var(--text2);display:flex;align-items:center;gap:5px}
.ql-dot{width:9px;height:9px;border-radius:2px;flex-shrink:0}
.g-grid{display:grid;grid-template-columns:repeat(auto-fill,minmax(240px,1fr));gap:12px}
.g-card{background:var(--surface);border:1px solid var(--border);border-radius:var(--rlg);overflow:hidden}
.g-head{display:flex;align-items:center;justify-content:space-between;padding:9px 13px;background:var(--surface2);border-bottom:1px solid var(--border)}
.g-name{font-family:'Bebas Neue',sans-serif;font-size:14px;letter-spacing:.06em;color:var(--goldf)}
.g-cols{display:flex;gap:8px;font-size:9px;font-weight:700;letter-spacing:.08em;color:var(--text3)}
.g-row{display:grid;grid-template-columns:14px minmax(0,1fr) 19px 19px 19px 19px 26px;gap:5px;align-items:center;padding:7px 13px;border-bottom:1px solid var(--border);transition:background .1s}
.g-row:last-child{border-bottom:none}
.g-row:hover{background:var(--dark3)}
.g-row.adv{border-left:3px solid var(--green);padding-left:10px}
.g-row.poss{border-left:3px solid var(--gold2);padding-left:10px}
.g-pos{font-size:9px;color:var(--text3);text-align:center}
.g-tname{font-size:11px;font-weight:500;color:var(--text);overflow:hidden;white-space:nowrap;text-overflow:ellipsis;display:flex;align-items:center;gap:5px}
.g-fl{font-size:13px;flex-shrink:0}
.g-num{text-align:center;font-size:11px;color:var(--text2)}
.g-pts{text-align:center;font-size:11px;font-weight:600;color:var(--text)}
/* Indicator on group teams that are predicted by participants */
.pred-dot{width:5px;height:5px;border-radius:50%;background:var(--gold2);flex-shrink:0;margin-left:auto;display:none}
.g-row.has-preds .pred-dot{display:block}

/* RESULTS */
.rfbtn{padding:6px 12px;font-size:10px;font-weight:700;letter-spacing:.04em;background:none;border:1px solid var(--border2);border-radius:var(--r);color:var(--text2);cursor:pointer;display:flex;align-items:center;gap:5px;transition:all .14s;margin-left:auto}
.rfbtn svg{width:12px;height:12px}
.spin{animation:spin .7s linear infinite}
@keyframes spin{to{transform:rotate(360deg)}}
.day-lbl{font-size:9px;font-weight:700;letter-spacing:.12em;text-transform:uppercase;color:var(--text3);padding:12px 0 7px;display:flex;align-items:center;gap:10px}
.day-lbl::after{content:'';flex:1;height:1px;background:var(--border)}
.mc{display:grid;grid-template-columns:76px minmax(0,1fr) 54px minmax(0,1fr) 58px;align-items:center;gap:8px;padding:10px 0;border-bottom:1px solid var(--border);transition:background .1s}
.mc:last-child{border-bottom:none}
.mc:hover{background:var(--dark3);margin:0 -8px;padding-left:8px;padding-right:8px;border-radius:var(--r)}
.mc.live-mc{border-left:3px solid var(--red2);padding-left:5px}
.mbadge{font-size:9px;font-weight:700;padding:3px 7px;border-radius:5px;text-align:center;letter-spacing:.04em;white-space:nowrap}
.b-ft{background:rgba(255,255,255,.06);color:var(--text2)}
.b-live{background:var(--red2);color:#fff}
.b-ns{background:rgba(26,95,200,.22);color:#6AAFF5;border:1px solid rgba(106,175,245,.18)}
.mhome{text-align:right;font-size:12px;font-weight:500;color:var(--text);overflow:hidden;white-space:nowrap;text-overflow:ellipsis}
.maway{text-align:left;font-size:12px;font-weight:500;color:var(--text);overflow:hidden;white-space:nowrap;text-overflow:ellipsis}
.dim{color:var(--text3)!important;font-weight:400!important}
.mscore-wrap{text-align:center}
.mscore{font-family:'Bebas Neue',sans-serif;font-size:17px;color:var(--text);letter-spacing:.04em}
.mvs{font-size:10px;color:var(--text3);font-weight:600}
.mven{font-size:9px;color:var(--text3);margin-top:1px;white-space:nowrap;overflow:hidden;text-overflow:ellipsis}
.mgrp{font-size:9px;font-weight:700;letter-spacing:.06em;color:var(--gold2);text-align:right}

/* FETCHING OVERLAY */
.fetching{display:none;position:fixed;bottom:20px;right:20px;background:var(--surface);border:1px solid var(--border2);border-radius:var(--r);padding:10px 14px;font-size:11px;color:var(--text2);align-items:center;gap:8px;z-index:999;box-shadow:0 4px 20px rgba(0,0,0,.4)}
.fetching.show{display:flex}

.empty{text-align:center;padding:36px;color:var(--text3);font-size:12px}
.footer{border-top:1px solid var(--border);padding:20px 0 28px;margin-top:28px;display:flex;align-items:center;justify-content:space-between;flex-wrap:wrap;gap:10px}
.footer-logo{display:flex;align-items:center;gap:8px}
.footer-logo img{height:30px;opacity:.6}
.footer-txt{font-size:10px;color:var(--text3)}

@media(max-width:680px){
  .hero{height:190px}.hero-title{font-size:36px}.hero-trophy{display:none}
  .stats-row{grid-template-columns:repeat(3,1fr)}
  .prize-row{grid-template-columns:1fr}
  .g-grid{grid-template-columns:1fr}
  .mc{grid-template-columns:58px minmax(0,1fr) 44px minmax(0,1fr) 38px;gap:5px}
  .mven{display:none}
}
</style>
</head>
<body>

<div class="hero">
  <img class="hero-img" src="data:image/jpeg;base64,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" alt="FIFA World Cup 2026"/>
  <div class="hero-overlay"></div>
  <div class="hero-content">
    <img class="hero-logo" src="data:image/jpeg;base64,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KxR4Xe394n9BXB6l8SvEOoysd8EMBPywpHkD8Tyaof8ACaa3/wA9of8AvyK+fzPFYmv+7wzUY9+r/DQ9GnkeYU5KdNpP1/4B3lloel6fg21nErD+Nhub8zWh1615n/wmmt/89of+/Io/4TTW/wDntD/35FfOTy3Ezd5ST+b/AMiKnD+YVZc1Sab82/8AI9MorzP/AITTW/8AntD/AN+RR/wmmt/89of+/IqP7Jr91/XyI/1axneP3v8AyPTKK8z/AOE01v8A57Q/9+RR/wAJprf/AD2h/wC/Io/smv3X9fIP9WsZ3j97/wAj0yivM/8AhNNb/wCe0P8A35FH/Caa3/z2h/78ij+ya/df18g/1axneP3v/I9MorzP/hNNb/57Q/8AfkUf8Jprf/PaH/vyKP7Jr91/XyD/AFaxneP3v/I9MorzP/hNNb/57Q/9+RR/wmmt/wDPaH/vyKP7Jr91/XyD/VrGd4/e/wDI9MorzP8A4TTW/wDntD/35FH/AAmmt/8APaH/AL8ij+ya/df18g/1axneP3v/ACPTKK8z/wCE01v/AJ7Q/wDfkUf8Jprf/PaH/vyKP7Jr91/XyD/VrGd4/e/8j0yivM/+E01v/ntD/wB+RR/wmmt/89of+/Io/smv3X9fIP8AVrGd4/e/8jn6KKK+jPvAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKBBRRRQAUUUUAFFFFAwooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAorS/s6L+/J+Y/wo/s6L+/J+Y/wquUz9ojNorS/s6L+/J+Y/wo/s6L+/J+Y/wo5Q9ojNorS/s6L+/J+Y/wo/s6L+/J+Y/wo5Q9ojNorS/s6L+/J+Y/wo/s6L+/J+Y/wo5Q9ojNorS/s6L+/J+Y/wAKP7Oi/vyfmP8ACjlD2iM2itL+zov78n5j/Cj+zov78n5j/CjlD2iM2itL+zov78n5j/Cj+zov78n5j/CjlD2iM2itL+zov78n5j/Cj+zov78n5j/CjlD2iM2itL+zov78n5j/AAo/s6L+/J+Y/wAKOUPaIzaK0v7Oi/vyfmP8KP7Oi/vyfmP8KOUPaIzaK0v7Oi/vyfmP8KP7Oi/vyfmP8KOUPaIzaK0v7Oi/vyfmP8KP7Oi/vyfmP8KOUPaIzaK0v7Oi/vyfmP8ACj+zov78n5j/AAo5Q9ojNorS/s6L+/J+Y/wo/s6L+/J+Y/wo5Q9ojNorS/s6L+/J+Y/wo/s6L+/J+Y/wo5Q9ojNorS/s6L+/J+Y/wo/s6L+/J+Y/wo5Q9ojNorS/s6L+/J+Y/wAKP7Oi/vyfmP8ACjlD2iM2itL+zov78n5j/Cj+zov78n5j/CjlD2iM2itL+zov78n5j/Cj+zov78n5j/CjlD2iM2itL+zov78n5j/Cj+zov78n5j/CjlD2iM2itL+zov78n5j/AAo/s6L+/J+Y/wAKOUPaIzaK0v7Oi/vyfmP8KP7Oi/vyfmP8KOUPaIzaK0v7Oi/vyfmP8KP7Oi/vyfmP8KOUPaIzaK0v7Oi/vyfmP8KP7Oi/vyfmP8KOUPaIzaK0v7Oi/vyfmP8ACj+zov78n5j/AAo5Q9oj/9k=" alt="FIFA 2026 Logo"/>
    <div>
      <div class="hero-eyebrow">Live Prediction Dashboard</div>
      <div class="hero-title">World Cup<br/><span>2026</span></div>
      <div class="hero-sub">
        <span class="live-pill"><span class="ldot"></span>Live Scores</span>
        Jun 11 &ndash; Jul 19 &nbsp;&middot;&nbsp; USA / Canada / Mexico
      </div>
    </div>
  </div>
  <img class="hero-trophy" src="data:image/jpeg;base64,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" alt="FIFA Trophy"/>
  <div class="api-status api-live" id="api-status">&#x25CF; Live data active</div>
</div>

<nav class="nav">
  <div class="nav-inner">
    <div class="nav-tabs">
      <button class="tab on" onclick="goTab('pred',this)">
        <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M9 11l3 3L22 4"/><path d="M21 12v7a2 2 0 01-2 2H5a2 2 0 01-2-2V5a2 2 0 012-2h11"/></svg>
        Prediction Table
      </button>
      <button class="tab" onclick="goTab('groups',this)">
        <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M3 3h7v7H3zM14 3h7v7h-7zM14 14h7v7h-7zM3 14h7v7H3z"/></svg>
        Group Standings
      </button>
      <button class="tab" onclick="goTab('results',this)">
        <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><rect x="3" y="4" width="18" height="18" rx="2"/><path d="M16 2v4M8 2v4M3 10h18"/></svg>
        Results &amp; Fixtures
      </button>
    </div>
    <div class="nav-right">
      <span class="last-update" id="last-update"></span>
      <button class="refresh-main" onclick="fetchLiveData()">
        <svg id="main-ico" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5"><polyline points="23 4 23 10 17 10"/><path d="M20.49 15a9 9 0 11-2.12-9.36L23 10"/></svg>
        Refresh Live
      </button>
    </div>
  </div>
</nav>

<div class="shell">
  <div class="stats-row">
    <div class="sc gold"><div class="sc-val">37</div><div class="sc-lbl">Participants</div></div>
    <div class="sc green"><div class="sc-val" id="sn-played">4</div><div class="sc-lbl">Played</div></div>
    <div class="sc red"><div class="sc-val" id="sn-goals">8</div><div class="sc-lbl">Goals</div></div>
    <div class="sc blue"><div class="sc-val" id="sn-live">0</div><div class="sc-lbl">Live Now</div></div>
    <div class="sc purple"><div class="sc-val">NPR 37K</div><div class="sc-lbl">Prize Pool</div></div>
  </div>

  <!-- PREDICTION -->
  <section id="sec-pred" class="sec on">
    <div class="prize-row">
      <div class="pz p1">
        <div class="pz-icon">&#x1F947;</div>
        <div class="pz-place">1st Prize</div>
        <div class="pz-amt">NPR 18,500</div>
        <div class="pz-rule">Picker of the Champion team</div>
        <div class="pz-winner pending" id="pw1">Tournament in progress&hellip;</div>
      </div>
      <div class="pz p2">
        <div class="pz-icon">&#x1F948;</div>
        <div class="pz-place">2nd Prize</div>
        <div class="pz-amt">NPR 11,100</div>
        <div class="pz-rule">Picker of the Runner-up team</div>
        <div class="pz-winner pending" id="pw2">Tournament in progress&hellip;</div>
      </div>
      <div class="pz p3">
        <div class="pz-icon">&#x1F949;</div>
        <div class="pz-place">3rd Prize</div>
        <div class="pz-amt">NPR 7,400</div>
        <div class="pz-rule">Picker of the 3rd-place team</div>
        <div class="pz-winner pending" id="pw3">Tournament in progress&hellip;</div>
      </div>
    </div>

    <div class="stitle">Win probability — based on tournament performance &amp; FIFA odds</div>
    <div class="prob-grid" id="prob-grid"></div>

    <div class="stitle">Participant prediction table</div>
    <div class="frow">
      <div class="srch">
        <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><circle cx="11" cy="11" r="8"/><path d="M21 21l-4.35-4.35"/></svg>
        <input type="text" id="pt-q" placeholder="Search name&hellip;" oninput="renderTable()"/>
      </div>
      <div class="chips">
        <button class="chip on" onclick="ptf('all',this)">All</button>
        <button class="chip" onclick="ptf('France',this)">&#x1F1EB;&#x1F1F7; France</button>
        <button class="chip" onclick="ptf('Spain',this)">&#x1F1EA;&#x1F1F8; Spain</button>
        <button class="chip" onclick="ptf('Argentina',this)">&#x1F1E6;&#x1F1F7; Argentina</button>
        <button class="chip" onclick="ptf('Portugal',this)">&#x1F1F5;&#x1F1F9; Portugal</button>
        <button class="chip" onclick="ptf('Brazil',this)">&#x1F1E7;&#x1F1F7; Brazil</button>
        <button class="chip" onclick="ptf('Germany',this)">&#x1F1E9;&#x1F1EA; Germany</button>
      </div>
    </div>
    <div class="tbl-wrap">
      <table class="pt">
        <thead><tr>
          <th>Rank</th><th>S.N</th><th>Name</th>
          <th>Paid</th><th>Predicted</th>
          <th class="c">Win Prob%</th>
          <th class="c">Stage</th>
          <th class="c">Grp Pts</th>
          <th class="c">R32 Pts</th>
          <th class="c">QF Pts</th>
          <th class="c">SF Pts</th>
          <th class="c">Final Pts</th>
          <th class="c">Total</th>
          <th class="c">Prize</th>
        </tr></thead>
        <tbody id="pt-body"></tbody>
        <tfoot id="pt-foot"></tfoot>
      </table>
    </div>
  </section>

  <!-- GROUPS -->
  <section id="sec-groups" class="sec">
    <div class="q-legend">
      <div class="ql"><div class="ql-dot" style="background:var(--green)"></div>Advance to Round of 32</div>
      <div class="ql"><div class="ql-dot" style="background:var(--gold2)"></div>Best 3rd-place (possible)</div>
      <div class="ql"><div class="ql-dot" style="background:var(--gold2);border-radius:50%"></div>Picked by participant(s)</div>
    </div>
    <div class="g-grid" id="groups-wrap"></div>
  </section>

  <!-- RESULTS -->
  <section id="sec-results" class="sec">
    <div class="frow">
      <div class="srch">
        <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><circle cx="11" cy="11" r="8"/><path d="M21 21l-4.35-4.35"/></svg>
        <input type="text" id="m-q" placeholder="Search team&hellip;" oninput="renderMatches()"/>
      </div>
      <div class="chips">
        <button class="chip on" onclick="fc('all',this)">All</button>
        <button class="chip" onclick="fc('FT',this)">&#x2713; Final</button>
        <button class="chip" onclick="fc('LIVE',this)">&#x25CF; Live</button>
        <button class="chip" onclick="fc('NS',this)">&#x23F0; Upcoming</button>
      </div>
      <button class="rfbtn" onclick="fetchLiveData()">
        <svg id="rfico" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><polyline points="23 4 23 10 17 10"/><path d="M20.49 15a9 9 0 11-2.12-9.36L23 10"/></svg> Refresh
      </button>
    </div>
    <div id="matches-wrap"></div>
  </section>

  <footer class="footer">
    <div class="footer-logo">
      <img src="data:image/jpeg;base64,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KxR4Xe394n9BXB6l8SvEOoysd8EMBPywpHkD8Tyaof8ACaa3/wA9of8AvyK+fzPFYmv+7wzUY9+r/DQ9GnkeYU5KdNpP1/4B3lloel6fg21nErD+Nhub8zWh1615n/wmmt/89of+/Io/4TTW/wDntD/35FfOTy3Ezd5ST+b/AMiKnD+YVZc1Sab82/8AI9MorzP/AITTW/8AntD/AN+RR/wmmt/89of+/IqP7Jr91/XyI/1axneP3v8AyPTKK8z/AOE01v8A57Q/9+RR/wAJprf/AD2h/wC/Io/smv3X9fIP9WsZ3j97/wAj0yivM/8AhNNb/wCe0P8A35FH/Caa3/z2h/78ij+ya/df18g/1axneP3v/I9MorzP/hNNb/57Q/8AfkUf8Jprf/PaH/vyKP7Jr91/XyD/AFaxneP3v/I9MorzP/hNNb/57Q/9+RR/wmmt/wDPaH/vyKP7Jr91/XyD/VrGd4/e/wDI9MorzP8A4TTW/wDntD/35FH/AAmmt/8APaH/AL8ij+ya/df18g/1axneP3v/ACPTKK8z/wCE01v/AJ7Q/wDfkUf8Jprf/PaH/vyKP7Jr91/XyD/VrGd4/e/8j0yivM/+E01v/ntD/wB+RR/wmmt/89of+/Io/smv3X9fIP8AVrGd4/e/8jn6KKK+jPvAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKBBRRRQAUUUUAFFFFAwooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAorS/s6L+/J+Y/wo/s6L+/J+Y/wquUz9ojNorS/s6L+/J+Y/wo/s6L+/J+Y/wo5Q9ojNorS/s6L+/J+Y/wo/s6L+/J+Y/wo5Q9ojNorS/s6L+/J+Y/wo/s6L+/J+Y/wo5Q9ojNorS/s6L+/J+Y/wAKP7Oi/vyfmP8ACjlD2iM2itL+zov78n5j/Cj+zov78n5j/CjlD2iM2itL+zov78n5j/Cj+zov78n5j/CjlD2iM2itL+zov78n5j/Cj+zov78n5j/CjlD2iM2itL+zov78n5j/AAo/s6L+/J+Y/wAKOUPaIzaK0v7Oi/vyfmP8KP7Oi/vyfmP8KOUPaIzaK0v7Oi/vyfmP8KP7Oi/vyfmP8KOUPaIzaK0v7Oi/vyfmP8KP7Oi/vyfmP8KOUPaIzaK0v7Oi/vyfmP8ACj+zov78n5j/AAo5Q9ojNorS/s6L+/J+Y/wo/s6L+/J+Y/wo5Q9ojNorS/s6L+/J+Y/wo/s6L+/J+Y/wo5Q9ojNorS/s6L+/J+Y/wo/s6L+/J+Y/wo5Q9ojNorS/s6L+/J+Y/wAKP7Oi/vyfmP8ACjlD2iM2itL+zov78n5j/Cj+zov78n5j/CjlD2iM2itL+zov78n5j/Cj+zov78n5j/CjlD2iM2itL+zov78n5j/Cj+zov78n5j/CjlD2iM2itL+zov78n5j/AAo/s6L+/J+Y/wAKOUPaIzaK0v7Oi/vyfmP8KP7Oi/vyfmP8KOUPaIzaK0v7Oi/vyfmP8KP7Oi/vyfmP8KOUPaIzaK0v7Oi/vyfmP8KP7Oi/vyfmP8KOUPaIzaK0v7Oi/vyfmP8ACj+zov78n5j/AAo5Q9oj/9k=" alt="FIFA 2026"/>
      <span class="footer-txt">FIFA World Cup 2026&trade; &nbsp;|&nbsp; Prediction Pool &nbsp;|&nbsp; Total Prize: NPR 37,000</span>
    </div>
    <span class="footer-txt" id="footer-ts"></span>
  </footer>
</div>

<div class="fetching" id="fetching-overlay">
  <svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" style="animation:spin .7s linear infinite"><polyline points="23 4 23 10 17 10"/><path d="M20.49 15a9 9 0 11-2.12-9.36L23 10"/></svg>
  Fetching live scores&hellip;
</div>

<script>
// ═══════════════════════════════════════════════
//  FLAGS
// ═══════════════════════════════════════════════
const F={
  France:'🇫🇷',Spain:'🇪🇸',Argentina:'🇦🇷',Portugal:'🇵🇹',Brazil:'🇧🇷',Germany:'🇩🇪',
  Mexico:'🇲🇽','South Africa':'🇿🇦','South Korea':'🇰🇷','Korea Republic':'🇰🇷',
  Czechia:'🇨🇿','Czech Republic':'🇨🇿',Canada:'🇨🇦','Bosnia and Herzegovina':'🇧🇦',
  Qatar:'🇶🇦',Switzerland:'🇨🇭',Brazil:'🇧🇷',Morocco:'🇲🇦',Haiti:'🇭🇹',
  Scotland:'🏴󠁧󠁢󠁳󠁣󠁴󠁿','United States':'🇺🇸',USA:'🇺🇸',Paraguay:'🇵🇾',
  Australia:'🇦🇺','Türkiye':'🇹🇷',Turkey:'🇹🇷',Curacao:'🇨🇼','Curaçao':'🇨🇼',
  Netherlands:'🇳🇱',Japan:'🇯🇵',Sweden:'🇸🇪',Tunisia:'🇹🇳','Saudi Arabia':'🇸🇦',
  'Cape Verde':'🇨🇻',Belgium:'🇧🇪',Egypt:'🇪🇬','New Zealand':'🇳🇿',
  Iran:'🇮🇷',Algeria:'🇩🇿',Norway:'🇳🇴',Ecuador:'🇪🇨','Ivory Coast':'🇨🇮',
  Uruguay:'🇺🇾',Panama:'🇵🇦','DR Congo':'🇨🇩',Colombia:'🇨🇴',Croatia:'🇭🇷',
  England:'🏴󠁧󠁢󠁥󠁮󠁧󠁿',Uzbekistan:'🇺🇿',Austria:'🇦🇹',Ghana:'🇬🇭',
  Senegal:'🇸🇳',Iraq:'🇮🇶',Jordan:'🇯🇴'
};
const fl=t=>F[t]||'🏳️';

// ═══════════════════════════════════════════════
//  PARTICIPANTS
// ═══════════════════════════════════════════════
const PARTS=[
  {sn:1,name:'Prabhat Sibakoti',team:'Argentina'},
  {sn:2,name:'Reshma Pandey',team:'Portugal'},
  {sn:3,name:'Arnisha Adhikari',team:'Spain'},
  {sn:4,name:'Dil Kumari Paudel',team:'Spain'},
  {sn:5,name:'Smriti Rajouria',team:'Spain'},
  {sn:6,name:'Keshab Karki',team:'France'},
  {sn:7,name:'Sangeeta Niraula Pokhrel',team:'France'},
  {sn:8,name:'Rujee Shrestha',team:'Spain'},
  {sn:9,name:'Govinda Nepal',team:'Argentina'},
  {sn:10,name:'Astha Bhattarai',team:'Argentina'},
  {sn:11,name:'Rakshya Aryal',team:'France'},
  {sn:12,name:'Unni Krishna Pandit',team:'Spain'},
  {sn:13,name:'Alisha Satyal',team:'Spain'},
  {sn:14,name:'Ritendra Dahal',team:'Brazil'},
  {sn:15,name:'Samridhi Singh',team:'France'},
  {sn:16,name:'Rojita Pokhrel',team:'Spain'},
  {sn:17,name:'Rojesh Basnet',team:'Germany'},
  {sn:18,name:'Santosh Prasai',team:'Brazil'},
  {sn:19,name:'Prativa Kuikel',team:'Spain'},
  {sn:20,name:'Lekhnath Gautam',team:'Argentina'},
  {sn:21,name:'Subista Giri',team:'France'},
  {sn:22,name:'Sachin Joshi',team:'France'},
  {sn:23,name:'Nitesh Adhikari',team:'France'},
  {sn:24,name:'Suraj Poudel',team:'France'},
  {sn:25,name:'Nirajan Thapa',team:'Brazil'},
  {sn:26,name:'Bhaskar Bahadur Dangi',team:'Argentina'},
  {sn:27,name:'Ram Bahadur Magar',team:'Brazil'},
  {sn:28,name:'Ishwar Budhathoki',team:'France'},
  {sn:29,name:'Aashish Timilsina',team:'Portugal'},
  {sn:30,name:'Ananta Baral',team:'France'},
  {sn:31,name:'Umesh Bista',team:'Brazil'},
  {sn:32,name:'Bikash K. Upadhaya',team:'Portugal'},
  {sn:33,name:'Prashiddha Dhungana',team:'Portugal'},
  {sn:34,name:'Kishor Pd. Lamichhane',team:'Germany'},
  {sn:35,name:'Yogendra Pd. Khatiwada',team:'France'},
  {sn:36,name:'Surendra Gautam',team:'Spain'},
  {sn:37,name:'Reshik Shrestha',team:'Portugal'},
];

// ═══════════════════════════════════════════════
//  LIVE TEAM PERFORMANCE STATE
//  Points: GroupStage win=3,draw=1,loss=0
//  R32 advance=+5, QF advance=+10, SF advance=+15, Final=+20, Champion=+30
//  Probability = based on wins+stage+FIFA odds blend
// ═══════════════════════════════════════════════
const TEAM_STATE = {
  // FORMAT: {gp,gw,gd,gl,gf,ga,stage,elim}
  // stage: 'group'|'r32'|'qf'|'sf'|'final'|'champion'|'eliminated'
  // points scored at each round
  France:     {gp:0,gw:0,gd:0,gl:0,gf:0,ga:0,stage:'group',pts:{grp:0,r32:0,qf:0,sf:0,fin:0}},
  Spain:      {gp:0,gw:0,gd:0,gl:0,gf:0,ga:0,stage:'group',pts:{grp:0,r32:0,qf:0,sf:0,fin:0}},
  Argentina:  {gp:0,gw:0,gd:0,gl:0,gf:0,ga:0,stage:'group',pts:{grp:0,r32:0,qf:0,sf:0,fin:0}},
  Portugal:   {gp:0,gw:0,gd:0,gl:0,gf:0,ga:0,stage:'group',pts:{grp:0,r32:0,qf:0,sf:0,fin:0}},
  Brazil:     {gp:0,gw:0,gd:0,gl:0,gf:0,ga:0,stage:'group',pts:{grp:0,r32:0,qf:0,sf:0,fin:0}},
  Germany:    {gp:0,gw:0,gd:0,gl:0,gf:0,ga:0,stage:'group',pts:{grp:0,r32:0,qf:0,sf:0,fin:0}},
};

// Base FIFA/bookmaker win probability (%)
const BASE_ODDS = {France:19.5,Spain:20.0,Argentina:17.5,Portugal:12.0,Brazil:13.5,Germany:8.0};

// ── LIVE MATCHES from FIFA.com data ──────────────────────────
// Real confirmed results as of June 13, 2026
let MATCHES = [
  {d:'Thu 11 Jun',h:'Mexico',a:'South Africa',hg:2,ag:0,st:'FT',gr:'A',tm:'FT',ve:'Estadio Azteca, Mexico City'},
  {d:'Fri 12 Jun',h:'Korea Republic',a:'Czechia',hg:2,ag:1,st:'FT',gr:'A',tm:'FT',ve:'Guadalajara Stadium'},
  {d:'Fri 12 Jun',h:'Canada',a:'Bosnia and Herzegovina',hg:1,ag:1,st:'FT',gr:'B',tm:'FT',ve:'Toronto Stadium'},
  {d:'Sat 13 Jun',h:'USA',a:'Paraguay',hg:4,ag:1,st:'FT',gr:'D',tm:'FT',ve:'Los Angeles Stadium'},
  {d:'Sat 13 Jun',h:'Qatar',a:'Switzerland',hg:null,ag:null,st:'NS',gr:'B',tm:'19:00',ve:'San Francisco Bay Area Stadium'},
  {d:'Sat 13 Jun',h:'Brazil',a:'Morocco',hg:null,ag:null,st:'NS',gr:'C',tm:'22:00',ve:'New York/New Jersey Stadium'},
  {d:'Sun 14 Jun',h:'Haiti',a:'Scotland',hg:null,ag:null,st:'NS',gr:'C',tm:'01:00',ve:'Boston Stadium'},
  {d:'Sun 14 Jun',h:'Australia',a:'Türkiye',hg:null,ag:null,st:'NS',gr:'D',tm:'04:00',ve:'BC Place Vancouver'},
  {d:'Sun 14 Jun',h:'Germany',a:'Curaçao',hg:null,ag:null,st:'NS',gr:'E',tm:'17:00',ve:''},
  {d:'Sun 14 Jun',h:'Ecuador',a:'Ivory Coast',hg:null,ag:null,st:'NS',gr:'E',tm:'20:00',ve:''},
  {d:'Sun 14 Jun',h:'Japan',a:'Netherlands',hg:null,ag:null,st:'NS',gr:'F',tm:'23:00',ve:''},
  {d:'Mon 15 Jun',h:'Tunisia',a:'Sweden',hg:null,ag:null,st:'NS',gr:'F',tm:'02:00',ve:''},
  {d:'Mon 15 Jun',h:'Spain',a:'Cape Verde',hg:null,ag:null,st:'NS',gr:'G',tm:'16:00',ve:''},
  {d:'Mon 15 Jun',h:'Saudi Arabia',a:'Uruguay',hg:null,ag:null,st:'NS',gr:'G',tm:'22:00',ve:''},
  {d:'Mon 15 Jun',h:'Belgium',a:'Egypt',hg:null,ag:null,st:'NS',gr:'H',tm:'19:00',ve:''},
  {d:'Tue 16 Jun',h:'New Zealand',a:'Iran',hg:null,ag:null,st:'NS',gr:'I',tm:'01:00',ve:''},
  {d:'Tue 16 Jun',h:'Senegal',a:'France',hg:null,ag:null,st:'NS',gr:'J',tm:'19:00',ve:''},
  {d:'Tue 16 Jun',h:'Norway',a:'Iraq',hg:null,ag:null,st:'NS',gr:'K',tm:'22:00',ve:''},
  {d:'Wed 17 Jun',h:'Algeria',a:'Argentina',hg:null,ag:null,st:'NS',gr:'H',tm:'01:00',ve:''},
  {d:'Wed 17 Jun',h:'Jordan',a:'Austria',hg:null,ag:null,st:'NS',gr:'D',tm:'04:00',ve:''},
  {d:'Wed 17 Jun',h:'DR Congo',a:'Portugal',hg:null,ag:null,st:'NS',gr:'L',tm:'17:00',ve:''},
];

// ── GROUP STANDINGS (real data from FIFA/ESPN Jun 13) ─────────
let GROUPS = [
  {n:'A',t:[
    {n:'Korea Republic',p:1,w:1,d:0,l:0,gf:2,ga:1,pt:3},
    {n:'Mexico',p:1,w:1,d:0,l:0,gf:2,ga:0,pt:3},
    {n:'Czechia',p:1,w:0,d:0,l:1,gf:1,ga:2,pt:0},
    {n:'South Africa',p:1,w:0,d:0,l:1,gf:0,ga:2,pt:0},
  ]},
  {n:'B',t:[
    {n:'Canada',p:1,w:0,d:1,l:0,gf:1,ga:1,pt:1},
    {n:'Bosnia and Herzegovina',p:1,w:0,d:1,l:0,gf:1,ga:1,pt:1},
    {n:'Qatar',p:0,w:0,d:0,l:0,gf:0,ga:0,pt:0},
    {n:'Switzerland',p:0,w:0,d:0,l:0,gf:0,ga:0,pt:0},
  ]},
  {n:'C',t:[{n:'Brazil',p:0,w:0,d:0,l:0,gf:0,ga:0,pt:0},{n:'Morocco',p:0,w:0,d:0,l:0,gf:0,ga:0,pt:0},{n:'Scotland',p:0,w:0,d:0,l:0,gf:0,ga:0,pt:0},{n:'Haiti',p:0,w:0,d:0,l:0,gf:0,ga:0,pt:0}]},
  {n:'D',t:[
    {n:'USA',p:1,w:1,d:0,l:0,gf:4,ga:1,pt:3},
    {n:'Paraguay',p:1,w:0,d:0,l:1,gf:1,ga:4,pt:0},
    {n:'Australia',p:0,w:0,d:0,l:0,gf:0,ga:0,pt:0},
    {n:'Türkiye',p:0,w:0,d:0,l:0,gf:0,ga:0,pt:0},
  ]},
  {n:'E',t:[{n:'Germany',p:0,w:0,d:0,l:0,gf:0,ga:0,pt:0},{n:'Ecuador',p:0,w:0,d:0,l:0,gf:0,ga:0,pt:0},{n:'Ivory Coast',p:0,w:0,d:0,l:0,gf:0,ga:0,pt:0},{n:'Curaçao',p:0,w:0,d:0,l:0,gf:0,ga:0,pt:0}]},
  {n:'F',t:[{n:'Netherlands',p:0,w:0,d:0,l:0,gf:0,ga:0,pt:0},{n:'Japan',p:0,w:0,d:0,l:0,gf:0,ga:0,pt:0},{n:'Sweden',p:0,w:0,d:0,l:0,gf:0,ga:0,pt:0},{n:'Tunisia',p:0,w:0,d:0,l:0,gf:0,ga:0,pt:0}]},
  {n:'G',t:[{n:'Spain',p:0,w:0,d:0,l:0,gf:0,ga:0,pt:0},{n:'Saudi Arabia',p:0,w:0,d:0,l:0,gf:0,ga:0,pt:0},{n:'Cape Verde',p:0,w:0,d:0,l:0,gf:0,ga:0,pt:0},{n:'Uzbekistan',p:0,w:0,d:0,l:0,gf:0,ga:0,pt:0}]},
  {n:'H',t:[{n:'Belgium',p:0,w:0,d:0,l:0,gf:0,ga:0,pt:0},{n:'Argentina',p:0,w:0,d:0,l:0,gf:0,ga:0,pt:0},{n:'Egypt',p:0,w:0,d:0,l:0,gf:0,ga:0,pt:0},{n:'New Zealand',p:0,w:0,d:0,l:0,gf:0,ga:0,pt:0}]},
  {n:'I',t:[{n:'Iran',p:0,w:0,d:0,l:0,gf:0,ga:0,pt:0},{n:'Algeria',p:0,w:0,d:0,l:0,gf:0,ga:0,pt:0},{n:'Norway',p:0,w:0,d:0,l:0,gf:0,ga:0,pt:0},{n:'New Zealand',p:0,w:0,d:0,l:0,gf:0,ga:0,pt:0}]},
  {n:'J',t:[{n:'France',p:0,w:0,d:0,l:0,gf:0,ga:0,pt:0},{n:'Senegal',p:0,w:0,d:0,l:0,gf:0,ga:0,pt:0},{n:'Iraq',p:0,w:0,d:0,l:0,gf:0,ga:0,pt:0},{n:'Panama',p:0,w:0,d:0,l:0,gf:0,ga:0,pt:0}]},
  {n:'K',t:[{n:'Portugal',p:0,w:0,d:0,l:0,gf:0,ga:0,pt:0},{n:'Croatia',p:0,w:0,d:0,l:0,gf:0,ga:0,pt:0},{n:'DR Congo',p:0,w:0,d:0,l:0,gf:0,ga:0,pt:0},{n:'Colombia',p:0,w:0,d:0,l:0,gf:0,ga:0,pt:0}]},
  {n:'L',t:[{n:'England',p:0,w:0,d:0,l:0,gf:0,ga:0,pt:0},{n:'Uzbekistan',p:0,w:0,d:0,l:0,gf:0,ga:0,pt:0},{n:'Austria',p:0,w:0,d:0,l:0,gf:0,ga:0,pt:0},{n:'Ghana',p:0,w:0,d:0,l:0,gf:0,ga:0,pt:0}]},
];

// ═══════════════════════════════════════════════
//  WIN PROBABILITY LOGIC
//  Formula: 
//    groupScore  = (wins * 3 + draws) per match played  → normalized 0-30
//    stageBonus  = r32:+20, qf:+35, sf:+50, final:+65, champ:+100
//    baseOdds    = FIFA market probability (%)
//    finalProb   = (baseOdds * 0.5) + (groupScore * 0.3) + (stageBonus * 0.2)
//    Then normalize across all 6 teams to sum ~100%
// ═══════════════════════════════════════════════
function calcProb(team){
  const s = TEAM_STATE[team];
  const base = BASE_ODDS[team] || 5;
  const gScore = s.gp > 0 ? ((s.gw*3 + s.gd) / (s.gp*3)) * 100 : 50;
  const STAGE_BONUS = {group:0,r32:20,qf:35,sf:50,final:65,champion:100,eliminated:-100};
  const stBonus = STAGE_BONUS[s.stage] || 0;
  if(s.stage === 'eliminated') return 0;
  let prob = (base * 0.5) + (gScore * 0.3) + (stBonus * 0.2);
  return Math.max(0, +prob.toFixed(1));
}

// Points a participant earns based on their team's progress
// Group stage: each win = 3pts, draw = 1pt for their team
// R32 advance = 5pts, QF = 10pts, SF = 15pts, Final = 20pts, Champ = 30pts
function calcParticipantPts(team){
  const s = TEAM_STATE[team];
  const grp = s.gw*3 + s.gd;
  const r32  = s.pts.r32  || 0;
  const qf   = s.pts.qf   || 0;
  const sf   = s.pts.sf   || 0;
  const fin  = s.pts.fin  || 0;
  return {grp, r32, qf, sf, fin, total: grp+r32+qf+sf+fin};
}

function stageLabel(stage){
  const map = {
    group:'Group Stage',r32:'Round of 32',qf:'Quarter-Final',
    sf:'Semi-Final',final:'Final',champion:'Champion 🏆',eliminated:'Eliminated'
  };
  return map[stage] || 'Group Stage';
}
function stageClass(stage){
  const map = {
    group:'st-grp',r32:'st-r32',qf:'st-qf',sf:'st-sf',
    final:'st-fin',champion:'st-champ',eliminated:'st-elim'
  };
  return map[stage] || 'st-grp';
}

// ═══════════════════════════════════════════════
//  RENDER PROBABILITY CARDS
// ═══════════════════════════════════════════════
function renderProbGrid(){
  const raw = Object.keys(TEAM_STATE).map(t=>({t, prob:calcProb(t)}));
  const sum = raw.reduce((s,x)=>s+x.prob,0);
  const normed = raw.map(x=>({...x, norm: sum>0?(x.prob/sum*100).toFixed(1):0}));
  normed.sort((a,b)=>b.norm-a.norm);
  const maxN = Math.max(...normed.map(x=>+x.norm));
  const BAR_COLORS = {France:'#4A90E2',Spain:'#E23D28',Argentina:'#74ACDF',Portugal:'#009246',Brazil:'#009C3B',Germany:'#9CA3AF'};
  const picks = {};
  PARTS.forEach(p=>{ picks[p.team]=(picks[p.team]||0)+1; });

  document.getElementById('prob-grid').innerHTML = normed.map(x=>{
    const s = TEAM_STATE[x.t];
    const top = +x.norm === +maxN;
    const pct = maxN>0 ? Math.round((+x.norm/maxN)*100) : 0;
    const stageInfo = s.stage==='eliminated'
      ? `<div class="prob-elim">&#x274C; Eliminated</div>`
      : `<div class="prob-stage">${stageLabel(s.stage)}</div>`;
    return `<div class="prob-card${top?' top':''}">
      <div class="prob-flag">${fl(x.t)}</div>
      <div class="prob-team">${x.t}</div>
      <div class="prob-meta">${picks[x.t]||0} pickers &middot; GP:${s.gp} W:${s.gw} D:${s.gd} L:${s.gl}</div>
      <div class="pbar"><div class="pbar-fill" style="width:${pct}%;background:${BAR_COLORS[x.t]||'#6AAFF5'}"></div></div>
      <div class="prob-pct">${x.norm}%</div>
      ${stageInfo}
    </div>`;
  }).join('');
}

// ═══════════════════════════════════════════════
//  RENDER TABLE
// ═══════════════════════════════════════════════
let ptFilter='all';
function renderTable(){
  const q=(document.getElementById('pt-q').value||'').toLowerCase();
  const rows = PARTS.map(p=>{
    const ppts = calcParticipantPts(p.team);
    const prob = calcProb(p.team);
    const s = TEAM_STATE[p.team];
    return {...p, ...ppts, prob, stage:s.stage};
  }).filter(p=>{
    if(q&&!p.name.toLowerCase().includes(q))return false;
    if(ptFilter!=='all'&&p.team!==ptFilter)return false;
    return true;
  }).sort((a,b)=>b.total-a.total||b.prob-a.prob);

  // Rank assignment
  let rank=1;
  rows.forEach((r,i)=>{
    if(i>0&&rows[i-1].total===r.total&&rows[i-1].prob===r.prob) r.rank=rows[i-1].rank;
    else{ r.rank=rank; }
    rank=i+2;
  });

  const rb=r=>{
    if(r===1)return`<span class="rbadge r1">1</span>`;
    if(r===2)return`<span class="rbadge r2">2</span>`;
    if(r===3)return`<span class="rbadge r3">3</span>`;
    return`<span class="rbadge rn">${r}</span>`;
  };
  const pzTag=r=>{
    if(r===1)return`<span class="prize-tag g1">🥇 18,500</span>`;
    if(r===2)return`<span class="prize-tag g2">🥈 11,100</span>`;
    if(r===3)return`<span class="prize-tag g3">🥉 7,400</span>`;
    return`<span style="color:var(--text3);font-size:10px">—</span>`;
  };
  const rval=v=>v>0?`<span class="rnd-val">${v}</span>`:`<span class="rnd-val z">—</span>`;
  const hiCls=r=>r===1?'hi1':r===2?'hi2':r===3?'hi3':'';
  const elimCls=s=>s==='eliminated'?'elim':'';
  const probColor=p=>p>=17?'var(--goldf)':p>=10?'var(--green2)':'var(--text2)';

  document.getElementById('pt-body').innerHTML=rows.map(p=>`
    <tr class="${hiCls(p.rank)} ${elimCls(p.stage)}">
      <td>${rb(p.rank)}</td>
      <td style="color:var(--text3);font-size:11px">${p.sn}</td>
      <td style="font-weight:600">${p.name}</td>
      <td style="font-family:'Bebas Neue',sans-serif;font-size:14px;color:var(--green2)">1,000</td>
      <td><div class="team-cell"><span class="tf">${fl(p.team)}</span>${p.team}</div></td>
      <td class="c"><span class="prob-val" style="color:${probColor(p.prob)}">${p.prob}%</span></td>
      <td class="c"><span class="stage-tag ${stageClass(p.stage)}">${stageLabel(p.stage)}</span></td>
      <td class="c">${rval(p.grp)}</td>
      <td class="c">${rval(p.r32)}</td>
      <td class="c">${rval(p.qf)}</td>
      <td class="c">${rval(p.sf)}</td>
      <td class="c">${rval(p.fin)}</td>
      <td class="c"><span class="pts-val${p.total===0?' z':' active'}">${p.total}</span></td>
      <td class="c">${pzTag(p.rank)}</td>
    </tr>`).join('');

  document.getElementById('pt-foot').innerHTML=`
    <tr style="background:var(--dark3)">
      <td colspan="3" style="font-size:11px;color:var(--text2);padding:9px 12px">Total (${rows.length})</td>
      <td style="font-family:'Bebas Neue',sans-serif;font-size:14px;color:var(--goldf);padding:9px 12px">NPR ${(rows.length*1000).toLocaleString()}</td>
      <td colspan="10" style="font-size:10px;color:var(--text3);padding:9px 12px">
        🥇 NPR 18,500 &nbsp;|&nbsp; 🥈 NPR 11,100 &nbsp;|&nbsp; 🥉 NPR 7,400 &nbsp;|&nbsp; Total Prize: NPR 37,000
      </td>
    </tr>`;
}

// ═══════════════════════════════════════════════
//  PRIZE WINNERS
// ═══════════════════════════════════════════════
function renderPrizes(){
  const CHAMP=null, RUNNER=null, THIRD=null;
  ['pw1','pw2','pw3'].forEach((id,i)=>{
    const el=document.getElementById(id);
    const team=[CHAMP,RUNNER,THIRD][i];
    if(!team){el.textContent='Tournament in progress…';el.className='pz-winner pending';return;}
    const pickers=PARTS.filter(p=>p.team===team).map(p=>p.name);
    el.innerHTML=pickers.length?pickers.map(n=>`<span>${n}</span>`).join(', '):'No picker';
    el.className='pz-winner won';
  });
}

// ═══════════════════════════════════════════════
//  GROUPS
// ═══════════════════════════════════════════════
function renderGroups(){
  const predTeams=new Set(PARTS.map(p=>p.team));
  document.getElementById('groups-wrap').innerHTML=GROUPS.map(g=>{
    const s=[...g.t].sort((a,b)=>b.pt-a.pt||(b.gf-b.ga)-(a.gf-a.ga)||b.gf-a.gf);
    return`<div class="g-card">
      <div class="g-head"><span class="g-name">Group ${g.n}</span>
        <div class="g-cols"><span>P</span><span>W</span><span>D</span><span>L</span><span>Pts</span></div>
      </div>
      ${s.map((t,i)=>`<div class="g-row${i<2?' adv':i===2?' poss':''}${predTeams.has(t.n)?' has-preds':''}">
        <span class="g-pos">${i+1}</span>
        <div class="g-tname"><span class="g-fl">${fl(t.n)}</span>${t.n}
          ${predTeams.has(t.n)?`<span style="font-size:9px;color:var(--gold2);margin-left:3px">★</span>`:''}
        </div>
        <span class="g-num">${t.p}</span>
        <span class="g-num">${t.w}</span>
        <span class="g-num">${t.d}</span>
        <span class="g-num">${t.l}</span>
        <span class="g-pts">${t.pt}</span>
      </div>`).join('')}
    </div>`;
  }).join('');
}

// ═══════════════════════════════════════════════
//  MATCHES
// ═══════════════════════════════════════════════
let matchFilter='all';
function renderMatches(){
  const q=(document.getElementById('m-q').value||'').toLowerCase();
  const src=MATCHES.filter(m=>{
    if(q&&!m.h.toLowerCase().includes(q)&&!m.a.toLowerCase().includes(q))return false;
    if(matchFilter==='FT'&&m.st!=='FT')return false;
    if(matchFilter==='LIVE'&&m.st!=='LIVE')return false;
    if(matchFilter==='NS'&&m.st!=='NS')return false;
    return true;
  });
  if(!src.length){document.getElementById('matches-wrap').innerHTML='<div class="empty">No matches found.</div>';return;}
  const byD={};
  src.forEach(m=>{if(!byD[m.d])byD[m.d]=[];byD[m.d].push(m);});
  document.getElementById('matches-wrap').innerHTML=Object.entries(byD).map(([day,ms])=>`
    <div class="day-lbl">${day}</div>
    ${ms.map(m=>{
      const ft=m.st==='FT',live=m.st==='LIVE';
      const bc=live?'b-live':ft?'b-ft':'b-ns';
      const bt=live?'● LIVE':ft?'FT':m.tm;
      const sc=ft||live?`${m.hg}&ndash;${m.ag}`:'<span class="mvs">VS</span>';
      return`<div class="mc${live?' live-mc':''}">
        <span class="mbadge ${bc}">${bt}</span>
        <div class="mhome${ft&&m.hg<m.ag?' dim':''}">${fl(m.h)} ${m.h}</div>
        <div class="mscore-wrap"><div class="mscore">${sc}</div><div class="mven">${m.ve}</div></div>
        <div class="maway${ft&&m.ag<m.hg?' dim':''}">${m.a} ${fl(m.a)}</div>
        <span class="mgrp">GRP ${m.gr}</span>
      </div>`;
    }).join('')}`).join('');

  const played=MATCHES.filter(m=>m.st==='FT').length;
  const goals=MATCHES.filter(m=>m.st==='FT').reduce((s,m)=>s+(m.hg||0)+(m.ag||0),0);
  const live=MATCHES.filter(m=>m.st==='LIVE').length;
  document.getElementById('sn-played').textContent=played;
  document.getElementById('sn-goals').textContent=goals;
  document.getElementById('sn-live').textContent=live;
}

// ═══════════════════════════════════════════════
//  FETCH LIVE DATA via Anthropic API + web_search
// ═══════════════════════════════════════════════
async function fetchLiveData(){
  const ico=document.getElementById('main-ico');
  const rfico=document.getElementById('rfico');
  const overlay=document.getElementById('fetching-overlay');
  ico.classList.add('spin');
  if(rfico)rfico.classList.add('spin');
  overlay.classList.add('show');

  try{
    const res=await fetch('https://api.anthropic.com/v1/messages',{
      method:'POST',
      headers:{'Content-Type':'application/json'},
      body:JSON.stringify({
        model:'claude-sonnet-4-20250514',
        max_tokens:1500,
        tools:[{type:'web_search_20250305',name:'web_search'}],
        messages:[{
          role:'user',
          content:`Search for "FIFA World Cup 2026 live scores results today ${new Date().toLocaleDateString('en-US',{month:'long',day:'numeric',year:'numeric'})}". 

Return ONLY a raw JSON object (no markdown) with this exact structure:
{
  "matches": [
    {"home":"TeamName","away":"TeamName","hg":2,"ag":1,"status":"FT","group":"A","date":"Sat 13 Jun"},
    ...only matches with known results or live now...
  ],
  "standings": {
    "A": [{"name":"Mexico","p":1,"w":1,"d":0,"l":0,"gf":2,"ga":0,"pt":3}, ...],
    ...only groups with played matches...
  }
}`
        }]
      })
    });
    const data=await res.json();
    const text=(data.content||[]).filter(b=>b.type==='text').map(b=>b.text).join('');
    
    // Extract JSON
    const match=text.match(/\{[\s\S]*\}/);
    if(match){
      try{
        const parsed=JSON.parse(match[0]);
        
        // Update matches
        if(parsed.matches&&Array.isArray(parsed.matches)){
          parsed.matches.forEach(pm=>{
            const idx=MATCHES.findIndex(m=>
              (m.h.toLowerCase().includes(pm.home.toLowerCase())||pm.home.toLowerCase().includes(m.h.toLowerCase()))&&
              (m.a.toLowerCase().includes(pm.away.toLowerCase())||pm.away.toLowerCase().includes(m.a.toLowerCase()))
            );
            if(idx>=0){
              MATCHES[idx].hg=pm.hg;
              MATCHES[idx].ag=pm.ag;
              MATCHES[idx].st=pm.status||'FT';
            }
          });
        }
        
        // Update group standings
        if(parsed.standings){
          Object.entries(parsed.standings).forEach(([grp,teams])=>{
            const gi=GROUPS.findIndex(g=>g.n===grp);
            if(gi>=0&&Array.isArray(teams)){
              teams.forEach(rt=>{
                const ti=GROUPS[gi].t.findIndex(t=>
                  t.n.toLowerCase().includes(rt.name.toLowerCase())||rt.name.toLowerCase().includes(t.n.toLowerCase())
                );
                if(ti>=0){
                  GROUPS[gi].t[ti]={...GROUPS[gi].t[ti],...rt,n:GROUPS[gi].t[ti].n};
                }
              });
            }
          });
        }
        
        // Update team state from standings for predicted teams
        ['France','Spain','Argentina','Portugal','Brazil','Germany'].forEach(team=>{
          GROUPS.forEach(g=>{
            const found=g.t.find(t=>t.n.toLowerCase()===team.toLowerCase()||
              t.n.toLowerCase().replace('ü','u')===team.toLowerCase().replace('ü','u'));
            if(found){
              TEAM_STATE[team].gp=found.p||0;
              TEAM_STATE[team].gw=found.w||0;
              TEAM_STATE[team].gd=found.d||0;
              TEAM_STATE[team].gl=found.l||0;
              TEAM_STATE[team].gf=found.gf||0;
              TEAM_STATE[team].ga=found.ga||0;
            }
          });
        });

        document.getElementById('api-status').className='api-status api-live';
        document.getElementById('api-status').textContent='● Live data active';
        refreshAll();
      }catch(e){console.warn('Parse error',e);}
    }
  }catch(e){
    document.getElementById('api-status').className='api-status api-offline';
    document.getElementById('api-status').textContent='Offline mode';
    console.warn('Fetch error',e);
  }
  
  ico.classList.remove('spin');
  if(rfico)rfico.classList.remove('spin');
  overlay.classList.remove('show');
  setTs();
}

function refreshAll(){
  renderProbGrid();
  renderPrizes();
  renderTable();
  renderGroups();
  renderMatches();
}

function goTab(id,btn){
  document.querySelectorAll('.sec').forEach(s=>s.classList.remove('on'));
  document.querySelectorAll('.tab').forEach(b=>b.classList.remove('on'));
  document.getElementById('sec-'+id).classList.add('on');
  btn.classList.add('on');
  if(id==='results')renderMatches();
}
function ptf(f,btn){
  ptFilter=f;
  document.querySelectorAll('#sec-pred .chips .chip').forEach(c=>c.classList.remove('on'));
  btn.classList.add('on');
  renderTable();
}
function fc(f,btn){
  matchFilter=f;
  document.querySelectorAll('#sec-results .chip').forEach(c=>c.classList.remove('on'));
  btn.classList.add('on');
  renderMatches();
}
function setTs(){
  const el=document.getElementById('footer-ts');
  const up=document.getElementById('last-update');
  const t=new Date().toLocaleTimeString('en-US',{hour:'2-digit',minute:'2-digit'});
  if(el)el.textContent='Updated '+t;
  if(up)up.textContent='Updated '+t;
}

// Auto-refresh every 90s
setInterval(()=>fetchLiveData(),90000);

// INIT
refreshAll();
setTs();
// Fetch live data on load
fetchLiveData();
</script>
</body>
</html>
