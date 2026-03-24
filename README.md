<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>PACEFIT</title>
<link href="https://fonts.googleapis.com/css2?family=Cormorant+Garamond:ital,wght@0,300;0,400;0,500;1,300;1,400&family=Montserrat:wght@300;400;500&family=Bebas+Neue&family=Syne:wght@400;600;700;800&family=Syne+Mono&display=swap" rel="stylesheet">
<style>
:root {
  --white:#ffffff; --bg:#F4F6F8; --card:#ffffff; --border:#E8EAED;
  --text:#111111; --sub:#6B7280; --muted:#9CA3AF; --blue:#1A56DB;
  --blue-lt:#EBF1FF; --blue-md:#3B82F6; --lime:#c8f000; --red:#EF4444;
  --green:#22C55E; --orange:#F97316; --dark:#050507; --nav-h:68px; --r:10px;
}
*,*::before,*::after{box-sizing:border-box;margin:0;padding:0}
html{font-size:16px;-webkit-font-smoothing:antialiased}
body{background:var(--dark);font-family:'Syne',sans-serif;min-height:100vh;overflow-x:hidden}
.screen{display:none;max-width:430px;margin:0 auto;min-height:100vh;padding-bottom:var(--nav-h);position:relative}
.screen.active{display:block}
.screen.light{background:var(--bg);color:var(--text)}
.topbar{display:flex;align-items:center;justify-content:space-between;padding:20px 18px 14px}
.topbar.light-bar{border-bottom:1px solid var(--border);background:var(--white)}
.brand{font-family:'Bebas Neue',sans-serif;font-size:26px;letter-spacing:4px;color:var(--blue)}
.page-title{font-family:'Bebas Neue',sans-serif;font-size:22px;letter-spacing:3px;color:var(--text)}
.topbar-right{display:flex;align-items:center;gap:10px}
.avatar-chip{width:34px;height:34px;border-radius:50%;background:var(--blue);display:flex;align-items:center;justify-content:center;font-family:'Bebas Neue',sans-serif;font-size:13px;color:#fff;letter-spacing:1px;cursor:pointer}
.icon-btn{width:34px;height:34px;border-radius:50%;border:none;background:var(--blue-lt);display:flex;align-items:center;justify-content:center;cursor:pointer}
.icon-btn svg{width:16px;height:16px}
@keyframes blink{0%,100%{opacity:1}50%{opacity:.2}}
@keyframes fadeUp{from{opacity:0;transform:translateY(10px)}to{opacity:1;transform:translateY(0)}}
@keyframes drawPath{from{stroke-dashoffset:700}to{stroke-dashoffset:0}}
@keyframes fillGrow{from{width:0}}
@keyframes arcDraw{from{stroke-dashoffset:427}to{stroke-dashoffset:115}}
@keyframes hintFade{0%{opacity:0}20%{opacity:1}80%{opacity:1}100%{opacity:0}}
.h-summary{background:var(--blue);padding:20px 18px;display:grid;grid-template-columns:repeat(3,1fr)}
.h-sum-item{text-align:center}
.h-sum-item+.h-sum-item{border-left:1px solid rgba(255,255,255,.15)}
.h-sum-val{font-family:'Bebas Neue',sans-serif;font-size:30px;letter-spacing:1px;color:#fff;line-height:1}
.h-sum-val small{font-size:14px;opacity:.7}
.h-sum-lbl{font-family:'Syne Mono',monospace;font-size:8px;letter-spacing:2px;color:rgba(255,255,255,.65);text-transform:uppercase;margin-top:3px}
.h-month-strip{background:var(--white);border-bottom:1px solid var(--border);padding:14px 18px 10px}
.h-month-label{font-family:'Bebas Neue',sans-serif;font-size:20px;letter-spacing:3px;color:var(--text);margin-bottom:10px}
.h-cal-grid{display:grid;grid-template-columns:repeat(7,1fr);gap:4px}
.h-cal-day{aspect-ratio:1;border-radius:50%;display:flex;align-items:center;justify-content:center;font-family:'Syne Mono',monospace;font-size:9px;color:var(--muted)}
.h-cal-day.has-workout{background:var(--blue-lt);color:var(--blue);font-weight:700}
.h-cal-day.today{background:var(--blue);color:#fff}
.h-cal-day.rest{opacity:.3}
.h-section{padding:14px 18px 6px;display:flex;justify-content:space-between;align-items:center}
.h-section-title{font-family:'Syne Mono',monospace;font-size:9px;letter-spacing:4px;color:var(--muted);text-transform:uppercase}
.h-section-filter{font-family:'Syne Mono',monospace;font-size:8px;letter-spacing:2px;color:var(--blue);text-transform:uppercase;background:none;border:none;cursor:pointer}
.h-list{padding:0 18px 8px;display:flex;flex-direction:column;gap:10px}
.wcard{background:var(--white);border:1px solid var(--border);border-radius:var(--r);overflow:hidden;cursor:pointer;transition:box-shadow .15s;animation:fadeUp .35s ease both}
.wcard:hover{box-shadow:0 3px 12px rgba(26,86,219,.08)}
.wcard:nth-child(1){animation-delay:.04s}.wcard:nth-child(2){animation-delay:.09s}.wcard:nth-child(3){animation-delay:.14s}.wcard:nth-child(4){animation-delay:.19s}
.wcard-header{display:flex;align-items:center;gap:12px;padding:14px 14px 10px;border-bottom:1px solid var(--border)}
.wcard-icon-wrap{width:42px;height:42px;border-radius:8px;display:flex;align-items:center;justify-content:center;flex-shrink:0}
.wcard-icon-wrap svg{width:20px;height:20px}
.wcard-icon-wrap.run{background:rgba(200,240,0,.12)}.wcard-icon-wrap.lift{background:rgba(239,68,68,.1)}.wcard-icon-wrap.bike{background:rgba(26,86,219,.1)}.wcard-icon-wrap.yoga{background:rgba(129,140,248,.1)}
.wcard-header-info{flex:1}
.wcard-title{font-family:'Bebas Neue',sans-serif;font-size:19px;letter-spacing:2px;color:var(--text);line-height:1}
.wcard-date{font-family:'Syne Mono',monospace;font-size:9px;letter-spacing:1px;color:var(--muted);margin-top:3px}
.wcard-badge{font-family:'Syne Mono',monospace;font-size:8px;letter-spacing:1px;padding:3px 8px;border-radius:4px;text-transform:uppercase}
.wcard-badge.pr{background:#FEF3C7;color:#D97706}.wcard-badge.good{background:rgba(34,197,94,.1);color:var(--green)}
.wcard-stats{display:grid;grid-template-columns:repeat(4,1fr);padding:12px 14px}
.wcard-stat+.wcard-stat{border-left:1px solid var(--border);padding-left:12px}
.wcard-stat-val{font-family:'Bebas Neue',sans-serif;font-size:20px;letter-spacing:1px;color:var(--text);line-height:1}
.wcard-stat-val small{font-size:11px;color:var(--muted)}
.wcard-stat-lbl{font-family:'Syne Mono',monospace;font-size:7px;letter-spacing:2px;color:var(--muted);text-transform:uppercase;margin-top:2px}
.r-header{background:var(--white);border-bottom:1px solid var(--border);padding:16px 18px 14px;display:flex;align-items:flex-end;justify-content:space-between}
.r-eyebrow{font-family:'Syne Mono',monospace;font-size:9px;letter-spacing:4px;color:var(--muted);text-transform:uppercase;margin-bottom:4px}
.r-heading{font-family:'Bebas Neue',sans-serif;font-size:34px;letter-spacing:3px;color:var(--text);line-height:1}
.r-new-btn{display:flex;align-items:center;gap:6px;background:var(--blue);color:#fff;border:none;border-radius:6px;padding:9px 14px;font-family:'Syne Mono',monospace;font-size:10px;letter-spacing:2px;text-transform:uppercase;cursor:pointer;transition:background .15s}
.r-new-btn:hover{background:#1649C0}
.r-filters{display:flex;gap:8px;padding:12px 18px;background:var(--white);border-bottom:1px solid var(--border)}
.r-pill{font-family:'Syne Mono',monospace;font-size:9px;letter-spacing:2px;text-transform:uppercase;padding:5px 14px;border-radius:20px;border:1px solid var(--border);background:none;color:var(--muted);cursor:pointer;transition:all .12s}
.r-pill.on{background:var(--blue);border-color:var(--blue);color:#fff}
.r-list{padding:14px 18px;display:flex;flex-direction:column;gap:14px}
.route-card{background:var(--white);border:1px solid var(--border);border-radius:var(--r);overflow:hidden;box-shadow:0 1px 3px rgba(0,0,0,.05);cursor:pointer;transition:box-shadow .15s,transform .15s;animation:fadeUp .4s ease both}
.route-card:hover{box-shadow:0 4px 16px rgba(26,86,219,.1);transform:translateY(-1px)}
.route-card:nth-child(1){animation-delay:.04s}.route-card:nth-child(2){animation-delay:.10s}.route-card:nth-child(3){animation-delay:.16s}
.rc-map{position:relative;height:130px;overflow:hidden}
.rc-map svg{width:100%;height:100%}
.rc-badge{position:absolute;top:9px;left:9px;font-family:'Syne Mono',monospace;font-size:8px;letter-spacing:2px;text-transform:uppercase;padding:3px 8px;border-radius:4px}
.rc-badge.run{background:rgba(26,86,219,.12);color:var(--blue)}.rc-badge.ride{background:rgba(34,197,94,.12);color:#16A34A}
.rc-dist{position:absolute;top:9px;right:9px;font-family:'Bebas Neue',sans-serif;font-size:15px;letter-spacing:2px;color:var(--blue);background:rgba(255,255,255,.92);padding:2px 8px;border-radius:4px;border:1px solid var(--blue-lt)}
.rc-body{padding:12px 14px;border-top:1px solid var(--border)}
.rc-name{font-family:'Bebas Neue',sans-serif;font-size:20px;letter-spacing:2px;color:var(--text);margin-bottom:8px}
.rc-stats{display:grid;grid-template-columns:repeat(4,1fr);gap:4px;margin-bottom:10px}
.rc-stat-val{font-family:'Bebas Neue',sans-serif;font-size:18px;letter-spacing:1px;color:var(--text);line-height:1}
.rc-stat-val.blue{color:var(--blue)}.rc-stat-val span{font-size:10px;color:var(--muted)}
.rc-stat-lbl{font-family:'Syne Mono',monospace;font-size:7px;letter-spacing:2px;color:var(--muted);text-transform:uppercase;margin-top:2px}
.rc-footer{display:flex;align-items:center;justify-content:space-between;border-top:1px solid var(--border);padding-top:9px}
.rc-last{font-family:'Syne Mono',monospace;font-size:9px;letter-spacing:1px;color:var(--muted)}
.rc-go-btn{display:flex;align-items:center;gap:5px;background:var(--blue);color:#fff;border:none;border-radius:6px;padding:7px 14px;font-family:'Syne Mono',monospace;font-size:9px;letter-spacing:2px;text-transform:uppercase;cursor:pointer;transition:background .15s}
.rc-go-btn:hover{background:#1649C0}
.route-path{animation:drawPath 1s cubic-bezier(.23,1,.32,1) both}
.route-card:nth-child(1) .route-path{animation-delay:.1s}.route-card:nth-child(2) .route-path{animation-delay:.25s}.route-card:nth-child(3) .route-path{animation-delay:.4s}
.p-hero{background:linear-gradient(135deg,#1A56DB 0%,#1649C0 100%);padding:28px 18px 24px;position:relative;overflow:hidden}
.p-hero::before{content:'';position:absolute;top:-40px;right:-40px;width:180px;height:180px;border-radius:50%;background:rgba(255,255,255,.06)}
.p-hero::after{content:'';position:absolute;bottom:-60px;left:-30px;width:200px;height:200px;border-radius:50%;background:rgba(255,255,255,.04)}
.p-hero-inner{position:relative;z-index:1}
.p-avatar-row{display:flex;align-items:center;gap:14px;margin-bottom:18px}
.p-avatar{width:64px;height:64px;border-radius:50%;background:rgba(255,255,255,.2);border:2px solid rgba(255,255,255,.4);display:flex;align-items:center;justify-content:center;font-family:'Bebas Neue',sans-serif;font-size:24px;letter-spacing:2px;color:#fff}
.p-name{font-family:'Bebas Neue',sans-serif;font-size:26px;letter-spacing:3px;color:#fff;line-height:1}
.p-handle{font-family:'Syne Mono',monospace;font-size:10px;letter-spacing:2px;color:rgba(255,255,255,.65);margin-top:3px}
.p-since{font-family:'Syne Mono',monospace;font-size:9px;letter-spacing:2px;color:rgba(255,255,255,.5);margin-top:2px}
.p-stats-row{display:grid;grid-template-columns:repeat(3,1fr)}
.p-stat{text-align:center}
.p-stat+.p-stat{border-left:1px solid rgba(255,255,255,.15)}
.p-stat-val{font-family:'Bebas Neue',sans-serif;font-size:28px;letter-spacing:1px;color:#fff;line-height:1}
.p-stat-val small{font-size:13px;opacity:.7}
.p-stat-lbl{font-family:'Syne Mono',monospace;font-size:7px;letter-spacing:2px;color:rgba(255,255,255,.6);text-transform:uppercase;margin-top:3px}
.p-prs{margin:14px 18px;background:var(--white);border:1px solid var(--border);border-radius:var(--r);overflow:hidden}
.p-pr-header{padding:12px 14px;border-bottom:1px solid var(--border);font-family:'Syne Mono',monospace;font-size:9px;letter-spacing:4px;color:var(--muted);text-transform:uppercase}
.p-pr-grid{display:grid;grid-template-columns:1fr 1fr}
.p-pr-item{padding:12px 14px}
.p-pr-item+.p-pr-item:nth-child(2n){border-left:1px solid var(--border)}
.p-pr-item:nth-child(n+3){border-top:1px solid var(--border)}
.p-pr-event{font-family:'Syne Mono',monospace;font-size:8px;letter-spacing:2px;color:var(--muted);text-transform:uppercase;margin-bottom:4px}
.p-pr-val{font-family:'Bebas Neue',sans-serif;font-size:22px;letter-spacing:1px;color:var(--text);line-height:1}
.p-pr-val small{font-size:11px;color:var(--muted)}
.p-pr-date{font-family:'Syne Mono',monospace;font-size:8px;letter-spacing:1px;color:var(--blue);margin-top:2px}
.p-section-label{padding:14px 18px 6px;font-family:'Syne Mono',monospace;font-size:9px;letter-spacing:4px;color:var(--muted);text-transform:uppercase}
.p-settings{margin:0 18px 10px;background:var(--white);border:1px solid var(--border);border-radius:var(--r);overflow:hidden}
.p-row{display:flex;align-items:center;justify-content:space-between;padding:14px 16px;border-bottom:1px solid var(--border);cursor:pointer;transition:background .12s}
.p-row:hover{background:var(--bg)}.p-row:last-child{border-bottom:none}
.p-row-left{display:flex;align-items:center;gap:12px}
.p-row-icon{width:34px;height:34px;border-radius:8px;background:var(--blue-lt);display:flex;align-items:center;justify-content:center}
.p-row-label{font-family:'Syne',sans-serif;font-size:14px;font-weight:600;color:var(--text)}
.p-row-sub{font-family:'Syne Mono',monospace;font-size:9px;letter-spacing:1px;color:var(--muted);margin-top:1px}
.p-row-right{display:flex;align-items:center;gap:8px}
.p-row-value{font-family:'Syne Mono',monospace;font-size:10px;letter-spacing:1px;color:var(--muted)}
.p-chevron svg{width:14px;height:14px}
.toggle{width:40px;height:22px;background:var(--blue);border-radius:11px;position:relative;cursor:pointer}
.toggle::after{content:'';position:absolute;top:3px;left:3px;width:16px;height:16px;border-radius:50%;background:#fff;transition:transform .2s;transform:translateX(18px)}
.toggle.off{background:var(--border)}.toggle.off::after{transform:translateX(0)}
.p-row.danger .p-row-label{color:var(--red)}.p-row.danger .p-row-icon{background:rgba(239,68,68,.08)}
nav{position:fixed;bottom:0;left:50%;transform:translateX(-50%);width:430px;max-width:100vw;height:var(--nav-h);background:var(--white);border-top:1px solid var(--border);display:flex;align-items:center;justify-content:space-around;padding:0 0 10px;z-index:1000}
.nbtn{display:flex;flex-direction:column;align-items:center;gap:4px;background:none;border:none;cursor:pointer;color:var(--muted);padding:4px 12px;transition:color .15s;flex:1}
.nbtn.on{color:var(--blue)}.nbtn svg{width:22px;height:22px}
.nbtn-label{font-family:'Syne Mono',monospace;font-size:7px;letter-spacing:2px;text-transform:uppercase}
.nstart{width:50px;height:50px;border-radius:50%;background:var(--blue);border:none;cursor:pointer;display:flex;align-items:center;justify-content:center;margin-top:-18px;box-shadow:0 4px 16px rgba(26,86,219,.35);transition:transform .15s,box-shadow .15s;flex-shrink:0}
.nstart:hover{transform:scale(1.07);box-shadow:0 6px 24px rgba(26,86,219,.5)}.nstart svg{width:20px;height:20px}
.sdot{width:5px;height:5px;border-radius:50%;background:#ddd;cursor:pointer;transition:background .3s,transform .3s}
.sdot.active{background:#111;transform:scale(1.3)}
#today{overflow:hidden !important;height:100vh !important;padding-bottom:0 !important}
#today.active{display:block !important}
.nbtn[data-tab="settings"].on{color:#1A56DB}
#settings input:focus,#settings textarea:focus{background:#FAFAFA !important}
</style>
</head>
<body>
<div id="today" class="screen active" style="background:#fafafa;overflow:hidden;height:100vh;padding-bottom:0;">
  <div id="voiceToast" style="position:fixed;top:24px;left:50%;transform:translateX(-50%) translateY(-140px);z-index:9999;background:#111;color:#fafafa;font-family:'Montserrat',sans-serif;font-size:11px;font-weight:300;letter-spacing:2px;padding:12px 24px;border-radius:2px;transition:transform .5s cubic-bezier(.23,1,.32,1);white-space:nowrap;text-transform:uppercase;pointer-events:none;">20 min · Well done, Ferdinand</div>
  <div style="position:absolute;top:0;left:0;right:0;z-index:100;display:flex;align-items:center;justify-content:space-between;padding:28px 28px 0;">
    <div style="font-family:'Montserrat',sans-serif;font-size:10px;font-weight:500;letter-spacing:5px;color:#111;text-transform:uppercase;">Pacefit</div>
    <div style="display:flex;align-items:center;gap:16px;">
      <div style="font-family:'Montserrat',sans-serif;font-size:9px;font-weight:300;letter-spacing:3px;color:#999;text-transform:uppercase;">Sun, Mar 22</div>
      <div style="width:30px;height:30px;border-radius:50%;background:#111;display:flex;align-items:center;justify-content:center;font-family:'Montserrat',sans-serif;font-size:10px;font-weight:500;color:#fafafa;letter-spacing:1px;cursor:pointer;" onclick="showTab('profile')">FA</div>
    </div>
  </div>
  <div style="position:relative;width:100%;height:100%;overflow:hidden;">
    <div id="slideTrack" style="display:flex;width:600%;height:100%;transition:transform .9s cubic-bezier(.77,0,.175,1);">

      <div class="dior-slide" style="flex:0 0 calc(100%/6);height:100%;background:#fafafa;display:flex;flex-direction:column;justify-content:center;padding:80px 36px 100px;">
        <div style="font-family:'Cormorant Garamond',serif;font-style:italic;font-weight:300;font-size:13px;letter-spacing:4px;color:#bbb;text-transform:uppercase;margin-bottom:48px;">Good morning</div>
        <div style="font-family:'Cormorant Garamond',serif;font-weight:300;font-size:72px;line-height:.9;letter-spacing:-1px;color:#111;margin-bottom:8px;">Ferdinand.</div>
        <div style="width:40px;height:1px;background:#111;margin:40px 0;"></div>
        <div style="font-family:'Montserrat',sans-serif;font-weight:300;font-size:11px;letter-spacing:4px;color:#999;text-transform:uppercase;margin-bottom:36px;">Daily Progress</div>
        <div style="position:relative;width:160px;height:160px;margin-bottom:36px;">
          <svg viewBox="0 0 160 160" style="width:100%;height:100%;transform:rotate(-90deg);">
            <circle cx="80" cy="80" r="68" fill="none" stroke="#f0f0f0" stroke-width="1.5"/>
            <circle cx="80" cy="80" r="68" fill="none" stroke="#111" stroke-width="1.5" stroke-dasharray="427" stroke-dashoffset="115" stroke-linecap="round" style="animation:arcDraw 2s cubic-bezier(.23,1,.32,1) both .3s"/>
          </svg>
          <div style="position:absolute;inset:0;display:flex;flex-direction:column;align-items:center;justify-content:center;">
            <div style="font-family:'Cormorant Garamond',serif;font-weight:300;font-size:40px;color:#111;line-height:1;">73</div>
            <div style="font-family:'Montserrat',sans-serif;font-weight:300;font-size:9px;letter-spacing:3px;color:#bbb;text-transform:uppercase;">percent</div>
          </div>
        </div>
        <div style="display:flex;flex-direction:column;gap:14px;">
          <div style="display:flex;justify-content:space-between;align-items:baseline;">
            <span style="font-family:'Montserrat',sans-serif;font-weight:300;font-size:9px;letter-spacing:3px;color:#bbb;text-transform:uppercase;">Steps</span>
            <span style="font-family:'Cormorant Garamond',serif;font-weight:400;font-size:20px;color:#111;">7,341 <span style="font-size:12px;color:#bbb;">/ 10,000</span></span>
          </div>
          <div style="height:1px;background:#f0f0f0;position:relative;"><div style="position:absolute;top:0;left:0;height:1px;background:#111;animation:fillGrow 1.5s cubic-bezier(.23,1,.32,1) both .8s;width:73.4%;"></div></div>
          <div style="display:flex;justify-content:space-between;align-items:baseline;">
            <span style="font-family:'Montserrat',sans-serif;font-weight:300;font-size:9px;letter-spacing:3px;color:#bbb;text-transform:uppercase;">Calories</span>
            <span style="font-family:'Cormorant Garamond',serif;font-weight:400;font-size:20px;color:#111;">486 <span style="font-size:12px;color:#bbb;">/ 650</span></span>
          </div>
          <div style="height:1px;background:#f0f0f0;position:relative;"><div style="position:absolute;top:0;left:0;height:1px;background:#111;animation:fillGrow 1.5s cubic-bezier(.23,1,.32,1) both 1s;width:74.8%;"></div></div>
        </div>
      </div>

      <div class="dior-slide" style="flex:0 0 calc(100%/6);height:100%;background:#111;display:flex;flex-direction:column;justify-content:center;padding:80px 36px 100px;">
        <div style="font-family:'Montserrat',sans-serif;font-weight:300;font-size:9px;letter-spacing:5px;color:#555;text-transform:uppercase;margin-bottom:60px;">Heart Rate</div>
        <div style="font-family:'Cormorant Garamond',serif;font-style:italic;font-weight:300;font-size:110px;line-height:.85;color:#fafafa;letter-spacing:-2px;" id="hrSlide">72</div>
        <div style="font-family:'Montserrat',sans-serif;font-weight:300;font-size:11px;letter-spacing:5px;color:#555;text-transform:uppercase;margin-top:16px;">bpm</div>
        <div style="width:40px;height:1px;background:#333;margin:40px 0;"></div>
        <div style="font-family:'Cormorant Garamond',serif;font-style:italic;font-size:16px;color:#666;letter-spacing:1px;">Resting · Normal</div>
        <div style="margin-top:48px;height:40px;opacity:.4;">
          <svg viewBox="0 0 300 40" preserveAspectRatio="none" style="width:100%;height:100%;">
            <path d="M0,32 Q20,32 25,20 T40,32 Q55,32 60,10 T80,32 Q100,32 105,24 T120,32 Q138,32 142,14 T160,32 Q178,32 182,4 T200,32 Q218,32 222,20 T240,32 Q258,32 262,12 T280,32 Q290,32 295,22 T300,28" fill="none" stroke="#fafafa" stroke-width="1" stroke-linecap="round"/>
          </svg>
        </div>
      </div>

      <div class="dior-slide" style="flex:0 0 calc(100%/6);height:100%;background:#fafafa;display:flex;flex-direction:column;justify-content:center;padding:80px 36px 100px;">
        <div style="font-family:'Montserrat',sans-serif;font-weight:300;font-size:9px;letter-spacing:5px;color:#bbb;text-transform:uppercase;margin-bottom:60px;">Today's Distance</div>
        <div style="font-family:'Cormorant Garamond',serif;font-weight:300;font-size:100px;line-height:.85;color:#111;letter-spacing:-2px;">5.2</div>
        <div style="font-family:'Montserrat',sans-serif;font-weight:300;font-size:11px;letter-spacing:5px;color:#bbb;text-transform:uppercase;margin-top:14px;">kilometres</div>
        <div style="width:40px;height:1px;background:#e0e0e0;margin:50px 0;"></div>
        <div style="display:grid;grid-template-columns:1fr 1fr;gap:0;">
          <div style="border-right:1px solid #f0f0f0;padding-right:24px;">
            <div style="font-family:'Cormorant Garamond',serif;font-size:36px;color:#111;line-height:1;">5:42</div>
            <div style="font-family:'Montserrat',sans-serif;font-weight:300;font-size:8px;letter-spacing:3px;color:#bbb;text-transform:uppercase;margin-top:6px;">Avg Pace / km</div>
          </div>
          <div style="padding-left:24px;">
            <div style="font-family:'Cormorant Garamond',serif;font-size:36px;color:#111;line-height:1;">164</div>
            <div style="font-family:'Montserrat',sans-serif;font-weight:300;font-size:8px;letter-spacing:3px;color:#bbb;text-transform:uppercase;margin-top:6px;">Cadence · spm</div>
          </div>
        </div>
        <div style="margin-top:48px;font-family:'Cormorant Garamond',serif;font-style:italic;font-size:15px;color:#bbb;">↑ 12% compared to last week</div>
      </div>

      <div class="dior-slide" style="flex:0 0 calc(100%/6);height:100%;background:#fafafa;display:flex;flex-direction:column;justify-content:center;padding:80px 36px 100px;">
        <div style="font-family:'Montserrat',sans-serif;font-weight:300;font-size:9px;letter-spacing:5px;color:#bbb;text-transform:uppercase;margin-bottom:60px;">Recovery</div>
        <div style="display:flex;align-items:baseline;gap:8px;margin-bottom:6px;">
          <div style="font-family:'Cormorant Garamond',serif;font-weight:300;font-size:100px;line-height:.85;color:#111;letter-spacing:-2px;">7</div>
          <div style="font-family:'Cormorant Garamond',serif;font-weight:300;font-size:40px;color:#ccc;">h 22</div>
        </div>
        <div style="font-family:'Montserrat',sans-serif;font-weight:300;font-size:11px;letter-spacing:5px;color:#bbb;text-transform:uppercase;margin-top:10px;">Sleep</div>
        <div style="width:40px;height:1px;background:#e0e0e0;margin:50px 0;"></div>
        <div style="font-family:'Cormorant Garamond',serif;font-style:italic;font-size:15px;color:#aaa;margin-bottom:36px;">34 minutes above your average</div>
        <div style="display:grid;grid-template-columns:1fr 1fr;gap:0;">
          <div style="border-right:1px solid #f0f0f0;padding-right:24px;">
            <div style="font-family:'Cormorant Garamond',serif;font-size:36px;color:#111;line-height:1;">48</div>
            <div style="font-family:'Montserrat',sans-serif;font-weight:300;font-size:8px;letter-spacing:3px;color:#bbb;text-transform:uppercase;margin-top:6px;">VO₂ Max</div>
          </div>
          <div style="padding-left:24px;">
            <div style="font-family:'Cormorant Garamond',serif;font-size:36px;color:#1A56DB;line-height:1;">+2.1</div>
            <div style="font-family:'Montserrat',sans-serif;font-weight:300;font-size:8px;letter-spacing:3px;color:#bbb;text-transform:uppercase;margin-top:6px;">30-day gain</div>
          </div>
        </div>
      </div>

      <div class="dior-slide" style="flex:0 0 calc(100%/6);height:100%;background:#111;display:flex;flex-direction:column;justify-content:center;padding:80px 36px 100px;">
        <div style="font-family:'Montserrat',sans-serif;font-weight:300;font-size:9px;letter-spacing:5px;color:#555;text-transform:uppercase;margin-bottom:48px;">Today's Sessions</div>
        <div style="display:flex;flex-direction:column;gap:0;">
          <div style="padding:22px 0;border-bottom:1px solid #222;">
            <div style="display:flex;justify-content:space-between;align-items:baseline;margin-bottom:6px;">
              <div style="font-family:'Cormorant Garamond',serif;font-weight:400;font-size:22px;color:#fafafa;letter-spacing:.5px;">Morning Run</div>
              <div style="font-family:'Cormorant Garamond',serif;font-style:italic;font-size:18px;color:#555;">312 kcal</div>
            </div>
            <div style="font-family:'Montserrat',sans-serif;font-weight:300;font-size:9px;letter-spacing:2px;color:#555;text-transform:uppercase;">06:14 · 4.8 km · 28 min</div>
          </div>
          <div style="padding:22px 0;border-bottom:1px solid #222;">
            <div style="display:flex;justify-content:space-between;align-items:baseline;margin-bottom:6px;">
              <div style="font-family:'Cormorant Garamond',serif;font-weight:400;font-size:22px;color:#fafafa;letter-spacing:.5px;">Upper Body</div>
              <div style="font-family:'Cormorant Garamond',serif;font-style:italic;font-size:18px;color:#555;">174 kcal</div>
            </div>
            <div style="font-family:'Montserrat',sans-serif;font-weight:300;font-size:9px;letter-spacing:2px;color:#555;text-transform:uppercase;">09:30 · 8 exercises · 45 min</div>
          </div>
          <div style="padding:22px 0;border-bottom:1px solid #222;">
            <div style="display:flex;justify-content:space-between;align-items:baseline;margin-bottom:6px;">
              <div style="font-family:'Cormorant Garamond',serif;font-weight:400;font-size:22px;color:#fafafa;letter-spacing:.5px;">Commute Ride</div>
              <div style="font-family:'Cormorant Garamond',serif;font-style:italic;font-size:18px;color:#555;">148 kcal</div>
            </div>
            <div style="font-family:'Montserrat',sans-serif;font-weight:300;font-size:9px;letter-spacing:2px;color:#555;text-transform:uppercase;">12:05 · 6.2 km · 20 min</div>
          </div>
          <div style="padding:22px 0;opacity:.35;">
            <div style="font-family:'Cormorant Garamond',serif;font-style:italic;font-size:18px;color:#fafafa;margin-bottom:6px;">Evening Yoga — 18:00</div>
            <div style="font-family:'Montserrat',sans-serif;font-weight:300;font-size:9px;letter-spacing:3px;color:#555;text-transform:uppercase;">Scheduled</div>
          </div>
        </div>
      </div>

      <div class="dior-slide" style="flex:0 0 calc(100%/6);height:100%;background:#fafafa;display:flex;flex-direction:column;justify-content:center;padding:80px 36px 100px;">
        <div style="font-family:'Montserrat',sans-serif;font-weight:300;font-size:9px;letter-spacing:5px;color:#bbb;text-transform:uppercase;margin-bottom:60px;">Week 12 · March</div>
        <div style="display:flex;align-items:flex-end;gap:10px;height:120px;margin-bottom:24px;">
          <div style="flex:1;display:flex;flex-direction:column;align-items:center;gap:10px;height:100%;justify-content:flex-end;"><div style="width:1px;background:#ddd;height:55%;"></div><div style="font-family:'Montserrat',sans-serif;font-size:8px;letter-spacing:1px;color:#ccc;text-transform:uppercase;">Mo</div></div>
          <div style="flex:1;display:flex;flex-direction:column;align-items:center;gap:10px;height:100%;justify-content:flex-end;"><div style="width:1px;background:#aaa;height:82%;"></div><div style="font-family:'Montserrat',sans-serif;font-size:8px;letter-spacing:1px;color:#ccc;text-transform:uppercase;">Tu</div></div>
          <div style="flex:1;display:flex;flex-direction:column;align-items:center;gap:10px;height:100%;justify-content:flex-end;"><div style="width:1px;background:#aaa;height:90%;"></div><div style="font-family:'Montserrat',sans-serif;font-size:8px;letter-spacing:1px;color:#ccc;text-transform:uppercase;">We</div></div>
          <div style="flex:1;display:flex;flex-direction:column;align-items:center;gap:10px;height:100%;justify-content:flex-end;"><div style="width:1px;background:#ddd;height:40%;"></div><div style="font-family:'Montserrat',sans-serif;font-size:8px;letter-spacing:1px;color:#ccc;text-transform:uppercase;">Th</div></div>
          <div style="flex:1;display:flex;flex-direction:column;align-items:center;gap:10px;height:100%;justify-content:flex-end;"><div style="width:1px;background:#aaa;height:100%;"></div><div style="font-family:'Montserrat',sans-serif;font-size:8px;letter-spacing:1px;color:#ccc;text-transform:uppercase;">Fr</div></div>
          <div style="flex:1;display:flex;flex-direction:column;align-items:center;gap:10px;height:100%;justify-content:flex-end;"><div style="width:1px;background:#ddd;height:62%;"></div><div style="font-family:'Montserrat',sans-serif;font-size:8px;letter-spacing:1px;color:#ccc;text-transform:uppercase;">Sa</div></div>
          <div style="flex:1;display:flex;flex-direction:column;align-items:center;gap:10px;height:100%;justify-content:flex-end;"><div style="width:2px;background:#111;height:73%;"></div><div style="font-family:'Montserrat',sans-serif;font-size:8px;letter-spacing:1px;color:#111;text-transform:uppercase;font-weight:500;">Su</div></div>
        </div>
        <div style="height:1px;background:#f0f0f0;margin-bottom:40px;"></div>
        <div style="display:grid;grid-template-columns:1fr 1fr 1fr;gap:0;">
          <div style="border-right:1px solid #f0f0f0;padding-right:20px;"><div style="font-family:'Cormorant Garamond',serif;font-size:30px;color:#111;line-height:1;">5</div><div style="font-family:'Montserrat',sans-serif;font-weight:300;font-size:8px;letter-spacing:2px;color:#bbb;text-transform:uppercase;margin-top:5px;">Sessions</div></div>
          <div style="border-right:1px solid #f0f0f0;padding:0 16px;"><div style="font-family:'Cormorant Garamond',serif;font-size:30px;color:#111;line-height:1;">32.4</div><div style="font-family:'Montserrat',sans-serif;font-weight:300;font-size:8px;letter-spacing:2px;color:#bbb;text-transform:uppercase;margin-top:5px;">km total</div></div>
          <div style="padding-left:16px;"><div style="font-family:'Cormorant Garamond',serif;font-size:30px;color:#111;line-height:1;">1,847</div><div style="font-family:'Montserrat',sans-serif;font-weight:300;font-size:8px;letter-spacing:2px;color:#bbb;text-transform:uppercase;margin-top:5px;">kcal</div></div>
        </div>
      </div>

    </div>
    <div style="position:absolute;bottom:calc(var(--nav-h) + 20px);left:0;right:0;display:flex;align-items:center;justify-content:center;gap:8px;z-index:50;">
      <button onclick="prevSlide()" style="background:none;border:none;cursor:pointer;padding:8px;color:#bbb;"><svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5" stroke-linecap="round"><polyline points="15 18 9 12 15 6"/></svg></button>
      <div id="slideDots" style="display:flex;gap:8px;">
        <div class="sdot active" onclick="goSlide(0)"></div>
        <div class="sdot" onclick="goSlide(1)"></div>
        <div class="sdot" onclick="goSlide(2)"></div>
        <div class="sdot" onclick="goSlide(3)"></div>
        <div class="sdot" onclick="goSlide(4)"></div>
        <div class="sdot" onclick="goSlide(5)"></div>
      </div>
      <button onclick="nextSlide()" style="background:none;border:none;cursor:pointer;padding:8px;color:#bbb;"><svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5" stroke-linecap="round"><polyline points="9 18 15 12 9 6"/></svg></button>
    </div>
    <div id="swipeHint" style="position:absolute;bottom:calc(var(--nav-h) + 56px);left:0;right:0;text-align:center;font-family:'Montserrat',sans-serif;font-weight:300;font-size:8px;letter-spacing:3px;color:#ccc;text-transform:uppercase;animation:hintFade 3s ease 1.5s both;">Swipe to explore</div>
  </div>
</div>
<div id="history" class="screen light">
  <div class="topbar light-bar">
    <div class="page-title">Workouts</div>
    <div class="topbar-right">
      <button class="icon-btn"><svg viewBox="0 0 24 24" fill="none" stroke="#1A56DB" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><circle cx="11" cy="11" r="8"/><line x1="21" y1="21" x2="16.65" y2="16.65"/></svg></button>
      <button class="icon-btn"><svg viewBox="0 0 24 24" fill="none" stroke="#1A56DB" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><line x1="4" y1="6" x2="20" y2="6"/><line x1="4" y1="12" x2="14" y2="12"/><line x1="4" y1="18" x2="10" y2="18"/></svg></button>
    </div>
  </div>
  <div class="h-summary">
    <div class="h-sum-item"><div class="h-sum-val">12</div><div class="h-sum-lbl">Workouts</div></div>
    <div class="h-sum-item"><div class="h-sum-val">48.3<small>km</small></div><div class="h-sum-lbl">Distance</div></div>
    <div class="h-sum-item"><div class="h-sum-val">6h<small>14m</small></div><div class="h-sum-lbl">Active Time</div></div>
  </div>
  <div class="h-month-strip">
    <div class="h-month-label">March 2026</div>
    <div class="h-cal-grid">
      <div class="h-cal-day" style="color:var(--muted);font-size:7px">Su</div><div class="h-cal-day" style="color:var(--muted);font-size:7px">Mo</div><div class="h-cal-day" style="color:var(--muted);font-size:7px">Tu</div><div class="h-cal-day" style="color:var(--muted);font-size:7px">We</div><div class="h-cal-day" style="color:var(--muted);font-size:7px">Th</div><div class="h-cal-day" style="color:var(--muted);font-size:7px">Fr</div><div class="h-cal-day" style="color:var(--muted);font-size:7px">Sa</div>
      <div class="h-cal-day has-workout">1</div><div class="h-cal-day rest">2</div><div class="h-cal-day has-workout">3</div><div class="h-cal-day has-workout">4</div><div class="h-cal-day rest">5</div><div class="h-cal-day has-workout">6</div><div class="h-cal-day rest">7</div>
      <div class="h-cal-day has-workout">8</div><div class="h-cal-day rest">9</div><div class="h-cal-day has-workout">10</div><div class="h-cal-day has-workout">11</div><div class="h-cal-day rest">12</div><div class="h-cal-day has-workout">13</div><div class="h-cal-day has-workout">14</div>
      <div class="h-cal-day rest">15</div><div class="h-cal-day has-workout">16</div><div class="h-cal-day has-workout">17</div><div class="h-cal-day rest">18</div><div class="h-cal-day has-workout">19</div><div class="h-cal-day has-workout">20</div><div class="h-cal-day rest">21</div>
      <div class="h-cal-day today">22</div><div class="h-cal-day" style="opacity:.3">23</div><div class="h-cal-day" style="opacity:.3">24</div><div class="h-cal-day" style="opacity:.3">25</div><div class="h-cal-day" style="opacity:.3">26</div><div class="h-cal-day" style="opacity:.3">27</div><div class="h-cal-day" style="opacity:.3">28</div>
    </div>
  </div>
  <div class="h-section"><span class="h-section-title">Recent</span><button class="h-section-filter">All Types</button></div>
  <div class="h-list">
    <div class="wcard">
      <div class="wcard-header">
        <div class="wcard-icon-wrap run"><svg viewBox="0 0 24 24" fill="none" stroke="#9bb800" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><circle cx="13" cy="4.5" r="1.5"/><path d="M7 21l3.5-6.5L13 17l2.5-5 2 3.5"/><path d="M8.5 12.5l2-5.5 3 2.5"/></svg></div>
        <div class="wcard-header-info"><div class="wcard-title">Morning Run</div><div class="wcard-date">Sun, Mar 22 · 06:14 AM</div></div>
        <div class="wcard-badge pr">PR</div>
      </div>
      <div class="wcard-stats">
        <div class="wcard-stat"><div class="wcard-stat-val">4.8<small>km</small></div><div class="wcard-stat-lbl">Distance</div></div>
        <div class="wcard-stat"><div class="wcard-stat-val">28<small>min</small></div><div class="wcard-stat-lbl">Duration</div></div>
        <div class="wcard-stat"><div class="wcard-stat-val">312<small>cal</small></div><div class="wcard-stat-lbl">Calories</div></div>
        <div class="wcard-stat"><div class="wcard-stat-val">5:42<small>/km</small></div><div class="wcard-stat-lbl">Pace</div></div>
      </div>
    </div>
    <div class="wcard">
      <div class="wcard-header">
        <div class="wcard-icon-wrap lift"><svg viewBox="0 0 24 24" fill="none" stroke="#EF4444" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M6.5 6.5v11M17.5 6.5v11"/><path d="M3 9.5h3.5m11 0H21M3 14.5h3.5m11 0H21M9.5 12h5"/></svg></div>
        <div class="wcard-header-info"><div class="wcard-title">Upper Body</div><div class="wcard-date">Sun, Mar 22 · 09:30 AM</div></div>
        <div class="wcard-badge good">Strong</div>
      </div>
      <div class="wcard-stats">
        <div class="wcard-stat"><div class="wcard-stat-val">8<small>ex</small></div><div class="wcard-stat-lbl">Exercises</div></div>
        <div class="wcard-stat"><div class="wcard-stat-val">45<small>min</small></div><div class="wcard-stat-lbl">Duration</div></div>
        <div class="wcard-stat"><div class="wcard-stat-val">174<small>cal</small></div><div class="wcard-stat-lbl">Calories</div></div>
        <div class="wcard-stat"><div class="wcard-stat-val">3<small>sets</small></div><div class="wcard-stat-lbl">Sets</div></div>
      </div>
    </div>
    <div class="wcard">
      <div class="wcard-header">
        <div class="wcard-icon-wrap bike"><svg viewBox="0 0 24 24" fill="none" stroke="#1A56DB" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><circle cx="5.5" cy="17.5" r="3.5"/><circle cx="18.5" cy="17.5" r="3.5"/><path d="M15 6a1 1 0 0 0-1-1h-1l-5 8h8"/><path d="M9 15l2.5-7"/></svg></div>
        <div class="wcard-header-info"><div class="wcard-title">Commute Ride</div><div class="wcard-date">Sun, Mar 22 · 12:05 PM</div></div>
      </div>
      <div class="wcard-stats">
        <div class="wcard-stat"><div class="wcard-stat-val">6.2<small>km</small></div><div class="wcard-stat-lbl">Distance</div></div>
        <div class="wcard-stat"><div class="wcard-stat-val">20<small>min</small></div><div class="wcard-stat-lbl">Duration</div></div>
        <div class="wcard-stat"><div class="wcard-stat-val">148<small>cal</small></div><div class="wcard-stat-lbl">Calories</div></div>
        <div class="wcard-stat"><div class="wcard-stat-val">18.4<small>km/h</small></div><div class="wcard-stat-lbl">Speed</div></div>
      </div>
    </div>
    <div class="wcard">
      <div class="wcard-header">
        <div class="wcard-icon-wrap run"><svg viewBox="0 0 24 24" fill="none" stroke="#9bb800" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><circle cx="13" cy="4.5" r="1.5"/><path d="M7 21l3.5-6.5L13 17l2.5-5 2 3.5"/><path d="M8.5 12.5l2-5.5 3 2.5"/></svg></div>
        <div class="wcard-header-info"><div class="wcard-title">Evening Run</div><div class="wcard-date">Fri, Mar 20 · 17:45 PM</div></div>
      </div>
      <div class="wcard-stats">
        <div class="wcard-stat"><div class="wcard-stat-val">7.1<small>km</small></div><div class="wcard-stat-lbl">Distance</div></div>
        <div class="wcard-stat"><div class="wcard-stat-val">41<small>min</small></div><div class="wcard-stat-lbl">Duration</div></div>
        <div class="wcard-stat"><div class="wcard-stat-val">389<small>cal</small></div><div class="wcard-stat-lbl">Calories</div></div>
        <div class="wcard-stat"><div class="wcard-stat-val">5:47<small>/km</small></div><div class="wcard-stat-lbl">Pace</div></div>
      </div>
    </div>
  </div>
</div>

<div id="routes" class="screen light">
  <div class="topbar light-bar">
    <div class="page-title">Routes</div>
    <div class="topbar-right"><button class="icon-btn"><svg viewBox="0 0 24 24" fill="none" stroke="#1A56DB" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><circle cx="11" cy="11" r="8"/><line x1="21" y1="21" x2="16.65" y2="16.65"/></svg></button></div>
  </div>
  <div class="r-header">
    <div><div class="r-eyebrow">Saved Routes</div><div class="r-heading">3 Routes</div></div>
    <button class="r-new-btn"><svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5" stroke-linecap="round"><line x1="12" y1="5" x2="12" y2="19"/><line x1="5" y1="12" x2="19" y2="12"/></svg>New Route</button>
  </div>
  <div class="r-filters">
    <button class="r-pill on">All</button><button class="r-pill">Run</button><button class="r-pill">Ride</button><button class="r-pill">Walk</button>
  </div>
  <div class="r-list">
    <div class="route-card">
      <div class="rc-map">
        <svg viewBox="0 0 320 130" preserveAspectRatio="xMidYMid meet"><rect width="320" height="130" fill="#EFF6FF"/><ellipse cx="260" cy="110" rx="70" ry="25" fill="#DBEAFE" opacity=".6"/><line x1="0" y1="32" x2="320" y2="32" stroke="#BFDBFE" stroke-width=".5"/><line x1="0" y1="65" x2="320" y2="65" stroke="#BFDBFE" stroke-width=".5"/><line x1="0" y1="98" x2="320" y2="98" stroke="#BFDBFE" stroke-width=".5"/><line x1="80" y1="0" x2="80" y2="130" stroke="#BFDBFE" stroke-width=".5"/><line x1="160" y1="0" x2="160" y2="130" stroke="#BFDBFE" stroke-width=".5"/><line x1="240" y1="0" x2="240" y2="130" stroke="#BFDBFE" stroke-width=".5"/><path class="route-path" d="M40,100 C50,75 70,57 100,48 S150,32 180,42 S230,65 260,74 S290,90 280,108 S240,122 200,117 S140,114 100,110 S50,114 40,100Z" fill="none" stroke="#1A56DB" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round" stroke-dasharray="600" stroke-dashoffset="600"/><circle cx="40" cy="100" r="5" fill="#1A56DB"/><circle cx="40" cy="100" r="9" fill="none" stroke="#1A56DB" stroke-width="1.5" opacity=".35"/></svg>
        <div class="rc-badge run">Run</div><div class="rc-dist">4.8 km</div>
      </div>
      <div class="rc-body">
        <div class="rc-name">Riverside Loop</div>
        <div class="rc-stats">
          <div><div class="rc-stat-val">4.8<span>km</span></div><div class="rc-stat-lbl">Distance</div></div>
          <div><div class="rc-stat-val">28<span>min</span></div><div class="rc-stat-lbl">Est. Time</div></div>
          <div><div class="rc-stat-val blue">+62<span>m</span></div><div class="rc-stat-lbl">Elevation</div></div>
          <div><div class="rc-stat-val">14×</div><div class="rc-stat-lbl">Runs</div></div>
        </div>
        <div class="rc-footer"><span class="rc-last">Last run 3 days ago</span><button class="rc-go-btn"><svg width="10" height="10" viewBox="0 0 24 24" fill="currentColor"><polygon points="5 3 19 12 5 21 5 3"/></svg>Start</button></div>
      </div>
    </div>
    <div class="route-card">
      <div class="rc-map">
        <svg viewBox="0 0 320 130" preserveAspectRatio="xMidYMid meet"><rect width="320" height="130" fill="#F0FDF4"/><ellipse cx="160" cy="65" rx="80" ry="40" fill="#DCFCE7" opacity=".6"/><line x1="0" y1="32" x2="320" y2="32" stroke="#BBF7D0" stroke-width=".5"/><line x1="0" y1="65" x2="320" y2="65" stroke="#BBF7D0" stroke-width=".5"/><line x1="0" y1="98" x2="320" y2="98" stroke="#BBF7D0" stroke-width=".5"/><line x1="80" y1="0" x2="80" y2="130" stroke="#BBF7D0" stroke-width=".5"/><line x1="160" y1="0" x2="160" y2="130" stroke="#BBF7D0" stroke-width=".5"/><line x1="240" y1="0" x2="240" y2="130" stroke="#BBF7D0" stroke-width=".5"/><path class="route-path" d="M80,32 L240,32 Q265,32 265,58 L265,90 Q265,110 240,110 L80,110 Q55,110 55,90 L55,58 Q55,32 80,32Z" fill="none" stroke="#1A56DB" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round" stroke-dasharray="700" stroke-dashoffset="700"/><circle cx="80" cy="32" r="5" fill="#1A56DB"/><circle cx="80" cy="32" r="9" fill="none" stroke="#1A56DB" stroke-width="1.5" opacity=".35"/></svg>
        <div class="rc-badge ride">Ride</div><div class="rc-dist">6.2 km</div>
      </div>
      <div class="rc-body">
        <div class="rc-name">Park Circuit</div>
        <div class="rc-stats">
          <div><div class="rc-stat-val">6.2<span>km</span></div><div class="rc-stat-lbl">Distance</div></div>
          <div><div class="rc-stat-val">22<span>min</span></div><div class="rc-stat-lbl">Est. Time</div></div>
          <div><div class="rc-stat-val blue">+18<span>m</span></div><div class="rc-stat-lbl">Elevation</div></div>
          <div><div class="rc-stat-val">6×</div><div class="rc-stat-lbl">Rides</div></div>
        </div>
        <div class="rc-footer"><span class="rc-last">Last ride today</span><button class="rc-go-btn"><svg width="10" height="10" viewBox="0 0 24 24" fill="currentColor"><polygon points="5 3 19 12 5 21 5 3"/></svg>Start</button></div>
      </div>
    </div>
    <div class="route-card">
      <div class="rc-map">
        <svg viewBox="0 0 320 130" preserveAspectRatio="xMidYMid meet"><rect width="320" height="130" fill="#FFF7ED"/><path d="M0,130 L80,80 L160,48 L220,64 L280,22 L320,38 L320,130Z" fill="#FED7AA" opacity=".4"/><line x1="0" y1="32" x2="320" y2="32" stroke="#FED7AA" stroke-width=".5"/><line x1="0" y1="65" x2="320" y2="65" stroke="#FED7AA" stroke-width=".5"/><line x1="0" y1="98" x2="320" y2="98" stroke="#FED7AA" stroke-width=".5"/><line x1="80" y1="0" x2="80" y2="130" stroke="#FED7AA" stroke-width=".5"/><line x1="160" y1="0" x2="160" y2="130" stroke="#FED7AA" stroke-width=".5"/><line x1="240" y1="0" x2="240" y2="130" stroke="#FED7AA" stroke-width=".5"/><path class="route-path" d="M30,115 C60,105 80,88 110,72 S150,52 180,48 S220,60 250,36 S278,20 298,24" fill="none" stroke="#1A56DB" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round" stroke-dasharray="500" stroke-dashoffset="500"/><circle cx="30" cy="115" r="5" fill="#1A56DB"/><circle cx="30" cy="115" r="9" fill="none" stroke="#1A56DB" stroke-width="1.5" opacity=".35"/><circle cx="298" cy="24" r="5" fill="#EF4444"/></svg>
        <div class="rc-badge run">Run</div><div class="rc-dist">7.1 km</div>
      </div>
      <div class="rc-body">
        <div class="rc-name">Hill Climb</div>
        <div class="rc-stats">
          <div><div class="rc-stat-val">7.1<span>km</span></div><div class="rc-stat-lbl">Distance</div></div>
          <div><div class="rc-stat-val">48<span>min</span></div><div class="rc-stat-lbl">Est. Time</div></div>
          <div><div class="rc-stat-val blue">+210<span>m</span></div><div class="rc-stat-lbl">Elevation</div></div>
          <div><div class="rc-stat-val">3×</div><div class="rc-stat-lbl">Runs</div></div>
        </div>
        <div class="rc-footer"><span class="rc-last">Last run 2 weeks ago</span><button class="rc-go-btn"><svg width="10" height="10" viewBox="0 0 24 24" fill="currentColor"><polygon points="5 3 19 12 5 21 5 3"/></svg>Start</button></div>
      </div>
    </div>
  </div>
</div>

<div id="profile" class="screen light">
  <div class="topbar light-bar">
    <div class="page-title">Profile</div>
    <div class="topbar-right"><button class="icon-btn" onclick="showTab('settings')"><svg viewBox="0 0 24 24" fill="none" stroke="#1A56DB" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><circle cx="12" cy="12" r="3"/><path d="M19.07 4.93A10 10 0 1 0 4.93 19.07"/><path d="M12 1v2M12 21v2M1 12h2M21 12h2"/></svg></button></div>
  </div>
  <div class="p-hero">
    <div class="p-hero-inner">
      <div class="p-avatar-row">
        <div class="p-avatar">FA</div>
        <div><div class="p-name">Ferdinand Ayim</div><div class="p-handle">@ferdinandayim</div><div class="p-since">Member since Jan 2023</div></div>
      </div>
      <div class="p-stats-row">
        <div class="p-stat"><div class="p-stat-val">247</div><div class="p-stat-lbl">Workouts</div></div>
        <div class="p-stat"><div class="p-stat-val">1,284<small>km</small></div><div class="p-stat-lbl">Distance</div></div>
        <div class="p-stat"><div class="p-stat-val">89,420<small>cal</small></div><div class="p-stat-lbl">Calories</div></div>
      </div>
    </div>
  </div>
  <div class="p-section-label">Personal Records</div>
  <div class="p-prs">
    <div class="p-pr-header">All-Time Best</div>
    <div class="p-pr-grid">
      <div class="p-pr-item"><div class="p-pr-event">5K Run</div><div class="p-pr-val">24:18</div><div class="p-pr-date">Feb 14, 2026</div></div>
      <div class="p-pr-item"><div class="p-pr-event">10K Run</div><div class="p-pr-val">51:02</div><div class="p-pr-date">Jan 5, 2026</div></div>
      <div class="p-pr-item"><div class="p-pr-event">Longest Run</div><div class="p-pr-val">18.4<small>km</small></div><div class="p-pr-date">Oct 12, 2025</div></div>
      <div class="p-pr-item"><div class="p-pr-event">Best Pace</div><div class="p-pr-val">4:58<small>/km</small></div><div class="p-pr-date">Mar 22, 2026</div></div>
    </div>
  </div>
  <div class="p-section-label">Account</div>
  <div class="p-settings">
    <div class="p-row" onclick="showTab('settings')">
      <div class="p-row-left"><div class="p-row-icon"><svg viewBox="0 0 24 24" fill="none" stroke="#1A56DB" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" width="16" height="16"><path d="M20 21v-2a4 4 0 0 0-4-4H8a4 4 0 0 0-4 4v2"/><circle cx="12" cy="7" r="4"/></svg></div><div><div class="p-row-label">Personal Info</div><div class="p-row-sub">Update your details</div></div></div>
      <div class="p-row-right"><div class="p-chevron"><svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round"><polyline points="9 18 15 12 9 6"/></svg></div></div>
    </div>
    <div class="p-row">
      <div class="p-row-left"><div class="p-row-icon"><svg viewBox="0 0 24 24" fill="none" stroke="#1A56DB" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" width="16" height="16"><rect x="3" y="11" width="18" height="11" rx="2" ry="2"/><path d="M7 11V7a5 5 0 0 1 10 0v4"/></svg></div><div><div class="p-row-label">Privacy</div><div class="p-row-sub">Who can see your workouts</div></div></div>
      <div class="p-row-right"><div class="p-row-value">Friends</div><div class="p-chevron"><svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round"><polyline points="9 18 15 12 9 6"/></svg></div></div>
    </div>
  </div>
  <div class="p-section-label">Preferences</div>
  <div class="p-settings">
    <div class="p-row"><div class="p-row-left"><div class="p-row-icon"><svg viewBox="0 0 24 24" fill="none" stroke="#1A56DB" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" width="16" height="16"><path d="M18 20V10"/><path d="M12 20V4"/><path d="M6 20v-6"/></svg></div><div><div class="p-row-label">Voice Feedback</div><div class="p-row-sub">During workouts</div></div></div><div class="p-row-right"><div class="toggle"></div></div></div>
    <div class="p-row"><div class="p-row-left"><div class="p-row-icon"><svg viewBox="0 0 24 24" fill="none" stroke="#1A56DB" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" width="16" height="16"><path d="M18 8A6 6 0 0 0 6 8c0 7-3 9-3 9h18s-3-2-3-9"/><path d="M13.73 21a2 2 0 0 1-3.46 0"/></svg></div><div><div class="p-row-label">Notifications</div><div class="p-row-sub">Goals and achievements</div></div></div><div class="p-row-right"><div class="toggle off"></div></div></div>
    <div class="p-row"><div class="p-row-left"><div class="p-row-icon"><svg viewBox="0 0 24 24" fill="none" stroke="#1A56DB" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" width="16" height="16"><circle cx="12" cy="12" r="10"/><polyline points="12 6 12 12 16 14"/></svg></div><div><div class="p-row-label">Auto-Pause</div><div class="p-row-sub">Pause when you stop</div></div></div><div class="p-row-right"><div class="toggle"></div></div></div>
  </div>
  <div class="p-section-label">Support</div>
  <div class="p-settings" style="margin-bottom:20px">
    <div class="p-row"><div class="p-row-left"><div class="p-row-icon"><svg viewBox="0 0 24 24" fill="none" stroke="#1A56DB" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" width="16" height="16"><circle cx="12" cy="12" r="10"/><path d="M9.09 9a3 3 0 0 1 5.83 1c0 2-3 3-3 3"/><line x1="12" y1="17" x2="12.01" y2="17"/></svg></div><div><div class="p-row-label">Help Center</div></div></div><div class="p-row-right"><div class="p-chevron"><svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round"><polyline points="9 18 15 12 9 6"/></svg></div></div></div>
    <div class="p-row danger"><div class="p-row-left"><div class="p-row-icon"><svg viewBox="0 0 24 24" fill="none" stroke="#EF4444" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" width="16" height="16"><path d="M9 21H5a2 2 0 0 1-2-2V5a2 2 0 0 1 2-2h4"/><polyline points="16 17 21 12 16 7"/><line x1="21" y1="12" x2="9" y2="12"/></svg></div><div><div class="p-row-label">Sign Out</div></div></div></div>
  </div>
</div>
<div id="settings" class="screen light" style="display:none;">
  <div class="topbar light-bar">
    <div class="page-title">My Info</div>
    <div class="topbar-right">
      <button class="icon-btn" onclick="showTab('profile')">
        <svg viewBox="0 0 24 24" fill="none" stroke="#1A56DB" stroke-width="2" stroke-linecap="round"><line x1="18" y1="6" x2="6" y2="18"/><line x1="6" y1="6" x2="18" y2="18"/></svg>
      </button>
    </div>
  </div>
  <div style="padding:20px 18px 0;">
    <div style="font-family:'Syne Mono',monospace;font-size:9px;letter-spacing:4px;color:#9CA3AF;text-transform:uppercase;margin-bottom:16px;">Personal Details</div>
    <div style="background:#fff;border:1px solid #E8EAED;border-radius:10px;overflow:hidden;margin-bottom:14px;">
      <div style="padding:14px 16px;border-bottom:1px solid #F3F4F6;">
        <label style="font-family:'Syne Mono',monospace;font-size:8px;letter-spacing:2px;color:#9CA3AF;text-transform:uppercase;display:block;margin-bottom:6px;">Full Name</label>
        <input id="pi-name" type="text" placeholder="e.g. Ferdinand Ayim" style="width:100%;border:none;outline:none;font-family:'Syne',sans-serif;font-size:15px;color:#111;background:transparent;"/>
      </div>
      <div style="padding:14px 16px;border-bottom:1px solid #F3F4F6;">
        <label style="font-family:'Syne Mono',monospace;font-size:8px;letter-spacing:2px;color:#9CA3AF;text-transform:uppercase;display:block;margin-bottom:6px;">Initials (shown on avatar)</label>
        <input id="pi-initials" type="text" maxlength="2" placeholder="FA" style="width:100%;border:none;outline:none;font-family:'Syne',sans-serif;font-size:15px;color:#111;background:transparent;"/>
      </div>
      <div style="display:grid;grid-template-columns:1fr 1fr 1fr;">
        <div style="padding:14px 16px;border-bottom:1px solid #F3F4F6;border-right:1px solid #F3F4F6;">
          <label style="font-family:'Syne Mono',monospace;font-size:8px;letter-spacing:2px;color:#9CA3AF;text-transform:uppercase;display:block;margin-bottom:6px;">Age</label>
          <input id="pi-age" type="number" placeholder="28" style="width:100%;border:none;outline:none;font-family:'Syne',sans-serif;font-size:15px;color:#111;background:transparent;"/>
        </div>
        <div style="padding:14px 16px;border-bottom:1px solid #F3F4F6;border-right:1px solid #F3F4F6;">
          <label style="font-family:'Syne Mono',monospace;font-size:8px;letter-spacing:2px;color:#9CA3AF;text-transform:uppercase;display:block;margin-bottom:6px;">Weight (kg)</label>
          <input id="pi-weight" type="number" placeholder="62" style="width:100%;border:none;outline:none;font-family:'Syne',sans-serif;font-size:15px;color:#111;background:transparent;"/>
        </div>
        <div style="padding:14px 16px;border-bottom:1px solid #F3F4F6;">
          <label style="font-family:'Syne Mono',monospace;font-size:8px;letter-spacing:2px;color:#9CA3AF;text-transform:uppercase;display:block;margin-bottom:6px;">Height (cm)</label>
          <input id="pi-height" type="number" placeholder="168" style="width:100%;border:none;outline:none;font-family:'Syne',sans-serif;font-size:15px;color:#111;background:transparent;"/>
        </div>
      </div>
      <div style="padding:14px 16px;border-bottom:1px solid #F3F4F6;">
        <label style="font-family:'Syne Mono',monospace;font-size:8px;letter-spacing:2px;color:#9CA3AF;text-transform:uppercase;display:block;margin-bottom:6px;">Daily Step Goal</label>
        <input id="pi-steps" type="text" placeholder="10,000" style="width:100%;border:none;outline:none;font-family:'Syne',sans-serif;font-size:15px;color:#111;background:transparent;"/>
      </div>
      <div style="padding:14px 16px;border-bottom:1px solid #F3F4F6;">
        <label style="font-family:'Syne Mono',monospace;font-size:8px;letter-spacing:2px;color:#9CA3AF;text-transform:uppercase;display:block;margin-bottom:6px;">Fitness Goal</label>
        <input id="pi-goal" type="text" placeholder="e.g. Run a half marathon" style="width:100%;border:none;outline:none;font-family:'Syne',sans-serif;font-size:15px;color:#111;background:transparent;"/>
      </div>
      <div style="padding:14px 16px;">
        <label style="font-family:'Syne Mono',monospace;font-size:8px;letter-spacing:2px;color:#9CA3AF;text-transform:uppercase;display:block;margin-bottom:6px;">Username / Handle</label>
        <input id="pi-handle" type="text" placeholder="@ferdinandayim" style="width:100%;border:none;outline:none;font-family:'Syne',sans-serif;font-size:15px;color:#111;background:transparent;"/>
      </div>
    </div>
    <button onclick="saveInfo()" style="width:100%;padding:14px;background:#1A56DB;color:#fff;border:none;border-radius:10px;cursor:pointer;font-family:'Syne Mono',monospace;font-size:10px;letter-spacing:3px;text-transform:uppercase;margin-bottom:28px;">Save & Apply</button>
  </div>
</div>

<nav>
  <button class="nbtn on" data-tab="today">
    <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><rect x="3" y="3" width="7" height="7" rx="1"/><rect x="14" y="3" width="7" height="7" rx="1"/><rect x="3" y="14" width="7" height="7" rx="1"/><rect x="14" y="14" width="7" height="7" rx="1"/></svg>
    <span class="nbtn-label">Today</span>
  </button>
  <button class="nbtn" data-tab="history">
    <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M14 2H6a2 2 0 0 0-2 2v16a2 2 0 0 0 2 2h12a2 2 0 0 0 2-2V8z"/><polyline points="14 2 14 8 20 8"/><line x1="16" y1="13" x2="8" y2="13"/><line x1="16" y1="17" x2="8" y2="17"/></svg>
    <span class="nbtn-label">History</span>
  </button>
  <button class="nstart">
    <svg viewBox="0 0 24 24" fill="#fff" stroke="none"><polygon points="5 3 19 12 5 21 5 3"/></svg>
  </button>
  <button class="nbtn" data-tab="routes">
    <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><polygon points="3 11 22 2 13 21 11 13 3 11"/></svg>
    <span class="nbtn-label">Routes</span>
  </button>
  <button class="nbtn" data-tab="profile">
    <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M20 21v-2a4 4 0 0 0-4-4H8a4 4 0 0 0-4 4v2"/><circle cx="12" cy="7" r="4"/></svg>
    <span class="nbtn-label">Profile</span>
  </button>
</nav>

<script>
  const allScreens = document.querySelectorAll('.screen');
  const navBtns = document.querySelectorAll('.nbtn[data-tab]');

  function showTab(id) {
    allScreens.forEach(s => { s.classList.remove('active'); s.style.display = 'none'; });
    navBtns.forEach(b => b.classList.remove('on'));
    const target = document.getElementById(id);
    if (target) {
      target.style.display = 'block';
      target.classList.add('active');
      window.scrollTo(0, 0);
    }
    document.querySelectorAll(`.nbtn[data-tab="${id}"]`).forEach(b => b.classList.add('on'));
    if (id === 'routes') {
      document.querySelectorAll('.route-path').forEach((p, i) => {
        p.style.animation = 'none';
        void p.offsetHeight;
        p.style.animation = `drawPath 1s cubic-bezier(.23,1,.32,1) ${i * .15}s both`;
      });
    }
  }

  navBtns.forEach(btn => btn.addEventListener('click', () => showTab(btn.dataset.tab)));

  const TOTAL_SLIDES = 6;
  let currentSlide = 0;
  const track = document.getElementById('slideTrack');
  const dots = document.querySelectorAll('.sdot');

  function goSlide(n) {
    currentSlide = Math.max(0, Math.min(TOTAL_SLIDES - 1, n));
    track.style.transform = `translateX(calc(-${currentSlide} * (100% / ${TOTAL_SLIDES})))`;
    dots.forEach((d, i) => d.classList.toggle('active', i === currentSlide));
  }

  function nextSlide() { goSlide(currentSlide + 1); }
  function prevSlide() { goSlide(currentSlide - 1); }

  let touchStartX = 0;
  document.getElementById('today').addEventListener('touchstart', e => { touchStartX = e.touches[0].clientX; }, { passive: true });
  document.getElementById('today').addEventListener('touchend', e => {
    const dx = e.changedTouches[0].clientX - touchStartX;
    if (Math.abs(dx) > 40) dx < 0 ? nextSlide() : prevSlide();
  });

  const hrSlideEl = document.getElementById('hrSlide');
  setInterval(() => { if (hrSlideEl) hrSlideEl.textContent = 72 + Math.floor(Math.random() * 5 - 2); }, 2400);

  document.querySelectorAll('.r-pill').forEach(pill => {
    pill.addEventListener('click', () => {
      document.querySelectorAll('.r-pill').forEach(p => p.classList.remove('on'));
      pill.classList.add('on');
    });
  });

  document.querySelectorAll('.toggle').forEach(t => {
    t.addEventListener('click', () => t.classList.toggle('off'));
  });

  const INFO_KEY = 'pacefit_user_info';

  function saveInfo() {
    const info = {
      name: document.getElementById('pi-name').value,
      initials: document.getElementById('pi-initials').value,
      age: document.getElementById('pi-age').value,
      weight: document.getElementById('pi-weight').value,
      height: document.getElementById('pi-height').value,
      steps: document.getElementById('pi-steps').value,
      goal: document.getElementById('pi-goal').value,
      handle: document.getElementById('pi-handle').value
    };
    localStorage.setItem(INFO_KEY, JSON.stringify(info));
    applyInfo(info);
    showTab('profile');
  }

  function applyInfo(info) {
    const first = info.name ? info.name.split(' ')[0] : 'Ferdinand';
    const initials = info.initials || (info.name ? info.name.split(' ').map(w => w[0]).join('').slice(0, 2).toUpperCase() : 'FA');
    document.querySelectorAll('.p-avatar').forEach(el => el.textContent = initials);
    document.querySelectorAll('.p-name').forEach(el => el.textContent = info.name || 'Ferdinand Ayim');
    document.querySelectorAll('.p-handle').forEach(el => el.textContent = info.handle || '@ferdinandayim');
    const greeting = document.querySelector('.dior-slide div[style*="font-size:72px"]');
    if (greeting) greeting.textContent = first + '.';
    const toast = document.getElementById('voiceToast');
    if (toast) toast.textContent = `20 min · Well done, ${first}`;
  }

  function loadSavedInfo() {
    const saved = localStorage.getItem(INFO_KEY);
    if (saved) {
      const info = JSON.parse(saved);
      applyInfo(info);
      if (info.name) document.getElementById('pi-name').value = info.name;
      if (info.initials) document.getElementById('pi-initials').value = info.initials;
      if (info.age) document.getElementById('pi-age').value = info.age;
      if (info.weight) document.getElementById('pi-weight').value = info.weight;
      if (info.height) document.getElementById('pi-height').value = info.height;
      if (info.steps) document.getElementById('pi-steps').value = info.steps;
      if (info.goal) document.getElementById('pi-goal').value = info.goal;
      if (info.handle) document.getElementById('pi-handle').value = info.handle;
    }
  }

  loadSavedInfo();

  let cachedVoice = null;
  function loadVoices() {
    const voices = window.speechSynthesis.getVoices();
    cachedVoice = voices.find(v =>
      v.name.toLowerCase().includes('samantha') ||
      v.name.toLowerCase().includes('karen') ||
      v.name.toLowerCase().includes('daniel') ||
      v.lang === 'en-GB' || v.lang === 'en-US'
    ) || voices[0] || null;
  }
  if ('speechSynthesis' in window) {
    loadVoices();
    window.speechSynthesis.onvoiceschanged = loadVoices;
  }

  function triggerVoiceNotification() {
    const saved = localStorage.getItem(INFO_KEY);
    const name = saved ? JSON.parse(saved).name?.split(' ')[0] : 'Ferdinand';
    if ('speechSynthesis' in window) {
      window.speechSynthesis.cancel();
      const msg = new SpeechSynthesisUtterance(
        `20 minutes in, ${name}. You are doing great. Keep your pace and stay hydrated.`
      );
      msg.rate = 0.88; msg.pitch = 0.95; msg.volume = 1;
      if (cachedVoice) msg.voice = cachedVoice;
      window.speechSynthesis.speak(msg);
    }
    const toast = document.getElementById('voiceToast');
    if (toast) {
      toast.style.transform = 'translateX(-50%) translateY(0)';
      setTimeout(() => { toast.style.transform = 'translateX(-50%) translateY(-140px)'; }, 4500);
    }
  }

  setTimeout(triggerVoiceNotification, 5000);
  setTimeout(triggerVoiceNotification, 20 * 60 * 1000);
</script>
</body>
</html>
