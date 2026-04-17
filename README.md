<!DOCTYPE html>
<html lang="ar" dir="rtl" data-lang="ar">
<head>
  <meta charset="UTF-8"/>
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Wadi 360 – Discover New Valley Like Never Before</title>
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link href="https://fonts.googleapis.com/css2?family=Amiri:wght@400;700&family=Cairo:wght@300;400;600;700;900&family=Cormorant+Garamond:wght@400;600;700&family=Jost:wght@300;400;600;700&display=swap" rel="stylesheet">
  <link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css"/>

<style>
/* ═══════════════════════════════════════════
   CSS VARIABLES & BASE
═══════════════════════════════════════════ */
:root {
  --sand:#C9A96E; --sand-light:#E8D5A3; --sand-pale:#F5EDDA;
  --dark-brown:#3D1F0D; --brown:#6B3A1F;
  --amber:#D4820A; --amber-glow:#F0A830;
  --sky-night:#0A0E1A; --sky-dusk:#1A1035;
  --teal:#0E8C7A; --teal-light:#14B89E;
  --white:#FDFAF4; --text-light:#F0E8D5; --text-muted:#B09070;
  --font-ar:'Cairo',sans-serif;
  --font-en:'Jost',sans-serif;
  --font-display-ar:'Amiri',serif;
  --font-display-en:'Cormorant Garamond',serif;
  --shadow:0 20px 60px rgba(0,0,0,0.4);
  --glow:0 0 30px rgba(212,130,10,0.4);
  --radius:16px;
}
*,*::before,*::after{box-sizing:border-box;margin:0;padding:0}
html{scroll-behavior:smooth}
body{
  background:var(--sky-night);color:var(--white);
  overflow-x:hidden;
  position:relative;
}

/* ── DESERT BACKGROUND LAYER ── */
body::before {
  content:'';
  position:fixed;
  inset:0;
  z-index:-2;
  background:
    linear-gradient(
      180deg,
      rgba(10, 14, 26, 0.4) 0%,
      rgba(26, 16, 53, 0.2) 40%,
      rgba(61, 32, 16, 0.5) 100%
    ),
    url('hibis_temple_bg_v2.png') no-repeat center center / cover;
  pointer-events:none;
}
body::after {
  content:'';
  position:fixed;
  bottom:0;
  left:-5%;
  width:110%;
  height:38%;
  z-index:-1;
  background:
    radial-gradient(ellipse 55% 140% at 25% 100%, #C9A96E 0%, transparent 65%),
    radial-gradient(ellipse 40% 110% at 72% 100%, #D4820A 0%, transparent 60%),
    radial-gradient(ellipse 30% 90%  at 50% 100%, #8B4A10 0%, transparent 70%),
    radial-gradient(ellipse 60% 80%  at 10% 100%, #5C2E06 0%, transparent 55%),
    radial-gradient(ellipse 45% 120% at 88% 100%, #C47020 0%, transparent 65%);
  pointer-events:none;
  animation:duneBreath 8s ease-in-out infinite alternate;
}
@keyframes duneBreath {
  from { transform:scaleY(1) translateY(0); opacity:.85; }
  to   { transform:scaleY(1.04) translateY(-6px); opacity:1; }
}

/* ── CAMEL SILHOUETTES ── */
.camel-layer {
  position:fixed;
  bottom:0;
  left:0;
  width:100%;
  height:220px;
  z-index:0;
  pointer-events:none;
  overflow:hidden;
}
.camel-svg {
  position:absolute;
  bottom:0;
  opacity:0.22;
  filter:drop-shadow(0 0 20px rgba(212,130,10,0.3));
}
.camel-svg.c1 { left:5%;  width:160px; animation:camelDrift 28s linear infinite; }
.camel-svg.c2 { left:60%; width:110px; animation:camelDrift 38s linear infinite 10s; opacity:0.15; }
.camel-svg.c3 { left:80%; width:80px;  animation:camelDrift 48s linear infinite 20s; opacity:0.12; }
@keyframes camelDrift {
  0%   { transform:translateX(0) scaleX(1);   }
  49%  { transform:translateX(8px) scaleX(1); }
  50%  { transform:translateX(8px) scaleX(-1);}
  99%  { transform:translateX(0) scaleX(-1);  }
  100% { transform:translateX(0) scaleX(1);   }
}

/* ── SAND DUST PARTICLES ── */
.sand-particles-bg {
  position:fixed;
  inset:0;
  z-index:0;
  pointer-events:none;
  overflow:hidden;
}
@keyframes sandFloat {
  0%   { transform:translateY(100vh) translateX(0); opacity:0; }
  10%  { opacity:0.4; }
  90%  { opacity:0.2; }
  100% { transform:translateY(-10vh) translateX(var(--dx,30px)); opacity:0; }
}

/* ── OASIS GLOW ── */
.oasis-glow {
  position:fixed;
  bottom: 12%;
  right: 12%;
  width:280px;
  height:120px;
  z-index:0;
  pointer-events:none;
  background:radial-gradient(ellipse, rgba(14,140,122,0.18) 0%, transparent 70%);
  border-radius:50%;
  animation:oasisPulse 5s ease-in-out infinite alternate;
  filter:blur(18px);
}
@keyframes oasisPulse {
  from { transform:scale(1);   opacity:0.6; }
  to   { transform:scale(1.15); opacity:1; }
}
[data-lang="ar"] body, body.lang-ar {
  font-family:var(--font-ar); direction:rtl;
}
[data-lang="en"] body, body.lang-en {
  font-family:var(--font-en); direction:ltr;
}
body { font-family:var(--font-ar); direction:rtl; }
a{text-decoration:none;color:inherit}
ul{list-style:none}
::-webkit-scrollbar{width:5px}
::-webkit-scrollbar-thumb{background:var(--amber);border-radius:3px}

/* ── LANGUAGE SWITCHING ── */
.en { display:none }
.ar { display:block }
html[data-lang="en"] .en { display:block }
html[data-lang="en"] .ar { display:none }
html[data-lang="en"] .en-inline { display:inline }
html[data-lang="en"] .ar-inline { display:none }
.en-inline { display:none }
.ar-inline { display:inline }
html[data-lang="en"] body { font-family:var(--font-en); direction:ltr }
html[data-lang="en"] { direction:ltr }

/* ── LANG TOGGLE BUTTON ── */
.lang-toggle {
  position:fixed; bottom:28px; left:28px; z-index:9999;
  background:linear-gradient(135deg,var(--amber),var(--amber-glow));
  color:var(--dark-brown); border:none; border-radius:50px;
  padding:10px 20px; font-weight:700; cursor:pointer;
  font-size:0.9rem; box-shadow:var(--glow);
  transition:all 0.3s; font-family:var(--font-ar);
  display:flex; align-items:center; gap:8px;
}
.lang-toggle:hover{transform:translateY(-3px) scale(1.05)}

/* ── CONTAINER ── */
.container{max-width:1200px;margin:0 auto;padding:0 24px}

/* ── SECTION LABELS ── */
.section-tag {
  display:inline-block; font-size:.75rem; font-weight:700;
  letter-spacing:3px; text-transform:uppercase;
  color:var(--amber-glow); border:1px solid var(--amber);
  padding:4px 14px; border-radius:20px; margin-bottom:16px;
}
.section-title {
  font-size:clamp(1.8rem,4vw,2.8rem); margin-bottom:48px; line-height:1.3;
}
html[data-lang="ar"] .section-title { font-family:var(--font-display-ar) }
html[data-lang="en"] .section-title { font-family:var(--font-display-en) }
.section-title span{color:var(--amber-glow)}

/* ═══════════════════════════════════════════
   NAVBAR
═══════════════════════════════════════════ */
.navbar {
  position:fixed; top:0; width:100%; z-index:999;
  display:flex; align-items:center; justify-content:space-between;
  padding:16px 40px; transition:all .4s;
}
.navbar.scrolled {
  background:rgba(10,14,26,.95); backdrop-filter:blur(20px);
  border-bottom:1px solid rgba(212,130,10,.2); padding:11px 40px;
}
.nav-logo {
  font-size:1.8rem; color:var(--amber-glow); font-weight:700; cursor:pointer;
}
html[data-lang="ar"] .nav-logo { font-family:var(--font-display-ar) }
html[data-lang="en"] .nav-logo { font-family:var(--font-display-en) }
.logo-w{color:var(--teal-light)} .logo-360{color:var(--amber);font-size:.85em}
.nav-links{display:flex;gap:28px}
.nav-links a {
  font-size:.9rem; font-weight:600; color:var(--text-light);
  transition:color .3s; position:relative;
}
.nav-links a::after {
  content:''; position:absolute; bottom:-4px;
  width:0; height:2px; background:var(--amber-glow); transition:width .3s;
}
html[data-lang="ar"] .nav-links a::after { right:0 }
html[data-lang="en"] .nav-links a::after { left:0 }
.nav-links a:hover{color:var(--amber-glow)}
.nav-links a:hover::after{width:100%}
.nav-cta {
  background:linear-gradient(135deg,var(--amber),var(--amber-glow));
  color:var(--dark-brown); border:none; padding:10px 24px;
  border-radius:25px; font-weight:700; cursor:pointer; font-size:.9rem;
  transition:all .3s; box-shadow:var(--glow);
}
.nav-cta:hover{transform:translateY(-2px);box-shadow:0 0 40px rgba(212,130,10,.6)}
.hamburger{display:none;background:none;border:none;color:var(--white);font-size:1.5rem;cursor:pointer}

/* ═══════════════════════════════════════════
   HERO
═══════════════════════════════════════════ */
.hero {
  min-height:100vh; position:relative; overflow:hidden;
  display:flex; align-items:center;
  background: transparent;
}
.hero-bg{position:absolute;inset:0}
.stars{position:absolute;inset:0}
.sand-layer{position:absolute;bottom:0;width:120%;left:-10%;border-radius:50% 50% 0 0/30% 30% 0 0}
.s1{height:35%;background:linear-gradient(180deg,#5C3A1A,#3D2010)}
.s2{height:25%;background:linear-gradient(180deg,#8B5E2E,#5C3A1A);left:-5%;width:115%}
.s3{height:18%;background:linear-gradient(180deg,var(--sand),#8B5E2E);left:5%;width:110%}
.dunes{
  position:absolute;bottom:0;width:100%;height:200px;
  background:radial-gradient(ellipse 40% 100% at 20% 100%,var(--sand) 0%,transparent 70%),
             radial-gradient(ellipse 35% 80% at 75% 100%,#C4913A 0%,transparent 60%),
             radial-gradient(ellipse 50% 120% at 50% 100%,var(--brown) 0%,transparent 80%);
}
.hero-content{
  position:relative;z-index:2;width:100%;max-width:1200px;
  margin:0 auto;padding:120px 40px 60px;
}
.hero-pre{
  font-size:1rem;color:var(--sand-light);letter-spacing:4px;
  text-transform:uppercase;margin-bottom:8px;
  opacity:0;animation:fadeUp .8s ease forwards .3s;
}
.hero-title{
  font-size:clamp(4rem,10vw,9rem);line-height:.9;margin-bottom:16px;
  opacity:0;animation:fadeUp .8s ease forwards .5s;
}
html[data-lang="ar"] .hero-title { font-family:var(--font-display-ar) }
html[data-lang="en"] .hero-title { font-family:var(--font-display-en) }
.title-wadi{color:var(--white);display:block}
.title-360{
  color:var(--amber-glow);display:block;margin-top:-10px;
  text-shadow:0 0 60px rgba(240,168,48,.6),0 0 120px rgba(240,168,48,.3);
}
.hero-sub{
  font-size:clamp(1rem,2.5vw,1.4rem);color:var(--sand-light);
  margin-bottom:16px;font-weight:600;
  opacity:0;animation:fadeUp .8s ease forwards .7s;
}
.hero-desc{
  max-width:600px;color:var(--text-muted);line-height:1.8;font-size:1rem;
  margin-bottom:40px;opacity:0;animation:fadeUp .8s ease forwards .9s;
}
.hero-btns{
  display:flex;gap:16px;flex-wrap:wrap;margin-bottom:60px;
  opacity:0;animation:fadeUp .8s ease forwards 1.1s;
}
.btn-primary{
  background:linear-gradient(135deg,var(--amber),var(--amber-glow));
  color:var(--dark-brown);border:none;padding:15px 34px;border-radius:30px;
  font-weight:700;font-size:1rem;cursor:pointer;transition:all .3s;
  box-shadow:var(--glow);display:flex;align-items:center;gap:8px;
}
.btn-primary:hover{transform:translateY(-3px);box-shadow:0 0 50px rgba(240,168,48,.7)}
.btn-secondary{
  background:transparent;color:var(--white);
  border:2px solid rgba(255,255,255,.3);
  padding:15px 34px;border-radius:30px;font-weight:600;font-size:1rem;
  cursor:pointer;transition:all .3s;display:flex;align-items:center;gap:8px;
}
.btn-secondary:hover{border-color:var(--teal-light);color:var(--teal-light);background:rgba(14,140,122,.1)}
.hero-stats{
  display:flex;align-items:center;gap:32px;flex-wrap:wrap;
  opacity:0;animation:fadeUp .8s ease forwards 1.3s;
}
.stat{text-align:center}
.stat-num{
  font-size:2.2rem;color:var(--amber-glow);font-weight:700;
}
html[data-lang="ar"] .stat-num { font-family:var(--font-display-ar) }
html[data-lang="en"] .stat-num { font-family:var(--font-display-en) }
.stat-unit{font-size:.85rem;color:var(--sand);margin-right:4px}
html[data-lang="en"] .stat-unit{margin-right:0;margin-left:4px}
.stat p{font-size:.75rem;color:var(--text-muted);margin-top:4px}
.stat-divider{width:1px;height:50px;background:rgba(255,255,255,.2)}
.scroll-hint{
  position:absolute;bottom:30px;left:50%;transform:translateX(-50%);
  display:flex;flex-direction:column;align-items:center;gap:8px;
  color:var(--text-muted);font-size:.75rem;animation:bounce 2s infinite;
}
.scroll-line{width:1px;height:50px;background:linear-gradient(to bottom,transparent,var(--amber))}

/* ═══════════════════════════════════════════
   ABOUT
═══════════════════════════════════════════ */
.about{padding:100px 0;background:linear-gradient(180deg,#0D1520,#0A0E1A)}
.about-grid{display:grid;grid-template-columns:1fr 1fr;gap:60px;align-items:center}
.about-text p{color:var(--text-muted);line-height:1.9;margin-bottom:20px;font-size:1rem}
.about-badges{display:flex;flex-wrap:wrap;gap:10px;margin-top:24px}
.badge{
  background:rgba(212,130,10,.15);border:1px solid rgba(212,130,10,.4);
  color:var(--amber-glow);padding:6px 16px;border-radius:20px;font-size:.85rem;
  transition:all .3s;
}
.badge:hover{background:rgba(212,130,10,.3);transform:translateY(-2px)}
.phone-mockup{
  width:240px;height:480px;margin:0 auto;
  background:#111;border-radius:36px;border:3px solid #333;padding:12px;
  box-shadow:0 30px 80px rgba(0,0,0,.6),0 0 40px rgba(212,130,10,.1);
  position:relative;
}
.phone-mockup::before{
  content:'';position:absolute;top:20px;left:50%;transform:translateX(-50%);
  width:60px;height:6px;background:#333;border-radius:3px;
}
.phone-screen{background:linear-gradient(180deg,#0D2030,#0A1A12);border-radius:28px;height:100%;overflow:hidden;position:relative}
.phone-map-demo{position:relative;height:calc(100% - 40px)}
.map-pin{
  position:absolute;font-size:.65rem;
  background:rgba(212,130,10,.9);color:#000;
  padding:3px 8px;border-radius:10px;font-weight:700;animation:pulse 2s infinite;
}
.p1{top:30%;right:20%} .p2{top:55%;right:40%} .p3{top:70%;right:10%}
.map-pin span{font-size:.6rem;display:block}
.phone-status-bar{
  position:absolute;bottom:0;width:100%;background:rgba(0,0,0,.8);
  padding:8px 12px;display:flex;justify-content:space-between;
  font-size:.65rem;color:var(--amber);
}

/* ═══════════════════════════════════════════
   FEATURES
═══════════════════════════════════════════ */
.features{padding:100px 0;background:linear-gradient(180deg,#0A0E1A,#0D1520)}
.features-grid{display:grid;grid-template-columns:2fr 1fr 1fr;grid-template-rows:auto auto;gap:20px}
.feature-card{
  background:linear-gradient(135deg,rgba(255,255,255,.05),rgba(255,255,255,.02));
  border:1px solid rgba(255,255,255,.08);border-radius:20px;padding:32px;
  transition:all .4s;position:relative;overflow:hidden;
}
.feature-card::before{
  content:'';position:absolute;top:0;left:0;right:0;height:2px;
  background:linear-gradient(90deg,transparent,var(--amber),transparent);
  opacity:0;transition:opacity .3s;
}
.feature-card:hover{transform:translateY(-6px);border-color:rgba(212,130,10,.3)}
.feature-card:hover::before{opacity:1}
.fc-main{
  grid-column:1;grid-row:1/3;
  background:linear-gradient(135deg,rgba(212,130,10,.15),rgba(212,130,10,.05));
  border-color:rgba(212,130,10,.3);
}
html[data-lang="en"] .fc-main { grid-column:1; grid-row:1/3; }
.fc-icon{font-size:2.5rem;margin-bottom:16px}
.feature-card h3{font-size:1.2rem;margin-bottom:12px;color:var(--amber-glow)}
.feature-card p{color:var(--text-muted);line-height:1.7;font-size:.9rem;margin-bottom:16px}
.feature-card ul li{color:var(--text-muted);font-size:.85rem;padding:4px 0}

/* ═══════════════════════════════════════════
   MAP
═══════════════════════════════════════════ */
.map-section{padding:100px 0;background:#060A10}
.map-wrapper{
  display:grid;grid-template-columns:300px 1fr;
  border-radius:var(--radius);overflow:hidden;box-shadow:var(--shadow);margin-bottom:40px;
}
.map-sidebar{
  background:#0D1520;padding:24px;
  border-right:1px solid rgba(255,255,255,.08);
}
html[data-lang="en"] .map-sidebar { border-right:none; border-left:1px solid rgba(255,255,255,.08) }
.map-sidebar h3{color:var(--amber-glow);margin-bottom:16px;font-size:1rem}
.location-filters{display:flex;flex-wrap:wrap;gap:8px;margin-bottom:20px}
.filter-btn{
  background:rgba(255,255,255,.05);border:1px solid rgba(255,255,255,.1);
  color:var(--text-muted);padding:6px 12px;border-radius:15px;
  font-size:.8rem;cursor:pointer;transition:all .2s;
}
html[data-lang="ar"] .filter-btn { font-family:var(--font-ar) }
html[data-lang="en"] .filter-btn { font-family:var(--font-en) }
.filter-btn.active,.filter-btn:hover{background:rgba(212,130,10,.2);border-color:var(--amber);color:var(--amber-glow)}
.locations-list{overflow-y:auto;max-height:360px}
.location-item{
  padding:12px;border-radius:10px;cursor:pointer;margin-bottom:8px;
  transition:all .2s;border:1px solid transparent;
}
.location-item:hover{background:rgba(212,130,10,.1);border-color:rgba(212,130,10,.2)}
.location-item.active{background:rgba(212,130,10,.2);border-color:var(--amber)}
.li-name{font-weight:700;font-size:.9rem;color:var(--white)}
.li-type{font-size:.75rem;color:var(--text-muted);margin-top:2px}
.li-emoji{font-size:1.2rem;float:left;margin-left:10px}
html[data-lang="en"] .li-emoji{float:right;margin-left:0;margin-right:10px}
#leafletMap{width:100%;height:500px}
.route-planner{
  background:linear-gradient(135deg,rgba(255,255,255,.04),rgba(255,255,255,.01));
  border:1px solid rgba(255,255,255,.08);border-radius:var(--radius);padding:28px;
}
.route-planner h3{margin-bottom:16px;font-size:1.1rem}
.route-options{display:flex;gap:12px;flex-wrap:wrap;margin-bottom:16px}
.route-btn{
  background:rgba(255,255,255,.06);border:1px solid rgba(255,255,255,.12);
  color:var(--white);padding:10px 20px;border-radius:20px;
  cursor:pointer;transition:all .2s;font-size:.9rem;
}
html[data-lang="ar"] .route-btn { font-family:var(--font-ar) }
html[data-lang="en"] .route-btn { font-family:var(--font-en) }
.route-btn:hover{background:rgba(14,140,122,.2);border-color:var(--teal-light);color:var(--teal-light);transform:translateY(-2px)}
.route-result{
  background:rgba(14,140,122,.1);border:1px solid rgba(14,140,122,.3);
  border-radius:10px;padding:16px;display:none;
  color:var(--text-light);line-height:1.8;font-size:.9rem;
}
.route-result.show{display:block;animation:fadeUp .4s ease}

/* ═══════════════════════════════════════════
   AR
═══════════════════════════════════════════ */
.ar-section{padding:100px 0;background:linear-gradient(180deg,#060A10,#0A0E1A)}
.ar-grid{display:grid;grid-template-columns:1fr 1fr;gap:60px;align-items:center}
.ar-viewport{
  width:100%;aspect-ratio:4/3;
  background:linear-gradient(135deg,#0A1A20,#050D15);
  border-radius:20px;overflow:hidden;position:relative;
  border:2px solid rgba(14,140,122,.4);box-shadow:0 0 40px rgba(14,140,122,.2);
}
.ar-overlay{position:absolute;inset:0;z-index:10;pointer-events:none}
.ar-scan-line{
  position:absolute;width:100%;height:2px;
  background:linear-gradient(90deg,transparent,var(--teal-light),transparent);
  animation:scanLine 3s linear infinite;
}
.ar-corner{position:absolute;width:30px;height:30px;border-color:var(--teal-light);border-style:solid}
.tl{top:16px;right:16px;border-width:2px 0 0 2px}
.tr{top:16px;left:16px;border-width:2px 2px 0 0}
.bl{bottom:16px;right:16px;border-width:0 0 2px 2px}
.br{bottom:16px;left:16px;border-width:0 2px 2px 0}
html[data-lang="en"] .tl{right:auto;left:16px;border-width:2px 2px 0 0}
html[data-lang="en"] .tr{left:auto;right:16px;border-width:2px 0 0 2px}
html[data-lang="en"] .bl{right:auto;left:16px;border-width:0 2px 2px 0}
html[data-lang="en"] .br{left:auto;right:16px;border-width:0 0 2px 2px}
.ar-info-popup{
  position:absolute;bottom:20px;right:16px;left:16px;
  background:rgba(0,0,0,.75);backdrop-filter:blur(10px);
  border:1px solid rgba(14,140,122,.5);border-radius:12px;padding:14px;
}
.ar-tag{font-size:.7rem;color:var(--teal-light);font-weight:700;letter-spacing:2px;display:block;margin-bottom:4px}
.ar-info-popup h4{font-size:1rem;margin-bottom:4px}
.ar-info-popup p{font-size:.8rem;color:var(--text-muted);line-height:1.5}
.ar-date{
  display:inline-block;margin-top:6px;
  background:rgba(212,130,10,.2);color:var(--amber);
  padding:2px 10px;border-radius:10px;font-size:.75rem;
}
.ar-scene{
  width:100%;height:100%;display:flex;align-items:flex-end;
  justify-content:center;padding-bottom:80px;perspective:600px;
}
.monument-3d{
  width:120px;height:120px;position:relative;
  transform-style:preserve-3d;
  animation:rotateMon 8s linear infinite;
}
.m-wall{position:absolute;width:100px;height:80px;background:linear-gradient(180deg,#C9A96E,#8B5E2E);border:1px solid rgba(255,255,255,.1)}
.front{transform:translateZ(50px);top:20px;left:10px}
.back{transform:translateZ(-50px) rotateY(180deg);top:20px;left:10px}
.left-w{transform:rotateY(-90deg) translateZ(10px);top:20px;width:100px}
.right-w{transform:rotateY(90deg) translateZ(90px);top:20px;width:100px}
.m-roof{position:absolute;width:120px;height:100px;top:0;left:0;background:linear-gradient(135deg,#E8C878,#B08040);transform:rotateX(90deg) translateZ(-40px)}
.m-tower{position:absolute;width:20px;height:40px;background:linear-gradient(180deg,#D4A060,#8B5E2E);bottom:0;border-top:6px solid #B08040}
.t1{right:0} .t2{left:0}
.ar-ground{position:absolute;bottom:60px;width:160px;height:20px;background:linear-gradient(180deg,#C9A96E,#8B5E2E);border-radius:50%;opacity:.7;filter:blur(4px)}
.ar-particles{position:absolute;inset:0}
.ar-particle{position:absolute;width:3px;height:3px;background:var(--amber);border-radius:50%;animation:floatParticle linear infinite;opacity:0}
.ar-controls{display:flex;flex-wrap:wrap;gap:10px;margin-top:16px}
.ar-btn{
  background:rgba(14,140,122,.15);border:1px solid rgba(14,140,122,.4);
  color:var(--teal-light);padding:8px 16px;border-radius:15px;
  cursor:pointer;font-size:.85rem;transition:all .2s;
}
html[data-lang="ar"] .ar-btn { font-family:var(--font-ar) }
html[data-lang="en"] .ar-btn { font-family:var(--font-en) }
.ar-btn:hover{background:rgba(14,140,122,.3);transform:translateY(-2px)}
.ar-camera{background:rgba(212,130,10,.15);border-color:rgba(212,130,10,.4);color:var(--amber)}
.ar-steps{display:flex;flex-direction:column;gap:24px;margin-bottom:32px}
.ar-step{display:flex;align-items:flex-start;gap:16px}
.step-num{
  font-size:2rem;color:var(--amber);font-weight:700;line-height:1;min-width:40px;
}
html[data-lang="ar"] .step-num { font-family:var(--font-display-ar) }
html[data-lang="en"] .step-num { font-family:var(--font-display-en) }
.ar-step h4{color:var(--white);margin-bottom:4px;font-size:1rem}
.ar-step p{color:var(--text-muted);font-size:.9rem;line-height:1.6}
.ar-tech-badges{display:flex;flex-wrap:wrap;gap:10px}
.ar-tech-badges span{background:rgba(255,255,255,.06);border:1px solid rgba(255,255,255,.1);padding:5px 14px;border-radius:15px;font-size:.8rem;color:var(--text-muted)}

/* ═══════════════════════════════════════════
   AUDIENCE
═══════════════════════════════════════════ */
.audience{padding:100px 0;background:linear-gradient(180deg,#0D1520,#060A10)}
.audience-grid{display:grid;grid-template-columns:repeat(3,1fr);gap:20px}
.aud-card{
  background:rgba(255,255,255,.03);border:1px solid rgba(255,255,255,.07);
  border-radius:var(--radius);padding:28px;text-align:center;transition:all .3s;
}
.aud-card:hover{background:rgba(212,130,10,.08);border-color:rgba(212,130,10,.3);transform:translateY(-6px)}
.aud-icon{font-size:2.5rem;margin-bottom:12px;display:block}
.aud-card h4{color:var(--white);margin-bottom:8px}
.aud-card p{color:var(--text-muted);font-size:.85rem;line-height:1.6}

/* ═══════════════════════════════════════════
   BUSINESS
═══════════════════════════════════════════ */
.business{padding:100px 0;background:#060A10}
.biz-timeline{display:flex;flex-direction:column}
.biz-phase{
  display:grid;grid-template-columns:180px 1fr;gap:40px;
  align-items:center;padding:32px 0;
  border-bottom:1px solid rgba(255,255,255,.06);position:relative;
}
.phase-label{
  font-weight:700;font-size:.85rem;letter-spacing:2px;
  text-transform:uppercase;padding:8px 16px;border-radius:20px;text-align:center;
}
.short{background:rgba(14,140,122,.2);color:var(--teal-light);border:1px solid var(--teal)}
.mid{background:rgba(212,130,10,.2);color:var(--amber-glow);border:1px solid var(--amber)}
.long{background:rgba(139,94,46,.2);color:var(--sand-light);border:1px solid var(--sand)}
.phase-content h4{color:var(--white);margin-bottom:8px;font-size:1.05rem}
.phase-content p{color:var(--text-muted);line-height:1.7;font-size:.9rem}

/* ═══════════════════════════════════════════
   AI CHAT
═══════════════════════════════════════════ */
.ai-guide{padding:100px 0;background:linear-gradient(180deg,#0A0E1A,#0D1520)}
.chat-container{
  background:rgba(255,255,255,.03);border:1px solid rgba(255,255,255,.08);
  border-radius:20px;overflow:hidden;max-width:700px;margin:0 auto;
}
.chat-messages{
  height:350px;overflow-y:auto;padding:24px;
  display:flex;flex-direction:column;gap:16px;
}
.msg{display:flex;gap:12px;align-items:flex-start}
.msg.user{flex-direction:row-reverse}
.msg-avatar{
  width:36px;height:36px;border-radius:50%;
  background:rgba(212,130,10,.2);display:flex;
  align-items:center;justify-content:center;font-size:1.1rem;flex-shrink:0;
}
.msg.user .msg-avatar{background:rgba(14,140,122,.2)}
.msg-bubble{
  background:rgba(255,255,255,.06);padding:12px 16px;
  border-radius:16px 4px 16px 16px;max-width:80%;
  color:var(--text-light);font-size:.9rem;line-height:1.6;
}
html[data-lang="en"] .msg-bubble { border-radius:4px 16px 16px 16px }
html[data-lang="en"] .msg.user .msg-bubble { border-radius:16px 4px 16px 16px }
.msg.user .msg-bubble{background:rgba(14,140,122,.2)}
.typing .msg-bubble{color:var(--text-muted);font-style:italic}
.chat-input-area{
  display:flex;gap:12px;padding:16px 24px;
  border-top:1px solid rgba(255,255,255,.08);background:rgba(0,0,0,.2);
}
.chat-input-area input{
  flex:1;background:rgba(255,255,255,.06);
  border:1px solid rgba(255,255,255,.12);border-radius:25px;
  padding:10px 20px;color:var(--white);font-size:.9rem;outline:none;
  transition:border-color .2s;
}
html[data-lang="ar"] .chat-input-area input { font-family:var(--font-ar) }
html[data-lang="en"] .chat-input-area input { font-family:var(--font-en) }
.chat-input-area input:focus{border-color:var(--amber)}
.chat-input-area input::placeholder{color:var(--text-muted)}
.chat-send{
  background:linear-gradient(135deg,var(--amber),var(--amber-glow));
  color:var(--dark-brown);border:none;padding:10px 20px;
  border-radius:25px;font-weight:700;cursor:pointer;transition:all .2s;font-size:.9rem;
}
html[data-lang="ar"] .chat-send { font-family:var(--font-ar) }
html[data-lang="en"] .chat-send { font-family:var(--font-en) }
.chat-send:hover{transform:scale(1.05)}
.chat-suggestions{display:flex;flex-wrap:wrap;gap:8px;padding:12px 24px 16px}
.chat-suggestions button{
  background:rgba(255,255,255,.04);border:1px solid rgba(255,255,255,.1);
  color:var(--text-muted);padding:6px 14px;border-radius:15px;
  font-size:.8rem;cursor:pointer;transition:all .2s;
}
html[data-lang="ar"] .chat-suggestions button { font-family:var(--font-ar) }
html[data-lang="en"] .chat-suggestions button { font-family:var(--font-en) }
.chat-suggestions button:hover{background:rgba(212,130,10,.1);color:var(--amber);border-color:var(--amber)}

/* ═══════════════════════════════════════════
   CONTACT
═══════════════════════════════════════════ */
.contact{padding:100px 0;background:linear-gradient(180deg,#060A10,#0A0E1A)}
.contact-grid{display:grid;grid-template-columns:1fr 1fr;gap:60px;align-items:start}
.contact-info p{color:var(--text-muted);line-height:1.8;margin-bottom:32px}
.contact-items{display:flex;flex-direction:column;gap:16px}
.ci{display:flex;align-items:center;gap:12px;color:var(--text-light)}
.ci span:first-child{font-size:1.3rem}
.contact-form{display:flex;flex-direction:column;gap:14px}
.contact-form input,.contact-form select,.contact-form textarea{
  background:rgba(255,255,255,.05);border:1px solid rgba(255,255,255,.1);
  border-radius:10px;padding:12px 16px;color:var(--white);
  font-size:.9rem;outline:none;transition:border-color .2s;
}
html[data-lang="ar"] .contact-form input,
html[data-lang="ar"] .contact-form select,
html[data-lang="ar"] .contact-form textarea { font-family:var(--font-ar) }
html[data-lang="en"] .contact-form input,
html[data-lang="en"] .contact-form select,
html[data-lang="en"] .contact-form textarea { font-family:var(--font-en) }
.contact-form input:focus,.contact-form select:focus,.contact-form textarea:focus{border-color:var(--amber)}
.contact-form select option{background:#0D1520}
.contact-form textarea{resize:none}

/* ═══════════════════════════════════════════
   FOOTER
═══════════════════════════════════════════ */
.footer{background:#030609;border-top:1px solid rgba(255,255,255,.06);padding:60px 0 24px}
.footer-top{display:grid;grid-template-columns:2fr 1fr 1fr;gap:40px;margin-bottom:40px}
.footer-brand p{color:var(--text-muted);font-size:.9rem;margin-top:12px;line-height:1.7}
.footer-credit strong{color:var(--amber)}
.footer-links h4{color:var(--amber-glow);margin-bottom:16px;font-size:.95rem}
.footer-links ul{display:flex;flex-direction:column;gap:10px}
.footer-links ul li,.footer-links ul a{color:var(--text-muted);font-size:.85rem;transition:color .2s}
.footer-links ul a:hover{color:var(--amber-glow)}
.footer-bottom{
  border-top:1px solid rgba(255,255,255,.06);padding-top:24px;
  display:flex;justify-content:space-between;
  color:var(--text-muted);font-size:.8rem;flex-wrap:wrap;gap:8px;
}
.nav-logo-footer{font-size:2rem;color:var(--amber-glow);font-weight:700}
html[data-lang="ar"] .nav-logo-footer { font-family:var(--font-display-ar) }
html[data-lang="en"] .nav-logo-footer { font-family:var(--font-display-en) }

/* ═══════════════════════════════════════════
   ANIMATIONS
═══════════════════════════════════════════ */
@keyframes fadeUp{from{opacity:0;transform:translateY(30px)}to{opacity:1;transform:translateY(0)}}
@keyframes bounce{0%,100%{transform:translateX(-50%) translateY(0)}50%{transform:translateX(-50%) translateY(-10px)}}
@keyframes pulse{0%,100%{transform:scale(1)}50%{transform:scale(1.06)}}
@keyframes scanLine{from{top:0}to{top:100%}}
@keyframes rotateMon{from{transform:rotateX(15deg) rotateY(0deg)}to{transform:rotateX(15deg) rotateY(360deg)}}
@keyframes floatParticle{0%{transform:translateY(100px) translateX(0);opacity:0}20%{opacity:.8}100%{transform:translateY(-50px) translateX(var(--x,20px));opacity:0}}
@keyframes twinkle{0%,100%{opacity:.3}50%{opacity:1}}
.reveal{opacity:0;transform:translateY(40px);transition:all .7s ease}
.reveal.visible{opacity:1;transform:translateY(0)}

/* ═══════════════════════════════════════════
   RESPONSIVE
═══════════════════════════════════════════ */
@media(max-width:992px){
  .about-grid,.ar-grid,.contact-grid{grid-template-columns:1fr}
  .footer-top{grid-template-columns:1fr 1fr}
  .features-grid{grid-template-columns:1fr 1fr}
  .fc-main{grid-column:1/3;grid-row:auto}
  .audience-grid{grid-template-columns:repeat(2,1fr)}
  .map-wrapper{grid-template-columns:1fr}
  .map-sidebar{border-right:none!important;border-left:none!important;border-bottom:1px solid rgba(255,255,255,.08)}
}
@media(max-width:768px){
  .navbar{padding:14px 20px}
  .nav-links,.nav-cta{display:none}
  .hamburger{display:block}
  .hero-content{padding:100px 20px 40px}
  .hero-stats{gap:16px}
  .features-grid{grid-template-columns:1fr}
  .fc-main{grid-column:auto}
  .audience-grid{grid-template-columns:1fr}
  .biz-phase{grid-template-columns:1fr;gap:16px}
  .footer-top{grid-template-columns:1fr}
  .footer-bottom{flex-direction:column;text-align:center}
  .lang-toggle{bottom:16px;left:16px;padding:8px 14px;font-size:.8rem}
}

/* ═══════════════════════════════════════════
   TEMPLE EXPLORER
═══════════════════════════════════════════ */
.temple-explorer {
  padding:100px 0;
  background:linear-gradient(180deg,#0D1520,#060A10);
  position:relative;
  overflow:hidden;
}
.temple-explorer::before {
  content:'';
  position:absolute;
  inset:0;
  background:
    radial-gradient(ellipse 60% 40% at 50% 0%, rgba(212,130,10,0.06) 0%, transparent 70%),
    radial-gradient(ellipse 50% 30% at 20% 100%, rgba(14,140,122,0.05) 0%, transparent 60%);
  pointer-events:none;
}
.explorer-wrap {
  display:grid;
  grid-template-columns:1fr 280px;
  gap:28px;
  align-items:start;
}
@media(max-width:900px){
  .explorer-wrap { grid-template-columns:1fr; }
}
.explorer-canvas-wrap {
  position:relative;
  border-radius:20px;
  overflow:hidden;
  border:2px solid rgba(212,130,10,0.35);
  box-shadow:0 0 50px rgba(212,130,10,0.12), 0 30px 80px rgba(0,0,0,0.5);
}
#templeCanvas {
  display:block;
  width:100%;
  background:#0D1A10;
  cursor:crosshair;
  touch-action:none;
}
.explorer-hud {
  position:absolute;
  top:12px;
  left:12px;
  right:12px;
  display:flex;
  justify-content:space-between;
  align-items:center;
  pointer-events:none;
  z-index:5;
}
.hud-tag {
  font-size:.7rem;
  font-weight:700;
  letter-spacing:3px;
  text-transform:uppercase;
  color:var(--amber-glow);
  background:rgba(0,0,0,0.6);
  backdrop-filter:blur(8px);
  border:1px solid rgba(212,130,10,0.4);
  padding:5px 14px;
  border-radius:20px;
}
.hud-controls {
  font-size:.7rem;
  color:var(--text-muted);
  background:rgba(0,0,0,0.5);
  backdrop-filter:blur(8px);
  padding:5px 12px;
  border-radius:15px;
  border:1px solid rgba(255,255,255,0.08);
}

/* Game Start Overlay */
.game-start-overlay {
  position:absolute; inset:0; background:rgba(10,14,26,0.85); backdrop-filter:blur(8px);
  z-index:15; display:flex; align-items:center; justify-content:center; text-align:center;
  transition:opacity 0.5s ease;
}
.game-start-overlay.hidden { opacity:0; pointer-events:none; }
.gso-content h3 { font-size:2rem; color:var(--amber-glow); margin-bottom:12px; }
html[data-lang="ar"] .gso-content h3 { font-family:var(--font-display-ar); }
html[data-lang="en"] .gso-content h3 { font-family:var(--font-display-en); }
.gso-content p { color:var(--text-muted); margin-bottom:24px; }

/* Mobile touch pad */
.touch-pad {
  display:none;
  position:absolute;
  bottom:16px;
  left:50%;
  transform:translateX(-50%);
  z-index:10;
  background:rgba(0,0,0,0.45);
  backdrop-filter:blur(10px);
  border-radius:16px;
  padding:12px;
  border:1px solid rgba(255,255,255,0.1);
}
@media(max-width:768px){ .touch-pad{display:block} }
.dpad {
  display:grid;
  grid-template-columns:repeat(3,44px);
  grid-template-rows:repeat(3,44px);
  gap:4px;
}
.dpad-btn {
  background:rgba(212,130,10,0.2);
  border:1px solid rgba(212,130,10,0.4);
  color:var(--amber-glow);
  border-radius:10px;
  font-size:1.1rem;
  cursor:pointer;
  display:flex;
  align-items:center;
  justify-content:center;
  user-select:none;
  -webkit-user-select:none;
  transition:background 0.1s;
  -webkit-tap-highlight-color:transparent;
}
.dpad-btn:active { background:rgba(212,130,10,0.45); }
.dpad-center { grid-column:2; grid-row:2; opacity:0.3; pointer-events:none; }

/* Info popup */
.temple-popup {
  position:absolute;
  bottom:80px;
  left:50%;
  transform:translateX(-50%) translateY(20px);
  width:min(380px, 90%);
  background:rgba(5,10,20,0.92);
  backdrop-filter:blur(16px);
  border:1px solid rgba(212,130,10,0.5);
  border-radius:16px;
  padding:20px 22px;
  z-index:20;
  opacity:0;
  pointer-events:none;
  transition:all 0.35s cubic-bezier(.4,0,.2,1);
  box-shadow:0 20px 60px rgba(0,0,0,0.7), 0 0 30px rgba(212,130,10,0.15);
}
@media(max-width:768px){
  .temple-popup { bottom:140px; }
}
.temple-popup.visible {
  opacity:1;
  pointer-events:auto;
  transform:translateX(-50%) translateY(0);
}
.popup-icon { font-size:2rem; margin-bottom:8px; display:block; }
.popup-title { color:var(--amber-glow); font-size:1.05rem; font-weight:700; margin-bottom:6px; }
html[data-lang="ar"] .popup-title { font-family:var(--font-display-ar); }
html[data-lang="en"] .popup-title { font-family:var(--font-display-en); }
.popup-era {
  font-size:.7rem;
  letter-spacing:2px;
  text-transform:uppercase;
  color:var(--teal-light);
  margin-bottom:10px;
  display:block;
}
.popup-text { color:var(--text-muted); font-size:.875rem; line-height:1.7; }
.popup-close {
  position:absolute;
  top:10px;
  right:12px;
  background:none;
  border:none;
  color:var(--text-muted);
  font-size:1.2rem;
  cursor:pointer;
  line-height:1;
  transition:color .2s;
  padding:4px;
}
html[data-lang="ar"] .popup-close { right:auto; left:12px; }
.popup-close:hover { color:var(--amber); }

/* Sidebar */
.explorer-sidebar { display:flex; flex-direction:column; gap:16px; }
.sidebar-card {
  background:rgba(255,255,255,0.03);
  border:1px solid rgba(255,255,255,0.08);
  border-radius:16px;
  padding:20px;
}
.sidebar-card h4 { color:var(--amber-glow); margin-bottom:12px; font-size:.9rem; letter-spacing:1px; }
.node-legend { display:flex; flex-direction:column; gap:10px; }
.node-item {
  display:flex;
  align-items:center;
  gap:10px;
  font-size:.82rem;
  color:var(--text-muted);
  cursor:pointer;
  transition:color .2s;
  padding:4px 0;
}
.node-item:hover { color:var(--amber-glow); }
.node-dot {
  width:14px; height:14px; border-radius:50%; flex-shrink:0;
  border:2px solid rgba(255,255,255,0.3);
  animation:pulse 2.5s ease-in-out infinite;
}
.node-dot.amber  { background:var(--amber); }
.node-dot.teal   { background:var(--teal-light); }
.node-dot.white  { background:var(--sand-light); }
.node-dot.gold   { background:#E8C878; }
.node-dot.orange { background:#E06020; }

.explorer-legend-title {
  font-size:.75rem; color:var(--text-muted); margin-bottom:14px;
  text-transform:uppercase; letter-spacing:2px;
}
.visited-count {
  font-size:1.8rem; font-weight:700; color:var(--amber-glow); display:block;
}
html[data-lang="ar"] .visited-count { font-family:var(--font-display-ar); }
html[data-lang="en"] .visited-count { font-family:var(--font-display-en); }
.visited-label { font-size:.8rem; color:var(--text-muted); }

@keyframes nodeGlow {
  0%,100% { box-shadow:0 0 6px 0 currentColor; transform:scale(1); }
  50%      { box-shadow:0 0 14px 4px currentColor; transform:scale(1.15); }
}

/* ═══════════════════════════════════════════
   ACCESSIBILITY STYLES
═══════════════════════════════════════════ */
/* Toolbar */
.a11y-toolbar {
  position: fixed;
  bottom: 28px;
  right: 28px;
  z-index: 10000;
  display: flex;
  flex-direction: column;
  align-items: flex-end;
  gap: 12px;
}
.a11y-toggle {
  background: linear-gradient(135deg, var(--teal), var(--teal-light));
  color: var(--white);
  border: none;
  border-radius: 50%;
  width: 50px;
  height: 50px;
  font-size: 1.5rem;
  cursor: pointer;
  box-shadow: 0 5px 20px rgba(0,0,0,0.5);
  display: flex;
  align-items: center;
  justify-content: center;
  transition: all 0.3s;
}
.a11y-toggle:hover { transform: scale(1.1); }
.a11y-menu {
  display: none;
  flex-direction: column;
  gap: 8px;
  background: rgba(10, 14, 26, 0.95);
  backdrop-filter: blur(10px);
  padding: 16px;
  border-radius: 16px;
  border: 1px solid rgba(255,255,255,0.1);
  box-shadow: 0 10px 30px rgba(0,0,0,0.6);
  min-width: 180px;
}
.a11y-menu.show { display: flex; animation: fadeUp 0.3s ease; }
.a11y-btn {
  background: rgba(255,255,255,0.05);
  border: 1px solid rgba(255,255,255,0.1);
  color: var(--white);
  padding: 10px 14px;
  border-radius: 12px;
  cursor: pointer;
  text-align: right;
  font-size: 0.85rem;
  transition: all 0.2s;
  display: flex;
  align-items: center;
  justify-content: space-between;
}
html[data-lang="en"] .a11y-btn { text-align: left; flex-direction: row-reverse; }
.a11y-btn:hover, .a11y-btn.active {
  background: rgba(20, 184, 158, 0.2);
  border-color: var(--teal-light);
  color: var(--teal-light);
}

/* High Contrast Mode */
body.high-contrast {
  --sky-night: #000000 !important;
  --sky-dusk: #000000 !important;
  --text-muted: #FFFFFF !important;
  --text-light: #FFFFFF !important;
  --white: #FFFFFF !important;
  --amber: #FFFF00 !important;
  --amber-glow: #FFFF00 !important;
  --teal: #00FFFF !important;
  --teal-light: #00FFFF !important;
  --dark-brown: #000000 !important;
  --brown: #000000 !important;
  background: #000000 !important;
  color: #FFFFFF !important;
}
body.high-contrast * { text-shadow: none !important; box-shadow: none !important; }
body.high-contrast section { background: #000000 !important; border-bottom: 2px solid #FFFF00 !important; }
body.high-contrast::before, body.high-contrast::after, body.high-contrast .sand-layer, body.high-contrast .hero-bg, body.high-contrast .sand-particles-bg, body.high-contrast .oasis-glow, body.high-contrast .camel-layer { display: none !important; }
body.high-contrast .feature-card, body.high-contrast .a11y-menu { background: #000000 !important; border: 2px solid #FFFF00 !important; }
body.high-contrast .btn-primary, body.high-contrast .nav-cta, body.high-contrast .chat-send { background: #FFFF00 !important; color: #000000 !important; border: 2px solid #FFFF00 !important; }

/* Simplified Visual Mode */
body.simplified-mode p:not(.hero-sub) { display: none !important; }
body.simplified-mode h2, body.simplified-mode h3, body.simplified-mode h4 { border-bottom: 1px solid var(--amber); padding-bottom: 8px; margin-bottom: 16px; }
body.simplified-mode .feature-card, body.simplified-mode .aud-card { text-align: center; display: flex; flex-direction: column; align-items: center; justify-content: center; padding: 40px 20px; }
body.simplified-mode .fc-icon, body.simplified-mode .aud-icon { font-size: 4rem; margin-bottom: 20px; }
body.simplified-mode .btn-primary, body.simplified-mode .nav-cta { padding: 20px 40px; font-size: 1.2rem; border-radius: 40px; }

/* Audio Focus Outline */
.audio-focus {
  outline: 4px solid var(--amber-glow) !important;
  outline-offset: 4px !important;
  background: rgba(212,130,10,0.1) !important;
}

/* Highlight spoken section */
.spoken-section {
  background: rgba(212,130,10,0.15) !important;
  border-radius: 8px;
  box-shadow: 0 0 15px rgba(212,130,10,0.3) !important;
  transition: background 0.3s ease, box-shadow 0.3s ease;
  position: relative;
  z-index: 5;
}
/* TTS Player Bar */
.tts-player-bar {
  position: fixed;
  bottom: -100px;
  left: 50%;
  transform: translateX(-50%);
  z-index: 10001;
  background: rgba(10, 14, 26, 0.95);
  backdrop-filter: blur(10px);
  border: 1px solid rgba(212, 130, 10, 0.4);
  border-radius: 30px;
  padding: 8px 18px;
  display: flex;
  align-items: center;
  gap: 14px;
  box-shadow: 0 10px 40px rgba(0,0,0,0.8), 0 0 20px rgba(212,130,10,0.2);
  transition: bottom 0.4s cubic-bezier(.4,0,.2,1);
}
.tts-player-bar.show {
  bottom: 24px;
}
.tts-ctrl-btn {
  background: transparent;
  border: none;
  color: var(--white);
  font-size: 1.2rem;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  width: 36px;
  height: 36px;
  border-radius: 50%;
  transition: background 0.2s;
}
.tts-ctrl-btn:hover { background: rgba(255,255,255,0.1); color: var(--amber-glow); }
.tts-status-text {
  font-size: 0.85rem;
  color: var(--text-light);
  max-width: 140px;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
  font-weight: 600;
}
</style>
</head>
<body>

<!-- ══ DESERT BACKGROUND ELEMENTS ══ -->
<div class="camel-layer" aria-hidden="true">
  <svg class="camel-svg c1" viewBox="0 0 200 100" fill="#C9A96E" xmlns="http://www.w3.org/2000/svg">
    <ellipse cx="80" cy="58" rx="48" ry="22"/><ellipse cx="130" cy="62" rx="32" ry="18"/>
    <ellipse cx="70" cy="38" rx="22" ry="18"/><ellipse cx="105" cy="42" rx="16" ry="13"/>
    <rect x="148" y="28" width="12" height="36" rx="6"/><ellipse cx="162" cy="22" rx="14" ry="10"/>
    <rect x="48" y="76" width="8" height="22" rx="4"/><rect x="68" y="76" width="8" height="22" rx="4"/>
    <rect x="108" y="76" width="8" height="22" rx="4"/><rect x="128" y="76" width="8" height="22" rx="4"/>
    <path d="M32 60 Q20 50 22 40" stroke="#C9A96E" stroke-width="4" fill="none" stroke-linecap="round"/>
  </svg>
  <svg class="camel-svg c2" viewBox="0 0 200 100" fill="#C9A96E" xmlns="http://www.w3.org/2000/svg">
    <ellipse cx="80" cy="58" rx="48" ry="22"/><ellipse cx="130" cy="62" rx="32" ry="18"/>
    <ellipse cx="70" cy="38" rx="22" ry="18"/><ellipse cx="105" cy="42" rx="16" ry="13"/>
    <rect x="148" y="28" width="12" height="36" rx="6"/><ellipse cx="162" cy="22" rx="14" ry="10"/>
    <rect x="48" y="76" width="8" height="22" rx="4"/><rect x="68" y="76" width="8" height="22" rx="4"/>
    <rect x="108" y="76" width="8" height="22" rx="4"/><rect x="128" y="76" width="8" height="22" rx="4"/>
  </svg>
  <svg class="camel-svg c3" viewBox="0 0 200 100" fill="#C9A96E" xmlns="http://www.w3.org/2000/svg">
    <ellipse cx="80" cy="58" rx="48" ry="22"/><ellipse cx="130" cy="62" rx="32" ry="18"/>
    <ellipse cx="70" cy="38" rx="22" ry="18"/><ellipse cx="105" cy="42" rx="16" ry="13"/>
    <rect x="148" y="28" width="12" height="36" rx="6"/><ellipse cx="162" cy="22" rx="14" ry="10"/>
    <rect x="48" y="76" width="8" height="22" rx="4"/><rect x="68" y="76" width="8" height="22" rx="4"/>
    <rect x="108" y="76" width="8" height="22" rx="4"/><rect x="128" y="76" width="8" height="22" rx="4"/>
  </svg>
</div>
<div class="sand-particles-bg" id="sandParticlesBg" aria-hidden="true"></div>
<div class="oasis-glow" aria-hidden="true"></div>

<!-- ══ LANG TOGGLE ══ -->
<button class="lang-toggle" id="langToggle" onclick="toggleLang()" aria-label="Toggle Language" tabindex="0">
  <span id="langFlag">🇬🇧</span>
  <span id="langLabel">English</span>
</button>

<!-- ══ ACCESSIBILITY TOOLBAR ══ -->
<div class="a11y-toolbar">
  <div class="a11y-menu" id="a11yMenu" role="menu">
    <button class="a11y-btn" onclick="toggleAudioMode()" id="btnAudioMode" role="menuitem">
      <span>🔊</span> <span class="ar-inline">قراءة صوتية</span><span class="en-inline">Speak Navigation</span>
    </button>
    <button class="a11y-btn" onclick="toggleSimplifiedMode()" id="btnSimplifiedMode" role="menuitem">
      <span>👁️</span> <span class="ar-inline">عرض مبسط</span><span class="en-inline">Simplified View</span>
    </button>
    <button class="a11y-btn" onclick="toggleHighContrast()" id="btnHighContrast" role="menuitem">
      <span>🌗</span> <span class="ar-inline">تباين عالٍ</span><span class="en-inline">High Contrast</span>
    </button>
    <div style="display:flex;gap:4px">
      <button class="a11y-btn" style="flex:1;justify-content:center" onclick="changeTextSize(1)" aria-label="Increase Text Size" role="menuitem">A+</button>
      <button class="a11y-btn" style="flex:1;justify-content:center" onclick="changeTextSize(-1)" aria-label="Decrease Text Size" role="menuitem">A-</button>
    </div>
  </div>
  <button class="a11y-toggle" id="a11yToggle" onclick="document.getElementById('a11yMenu').classList.toggle('show')" aria-label="Accessibility Menu" tabindex="0">
    ♿
  </button>
</div>

<!-- ══ TTS PLAYER BAR ══ -->
<div class="tts-player-bar" id="ttsPlayerBar">
  <button class="tts-ctrl-btn" id="ttsPlayPauseBtn" onclick="toggleTTSPlayback()">⏸️</button>
  <button class="tts-ctrl-btn" onclick="stopTTS()">⏹️</button>
  <span class="tts-status-text" id="ttsStatusText"><span class="ar">يتم القراءة...</span><span class="en">Listening...</span></span>
</div>

<!-- ══ NAVBAR ══ -->
<nav class="navbar" id="navbar">
  <div class="nav-logo">
    <span class="logo-w">W</span>adi<span class="logo-360">360</span>
  </div>
  <ul class="nav-links">
    <li><a href="#hero"><span class="ar">الرئيسية</span><span class="en">Home</span></a></li>
    <li><a href="#about"><span class="ar">عن المشروع</span><span class="en">About</span></a></li>
    <li><a href="#features"><span class="ar">المميزات</span><span class="en">Features</span></a></li>
    <li><a href="#mapSection"><span class="ar">الخريطة</span><span class="en">Map</span></a></li>
    <li><a href="#ar"><span class="ar">AR</span><span class="en">AR</span></a></li>
    <li><a href="#templeExplorer"><span class="ar">مستكشف المعبد</span><span class="en">Temple Explorer</span></a></li>
    <li><a href="#contact"><span class="ar">تواصل</span><span class="en">Contact</span></a></li>
  </ul>
  <div style="display:flex;">
    <button class="nav-cta" onclick="startGlobalTTS()" style="background:rgba(212,130,10,0.2);border:1px solid var(--amber);color:var(--amber-glow);margin-right:12px;margin-left:12px;">
      🔊 <span class="ar-inline">استمع للصفحة</span><span class="en-inline">Read Aloud</span>
    </button>
    <button class="nav-cta" onclick="document.getElementById('mapSection').scrollIntoView({behavior:'smooth'})">
      <span class="ar">استكشف الآن</span><span class="en">Explore Now</span>
    </button>
  </div>
  <button class="hamburger" id="hamburger">&#9776;</button>
</nav>

<!-- ══ HERO ══ -->
<section class="hero" id="hero">
  <div class="hero-bg">
    <div class="sand-layer s1"></div>
    <div class="sand-layer s2"></div>
    <div class="sand-layer s3"></div>
    <div class="stars" id="stars"></div>
    <div class="dunes"></div>
  </div>
  <div class="hero-content">
    <p class="hero-pre"><span class="ar">مرحباً بكم في</span><span class="en">Welcome to</span></p>
    <h1 class="hero-title">
      <span class="title-wadi">Wadi</span>
      <span class="title-360">360</span>
    </h1>
    <p class="hero-sub">
      <span class="ar">اكتشف الوادي الجديد كما لم تره من قبل</span>
      <span class="en">Discover New Valley Like Never Before</span>
    </p>
    <p class="hero-desc">
      <span class="ar">منصة سياحية ذكية تجمع بين الذكاء الاصطناعي والواقع المعزز لتحوّل كل زيارة إلى رحلة تفاعلية لا تُنسى</span>
      <span class="en">A smart tourism platform combining AI and Augmented Reality to transform every visit into an unforgettable interactive journey through Egypt's hidden gem.</span>
    </p>
    <div class="hero-btns">
      <button class="btn-primary" onclick="document.getElementById('mapSection').scrollIntoView({behavior:'smooth'})">
        <span>🗺️</span>
        <span class="ar">ابدأ الاستكشاف</span><span class="en">Start Exploring</span>
      </button>
      <button class="btn-secondary" onclick="document.getElementById('ar').scrollIntoView({behavior:'smooth'})">
        <span>🔮</span>
        <span class="ar">جرّب الواقع المعزز</span><span class="en">Try AR Experience</span>
      </button>
    </div>
    <div class="hero-stats">
      <div class="stat">
        <span class="stat-num" data-target="440098">0</span><span class="stat-unit ar-inline">كم²</span><span class="stat-unit en-inline">km²</span>
        <p><span class="ar">أكبر محافظة في مصر</span><span class="en">Egypt's largest governorate</span></p>
      </div>
      <div class="stat-divider"></div>
      <div class="stat">
        <span class="stat-num" data-target="19">0</span><span class="stat-unit ar-inline">مليون</span><span class="stat-unit en-inline">M</span>
        <p><span class="ar">سائح في مصر 2025</span><span class="en">tourists in Egypt 2025</span></p>
      </div>
      <div class="stat-divider"></div>
      <div class="stat">
        <span class="stat-num" data-target="30">0</span><span class="stat-unit ar-inline">مليون</span><span class="stat-unit en-inline">M</span>
        <p><span class="ar">هدف مصر 2030</span><span class="en">Egypt's 2030 target</span></p>
      </div>
    </div>
  </div>
  <div class="scroll-hint">
    <div class="scroll-line"></div>
    <span class="ar">تمرير للأسفل</span><span class="en">Scroll down</span>
  </div>
</section>

<!-- ══ ABOUT ══ -->
<section class="about" id="about">
  <div class="container">
    <div class="section-tag"><span class="ar">عن المشروع</span><span class="en">About</span></div>
    <h2 class="section-title">
      <span class="ar">ما هو <span>Wadi 360</span>؟</span>
      <span class="en">What is <span>Wadi 360</span>?</span>
    </h2>
    <div class="about-grid">
      <div class="about-text">
        <p>
          <span class="ar">Wadi360 هو منصة سياحية مبتكرة مصمّمة لإعادة تعريف طريقة استكشاف الوادي الجديد في مصر. إنه ليس مجرد موقع إلكتروني — بل دليل رقمي ذكي يربط الماضي بالحاضر من خلال تقنيات غامرة.</span>
          <span class="en">Wadi360 is an innovative tourism platform designed to redefine the way visitors explore Egypt's New Valley. It is not just a website — it is a smart digital guide that connects the past with the present through immersive technologies.</span>
        </p>
        <p>
          <span class="ar">من خلال دمج الواقع المعزز وتجارب الذكاء الاصطناعي والخرائط التفاعلية ومسارات السفر المخصصة، يحوّل Wadi360 كل زيارة إلى رحلة تفاعلية حية.</span>
          <span class="en">By integrating AR, AI-powered experiences, interactive maps, and personalized travel routes, Wadi360 transforms every visit into a living, interactive journey.</span>
        </p>
        <div class="about-badges">
          <span class="badge">🤖 <span class="ar">ذكاء اصطناعي</span><span class="en">AI Powered</span></span>
          <span class="badge">🔮 <span class="ar">واقع معزز</span><span class="en">Augmented Reality</span></span>
          <span class="badge">🗺️ <span class="ar">خرائط تفاعلية</span><span class="en">Interactive Maps</span></span>
          <span class="badge">🎧 <span class="ar">مرشد صوتي</span><span class="en">Audio Guide</span></span>
          <span class="badge">♿ <span class="ar">شامل للجميع</span><span class="en">Inclusive</span></span>
        </div>
      </div>
      <div class="about-visual">
        <div class="phone-mockup">
          <div class="phone-screen">
            <div class="phone-map-demo">
              <div class="map-pin p1">📍<span><span class="ar">واحة الداخلة</span><span class="en">Dakhla Oasis</span></span></div>
              <div class="map-pin p2">📍<span><span class="ar">قصر الزيان</span><span class="en">Qasr El-Zayan</span></span></div>
              <div class="map-pin p3">📍<span><span class="ar">معبد هيبيس</span><span class="en">Hibis Temple</span></span></div>
            </div>
            <div class="phone-status-bar">
              <span>Wadi360</span><span>📡 AR <span class="ar">نشط</span><span class="en">Live</span></span>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</section>

<!-- ══ FEATURES ══ -->
<section class="features" id="features">
  <div class="container">
    <div class="section-tag"><span class="ar">المميزات</span><span class="en">Features</span></div>
    <h2 class="section-title">
      <span class="ar">لماذا <span>Wadi 360</span>؟</span>
      <span class="en">Why <span>Wadi 360</span>?</span>
    </h2>
    <div class="features-grid">
      <div class="feature-card fc-main reveal">
        <div class="fc-icon">🤖</div>
        <h3><span class="ar">الذكاء الاصطناعي</span><span class="en">Artificial Intelligence</span></h3>
        <p><span class="ar">توليد مقاطع فيديو تُعيد بناء المعالم التاريخية وتوصيات مخصصة بناءً على اهتماماتك</span><span class="en">Generate dynamic videos reconstructing historical landmarks with personalized smart recommendations based on your interests.</span></p>
        <ul>
          <li>✓ <span class="ar">تعليق صوتي بلغات متعددة</span><span class="en">Multi-language audio narration</span></li>
          <li>✓ <span class="ar">تحليل سلوك المستخدم</span><span class="en">User behavior analysis</span></li>
          <li>✓ <span class="ar">اقتراحات ذكية للمسارات</span><span class="en">Smart route suggestions</span></li>
        </ul>
      </div>
      <div class="feature-card reveal">
        <div class="fc-icon">🔮</div>
        <h3><span class="ar">الواقع المعزز</span><span class="en">Augmented Reality</span></h3>
        <p><span class="ar">شاهد المعالم التاريخية كما كانت في الماضي مباشرةً من متصفح هاتفك</span><span class="en">View historical monuments as they appeared in the past directly through your smartphone browser.</span></p>
      </div>
      <div class="feature-card reveal">
        <div class="fc-icon">🗺️</div>
        <h3><span class="ar">خرائط تفاعلية</span><span class="en">Interactive Maps</span></h3>
        <p><span class="ar">تتبع موقعك الفعلي واحصل على مسارات سياحية مخصصة لاهتماماتك</span><span class="en">Track your real location and get customized tourism routes tailored to your interests.</span></p>
      </div>
      <div class="feature-card reveal">
        <div class="fc-icon">🏛️</div>
        <h3><span class="ar">التراث الثقافي</span><span class="en">Cultural Heritage</span></h3>
        <p><span class="ar">استكشف المعالم الأثرية والدينية والطبيعية بعمق تاريخي غير مسبوق</span><span class="en">Explore archaeological, religious, and natural landmarks with unprecedented historical depth.</span></p>
      </div>
      <div class="feature-card reveal">
        <div class="fc-icon">🏪</div>
        <h3><span class="ar">دعم الأعمال المحلية</span><span class="en">Local Business Support</span></h3>
        <p><span class="ar">ادعم الفنادق والمطاعم والمرشدين السياحيين المحليين المندمجين في المنصة</span><span class="en">Support local hotels, restaurants, and tour guides integrated into the platform.</span></p>
      </div>
      <div class="feature-card reveal">
        <div class="fc-icon">♿</div>
        <h3><span class="ar">سياحة شاملة</span><span class="en">Inclusive Tourism</span></h3>
        <p><span class="ar">ميزات إمكانية الوصول لذوي الإعاقات تشمل الأدلة الصوتية والملاحة الميسّرة</span><span class="en">Accessibility features for people with disabilities including audio guides and accessible navigation.</span></p>
      </div>
    </div>
  </div>
</section>

<!-- ══ MAP ══ -->
<section class="map-section" id="mapSection">
  <div class="container">
    <div class="section-tag"><span class="ar">استكشف</span><span class="en">Explore</span></div>
    <h2 class="section-title">
      <span class="ar">خريطة <span>الوادي الجديد</span> التفاعلية</span>
      <span class="en"><span>New Valley</span> Interactive Map</span>
    </h2>
    <div class="map-wrapper">
      <div class="map-sidebar">
        <h3><span class="ar">المواقع السياحية</span><span class="en">Tourist Sites</span></h3>
        <div class="location-filters">
          <button class="filter-btn active" data-filter="all"><span class="ar">الكل</span><span class="en">All</span></button>
          <button class="filter-btn" data-filter="historical"><span class="ar">تاريخية</span><span class="en">Historical</span></button>
          <button class="filter-btn" data-filter="nature"><span class="ar">طبيعية</span><span class="en">Nature</span></button>
          <button class="filter-btn" data-filter="religious"><span class="ar">دينية</span><span class="en">Religious</span></button>
        </div>
        <div class="locations-list" id="locationsList"></div>
      </div>
      <div id="leafletMap"></div>
    </div>
    <div class="route-planner">
      <h3>🧭 <span class="ar">خطط لرحلتك</span><span class="en">Plan Your Route</span></h3>
      <div class="route-options">
        <button class="route-btn" onclick="planRoute('history')">🏛️ <span class="ar">تاريخية</span><span class="en">Historical</span></button>
        <button class="route-btn" onclick="planRoute('adventure')">🏕️ <span class="ar">مغامرة</span><span class="en">Adventure</span></button>
        <button class="route-btn" onclick="planRoute('nature')">🌿 <span class="ar">طبيعية</span><span class="en">Nature</span></button>
        <button class="route-btn" onclick="planRoute('religious')">🕌 <span class="ar">دينية</span><span class="en">Religious</span></button>
      </div>
      <div class="route-result" id="routeResult"></div>
    </div>
  </div>
</section>

<!-- ══ AR ══ -->
<section class="ar-section" id="ar">
  <div class="container">
    <div class="section-tag"><span class="ar">تقنية متقدمة</span><span class="en">Advanced Tech</span></div>
    <h2 class="section-title">
      <span class="ar">تجربة <span>الواقع المعزز</span></span>
      <span class="en">The <span>AR Experience</span></span>
    </h2>
    <div class="ar-grid">
      <div class="ar-demo">
        <div class="ar-viewport" id="arViewport">
          <div class="ar-overlay">
            <div class="ar-scan-line"></div>
            <div class="ar-corner tl"></div><div class="ar-corner tr"></div>
            <div class="ar-corner bl"></div><div class="ar-corner br"></div>
            <div class="ar-info-popup">
              <span class="ar-tag">🔮 AR <span class="ar">نشط</span><span class="en">ACTIVE</span></span>
              <h4 id="arSiteName">Qasr El-Zayan / قصر الزيان</h4>
              <p id="arSiteDesc">Built in the 12th century, one of the most beautiful Islamic palaces in New Valley.</p>
              <span class="ar-date" id="arSiteDate">12th Century AD</span>
            </div>
          </div>
          <div class="ar-scene" id="arScene">
            <div class="ar-monument" id="arMonument">
              <div class="monument-3d">
                <div class="m-wall front"></div><div class="m-wall back"></div>
                <div class="m-wall left-w"></div><div class="m-wall right-w"></div>
                <div class="m-roof"></div>
                <div class="m-tower t1"></div><div class="m-tower t2"></div>
              </div>
            </div>
            <div class="ar-ground"></div>
            <div class="ar-particles" id="arParticles"></div>
          </div>
        </div>
        <div class="ar-controls">
          <button class="ar-btn active-ar" onclick="changeARSite(0)"><span class="ar">قصر الزيان</span><span class="en">Qasr El-Zayan</span></button>
          <button class="ar-btn" onclick="changeARSite(1)"><span class="ar">معبد هيبيس</span><span class="en">Hibis Temple</span></button>
          <button class="ar-btn" onclick="changeARSite(2)"><span class="ar">واحة سيوة</span><span class="en">Siwa Oasis</span></button>
          <button class="ar-btn ar-camera" onclick="toggleCamera()">📷 <span class="ar">تفعيل الكاميرا</span><span class="en">Enable Camera</span></button>
        </div>
      </div>
      <div class="ar-info">
        <h3><span class="ar">كيف يعمل الواقع المعزز؟</span><span class="en">How does AR work?</span></h3>
        <div class="ar-steps">
          <div class="ar-step reveal">
            <span class="step-num">01</span>
            <div>
              <h4><span class="ar">وجّه كاميرتك</span><span class="en">Point Your Camera</span></h4>
              <p><span class="ar">افتح المنصة ووجّه كاميرا هاتفك نحو أي معلم أثري</span><span class="en">Open the platform and point your phone camera toward any historical landmark.</span></p>
            </div>
          </div>
          <div class="ar-step reveal">
            <span class="step-num">02</span>
            <div>
              <h4><span class="ar">التعرف التلقائي</span><span class="en">Auto Recognition</span></h4>
              <p><span class="ar">يتعرف الذكاء الاصطناعي على الموقع فوراً ويبدأ التراكب الرقمي</span><span class="en">AI instantly recognizes the site and begins digital overlay.</span></p>
            </div>
          </div>
          <div class="ar-step reveal">
            <span class="step-num">03</span>
            <div>
              <h4><span class="ar">استكشف التاريخ</span><span class="en">Explore History</span></h4>
              <p><span class="ar">شاهد الموقع كما كان في الماضي مع شرح تفاعلي كامل</span><span class="en">See the site as it appeared in the past with full interactive narration.</span></p>
            </div>
          </div>
        </div>
        <div class="ar-tech-badges">
          <span>WebXR</span><span>AR.js</span><span>Three.js</span><span>TensorFlow.js</span>
        </div>
      </div>
    </div>
  </div>
</section>

<!-- ══ AUDIENCE ══ -->
<section class="audience" id="audience">
  <div class="container">
    <div class="section-tag"><span class="ar">الجمهور المستهدف</span><span class="en">Target Audience</span></div>
    <h2 class="section-title">
      <span class="ar">Wadi 360 <span>للجميع</span></span>
      <span class="en">Wadi 360 is <span>for Everyone</span></span>
    </h2>
    <div class="audience-grid">
      <div class="aud-card reveal"><div class="aud-icon">🌍</div><h4><span class="ar">السياح الدوليون</span><span class="en">International Tourists</span></h4><p><span class="ar">تجارب أصيلة وغامرة في قلب الصحراء المصرية</span><span class="en">Authentic, immersive experiences in the heart of the Egyptian desert.</span></p></div>
      <div class="aud-card reveal"><div class="aud-icon">🇪🇬</div><h4><span class="ar">السياح المحليون</span><span class="en">Local Tourists</span></h4><p><span class="ar">اكتشف كنوز مصر المخفية خارج القاهرة والأقصر</span><span class="en">Discover Egypt's hidden treasures beyond Cairo and Luxor.</span></p></div>
      <div class="aud-card reveal"><div class="aud-icon">📱</div><h4><span class="ar">الشباب التقني</span><span class="en">Tech-Savvy Youth</span></h4><p><span class="ar">تجارب رقمية تفاعلية بأحدث تقنيات AR والذكاء الاصطناعي</span><span class="en">Digital interactive experiences using the latest AR and AI technologies.</span></p></div>
      <div class="aud-card reveal"><div class="aud-icon">🏛️</div><h4><span class="ar">عشاق التاريخ</span><span class="en">History Enthusiasts</span></h4><p><span class="ar">رحلة عميقة في أعماق التاريخ المصري الغني</span><span class="en">A deep journey into the rich depths of Egyptian history.</span></p></div>
      <div class="aud-card reveal"><div class="aud-icon">🏕️</div><h4><span class="ar">محبو المغامرات</span><span class="en">Adventure Seekers</span></h4><p><span class="ar">سفاري صحراوية وواحات طبيعية لا مثيل لها</span><span class="en">Desert safaris and unparalleled natural oases.</span></p></div>
      <div class="aud-card reveal"><div class="aud-icon">♿</div><h4><span class="ar">ذوو الإعاقات</span><span class="en">People with Disabilities</span></h4><p><span class="ar">تصميم شامل مع أدلة صوتية وملاحة ميسّرة</span><span class="en">Inclusive design with audio guides and accessible navigation.</span></p></div>
    </div>
  </div>
</section>

<!-- ══ BUSINESS MODEL ══ -->
<section class="business" id="business">
  <div class="container">
    <div class="section-tag"><span class="ar">نموذج الأعمال</span><span class="en">Business Model</span></div>
    <h2 class="section-title">
      <span class="ar">استراتيجية <span>الإيرادات</span></span>
      <span class="en"><span>Revenue</span> Strategy</span>
    </h2>
    <div class="biz-timeline">
      <div class="biz-phase reveal">
        <div class="phase-label short"><span class="ar">المدى القصير</span><span class="en">Short-Term</span></div>
        <div class="phase-content">
          <h4>📢 <span class="ar">الإعلانات والقوائم المميزة</span><span class="en">Advertising & Featured Listings</span></h4>
          <p><span class="ar">إدراج الفنادق والمطاعم والمرشدين السياحيين مقابل رسوم شهرية</span><span class="en">Listing hotels, restaurants, and tour guides for a monthly subscription fee.</span></p>
        </div>
      </div>
      <div class="biz-phase reveal">
        <div class="phase-label mid"><span class="ar">المدى المتوسط</span><span class="en">Mid-Term</span></div>
        <div class="phase-content">
          <h4>💳 <span class="ar">العمولات على الحجوزات</span><span class="en">Commission-Based Bookings</span></h4>
          <p><span class="ar">نسبة عمولة من كل حجز فندقي أو جولة سياحية يتم عبر المنصة</span><span class="en">Commission percentage from every hotel booking or tour made through the platform.</span></p>
        </div>
      </div>
      <div class="biz-phase reveal">
        <div class="phase-label long"><span class="ar">المدى البعيد</span><span class="en">Long-Term</span></div>
        <div class="phase-content">
          <h4>📊 <span class="ar">الاشتراكات والتحليلات</span><span class="en">Subscriptions & Analytics</span></h4>
          <p><span class="ar">خدمات تحليل بيانات السياحة للجهات الحكومية وشركات الاستثمار</span><span class="en">Tourism data analytics services for government entities and investment companies.</span></p>
        </div>
      </div>
    </div>
  </div>
</section>

<!-- ══ AI CHAT ══ -->
<section class="ai-guide" id="aiguide">
  <div class="container">
    <div class="section-tag"><span class="ar">مرشدك الذكي</span><span class="en">Smart Guide</span></div>
    <h2 class="section-title">
      <span class="ar">اسأل <span>مرشد Wadi 360</span></span>
      <span class="en">Ask <span>Wadi 360 Guide</span></span>
    </h2>
    <div class="chat-container">
      <div class="chat-messages" id="chatMessages">
        <div class="msg bot">
          <div class="msg-avatar">🤖</div>
          <div class="msg-bubble" id="welcomeMsg">
            <span class="ar">أهلاً! أنا مرشدك الذكي في رحلة الوادي الجديد. اسألني عن أي موقع أو رحلة أو تراث في المنطقة!</span>
            <span class="en">Hello! I'm your smart guide for the New Valley journey. Ask me about any site, trip, or heritage in the region!</span>
          </div>
        </div>
      </div>
      <div class="chat-input-area">
        <input type="text" id="chatInput" onkeydown="if(event.key==='Enter')sendMessage()"/>
        <button onclick="sendMessage()" class="chat-send">
          <span class="ar">إرسال ✈️</span><span class="en">Send ✈️</span>
        </button>
      </div>
      <div class="chat-suggestions" id="chatSuggestions"></div>
    </div>
  </div>
</section>

<!-- ══ CONTACT ══ -->
<section class="contact" id="contact">
  <div class="container">
    <div class="section-tag"><span class="ar">تواصل معنا</span><span class="en">Get in Touch</span></div>
    <h2 class="section-title">
      <span class="ar">هل أنت مستعد للـ<span>انطلاق؟</span></span>
      <span class="en">Ready to <span>Launch?</span></span>
    </h2>
    <div class="contact-grid">
      <div class="contact-info">
        <p>
          <span class="ar">سواء كنت مستثمراً أو شريكاً تجارياً أو مسافراً متحمساً، يسعدنا التواصل معك.</span>
          <span class="en">Whether you're an investor, business partner, or an enthusiastic traveler, we'd love to connect.</span>
        </p>
        <div class="contact-items">
          <div class="ci"><span>📧</span><a href="mailto:malakanwer23@gmail.com">malakanwer23@gmail.com</a></div>
          <div class="ci"><span>📞</span><span>01229121002</span></div>
          <div class="ci"><span>📍</span><span><span class="ar">الخارجة، الوادي الجديد، مصر</span><span class="en">Kharga, New Valley, Egypt</span></span></div>
        </div>
      </div>
      <form class="contact-form" onsubmit="submitForm(event)">
        <input type="text" name="name" required id="nameInput"/>
        <input type="email" name="email" required id="emailInput"/>
        <select name="subject" id="inquiryType">
          <option value=""><span class="ar">نوع الاستفسار</span><span class="en">Inquiry Type</span></option>
        </select>
        <textarea name="message" rows="4" required id="msgInput"></textarea>
        <button type="submit" class="btn-primary">
          <span class="ar">إرسال الرسالة 🚀</span><span class="en">Send Message 🚀</span>
        </button>
      </form>
    </div>
  </div>
</section>

<!-- ══ TEMPLE EXPLORER ══ -->
<section class="temple-explorer" id="templeExplorer">
  <div class="container">
    <div class="section-tag"><span class="ar">تجربة تفاعلية</span><span class="en">Interactive Experience</span></div>
    <h2 class="section-title">
      <span class="ar"><span>مستكشف المعبد</span> — جولة تفاعلية</span>
      <span class="en"><span>Temple Explorer</span> — Interactive Journey</span>
    </h2>
    <div class="explorer-wrap">
      <div class="explorer-canvas-wrap">
        <div class="explorer-hud">
          <span class="hud-tag">🏛️ <span class="ar">مستكشف المعبد</span><span class="en">Temple Explorer</span></span>
          <span class="hud-controls"><span class="ar">↑↓←→ للتحرك · اقترب من النقاط الذهبية</span><span class="en">Arrow keys / WASD · Reach golden nodes</span></span>
        </div>
        <canvas id="templeCanvas" width="720" height="480" aria-label="Interactive Temple Explorer Game" role="img">
          Your browser does not support HTML canvas.
        </canvas>
        <div class="sr-only" style="position:absolute;left:-10000px;top:auto;width:1px;height:1px;overflow:hidden;" tabindex="0">
          <p class="ar">خريطة تفاعلية أثرية. استخدم لوحة المفاتيح للتنقل بين المواقع مثل واحة الداخلة، معبد هيبيس، ومقابر مزواقة. تتوفر هذه المعلومات مفصلة في قائمة المواقع.</p>
          <p class="en">Interactive archaeological map. Use keyboard commands to navigate to sites like Dakhla Oasis, Hibis Temple, and Muzawaka Tombs. Detailed information is available in the sites list.</p>
        </div>
        <!-- Game Start Overlay -->
        <div class="game-start-overlay" id="gameStartOverlay">
          <div class="gso-content">
            <h3><span class="ar">مستكشف المعبد</span><span class="en">Temple Explorer</span></h3>
            <p><span class="ar">استكشف الآثار واكتشف أسرار الوادي الجديد</span><span class="en">Explore ruins and discover New Valley secrets</span></p>
            <button class="btn-primary" onclick="startTempleGame()" style="margin:0 auto;">
              <span>▶️</span>
              <span class="ar">ابدأ الجولة</span><span class="en">Start Tour</span>
            </button>
          </div>
        </div>
        <!-- Mobile D-Pad -->
        <div class="touch-pad">
          <div class="dpad">
            <div></div>
            <button class="dpad-btn" id="dUp">▲</button>
            <div></div>
            <button class="dpad-btn" id="dLeft">◀</button>
            <div class="dpad-center">⊙</div>
            <button class="dpad-btn" id="dRight">▶</button>
            <div></div>
            <button class="dpad-btn" id="dDown">▼</button>
            <div></div>
          </div>
        </div>
        <!-- Info Popup -->
        <div class="temple-popup" id="templePopup">
          <button class="popup-close" onclick="closeTemplePopup()">✕</button>
          <span class="popup-icon" id="popupIcon">🏛️</span>
          <div class="popup-title" id="popupTitle">Hibis Temple</div>
          <span class="popup-era" id="popupEra">589 BC</span>
          <p class="popup-text" id="popupText">Loading...</p>
        </div>
      </div>
      <div class="explorer-sidebar">
        <div class="sidebar-card">
          <p class="explorer-legend-title"><span class="ar">نقاط المعلومات</span><span class="en">Info Nodes</span></p>
          <div class="node-legend" id="nodeLegend"></div>
        </div>
        <div class="sidebar-card" style="text-align:center">
          <h4><span class="ar">مُكتشف</span><span class="en">Discovered</span></h4>
          <span class="visited-count" id="visitedCount">0</span>
          <span class="visited-label">/ <span id="totalNodes">5</span> <span class="ar">مواقع</span><span class="en">sites</span></span>
        </div>
        <div class="sidebar-card">
          <h4 style="margin-bottom:8px;font-size:.85rem"><span class="ar">نصيحة</span><span class="en">Tip</span></h4>
          <p style="color:var(--text-muted);font-size:.8rem;line-height:1.6">
            <span class="ar">اقترب من النقاط اللامعة لاكتشاف أسرار المعابد التاريخية في الوادي الجديد.</span>
            <span class="en">Walk close to the glowing nodes to unlock historical secrets of New Valley's ancient temples.</span>
          </p>
        </div>
      </div>
    </div>
  </div>
</section>

<!-- ══ FOOTER ══ -->
<footer class="footer">
  <div class="container">
    <div class="footer-top">
      <div class="footer-brand">
        <div class="nav-logo-footer"><span class="logo-w">W</span>adi<span class="logo-360">360</span></div>
        <p><span class="ar">اكتشف الوادي الجديد كما لم تره من قبل</span><span class="en">Discover New Valley Like Never Before</span></p>
        <p class="footer-credit">Presented by <strong>ARath</strong></p>
      </div>
      <div class="footer-links">
        <h4><span class="ar">روابط سريعة</span><span class="en">Quick Links</span></h4>
        <ul>
          <li><a href="#about"><span class="ar">عن المشروع</span><span class="en">About</span></a></li>
          <li><a href="#features"><span class="ar">المميزات</span><span class="en">Features</span></a></li>
          <li><a href="#mapSection"><span class="ar">الخريطة</span><span class="en">Map</span></a></li>
          <li><a href="#ar"><span class="ar">الواقع المعزز</span><span class="en">AR Experience</span></a></li>
          <li><a href="#templeExplorer"><span class="ar">مستكشف المعبد</span><span class="en">Temple Explorer</span></a></li>
        </ul>
      </div>
      <div class="footer-links">
        <h4><span class="ar">التقنيات</span><span class="en">Technologies</span></h4>
        <ul>
          <li>HTML5 / CSS3 / JS</li>
          <li>Leaflet.js / Maps API</li>
          <li>WebXR / AR.js</li>
          <li>Claude AI API</li>
        </ul>
      </div>
    </div>
    <div class="footer-bottom">
      <p>© 2025 Wadi360 – ARath Team. <span class="ar">جميع الحقوق محفوظة.</span><span class="en">All rights reserved.</span></p>
      <p><span class="ar">مشروع تخرج – مرحلة الكبار</span><span class="en">Graduation Project – Senior Track</span></p>
    </div>
  </div>
</footer>

<script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>
<script>
/* ═══════════════════════════════════════════
   LANGUAGE SYSTEM
═══════════════════════════════════════════ */
let currentLang = 'ar';

function toggleLang() {
  currentLang = currentLang === 'ar' ? 'en' : 'ar';
  const html = document.documentElement;
  html.setAttribute('data-lang', currentLang);
  html.setAttribute('dir', currentLang === 'ar' ? 'rtl' : 'ltr');
  html.setAttribute('lang', currentLang);
  document.getElementById('langFlag').textContent = currentLang === 'ar' ? '🇬🇧' : '🇸🇦';
  document.getElementById('langLabel').textContent = currentLang === 'ar' ? 'English' : 'عربي';
  // Update chat placeholder
  const inp = document.getElementById('chatInput');
  inp.placeholder = currentLang === 'ar' ? 'اسأل عن الوادي الجديد...' : 'Ask about New Valley...';
  // Re-render suggestions
  renderSuggestions();
  // Re-render location list
  renderLocationList(currentFilter);
  // Update form placeholders
  updateFormPlaceholders();
  // Update select options
  updateSelectOptions();
}

function updateFormPlaceholders() {
  const isEn = currentLang === 'en';
  document.getElementById('nameInput').placeholder   = isEn ? 'Your Name' : 'اسمك الكريم';
  document.getElementById('emailInput').placeholder  = isEn ? 'Email Address' : 'بريدك الإلكتروني';
  document.getElementById('msgInput').placeholder    = isEn ? 'Your message...' : 'رسالتك...';
}

function updateSelectOptions() {
  const isEn = currentLang === 'en';
  const sel = document.getElementById('inquiryType');
  const opts = isEn
    ? ['Inquiry Type','Business Partnership','Investment','Tour Booking','General Inquiry']
    : ['نوع الاستفسار','شراكة تجارية','استثمار','حجز رحلة','استفسار عام'];
  sel.innerHTML = opts.map((o,i) => `<option value="${i===0?'':o}">${o}</option>`).join('');
}

/* ═══════════════════════════════════════════
   NAVBAR SCROLL
═══════════════════════════════════════════ */
window.addEventListener('scroll', () => {
  document.getElementById('navbar').classList.toggle('scrolled', scrollY > 60);
});

/* ═══════════════════════════════════════════
   STARS
═══════════════════════════════════════════ */
(function createStars() {
  const c = document.getElementById('stars');
  for (let i = 0; i < 200; i++) {
    const s = document.createElement('div');
    const sz = Math.random() * 2.5 + 0.5;
    s.style.cssText = `position:absolute;width:${sz}px;height:${sz}px;background:#fff;border-radius:50%;
      top:${Math.random()*60}%;left:${Math.random()*100}%;opacity:${Math.random()*.7+.2};
      animation:twinkle ${Math.random()*3+2}s ease-in-out infinite;animation-delay:${Math.random()*4}s`;
    c.appendChild(s);
  }
})();

/* ═══════════════════════════════════════════
   COUNTERS
═══════════════════════════════════════════ */
const statsObs = new IntersectionObserver(entries => {
  entries.forEach(e => {
    if (!e.isIntersecting) return;
    const el = e.target, target = +el.dataset.target, start = performance.now();
    function step(now) {
      const p = Math.min((now-start)/2000, 1);
      const ease = 1 - Math.pow(1-p, 3);
      el.textContent = Math.floor(ease*target).toLocaleString();
      if (p < 1) requestAnimationFrame(step);
    }
    requestAnimationFrame(step);
    statsObs.unobserve(el);
  });
}, { threshold:.5 });
document.querySelectorAll('.stat-num').forEach(n => statsObs.observe(n));

/* ═══════════════════════════════════════════
   REVEAL ANIMATION
═══════════════════════════════════════════ */
const revObs = new IntersectionObserver(entries => {
  entries.forEach((e, i) => {
    if (!e.isIntersecting) return;
    setTimeout(() => e.target.classList.add('visible'), i * 80);
    revObs.unobserve(e.target);
  });
}, { threshold:.1 });
document.querySelectorAll('.reveal').forEach(el => revObs.observe(el));

/* ═══════════════════════════════════════════
   MAP
═══════════════════════════════════════════ */
const locations = [
  { id:1, nameAr:'واحة الداخلة',   nameEn:'Dakhla Oasis',     lat:25.4889, lng:28.9969, type:'nature',     emoji:'🌴', descAr:'واحة خضراء ساحرة وسط الصحراء بمياه عيونها الطبيعية الدافئة', descEn:'A lush green oasis in the desert with warm natural spring waters.' },
  { id:2, nameAr:'واحة الخارجة',   nameEn:'Kharga Oasis',     lat:25.4380, lng:30.5461, type:'nature',     emoji:'🌿', descAr:'عاصمة الوادي الجديد وبوابته الكبرى الحافلة بالتراث',          descEn:'Capital of the New Valley, a grand gateway rich in heritage.' },
  { id:3, nameAr:'معبد هيبيس',     nameEn:'Hibis Temple',     lat:25.4525, lng:30.5383, type:'historical', emoji:'🏛️', descAr:'أقدم معبد مصري مكتمل، يعود إلى عصر الملك أحمس الثاني',     descEn:"Egypt's oldest complete temple, dating to King Ahmose II." },
  { id:4, nameAr:'قصر الزيان',     nameEn:'Qasr El-Zayan',    lat:25.5200, lng:28.8500, type:'historical', emoji:'🏰', descAr:'قصر أثري إسلامي من القرن الثاني عشر الميلادي',              descEn:'An Islamic archaeological palace from the 12th century AD.' },
  { id:5, nameAr:'عيون الدير',     nameEn:'Ain El-Deir',      lat:25.5000, lng:29.0000, type:'nature',     emoji:'♨️', descAr:'ينابيع طبيعية دافئة ذات خصائص علاجية مذهلة',               descEn:'Warm natural springs with remarkable therapeutic properties.' },
  { id:6, nameAr:'مقبرة مزواقة',   nameEn:'Muzawaka Tombs',   lat:25.4700, lng:28.9600, type:'historical', emoji:'⚱️', descAr:'مقابر أثرية نادرة تضم رسومات جدارية رومانية استثنائية',   descEn:'Rare archaeological tombs with exceptional Roman wall paintings.' },
  { id:7, nameAr:'دير الملاك',     nameEn:'Deir El-Malak',    lat:25.5100, lng:28.9800, type:'religious',  emoji:'⛪', descAr:'دير مسيحي قديم يمتد تاريخه إلى القرون الأولى للمسيحية',   descEn:'An ancient Christian monastery dating to the early centuries of Christianity.' },
  { id:8, nameAr:'جبل الطير',      nameEn:'Gebel El-Teir',    lat:25.3500, lng:30.4000, type:'nature',     emoji:'🦅', descAr:'قمة صخرية رائعة تتيح إطلالة بانورامية خلابة على الوادي',  descEn:'A magnificent rock peak offering stunning panoramic views of the valley.' }
];

let leafletMap, markers = [], currentFilter = 'all';

function initMap() {
  leafletMap = L.map('leafletMap', { center:[25.49,29.5], zoom:7 });
  L.tileLayer('https://{s}.basemaps.cartocdn.com/dark_all/{z}/{x}/{y}{r}.png', {
    attribution:'© OpenStreetMap © CARTO', maxZoom:19
  }).addTo(leafletMap);

  locations.forEach(loc => {
    const icon = L.divIcon({
      className:'',
      html:`<div style="background:linear-gradient(135deg,#D4820A,#F0A830);color:#3D1F0D;
        width:36px;height:36px;border-radius:50% 50% 50% 0;transform:rotate(-45deg);
        display:flex;align-items:center;justify-content:center;font-size:16px;
        box-shadow:0 4px 15px rgba(212,130,10,.5);border:2px solid rgba(255,255,255,.3);">
        <span style="transform:rotate(45deg)">${loc.emoji}</span></div>`,
      iconSize:[36,36], iconAnchor:[18,36]
    });
    const m = L.marker([loc.lat,loc.lng],{icon}).addTo(leafletMap);
    m.bindPopup(`
      <div style="font-family:'Cairo',sans-serif;direction:rtl;padding:8px;min-width:200px;">
        <h3 style="color:#D4820A;margin-bottom:6px;">${loc.emoji} ${currentLang==='ar'?loc.nameAr:loc.nameEn}</h3>
        <p style="color:#555;font-size:13px;line-height:1.5;">${currentLang==='ar'?loc.descAr:loc.descEn}</p>
      </div>`, { maxWidth:260 });
    markers.push({ m, loc });
  });
  renderLocationList('all');
}

function typeLabel(t) {
  const map = { historical:['تاريخي','Historical'], nature:['طبيعي','Nature'], religious:['ديني','Religious'] };
  return map[t] ? map[t][currentLang==='en'?1:0] : t;
}

function renderLocationList(filter) {
  const list = document.getElementById('locationsList');
  list.innerHTML = '';
  locations.filter(l => filter==='all' || l.type===filter).forEach(loc => {
    const el = document.createElement('div');
    el.className = 'location-item';
    el.innerHTML = `<span class="li-emoji">${loc.emoji}</span>
      <div class="li-name">${currentLang==='ar'?loc.nameAr:loc.nameEn}</div>
      <div class="li-type">${typeLabel(loc.type)}</div>`;
    el.onclick = () => {
      leafletMap.setView([loc.lat,loc.lng],11,{animate:true});
      const found = markers.find(x=>x.loc.id===loc.id);
      if(found) found.m.openPopup();
      document.querySelectorAll('.location-item').forEach(i=>i.classList.remove('active'));
      el.classList.add('active');
    };
    list.appendChild(el);
  });
}

document.querySelectorAll('.filter-btn').forEach(btn => {
  btn.onclick = () => {
    document.querySelectorAll('.filter-btn').forEach(b=>b.classList.remove('active'));
    btn.classList.add('active');
    currentFilter = btn.dataset.filter;
    renderLocationList(currentFilter);
    markers.forEach(({m,loc}) => {
      if(currentFilter==='all'||loc.type===currentFilter) m.addTo(leafletMap);
      else m.remove();
    });
  };
});

const routesData = {
  history: {
    ar:{ label:'🏛️ المسار التاريخي', stops:['معبد هيبيس','مقبرة مزواقة','قصر الزيان'], dur:'2-3 أيام', tip:'📝 ابدأ في الصباح الباكر لتتجنب الحرارة.' },
    en:{ label:'🏛️ Historical Route',  stops:['Hibis Temple','Muzawaka Tombs','Qasr El-Zayan'], dur:'2-3 days', tip:'📝 Start early in the morning to avoid the heat.' }
  },
  adventure: {
    ar:{ label:'🏕️ مسار المغامرة', stops:['جبل الطير','واحة الداخلة','عيون الدير'], dur:'3-4 أيام', tip:'📝 احضر معدات التخييم واستأجر دليلاً محلياً.' },
    en:{ label:'🏕️ Adventure Route',  stops:['Gebel El-Teir','Dakhla Oasis','Ain El-Deir'], dur:'3-4 days', tip:'📝 Bring camping gear and hire a local guide.' }
  },
  nature: {
    ar:{ label:'🌿 المسار الطبيعي', stops:['واحة الخارجة','عيون الدير','واحة الداخلة'], dur:'3 أيام', tip:'📝 أفضل وقت: أكتوبر – نوفمبر.' },
    en:{ label:'🌿 Nature Route',     stops:['Kharga Oasis','Ain El-Deir','Dakhla Oasis'], dur:'3 days', tip:'📝 Best time: October – November.' }
  },
  religious: {
    ar:{ label:'🕌 المسار الديني', stops:['دير الملاك','معبد هيبيس','واحة الخارجة'], dur:'2 أيام', tip:'📝 احترم قواعد اللباس المحتشم عند زيارة الأماكن الدينية.' },
    en:{ label:'⛪ Religious Route', stops:['Deir El-Malak','Hibis Temple','Kharga Oasis'], dur:'2 days', tip:'📝 Respect dress codes when visiting religious sites.' }
  }
};
window.planRoute = function(type) {
  const r = routesData[type][currentLang];
  const sep = currentLang==='ar' ? ' ← ' : ' → ';
  const res = document.getElementById('routeResult');
  res.innerHTML = `<strong style="color:#F0A830;">${r.label}</strong><br>
    <span style="color:#B09070;">${currentLang==='ar'?'المحطات:':'Stops:'} </span>${r.stops.join(sep)}<br>
    <span style="color:#B09070;">${currentLang==='ar'?'المدة:':'Duration:'} </span><strong>${r.dur}</strong><br><br>${r.tip}`;
  res.classList.add('show');
};

/* Map Init via IntersectionObserver */
const mapObserver = new IntersectionObserver(entries => {
  if (entries[0].isIntersecting) { initMap(); mapObserver.disconnect(); }
}, { threshold:.1 });
mapObserver.observe(document.getElementById('mapSection'));

/* ═══════════════════════════════════════════
   AR DEMO
═══════════════════════════════════════════ */
const arSites = [
  { en:{ name:'Qasr El-Zayan',  desc:'Built in the 12th century AD, one of the most beautiful Islamic palaces in the New Valley.', date:'12th Century AD' },
    ar:{ name:'قصر الزيان',    desc:'بُني في القرن الثاني عشر الميلادي، يُعدّ من أجمل القصور الإسلامية في الوادي الجديد.', date:'القرن الثاني عشر م' } },
  { en:{ name:'Hibis Temple',   desc:"Egypt's oldest complete temple, built during the reign of King Ahmose II, 589 BC.", date:'589 BC' },
    ar:{ name:'معبد هيبيس',    desc:'أقدم معبد مصري مكتمل يعود للفراعنة، يزخر بالنقوش المصرية والفارسية الرائعة.', date:'589 ق.م' } },
  { en:{ name:'Siwa Oasis',     desc:'The legendary oasis visited by Alexander the Great, blending nature and history.', date:'331 BC – Alexander the Great' },
    ar:{ name:'واحة سيوة',     desc:'الواحة الأسطورية التي زارها الإسكندر الأكبر، تجمع بين الطبيعة والتاريخ.', date:'331 ق.م – الإسكندر الأكبر' } },
];

window.changeARSite = function(idx) {
  const s = arSites[idx][currentLang];
  document.getElementById('arSiteName').textContent = s.name;
  document.getElementById('arSiteDesc').textContent = s.desc;
  document.getElementById('arSiteDate').textContent = s.date;
  const mon = document.getElementById('arMonument');
  mon.style.opacity = '0';
  setTimeout(() => { mon.style.opacity = '1'; }, 300);
  document.querySelectorAll('.ar-btn:not(.ar-camera)').forEach((b,i) => {
    b.style.background = i===idx ? 'rgba(14,140,122,.4)' : '';
  });
};

window.toggleCamera = function() {
  if (navigator.mediaDevices?.getUserMedia) {
    navigator.mediaDevices.getUserMedia({ video:{ facingMode:'environment' } })
      .then(stream => {
        const video = document.createElement('video');
        video.srcObject = stream; video.autoplay = true; video.muted = true;
        video.style.cssText = 'position:absolute;inset:0;width:100%;height:100%;object-fit:cover;z-index:1;';
        document.getElementById('arScene').prepend(video);
        const btn = document.querySelector('.ar-camera');
        btn.innerHTML = `📷 <span class="ar">إيقاف</span><span class="en">Stop</span>`;
        btn.onclick = () => { stream.getTracks().forEach(t=>t.stop()); video.remove(); btn.innerHTML=`📷 <span class="ar">تفعيل الكاميرا</span><span class="en">Enable Camera</span>`; btn.onclick=window.toggleCamera; };
      })
      .catch(() => alert(currentLang==='ar'?'يرجى منح إذن الكاميرا.':'Please allow camera access.'));
  } else {
    alert(currentLang==='ar'?'متصفحك لا يدعم الكاميرا.':'Your browser does not support camera access.');
  }
};

/* AR Particles */
setInterval(() => {
  const c = document.getElementById('arParticles');
  if(!c) return;
  const p = document.createElement('div');
  p.className = 'ar-particle';
  p.style.cssText = `left:${Math.random()*100}%;bottom:20%;--x:${(Math.random()-.5)*80}px;
    animation-duration:${Math.random()*2+2}s;
    width:${Math.random()*4+2}px;height:${Math.random()*4+2}px;
    background:${Math.random()>.5?'#F0A830':'#14B89E'};`;
  c.appendChild(p);
  setTimeout(()=>p.remove(),4000);
}, 300);

/* ═══════════════════════════════════════════
   AI CHAT
═══════════════════════════════════════════ */
const SYSTEM_PROMPT = `You are a smart bilingual tour guide named "Wadi 360 Guide" specializing in Egypt's New Valley governorate.
Respond in the same language the user writes in (Arabic or English).
Keep answers concise (3-5 sentences), friendly, and practical.
You know about: Dakhla Oasis, Kharga Oasis, Hibis Temple, Qasr El-Zayan, Muzawaka Tombs, Deir El-Malak, Gebel El-Teir, Ain El-Deir, best travel seasons (Oct-Mar), local hotels, restaurants, activities, distances, history, and the Wadi360 platform.`;

const suggestionsData = {
  ar: ['🏛️ أجمل المواقع','🌤️ أفضل وقت للزيارة','🗓️ مسار 3 أيام','👨‍👩‍👧 للعائلات'],
  en: ['🏛️ Best Sites','🌤️ Best Season','🗓️ 3-Day Route','👨‍👩‍👧 Family-Friendly']
};
const suggestionsQuestions = {
  ar: ['ما هي أجمل المواقع في الوادي الجديد؟','ما هو أفضل وقت لزيارة الوادي الجديد؟','اقترح لي مسار رحلة لمدة 3 أيام','ما هي الأنشطة المتاحة للعائلات؟'],
  en: ['What are the best sites in the New Valley?','What is the best time to visit New Valley?','Suggest a 3-day itinerary for me','What activities are available for families?']
};

function renderSuggestions() {
  const c = document.getElementById('chatSuggestions');
  const labels = suggestionsData[currentLang];
  const qs = suggestionsQuestions[currentLang];
  c.innerHTML = labels.map((l,i)=>`<button onclick="quickAsk('${qs[i]}')">${l}</button>`).join('');
}
renderSuggestions();

let chatHistory = [];
const chatMsgs = document.getElementById('chatMessages');

function addMsg(text, isUser=false, isTyping=false) {
  const d = document.createElement('div');
  d.className = `msg ${isUser?'user':'bot'} ${isTyping?'typing':''}`;
  d.innerHTML = `<div class="msg-avatar">${isUser?'👤':'🤖'}</div><div class="msg-bubble">${text}</div>`;
  chatMsgs.appendChild(d);
  chatMsgs.scrollTop = chatMsgs.scrollHeight;
  return d;
}

window.sendMessage = async function() {
  const inp = document.getElementById('chatInput');
  const text = inp.value.trim();
  if (!text) return;
  inp.value = '';
  addMsg(text, true);
  chatHistory.push({ role:'user', content:text });
  const typing = addMsg(currentLang==='ar'?'يكتب...':'Typing...', false, true);
  try {
    const res = await fetch('https://api.anthropic.com/v1/messages', {
      method:'POST',
      headers:{'Content-Type':'application/json'},
      body:JSON.stringify({ model:'claude-sonnet-4-20250514', max_tokens:1000, system:SYSTEM_PROMPT, messages:chatHistory })
    });
    const data = await res.json();
    const reply = data.content?.[0]?.text || (currentLang==='ar'?'عذراً، حدث خطأ.':'Sorry, an error occurred.');
    typing.remove();
    addMsg(reply);
    chatHistory.push({ role:'assistant', content:reply });
  } catch {
    typing.remove();
    addMsg(currentLang==='ar'?'عذراً، تحقق من الاتصال.':'Sorry, check your connection.');
  }
};
window.quickAsk = function(q) { document.getElementById('chatInput').value=q; window.sendMessage(); };

/* ═══════════════════════════════════════════
   CONTACT FORM
═══════════════════════════════════════════ */
window.submitForm = async function(e) {
  e.preventDefault();
  const form = e.target;
  const btn = form.querySelector('button[type=submit]');
  const origText = btn.innerHTML;
  btn.innerHTML = currentLang==='ar' ? '⏳ جاري الإرسال...' : '⏳ Sending...';
  
  try {
    // IMPORTANT: Replace 'YOUR_FORM_ID_HERE' with your actual Formspree Form ID (e.g. 'xwpeqjld')
    const res = await fetch("https://formspree.io/f/xlgadzjv", {
      method: "POST",
      body: new FormData(form),
      headers: { 'Accept': 'application/json' }
    });
    if(res.ok) {
      btn.innerHTML = currentLang==='ar' ? '✅ تم الإرسال بنجاح!' : '✅ Sent Successfully!';
      btn.style.cssText += 'background:linear-gradient(135deg,#0E8C7A,#14B89E)!important;color:#fff!important;';
      form.reset();
    } else {
      btn.innerHTML = currentLang==='ar' ? '❌ خطأ في الإرسال' : '❌ Error Sending';
    }
  } catch(err) {
    btn.innerHTML = currentLang==='ar' ? '❌ خطأ في الإرسال' : '❌ Error Sending';
  }
  
  setTimeout(() => { btn.innerHTML = origText; btn.style.cssText=''; }, 4000);
};

/* ── INIT PLACEHOLDERS & SELECT ── */
updateFormPlaceholders();
updateSelectOptions();
document.getElementById('chatInput').placeholder = 'اسأل عن الوادي الجديد...';

/* ═══════════════════════════════════════════
   SAND PARTICLES BACKGROUND
═══════════════════════════════════════════ */
(function initSandParticles() {
  const container = document.getElementById('sandParticlesBg');
  if (!container) return;
  function spawnParticle() {
    const p = document.createElement('div');
    const size = Math.random() * 3 + 1;
    const duration = Math.random() * 12 + 8;
    const dx = (Math.random() - 0.5) * 120;
    const hue = Math.random() > 0.5 ? '#C9A96E' : '#D4820A';
    p.style.cssText = `
      position:absolute;
      width:${size}px;height:${size}px;
      background:${hue};
      border-radius:50%;
      left:${Math.random() * 100}%;
      bottom:0;
      opacity:0;
      --dx:${dx}px;
      animation:sandFloat ${duration}s ease-in-out forwards;
      animation-delay:${Math.random() * 4}s;
    `;
    container.appendChild(p);
    setTimeout(() => p.remove(), (duration + 4) * 1000);
  }
  // Spawn initial batch
  for (let i = 0; i < 12; i++) spawnParticle();
  setInterval(spawnParticle, 1200);
})();

/* ═══════════════════════════════════════════
   TEMPLE EXPLORER GAME
═══════════════════════════════════════════ */
(function initTempleExplorer() {
  const canvas = document.getElementById('templeCanvas');
  if (!canvas) return;
  const ctx = canvas.getContext('2d');

  // Responsive canvas sizing
  function resizeCanvas() {
    const wrap = canvas.parentElement;
    const w = wrap.clientWidth;
    const ratio = 480 / 720;
    canvas.style.width  = w + 'px';
    canvas.style.height = (w * ratio) + 'px';
  }
  resizeCanvas();
  window.addEventListener('resize', resizeCanvas);

  const W = 720, H = 480;

  // ── Temple map layout (walls as rects {x,y,w,h}) ──
  const walls = [
    // Outer boundary walls
    {x:0,   y:0,   w:W,   h:14},
    {x:0,   y:H-14,w:W,   h:14},
    {x:0,   y:0,   w:14,  h:H},
    {x:W-14,y:0,   w:14,  h:H},
    // Main hall top wall with gate
    {x:60,  y:80,  w:200, h:14},
    {x:320, y:80,  w:200, h:14},
    // Main hall bottom wall with gate
    {x:60,  y:230, w:140, h:14},
    {x:270, y:230, w:170, h:14},
    {x:510, y:230, w:110, h:14},
    // Side corridors
    {x:60,  y:80,  w:14,  h:164},
    {x:646, y:80,  w:14,  h:164},
    // Inner sanctuary
    {x:200, y:290, w:180, h:14},
    {x:200, y:290, w:14,  h:120},
    {x:380, y:290, w:14,  h:120},
    {x:200, y:396, w:180, h:14},
    // Side chambers
    {x:60,  y:290, w:100, h:14},
    {x:60,  y:290, w:14,  h:110},
    {x:60,  y:400, w:100, h:14},
    {x:546, y:290, w:110, h:14},
    {x:636, y:290, w:14,  h:110},
    {x:546, y:400, w:104, h:14},
    // Inner columns (decorative obstacles)
    {x:130, y:120, w:20,  h:20},
    {x:240, y:120, w:20,  h:20},
    {x:460, y:120, w:20,  h:20},
    {x:570, y:120, w:20,  h:20},
    {x:250, y:330, w:16,  h:16},
    {x:454, y:330, w:16,  h:16},
  ];

  // ── Info Nodes ──
  const nodesData = [
    {
      id:'hibis',
      x:360, y:150,
      color:'#F0A830',
      icon:'🏛️',
      ar:{ title:'معبد هيبيس', era:'589 ق.م', text:'أقدم معبد مصري مكتمل على الإطلاق، بُني في عهد الفرعون أحمس الثاني. تزخر جدرانه بنقوش فارسية ومصرية نادرة تروي قصص الآلهة والملوك.' },
      en:{ title:'Hibis Temple', era:'589 BC', text:'The oldest complete temple in Egypt, built during the reign of King Ahmose II. Its walls are adorned with rare Persian and Egyptian inscriptions narrating stories of gods and pharaohs.' }
    },
    {
      id:'muzawaka',
      x:110, y:340,
      color:'#14B89E',
      icon:'🎨',
      ar:{ title:'مقابر مزواقة', era:'القرن الأول م', text:'مقابر رومانية مزينة بجداريات ملونة رائعة تمزج بين الفن المصري والروماني واليوناني. تُعدّ من أجمل المقابر الرومانية في العالم.' },
      en:{ title:'Muzawaka Tombs', era:'1st Century AD', text:'Roman-era tombs decorated with stunning colorful murals blending Egyptian, Roman, and Greek art. Considered among the most beautiful Roman tombs in the world.' }
    },
    {
      id:'zayan',
      x:590, y:340,
      color:'#E8C878',
      icon:'🕌',
      ar:{ title:'قصر الزيان', era:'القرن الثاني عشر م', text:'قصر إسلامي مذهل يعود للعصر الأيوبي، يتميز بأبراجه الشاهقة وجدرانه السميكة التي صمدت عبر القرون في قلب الصحراء.' },
      en:{ title:'Qasr El-Zayan', era:'12th Century AD', text:'A magnificent Islamic palace from the Ayyubid era, distinguished by its towering minarets and thick walls that have stood for centuries in the heart of the desert.' }
    },
    {
      id:'dakhla',
      x:250, y:340,
      color:'#E06020',
      icon:'🌴',
      ar:{ title:'واحة الداخلة', era:'ما قبل التاريخ', text:'واحة خضراء خلابة تحيط بها الكثبان الرملية الذهبية. سكنها البشر منذ آلاف السنين وتضم آثاراً تمتد من العصر الحجري حتى العصر الإسلامي.' },
      en:{ title:'Dakhla Oasis', era:'Prehistoric Era', text:'A lush green oasis surrounded by golden sand dunes. Inhabited by humans for thousands of years, it contains artifacts spanning from the Stone Age to the Islamic era.' }
    },
    {
      id:'kharga',
      x:470, y:340,
      color:'#C9A96E',
      icon:'⚱️',
      ar:{ title:'واحة الخارجة', era:'3000 ق.م', text:'أكبر واحات الوادي الجديد وأهمها تاريخياً. كانت محطة رئيسية في طريق الكاروان القديم الذي يربط مصر بأفريقيا، وتضم آثاراً فرعونية ورومانية فريدة.' },
      en:{ title:'Kharga Oasis', era:'3000 BC', text:'The largest and historically most significant oasis in New Valley. It was a key stop on the ancient caravan route connecting Egypt to Africa, with unique Pharaonic and Roman artifacts.' }
    },
  ];

  // Build legend
  const legend = document.getElementById('nodeLegend');
  const total = nodesData.length;
  document.getElementById('totalNodes').textContent = total;
  nodesData.forEach(n => {
    const item = document.createElement('div');
    item.className = 'node-item';
    item.id = 'legend-' + n.id;
    item.innerHTML = `<span class="node-dot" style="background:${n.color};color:${n.color}"></span>
      <span>${n.icon} <span class="ar">${n.ar.title}</span><span class="en">${n.en.title}</span></span>`;
    legend.appendChild(item);
  });

  // ── Player ──
  const player = { x:360, y:150, size:10, speed:2.8, color:'#FDFAF4', facing:0 };
  // Move player to entrance
  player.x = 360; player.y = 50;

  const visited = new Set();
  let activeNode = null;
  let popupVisible = false;

  const keys = {};
  window.addEventListener('keydown', e => {
    if (e.target.tagName === 'INPUT' || e.target.tagName === 'TEXTAREA' || e.target.tagName === 'SELECT') return;
    if (['ArrowUp','ArrowDown','ArrowLeft','ArrowRight','w','a','s','d'].includes(e.key)) {
      e.preventDefault();
      keys[e.key] = true;
    }
  });
  window.addEventListener('keyup', e => { keys[e.key] = false; });

  // Mobile d-pad
  const dpad = {
    dUp:    () => { keys['ArrowUp']    = true;  },
    dDown:  () => { keys['ArrowDown']  = true;  },
    dLeft:  () => { keys['ArrowLeft']  = true;  },
    dRight: () => { keys['ArrowRight'] = true;  },
  };
  ['dUp','dDown','dLeft','dRight'].forEach(id => {
    const btn = document.getElementById(id);
    if (!btn) return;
    btn.addEventListener('touchstart', e => { e.preventDefault(); dpad[id](); }, {passive:false});
    btn.addEventListener('touchend',   e => {
      e.preventDefault();
      const map = {dUp:'ArrowUp',dDown:'ArrowDown',dLeft:'ArrowLeft',dRight:'ArrowRight'};
      keys[map[id]] = false;
    }, {passive:false});
    btn.addEventListener('mousedown', () => dpad[id]());
    btn.addEventListener('mouseup',   () => {
      const map = {dUp:'ArrowUp',dDown:'ArrowDown',dLeft:'ArrowLeft',dRight:'ArrowRight'};
      keys[map[id]] = false;
    });
  });

  function collides(nx, ny) {
    const r = player.size;
    return walls.some(w => nx+r>w.x && nx-r<w.x+w.w && ny+r>w.y && ny-r<w.y+w.h);
  }

  function updatePlayer() {
    let dx = 0, dy = 0;
    if (keys['ArrowUp']    || keys['w']) dy = -1;
    if (keys['ArrowDown']  || keys['s']) dy =  1;
    if (keys['ArrowLeft']  || keys['a']) dx = -1;
    if (keys['ArrowRight'] || keys['d']) dx =  1;
    if (dx && dy) { dx *= 0.707; dy *= 0.707; }
    if (dx || dy) {
      player.facing = Math.atan2(dy, dx);
      const nx = player.x + dx * player.speed;
      const ny = player.y + dy * player.speed;
      if (!collides(nx, player.y)) player.x = Math.max(player.size, Math.min(W-player.size, nx));
      if (!collides(player.x, ny)) player.y = Math.max(player.size, Math.min(H-player.size, ny));
    }
  }

  function checkNodes(time) {
    nodesData.forEach(n => {
      const dist = Math.hypot(player.x - n.x, player.y - n.y);
      if (dist < 28 && !visited.has(n.id)) {
        visited.add(n.id);
        document.getElementById('visitedCount').textContent = visited.size;
        const legendItem = document.getElementById('legend-' + n.id);
        if (legendItem) legendItem.style.opacity = '0.45';
        activeNode = n;
        showPopup(n);
      }
    });
  }

  function showPopup(n) {
    const lang = currentLang === 'ar' ? n.ar : n.en;
    document.getElementById('popupIcon').textContent  = n.icon;
    document.getElementById('popupTitle').textContent = lang.title;
    document.getElementById('popupEra').textContent   = lang.era;
    document.getElementById('popupText').textContent  = lang.text;
    document.getElementById('templePopup').classList.add('visible');
    popupVisible = true;
  }

  window.closeTemplePopup = function() {
    document.getElementById('templePopup').classList.remove('visible');
    popupVisible = false;
  };

  // ── Drawing ──
  function drawFloor() {
    // Floor tiles - sandy stone
    const tileSize = 40;
    for (let tx = 0; tx < W; tx += tileSize) {
      for (let ty = 0; ty < H; ty += tileSize) {
        const shade = ((tx/tileSize + ty/tileSize) % 2 === 0) ? '#12201A' : '#0F1A15';
        ctx.fillStyle = shade;
        ctx.fillRect(tx, ty, tileSize, tileSize);
      }
    }
    // Sand path texture
    ctx.fillStyle = 'rgba(201,169,110,0.03)';
    for (let i = 0; i < 30; i++) {
      ctx.fillRect(
        (Math.sin(i*47.3)*0.5+0.5)*W,
        (Math.cos(i*31.7)*0.5+0.5)*H,
        Math.random()*30+10, Math.random()*6+2
      );
    }
  }

  function drawWalls() {
    walls.forEach(w => {
      // Stone gradient
      const grad = ctx.createLinearGradient(w.x, w.y, w.x+w.w, w.y+w.h);
      grad.addColorStop(0,   '#6B3A1F');
      grad.addColorStop(0.5, '#8B5E2E');
      grad.addColorStop(1,   '#5C3A1A');
      ctx.fillStyle = grad;
      ctx.fillRect(w.x, w.y, w.w, w.h);
      // Highlight edge
      ctx.fillStyle = 'rgba(228,185,110,0.18)';
      ctx.fillRect(w.x, w.y, w.w, 2);
      ctx.fillStyle = 'rgba(0,0,0,0.3)';
      ctx.fillRect(w.x, w.y+w.h-2, w.w, 2);
    });
  }

  function drawNodes(time) {
    nodesData.forEach(n => {
      const pulse = Math.sin(time/600 + n.x) * 0.4 + 0.6;
      if (visited.has(n.id)) {
        // Visited: dim glow
        ctx.beginPath();
        ctx.arc(n.x, n.y, 10, 0, Math.PI*2);
        ctx.fillStyle = 'rgba(255,255,255,0.08)';
        ctx.fill();
        ctx.font = '14px serif';
        ctx.textAlign = 'center';
        ctx.textBaseline = 'middle';
        ctx.globalAlpha = 0.4;
        ctx.fillText(n.icon, n.x, n.y);
        ctx.globalAlpha = 1;
      } else {
        // Animated glow ring
        const radius = 14 + pulse * 4;
        const glowGrad = ctx.createRadialGradient(n.x, n.y, 2, n.x, n.y, radius + 12);
        glowGrad.addColorStop(0, n.color + 'CC');
        glowGrad.addColorStop(0.5, n.color + '44');
        glowGrad.addColorStop(1, 'transparent');
        ctx.beginPath();
        ctx.arc(n.x, n.y, radius + 12, 0, Math.PI*2);
        ctx.fillStyle = glowGrad;
        ctx.fill();
        // Core dot
        ctx.beginPath();
        ctx.arc(n.x, n.y, radius * 0.55, 0, Math.PI*2);
        ctx.fillStyle = n.color;
        ctx.fill();
        // Icon
        ctx.font = '13px serif';
        ctx.textAlign = 'center';
        ctx.textBaseline = 'middle';
        ctx.fillText(n.icon, n.x, n.y);
      }
    });
  }

  function drawPlayer(time) {
    const bob = Math.sin(time / 200) * 1.2;
    const px = player.x, py = player.y + bob;
    // Shadow
    ctx.beginPath();
    ctx.ellipse(px, py + player.size - 1, player.size * 0.8, player.size * 0.3, 0, 0, Math.PI*2);
    ctx.fillStyle = 'rgba(0,0,0,0.4)';
    ctx.fill();
    // Body
    ctx.beginPath();
    ctx.arc(px, py, player.size, 0, Math.PI * 2);
    const bodyGrad = ctx.createRadialGradient(px-2, py-2, 1, px, py, player.size);
    bodyGrad.addColorStop(0, '#FDFAF4');
    bodyGrad.addColorStop(1, '#C9A96E');
    ctx.fillStyle = bodyGrad;
    ctx.fill();
    // Direction dot
    const eyeX = px + Math.cos(player.facing) * (player.size * 0.5);
    const eyeY = py + Math.sin(player.facing) * (player.size * 0.5);
    ctx.beginPath();
    ctx.arc(eyeX, eyeY, 2.5, 0, Math.PI*2);
    ctx.fillStyle = '#3D1F0D';
    ctx.fill();
    // Aura ring
    ctx.beginPath();
    ctx.arc(px, py, player.size + 4, 0, Math.PI*2);
    ctx.strokeStyle = 'rgba(240,168,48,0.5)';
    ctx.lineWidth = 1.5;
    ctx.stroke();
  }

  function drawDecorations(time) {
    // Hieroglyph-like symbols on walls
    ctx.font = '9px serif';
    ctx.textAlign = 'center';
    ctx.textBaseline = 'middle';
    ctx.fillStyle = 'rgba(212,130,10,0.25)';
    const symbols = ['𓂀','𓃒','𓆓','𓇋','𓉔','𓊃','𓋴','𓌂','𓍯'];
    const positions = [
      [100,155],[200,155],[460,155],[560,155],
      [37,200],[683,200],[37,350],[683,350],
    ];
    positions.forEach(([x,y], i) => {
      ctx.fillText(symbols[i % symbols.length], x, y);
    });
    // Subtle grid overlay (stone joints)
    ctx.strokeStyle = 'rgba(0,0,0,0.12)';
    ctx.lineWidth = 0.5;
    for (let gx = 0; gx < W; gx += 40) {
      ctx.beginPath(); ctx.moveTo(gx,0); ctx.lineTo(gx,H); ctx.stroke();
    }
    for (let gy = 0; gy < H; gy += 40) {
      ctx.beginPath(); ctx.moveTo(0,gy); ctx.lineTo(W,gy); ctx.stroke();
    }
    // Ambient corner torches
    const torchFlicker = 0.7 + Math.sin(time/150)*0.3;
    [
      [100,108],[240,108],[460,108],[590,108],
      [100,222],[590,222],
      [80,290],[640,290],
      [220,295],[390,295],
    ].forEach(([tx,ty]) => {
      const tGrad = ctx.createRadialGradient(tx,ty,1,tx,ty,18);
      tGrad.addColorStop(0, `rgba(240,168,48,${0.5*torchFlicker})`);
      tGrad.addColorStop(1, 'transparent');
      ctx.beginPath();
      ctx.arc(tx, ty, 18, 0, Math.PI*2);
      ctx.fillStyle = tGrad;
      ctx.fill();
      ctx.beginPath();
      ctx.arc(tx, ty, 3, 0, Math.PI*2);
      ctx.fillStyle = `rgba(255,220,100,${torchFlicker})`;
      ctx.fill();
    });
    // Entrance text
    ctx.font = 'bold 10px sans-serif';
    ctx.textAlign = 'center';
    ctx.fillStyle = 'rgba(201,169,110,0.5)';
    ctx.fillText('ENTRANCE', 360, 32);
  }

  // ── Game loop ──
  let gameStarted = false;
  window.startTempleGame = function() {
    gameStarted = true;
    const overlay = document.getElementById('gameStartOverlay');
    if (overlay) overlay.classList.add('hidden');
  };

  let lastTime = 0;
  function gameLoop(time) {
    if (!lastTime) lastTime = time;
    lastTime = time;
    ctx.clearRect(0, 0, W, H);
    drawFloor();
    drawDecorations(time);
    drawWalls();
    drawNodes(time);
    if (gameStarted) {
      updatePlayer();
      checkNodes(time);
    }
    drawPlayer(time);
    requestAnimationFrame(gameLoop);
  }
  requestAnimationFrame(gameLoop);

  // Update popup language on lang switch (observe toggleLang)
  const origToggle = window.toggleLang;
  window.toggleLang = function() {
    origToggle();
    if (popupVisible && activeNode) showPopup(activeNode);
    // Update legend labels
    document.querySelectorAll('[id^="legend-"]').forEach(item => {
      item.querySelectorAll('.ar, .en').forEach(el => {
        el.style.display = (el.classList.contains(currentLang)) ? 'inline' : 'none';
      });
    });
  };
})();

/* ═══════════════════════════════════════════
   ACCESSIBILITY LOGIC
═══════════════════════════════════════════ */
let a11yState = { audio: false, simplified: false, contrast: false, textSize: 16 };
let synth = window.speechSynthesis;

function speakText(text) {
  if (!a11yState.audio || !synth) return;
  synth.cancel();
  if(!text) return;
  const ut = new SpeechSynthesisUtterance(text);
  ut.lang = currentLang === 'ar' ? 'ar-SA' : 'en-US';
  synth.speak(ut);
}

window.toggleAudioMode = function() {
  a11yState.audio = !a11yState.audio;
  const btn = document.getElementById('btnAudioMode');
  if (a11yState.audio) {
    btn.classList.add('active');
    speakText(currentLang === 'ar' ? 'تم تفعيل القراءة الصوتية. مرر الماوس فوق العناصر للاستماع' : 'Audio mode activated. Hover over elements to listen.');
  } else {
    btn.classList.remove('active');
    synth.cancel();
  }
};

window.toggleSimplifiedMode = function() {
  a11yState.simplified = !a11yState.simplified;
  document.body.classList.toggle('simplified-mode', a11yState.simplified);
  document.getElementById('btnSimplifiedMode').classList.toggle('active', a11yState.simplified);
  if (a11yState.audio) speakText(currentLang === 'ar' ? 'وضع العرض المبسط' : 'Simplified view toggled');
};

window.toggleHighContrast = function() {
  a11yState.contrast = !a11yState.contrast;
  document.body.classList.toggle('high-contrast', a11yState.contrast);
  document.getElementById('btnHighContrast').classList.toggle('active', a11yState.contrast);
  if (a11yState.audio) speakText(currentLang === 'ar' ? 'وضع التباين العالي' : 'High contrast toggled');
};

window.changeTextSize = function(dir) {
  a11yState.textSize = Math.max(12, Math.min(24, a11yState.textSize + (dir * 2)));
  document.documentElement.style.fontSize = a11yState.textSize + 'px';
  if (a11yState.audio) speakText(currentLang === 'ar' ? 'تم تغيير حجم النص' : 'Text size changed');
};

// Global event listeners for Audio Mode
document.addEventListener('mouseover', (e) => {
  if (!a11yState.audio) return;
  const el = e.target.closest('button, a, h1, h2, h3, h4, p, .feature-card, .map-pin, .aud-card, .node-item, .a11y-btn');
  if (!el) return;
  
  document.querySelectorAll('.audio-focus').forEach(n => n.classList.remove('audio-focus'));
  el.classList.add('audio-focus');
  
  let textToSay = el.getAttribute('aria-label') || el.innerText;
  if(textToSay) speakText(textToSay);
});

document.addEventListener('mouseout', (e) => {
  if (e.target.classList) e.target.classList.remove('audio-focus');
});

/* ═══════════════════════════════════════════
   GLOBAL CONTINUOUS TTS (READ ALOUD)
═══════════════════════════════════════════ */
let ttsElementsQueue = [];
let ttsCurrentIndex = 0;
let ttsIsPlaying = false;
let ttsIsPaused = false;
let currentTTSUtterance = null;

window.startGlobalTTS = function() {
  stopTTS();
  const rawElements = Array.from(document.querySelectorAll('section h2, section h3, section h4, section p, .feature-card p, .stat, .msg-bubble'));
  
  ttsElementsQueue = rawElements.filter(el => {
    return el.innerText.trim().length > 5 && el.offsetParent !== null; 
  });
  
  ttsCurrentIndex = 0;
  if(ttsElementsQueue.length > 0) {
    document.getElementById('ttsPlayerBar').classList.add('show');
    readNextElement();
  }
};

function readNextElement() {
  if (ttsCurrentIndex >= ttsElementsQueue.length) {
    stopTTS();
    return;
  }
  const el = ttsElementsQueue[ttsCurrentIndex];
  
  document.querySelectorAll('.spoken-section').forEach(n => n.classList.remove('spoken-section'));
  el.classList.add('spoken-section');
  
  // Smooth scroll with offset for sticky navbar
  const y = el.getBoundingClientRect().top + window.scrollY - 120;
  window.scrollTo({top: y, behavior: 'smooth'});
  
  const clone = el.cloneNode(true);
  clone.querySelectorAll('.ar, .en, .ar-inline, .en-inline').forEach(sub => {
    const isAr = sub.classList.contains('ar') || sub.classList.contains('ar-inline');
    const isEn = sub.classList.contains('en') || sub.classList.contains('en-inline');
    if (currentLang === 'ar' && isEn) sub.remove();
    if (currentLang === 'en' && isAr) sub.remove();
  });
  
  const text = clone.innerText.trim().replace(/\s+/g, ' ');
  
  if(!text) {
    ttsCurrentIndex++;
    return readNextElement();
  }

  const ut = new SpeechSynthesisUtterance(text);
  ut.lang = currentLang === 'ar' ? 'ar-SA' : 'en-US';
  
  ut.onend = () => {
    ttsCurrentIndex++;
    readNextElement();
  };
  ut.onerror = () => {
    ttsCurrentIndex++;
    readNextElement();
  };
  
  currentTTSUtterance = ut;
  window.speechSynthesis.speak(ut);
  ttsIsPlaying = true;
  ttsIsPaused = false;
  updateTTSPlayButton();
}

window.toggleTTSPlayback = function() {
  if (!currentTTSUtterance) return;
  if (ttsIsPaused) {
    window.speechSynthesis.resume();
    ttsIsPaused = false;
  } else {
    window.speechSynthesis.pause();
    ttsIsPaused = true;
  }
  updateTTSPlayButton();
};

window.stopTTS = function() {
  window.speechSynthesis.cancel();
  document.querySelectorAll('.spoken-section').forEach(n => n.classList.remove('spoken-section'));
  ttsIsPlaying = false;
  ttsIsPaused = false;
  currentTTSUtterance = null;
  ttsElementsQueue = [];
  document.getElementById('ttsPlayerBar').classList.remove('show');
};

function updateTTSPlayButton() {
  const b = document.getElementById('ttsPlayPauseBtn');
  b.innerHTML = ttsIsPaused ? '▶️' : '⏸️';
}

const overrideToggleLangTTS = window.toggleLang;
window.toggleLang = function() {
  if(ttsIsPlaying) stopTTS();
  overrideToggleLangTTS();
};
</script>
</body>
</html>
