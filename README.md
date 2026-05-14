<!DOCTYPE html>
<html lang="en" data-lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Kervan — Driver Portal</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Cinzel:wght@400;700;900&family=Outfit:wght@300;400;500;600;700&display=swap" rel="stylesheet">
<style>
/* ============================================================
   CSS VARIABLES & RESET
   ============================================================ */
:root {
  --gold: #C9A84C;
  --gold-light: #E8C97A;
  --gold-dark: #8B6914;
  --sand: #F5EDD6;
  --sand-dark: #E8D9B0;
  --ink: #1A1209;
  --ink-mid: #2D2010;
  --brown: #4A2C0A;
  --cream: #FDF8EE;
  --rust: #B5451B;
  --teal: #1A6B6B;
  --green: #2A7A3B;
  --green-light: #3DAB54;
  --red: #C0392B;
  --shadow: rgba(26,18,9,0.3);
  --panel: rgba(255,255,255,0.025);
  --border: rgba(201,168,76,0.18);
}
*, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }
html { scroll-behavior: smooth; }
body {
  font-family: 'Outfit', sans-serif;
  background: var(--ink);
  color: var(--sand);
  overflow-x: hidden;
  min-height: 100vh;
}
body::after {
  content:'';
  position:fixed;inset:0;
  background-image:url("data:image/svg+xml,%3Csvg viewBox='0 0 200 200' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='n'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.85' numOctaves='4' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23n)' opacity='0.035'/%3E%3C/svg%3E");
  pointer-events:none;z-index:9999;opacity:0.5;
}

/* ============================================================
   LOGIN / AUTH SCREEN
   ============================================================ */
#auth-screen {
  position: fixed; inset: 0; z-index: 5000;
  background: radial-gradient(ellipse 120% 100% at 50% 0%, #2D1A08 0%, #1A1209 60%, #0F0A05 100%);
  display: flex; align-items: center; justify-content: center;
  transition: opacity 0.7s ease, transform 0.7s ease;
}
#auth-screen.exit { opacity: 0; transform: scale(1.03); pointer-events: none; }
#auth-screen.gone { display: none; }

.auth-stars { position: absolute; inset: 0; overflow: hidden; pointer-events:none; }
.star {
  position:absolute; width:2px; height:2px;
  background:rgba(201,168,76,0.6); border-radius:50%;
  animation:twinkle var(--dur) ease-in-out infinite var(--delay);
}
@keyframes twinkle { 0%,100%{opacity:0.2;transform:scale(1);}50%{opacity:1;transform:scale(1.5);} }

.auth-ring {
  position:absolute; width:500px; height:500px;
  border:1px solid rgba(201,168,76,0.1); border-radius:50%;
  animation:rotateRing 25s linear infinite;
}
.auth-ring:nth-child(2){ width:750px;height:750px;border-color:rgba(201,168,76,0.05);animation-direction:reverse;animation-duration:40s; }
@keyframes rotateRing{ to{transform:rotate(360deg);} }

.auth-box {
  position: relative; z-index: 2;
  width: 100%; max-width: 460px;
  padding: 0 1.5rem;
  display: flex; flex-direction: column; align-items: center;
  animation: authRise 1s cubic-bezier(0.22,1,0.36,1) 0.3s both;
}
@keyframes authRise{ from{opacity:0;transform:translateY(30px);}to{opacity:1;transform:translateY(0);} }

.auth-logo {
  display: flex; align-items: center; gap: 0.9rem;
  margin-bottom: 0.5rem;
}
.auth-logo-camel { width: 54px; filter: drop-shadow(0 0 20px rgba(201,168,76,0.5)); }
.auth-logo-name {
  font-family: 'Cinzel', serif; font-size: 2.8rem; font-weight: 900;
  color: var(--gold); letter-spacing: 0.12em;
  text-shadow: 0 0 40px rgba(201,168,76,0.25);
}
.auth-role-badge {
  font-size: 0.65rem; letter-spacing: 0.4em; text-transform: uppercase;
  color: rgba(245,237,214,0.4); margin-bottom: 2.2rem;
}

.auth-card {
  width: 100%;
  background: rgba(255,255,255,0.028);
  border: 1px solid rgba(201,168,76,0.22);
  padding: 2.5rem;
  position: relative; overflow: hidden;
}
.auth-card::before {
  content:''; position:absolute; top:0; left:0; right:0; height:2px;
  background: linear-gradient(90deg, transparent, var(--gold), transparent);
}

.auth-tabs {
  display: flex; border-bottom: 1px solid var(--border); margin-bottom: 2rem;
}
.auth-tab {
  flex: 1; padding: 0.75rem; background: none; border: none; cursor: pointer;
  font-family: 'Outfit', sans-serif; font-size: 0.8rem; font-weight: 600;
  letter-spacing: 0.12em; text-transform: uppercase; color: rgba(245,237,214,0.35);
  transition: all 0.3s; border-bottom: 2px solid transparent; margin-bottom: -1px;
}
.auth-tab.active { color: var(--gold); border-bottom-color: var(--gold); }

.auth-form { display: flex; flex-direction: column; gap: 1.1rem; }
.auth-form-panel { display: none; }
.auth-form-panel.active { display: flex; flex-direction: column; gap: 1.1rem; }

.field-group { display: flex; flex-direction: column; gap: 0.45rem; }
.field-label {
  font-size: 0.65rem; letter-spacing: 0.2em; text-transform: uppercase;
  color: rgba(245,237,214,0.45); font-weight: 600;
}
.field-input {
  background: rgba(255,255,255,0.04); border: 1px solid rgba(201,168,76,0.2);
  color: var(--sand); font-family: 'Outfit', sans-serif; font-size: 0.9rem;
  padding: 0.75rem 1rem; outline: none; transition: border-color 0.25s;
  width: 100%;
}
.field-input:focus { border-color: var(--gold); }
.field-input::placeholder { color: rgba(245,237,214,0.25); }

.field-row { display: grid; grid-template-columns: 1fr 1fr; gap: 0.8rem; }

.auth-btn {
  background: var(--gold); color: var(--ink); border: none; padding: 0.9rem;
  font-family: 'Outfit', sans-serif; font-size: 0.82rem; font-weight: 700;
  letter-spacing: 0.15em; text-transform: uppercase; cursor: pointer; transition: all 0.3s;
  clip-path: polygon(0 0, calc(100% - 10px) 0, 100% 10px, 100% 100%, 10px 100%, 0 calc(100% - 10px));
  margin-top: 0.5rem;
}
.auth-btn:hover { background: var(--gold-light); transform: translateY(-2px); }

.auth-divider {
  display: flex; align-items: center; gap: 1rem;
  font-size: 0.7rem; color: rgba(245,237,214,0.25); letter-spacing: 0.1em;
}
.auth-divider::before, .auth-divider::after {
  content:''; flex:1; height:1px; background: rgba(201,168,76,0.15);
}

.auth-forgot {
  text-align: right; font-size: 0.72rem; color: rgba(201,168,76,0.6);
  cursor: pointer; transition: color 0.2s;
}
.auth-forgot:hover { color: var(--gold); }

.auth-phone-group { display: flex; gap: 0.5rem; }
.auth-phone-prefix {
  background: rgba(255,255,255,0.04); border: 1px solid rgba(201,168,76,0.2);
  color: var(--sand); font-size: 0.9rem; padding: 0.75rem 0.8rem;
  font-family: 'Outfit', sans-serif; outline: none; width: 80px; text-align: center;
  border-right: none;
}
.auth-phone-prefix:focus { border-color: var(--gold); }
.auth-phone-group .field-input { border-left: 1px solid rgba(201,168,76,0.2); }

.auth-terms {
  font-size: 0.68rem; color: rgba(245,237,214,0.3); line-height: 1.6;
  text-align: center; margin-top: 0.3rem;
}
.auth-terms a { color: rgba(201,168,76,0.7); cursor: pointer; }

.auth-error {
  background: rgba(181,69,27,0.15); border: 1px solid rgba(181,69,27,0.4);
  color: #E8835A; font-size: 0.78rem; padding: 0.65rem 0.9rem;
  display: none; letter-spacing: 0.04em;
}
.auth-error.visible { display: block; }

.auth-success {
  background: rgba(42,122,59,0.15); border: 1px solid rgba(42,122,59,0.4);
  color: #7DD88A; font-size: 0.78rem; padding: 0.65rem 0.9rem;
  display: none; letter-spacing: 0.04em;
}
.auth-success.visible { display: block; }

/* OTP panel */
.otp-inputs { display: flex; gap: 0.7rem; justify-content: center; }
.otp-input {
  width: 52px; height: 60px; text-align: center; font-size: 1.5rem; font-weight: 700;
  background: rgba(255,255,255,0.04); border: 1px solid rgba(201,168,76,0.2);
  color: var(--gold); font-family: 'Cinzel', serif; outline: none; transition: border-color 0.2s;
}
.otp-input:focus { border-color: var(--gold); }
.otp-resend { font-size: 0.72rem; color: rgba(245,237,214,0.35); text-align: center; }
.otp-resend span { color: var(--gold); cursor: pointer; }

/* ============================================================
   MAIN LAYOUT
   ============================================================ */
#main-app { display: none; min-height: 100vh; }
#main-app.visible { display: flex; }

/* SIDEBAR */
.sidebar {
  width: 260px; min-height: 100vh; background: #0F0A05;
  border-right: 1px solid var(--border);
  display: flex; flex-direction: column;
  position: fixed; top:0; left:0; bottom:0;
  z-index: 100; transition: transform 0.3s;
}

.sidebar-logo {
  display: flex; align-items: center; gap: 0.7rem;
  padding: 1.5rem 1.8rem;
  border-bottom: 1px solid var(--border);
  text-decoration: none;
}
.sidebar-logo-camel { width: 32px; filter: drop-shadow(0 0 8px rgba(201,168,76,0.4)); }
.sidebar-logo-name { font-family: 'Cinzel', serif; font-size: 1.4rem; font-weight: 900; color: var(--gold); letter-spacing: 0.1em; }
.sidebar-logo-sub { font-size: 0.55rem; letter-spacing: 0.25em; color: rgba(245,237,214,0.3); text-transform: uppercase; }

/* Driver quick profile */
.sidebar-profile {
  padding: 1.3rem 1.8rem;
  border-bottom: 1px solid var(--border);
  display: flex; align-items: center; gap: 0.9rem;
}
.sidebar-avatar {
  width: 46px; height: 46px; border-radius: 50%;
  background: linear-gradient(135deg, var(--gold-dark), var(--gold));
  display: flex; align-items: center; justify-content: center;
  font-family: 'Cinzel', serif; font-size: 1.1rem; font-weight: 700; color: var(--ink);
  flex-shrink: 0; position: relative; cursor: pointer;
}
.online-badge {
  position: absolute; bottom: 1px; right: 1px;
  width: 13px; height: 13px; border-radius: 50%;
  background: var(--green-light); border: 2px solid #0F0A05;
  transition: background 0.3s;
}
.online-badge.offline { background: #888; }
.sidebar-profile-info { flex: 1; min-width: 0; }
.sidebar-profile-name { font-size: 0.9rem; font-weight: 600; color: var(--cream); white-space: nowrap; overflow: hidden; text-overflow: ellipsis; }
.sidebar-profile-id { font-size: 0.67rem; color: rgba(245,237,214,0.35); letter-spacing: 0.08em; margin-top: 0.1rem; }
.sidebar-profile-status {
  font-size: 0.62rem; font-weight: 600; letter-spacing: 0.1em; text-transform: uppercase;
  color: var(--green-light); margin-top: 0.2rem; display: flex; align-items: center; gap: 0.3rem;
  cursor: pointer; transition: color 0.2s;
}
.sidebar-profile-status::before { content:''; width:6px; height:6px; border-radius:50%; background:currentColor; }
.sidebar-profile-status.offline { color: rgba(245,237,214,0.35); }

/* Nav */
.sidebar-nav { flex: 1; padding: 1rem 0; overflow-y: auto; }
.nav-section-label {
  font-size: 0.58rem; letter-spacing: 0.3em; text-transform: uppercase;
  color: rgba(245,237,214,0.25); padding: 0.8rem 1.8rem 0.4rem;
  font-weight: 600;
}
.nav-item {
  display: flex; align-items: center; gap: 0.85rem;
  padding: 0.7rem 1.8rem; cursor: pointer; transition: all 0.2s;
  border-left: 2px solid transparent; position: relative;
  color: rgba(245,237,214,0.48); font-size: 0.85rem; font-weight: 500;
  text-decoration: none;
}
.nav-item:hover { color: var(--cream); background: rgba(201,168,76,0.06); }
.nav-item.active { color: var(--gold); background: rgba(201,168,76,0.1); border-left-color: var(--gold); }
.nav-item-icon { font-size: 1.05rem; width: 22px; text-align: center; flex-shrink: 0; }
.nav-badge {
  margin-left: auto; background: var(--rust); color: var(--cream);
  font-size: 0.6rem; font-weight: 700; padding: 0.15rem 0.5rem; border-radius: 999px;
  min-width: 18px; text-align: center;
}
.nav-badge.gold { background: var(--gold); color: var(--ink); }

.sidebar-bottom { padding: 1.3rem 1.8rem; border-top: 1px solid var(--border); }
.sidebar-toggle-online {
  width: 100%; padding: 0.7rem; background: rgba(42,122,59,0.15);
  border: 1px solid rgba(42,122,59,0.4); color: var(--green-light);
  font-family: 'Outfit', sans-serif; font-size: 0.78rem; font-weight: 700;
  letter-spacing: 0.12em; text-transform: uppercase; cursor: pointer;
  transition: all 0.3s; display: flex; align-items: center; justify-content: center; gap: 0.6rem;
}
.sidebar-toggle-online:hover { background: rgba(42,122,59,0.25); }
.sidebar-toggle-online.offline { background: rgba(255,255,255,0.04); border-color: rgba(255,255,255,0.12); color: rgba(245,237,214,0.4); }
.sidebar-toggle-online::before { content:'●'; font-size:0.65rem; }

/* ============================================================
   MAIN CONTENT
   ============================================================ */
.main-content {
  margin-left: 260px; flex: 1; min-height: 100vh;
  display: flex; flex-direction: column;
}

/* Top bar */
.topbar {
  background: rgba(15,10,5,0.9); backdrop-filter: blur(12px);
  border-bottom: 1px solid var(--border);
  padding: 0.9rem 2.5rem; display: flex; align-items: center; justify-content: space-between;
  position: sticky; top: 0; z-index: 50;
}
.topbar-left { display: flex; align-items: center; gap: 1.2rem; }
.page-title { font-family: 'Cinzel', serif; font-size: 1.1rem; font-weight: 700; color: var(--cream); letter-spacing: 0.06em; }
.page-breadcrumb { font-size: 0.72rem; color: rgba(245,237,214,0.3); letter-spacing: 0.08em; }
.topbar-right { display: flex; align-items: center; gap: 1rem; }

.topbar-btn {
  background: none; border: 1px solid var(--border); color: rgba(245,237,214,0.55);
  padding: 0.5rem 0.9rem; font-family: 'Outfit', sans-serif; font-size: 0.75rem;
  font-weight: 600; letter-spacing: 0.1em; text-transform: uppercase;
  cursor: pointer; transition: all 0.25s; display: flex; align-items: center; gap: 0.4rem;
}
.topbar-btn:hover { border-color: var(--gold); color: var(--gold); }
.topbar-btn.primary { background: var(--gold); border-color: var(--gold); color: var(--ink); }
.topbar-btn.primary:hover { background: var(--gold-light); }

.notif-dot {
  position: relative; display: flex; align-items: center;
}
.notif-dot::after {
  content:''; position:absolute; top:-2px; right:-2px;
  width:8px; height:8px; background: var(--rust); border-radius:50%;
  border: 2px solid #0F0A05;
}

/* ============================================================
   PAGES
   ============================================================ */
.page { display: none; padding: 2rem 2.5rem; animation: pageFade 0.3s ease; }
.page.active { display: block; }
@keyframes pageFade { from{opacity:0;transform:translateY(8px);} to{opacity:1;transform:translateY(0);} }

/* Section header */
.section-header { margin-bottom: 1.8rem; }
.section-title { font-family: 'Cinzel', serif; font-size: 1.4rem; font-weight: 700; color: var(--cream); margin-bottom: 0.4rem; }
.section-sub { font-size: 0.82rem; color: rgba(245,237,214,0.4); }

/* Grid helpers */
.grid-2 { display: grid; grid-template-columns: 1fr 1fr; gap: 1.3rem; }
.grid-3 { display: grid; grid-template-columns: repeat(3,1fr); gap: 1.3rem; }
.grid-4 { display: grid; grid-template-columns: repeat(4,1fr); gap: 1.3rem; }
.grid-auto { display: grid; grid-template-columns: repeat(auto-fit,minmax(240px,1fr)); gap: 1.3rem; }

/* Card */
.card {
  background: var(--panel); border: 1px solid var(--border);
  padding: 1.6rem; position: relative; overflow: hidden;
}
.card-gold-top::before { content:''; position:absolute; top:0; left:0; right:0; height:2px; background:linear-gradient(90deg,transparent,var(--gold),transparent); }
.card-label { font-size: 0.62rem; letter-spacing: 0.22em; text-transform: uppercase; color: rgba(245,237,214,0.35); margin-bottom: 0.5rem; font-weight: 600; }
.card-value { font-family: 'Cinzel', serif; font-size: 2rem; font-weight: 700; color: var(--cream); line-height: 1; }
.card-value.gold { color: var(--gold); }
.card-value.green { color: var(--green-light); }
.card-value.red { color: #E8835A; }
.card-meta { font-size: 0.73rem; color: rgba(245,237,214,0.35); margin-top: 0.5rem; display: flex; align-items: center; gap: 0.35rem; }
.card-meta.up { color: var(--green-light); }
.card-meta.down { color: #E8835A; }

/* ============================================================
   DASHBOARD SPECIFIC
   ============================================================ */
/* Live Status Banner */
.live-banner {
  display: flex; align-items: center; justify-content: space-between;
  background: rgba(42,122,59,0.12); border: 1px solid rgba(42,122,59,0.3);
  padding: 1rem 1.5rem; margin-bottom: 1.8rem;
}
.live-banner.offline-banner { background: rgba(255,255,255,0.03); border-color: rgba(255,255,255,0.1); }
.live-indicator { display: flex; align-items: center; gap: 0.7rem; }
.live-dot { width: 10px; height: 10px; border-radius: 50%; background: var(--green-light); animation: pulse 2s ease infinite; }
.live-dot.off { background: rgba(245,237,214,0.25); animation: none; }
@keyframes pulse { 0%,100%{opacity:1;box-shadow:0 0 0 0 rgba(61,171,84,0.4);} 50%{opacity:0.8;box-shadow:0 0 0 8px rgba(61,171,84,0);} }
.live-text { font-size: 0.8rem; font-weight: 600; color: var(--green-light); letter-spacing: 0.08em; }
.live-text.off { color: rgba(245,237,214,0.4); }
.live-since { font-size: 0.72rem; color: rgba(245,237,214,0.35); }
.live-timer { font-family: 'Cinzel', serif; font-size: 1.1rem; font-weight: 700; color: var(--gold); }

/* Earnings mini chart */
.earnings-chart { display: flex; align-items: flex-end; gap: 4px; height: 56px; margin-top: 1.1rem; }
.bar {
  flex: 1; background: rgba(201,168,76,0.25); border-radius: 2px 2px 0 0;
  transition: background 0.2s; position: relative; cursor: pointer;
  min-height: 4px;
}
.bar:hover { background: rgba(201,168,76,0.55); }
.bar.today { background: var(--gold); }
.bar-label { position:absolute; bottom:-18px; left:50%; transform:translateX(-50%); font-size:0.55rem; color:rgba(245,237,214,0.3); white-space:nowrap; }

/* Recent trips */
.trip-list { display: flex; flex-direction: column; gap: 0.75rem; }
.trip-item {
  display: flex; align-items: center; gap: 1.1rem;
  padding: 0.9rem 1.1rem;
  background: rgba(255,255,255,0.025); border: 1px solid rgba(201,168,76,0.1);
  transition: border-color 0.2s;
}
.trip-item:hover { border-color: rgba(201,168,76,0.3); }
.trip-status-dot { width: 8px; height: 8px; border-radius: 50%; flex-shrink: 0; }
.trip-status-dot.completed { background: var(--green-light); }
.trip-status-dot.cancelled { background: #E8835A; }
.trip-status-dot.active { background: var(--gold); animation: pulse 1.5s ease infinite; }
.trip-info { flex: 1; min-width: 0; }
.trip-route { font-size: 0.85rem; font-weight: 600; color: var(--cream); margin-bottom: 0.2rem; white-space: nowrap; overflow: hidden; text-overflow: ellipsis; }
.trip-meta { font-size: 0.7rem; color: rgba(245,237,214,0.35); display: flex; gap: 0.8rem; }
.trip-fare { font-family: 'Cinzel', serif; font-size: 0.95rem; font-weight: 700; color: var(--gold); flex-shrink: 0; }
.trip-rating { display: flex; align-items: center; gap: 0.2rem; font-size: 0.72rem; color: rgba(245,237,214,0.4); flex-shrink: 0; }
.trip-rating span { color: var(--gold); }

/* Rating ring */
.rating-ring { position: relative; width: 100px; height: 100px; margin: 0 auto 1rem; }
.rating-ring svg { transform: rotate(-90deg); }
.rating-ring-bg { fill: none; stroke: rgba(201,168,76,0.12); stroke-width: 6; }
.rating-ring-fill { fill: none; stroke: var(--gold); stroke-width: 6; stroke-linecap: round; transition: stroke-dashoffset 1s ease; }
.rating-center { position: absolute; inset: 0; display: flex; flex-direction: column; align-items: center; justify-content: center; }
.rating-num { font-family: 'Cinzel', serif; font-size: 1.6rem; font-weight: 900; color: var(--gold); line-height: 1; }
.rating-of { font-size: 0.62rem; color: rgba(245,237,214,0.3); }

/* ============================================================
   NEW RIDE REQUEST
   ============================================================ */
.ride-request-overlay {
  position: fixed; inset: 0; z-index: 2000;
  background: rgba(0,0,0,0.7); backdrop-filter: blur(6px);
  display: flex; align-items: center; justify-content: center;
  opacity: 0; pointer-events: none; transition: opacity 0.35s;
}
.ride-request-overlay.visible { opacity: 1; pointer-events: all; }

.ride-request-card {
  background: #0F0A05; border: 1px solid var(--gold);
  width: 100%; max-width: 420px; padding: 2rem;
  position: relative; animation: requestPop 0.4s cubic-bezier(0.22,1,0.36,1);
}
@keyframes requestPop { from{opacity:0;transform:scale(0.88) translateY(20px);} to{opacity:1;transform:scale(1) translateY(0);} }
.ride-request-card::before { content:''; position:absolute; top:0; left:0; right:0; height:2px; background:var(--gold); }

.request-header { display: flex; justify-content: space-between; align-items: flex-start; margin-bottom: 1.3rem; }
.request-title { font-family: 'Cinzel', serif; font-size: 1rem; font-weight: 700; color: var(--gold); letter-spacing: 0.1em; }
.request-timer {
  font-family: 'Cinzel', serif; font-size: 1.8rem; font-weight: 900; color: var(--rust);
  line-height: 1;
}
.request-timer-label { font-size: 0.6rem; color: rgba(245,237,214,0.35); letter-spacing: 0.15em; text-align: right; text-transform: uppercase; }

.request-passenger { display: flex; align-items: center; gap: 0.9rem; margin-bottom: 1.3rem; padding: 0.9rem; background: rgba(255,255,255,0.03); border: 1px solid var(--border); }
.request-pax-avatar {
  width: 42px; height: 42px; border-radius: 50%;
  background: linear-gradient(135deg,#2D1A08,#4A2C0A);
  display: flex; align-items: center; justify-content: center; font-size: 1.2rem; flex-shrink: 0;
}
.request-pax-name { font-size: 0.92rem; font-weight: 600; color: var(--cream); }
.request-pax-rating { font-size: 0.72rem; color: rgba(245,237,214,0.4); }
.request-pax-rating span { color: var(--gold); }
.request-pax-trips { font-size: 0.68rem; color: rgba(245,237,214,0.28); margin-top: 0.1rem; }

.request-route { margin-bottom: 1.3rem; }
.request-route-item { display: flex; align-items: flex-start; gap: 0.8rem; padding: 0.5rem 0; }
.request-route-icon { font-size: 0.9rem; width: 20px; text-align: center; flex-shrink: 0; margin-top: 0.05rem; }
.request-route-label { font-size: 0.63rem; letter-spacing: 0.15em; text-transform: uppercase; color: rgba(245,237,214,0.3); margin-bottom: 0.1rem; }
.request-route-place { font-size: 0.88rem; color: var(--cream); font-weight: 500; }
.request-route-line { width: 1px; height: 18px; background: rgba(201,168,76,0.2); margin: 0 9px; }

.request-stats { display: grid; grid-template-columns: repeat(3,1fr); gap: 0.6rem; margin-bottom: 1.5rem; }
.request-stat { text-align: center; padding: 0.6rem; background: rgba(255,255,255,0.03); border: 1px solid var(--border); }
.request-stat-val { font-family: 'Cinzel', serif; font-size: 1.1rem; font-weight: 700; color: var(--gold); }
.request-stat-label { font-size: 0.6rem; color: rgba(245,237,214,0.3); letter-spacing: 0.1em; text-transform: uppercase; margin-top: 0.2rem; }

.request-actions { display: grid; grid-template-columns: 1fr 1fr; gap: 0.8rem; }
.request-accept {
  background: var(--green); color: var(--cream); border: none; padding: 0.9rem;
  font-family: 'Outfit', sans-serif; font-size: 0.85rem; font-weight: 700;
  letter-spacing: 0.12em; text-transform: uppercase; cursor: pointer; transition: all 0.3s;
}
.request-accept:hover { background: var(--green-light); }
.request-decline {
  background: none; color: rgba(245,237,214,0.4); border: 1px solid rgba(255,255,255,0.12);
  padding: 0.9rem; font-family: 'Outfit', sans-serif; font-size: 0.85rem; font-weight: 600;
  letter-spacing: 0.1em; text-transform: uppercase; cursor: pointer; transition: all 0.3s;
}
.request-decline:hover { border-color: var(--rust); color: #E8835A; }

.request-progress { width: 100%; height: 3px; background: rgba(255,255,255,0.08); margin-bottom: 1.3rem; }
.request-progress-fill { height: 100%; background: var(--rust); transition: width 0.1s linear; }

/* ============================================================
   TRIPS PAGE
   ============================================================ */
.trips-filters { display: flex; gap: 0.7rem; flex-wrap: wrap; margin-bottom: 1.5rem; }
.filter-btn {
  background: none; border: 1px solid var(--border); color: rgba(245,237,214,0.45);
  padding: 0.45rem 1rem; font-family: 'Outfit', sans-serif; font-size: 0.73rem; font-weight: 600;
  letter-spacing: 0.1em; text-transform: uppercase; cursor: pointer; transition: all 0.2s;
}
.filter-btn.active, .filter-btn:hover { border-color: var(--gold); color: var(--gold); background: rgba(201,168,76,0.08); }

.trips-table { width: 100%; border-collapse: collapse; }
.trips-table th {
  font-size: 0.62rem; letter-spacing: 0.2em; text-transform: uppercase;
  color: rgba(245,237,214,0.3); font-weight: 600; text-align: left;
  padding: 0.6rem 1rem; border-bottom: 1px solid var(--border);
}
.trips-table td {
  padding: 0.95rem 1rem; border-bottom: 1px solid rgba(201,168,76,0.07);
  font-size: 0.82rem; color: rgba(245,237,214,0.7); vertical-align: middle;
}
.trips-table tr:hover td { background: rgba(201,168,76,0.04); }
.status-badge {
  display: inline-block; padding: 0.2rem 0.6rem;
  font-size: 0.6rem; font-weight: 700; letter-spacing: 0.1em; text-transform: uppercase;
}
.status-badge.completed { background: rgba(42,122,59,0.2); color: var(--green-light); border: 1px solid rgba(42,122,59,0.3); }
.status-badge.cancelled { background: rgba(181,69,27,0.2); color: #E8835A; border: 1px solid rgba(181,69,27,0.3); }
.status-badge.active { background: rgba(201,168,76,0.15); color: var(--gold); border: 1px solid rgba(201,168,76,0.3); }

/* ============================================================
   EARNINGS PAGE
   ============================================================ */
.earnings-period-selector { display: flex; gap: 0; margin-bottom: 1.8rem; }
.period-btn {
  padding: 0.55rem 1.2rem; background: none; border: 1px solid var(--border);
  color: rgba(245,237,214,0.4); font-family: 'Outfit', sans-serif; font-size: 0.75rem;
  font-weight: 600; letter-spacing: 0.1em; text-transform: uppercase; cursor: pointer;
  transition: all 0.2s; border-right: none;
}
.period-btn:last-child { border-right: 1px solid var(--border); }
.period-btn.active, .period-btn:hover { background: rgba(201,168,76,0.1); color: var(--gold); border-color: var(--gold); }

.big-bar-chart { display: flex; align-items: flex-end; gap: 8px; height: 160px; padding: 0 0.5rem; }
.big-bar {
  flex: 1; background: rgba(201,168,76,0.2); border-radius: 3px 3px 0 0;
  cursor: pointer; transition: background 0.2s; position: relative; min-height: 8px;
}
.big-bar:hover { background: rgba(201,168,76,0.45); }
.big-bar.highlight { background: var(--gold); }
.big-bar-tooltip {
  position: absolute; bottom: calc(100% + 6px); left: 50%; transform: translateX(-50%);
  background: var(--ink); border: 1px solid var(--gold); padding: 0.3rem 0.6rem;
  font-size: 0.68rem; color: var(--gold); white-space: nowrap;
  opacity: 0; pointer-events: none; transition: opacity 0.15s;
}
.big-bar:hover .big-bar-tooltip { opacity: 1; }
.big-bar-label { position: absolute; bottom: -20px; left: 50%; transform: translateX(-50%); font-size: 0.62rem; color: rgba(245,237,214,0.35); white-space: nowrap; }

.chart-x-labels { display: flex; padding: 22px 0.5rem 0; gap: 8px; }
.chart-x-labels span { flex: 1; text-align: center; font-size: 0.62rem; color: rgba(245,237,214,0.3); }

.breakdown-list { display: flex; flex-direction: column; gap: 0.7rem; margin-top: 0.5rem; }
.breakdown-item { display: flex; align-items: center; gap: 1rem; }
.breakdown-label { flex: 1; font-size: 0.8rem; color: rgba(245,237,214,0.6); }
.breakdown-bar-wrap { flex: 2; height: 6px; background: rgba(255,255,255,0.06); border-radius: 3px; overflow: hidden; }
.breakdown-bar-fill { height: 100%; border-radius: 3px; background: var(--gold); }
.breakdown-value { font-size: 0.8rem; font-weight: 600; color: var(--cream); min-width: 90px; text-align: right; }

.withdraw-section { margin-top: 1.5rem; padding: 1.5rem; background: var(--panel); border: 1px solid var(--border); }
.withdraw-row { display: flex; align-items: center; gap: 1rem; flex-wrap: wrap; }
.withdraw-balance { display: flex; flex-direction: column; }
.withdraw-balance-label { font-size: 0.62rem; color: rgba(245,237,214,0.35); letter-spacing: 0.15em; text-transform: uppercase; }
.withdraw-balance-amount { font-family: 'Cinzel', serif; font-size: 1.8rem; font-weight: 700; color: var(--green-light); }
.withdraw-balance-sub { font-size: 0.7rem; color: rgba(245,237,214,0.3); }
.withdraw-btn {
  background: var(--gold); color: var(--ink); border: none; padding: 0.75rem 1.8rem;
  font-family: 'Outfit', sans-serif; font-size: 0.8rem; font-weight: 700; letter-spacing: 0.12em;
  text-transform: uppercase; cursor: pointer; transition: all 0.3s;
}
.withdraw-btn:hover { background: var(--gold-light); }
.withdraw-methods { display: flex; gap: 0.7rem; margin-top: 1rem; flex-wrap: wrap; }
.pay-method {
  display: flex; align-items: center; gap: 0.5rem;
  padding: 0.5rem 0.9rem; border: 1px solid var(--border);
  background: rgba(255,255,255,0.025); cursor: pointer; transition: all 0.2s;
  font-size: 0.78rem; color: rgba(245,237,214,0.55);
}
.pay-method:hover { border-color: var(--gold); color: var(--cream); }
.pay-method.selected { border-color: var(--gold); color: var(--gold); background: rgba(201,168,76,0.08); }
.pay-method-icon { font-size: 1.1rem; }

/* ============================================================
   PROFILE PAGE
   ============================================================ */
.profile-hero {
  background: linear-gradient(135deg, rgba(201,168,76,0.08) 0%, rgba(255,255,255,0.02) 100%);
  border: 1px solid var(--border); padding: 2rem; display: flex; align-items: center; gap: 2rem;
  margin-bottom: 1.8rem; position: relative; overflow: hidden;
}
.profile-hero::before { content:''; position:absolute; top:0; left:0; right:0; height:2px; background:linear-gradient(90deg,transparent,var(--gold),transparent); }
.profile-avatar-big {
  width: 90px; height: 90px; border-radius: 50%;
  background: linear-gradient(135deg, var(--gold-dark), var(--gold));
  display: flex; align-items: center; justify-content: center;
  font-family: 'Cinzel', serif; font-size: 2rem; font-weight: 900; color: var(--ink);
  border: 3px solid var(--gold); flex-shrink: 0; cursor: pointer; position: relative;
}
.profile-avatar-edit {
  position: absolute; bottom: 0; right: 0;
  width: 26px; height: 26px; background: var(--gold); border-radius: 50%;
  display: flex; align-items: center; justify-content: center;
  font-size: 0.7rem; cursor: pointer; border: 2px solid #0F0A05;
}
.profile-main-info { flex: 1; }
.profile-name { font-family: 'Cinzel', serif; font-size: 1.6rem; font-weight: 700; color: var(--cream); }
.profile-id-row { display: flex; align-items: center; gap: 1rem; margin: 0.4rem 0; flex-wrap: wrap; }
.profile-driver-id { font-size: 0.72rem; color: rgba(245,237,214,0.35); letter-spacing: 0.1em; }
.profile-verified {
  display: flex; align-items: center; gap: 0.35rem;
  font-size: 0.68rem; font-weight: 600; letter-spacing: 0.1em; text-transform: uppercase;
  color: var(--green-light);
}
.profile-verified::before { content:'✓'; }
.profile-stats-row { display: flex; gap: 2rem; margin-top: 0.8rem; flex-wrap: wrap; }
.profile-stat { text-align: center; }
.profile-stat-val { font-family: 'Cinzel', serif; font-size: 1.2rem; font-weight: 700; color: var(--gold); }
.profile-stat-label { font-size: 0.62rem; color: rgba(245,237,214,0.35); letter-spacing: 0.1em; text-transform: uppercase; }

.form-section { margin-bottom: 2rem; }
.form-section-title {
  font-family: 'Cinzel', serif; font-size: 0.85rem; font-weight: 700; color: var(--gold);
  letter-spacing: 0.15em; text-transform: uppercase; margin-bottom: 1.2rem;
  padding-bottom: 0.6rem; border-bottom: 1px solid var(--border); display: flex; align-items: center; gap: 0.7rem;
}
.form-grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(240px, 1fr)); gap: 1rem; }
.save-btn {
  background: var(--gold); color: var(--ink); border: none; padding: 0.8rem 2rem;
  font-family: 'Outfit', sans-serif; font-size: 0.8rem; font-weight: 700; letter-spacing: 0.12em;
  text-transform: uppercase; cursor: pointer; transition: all 0.3s;
}
.save-btn:hover { background: var(--gold-light); }

/* Document upload */
.doc-grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(200px, 1fr)); gap: 1rem; }
.doc-card {
  border: 2px dashed rgba(201,168,76,0.2); padding: 1.3rem;
  display: flex; flex-direction: column; align-items: center; text-align: center;
  cursor: pointer; transition: all 0.3s; position: relative; overflow: hidden;
}
.doc-card:hover { border-color: rgba(201,168,76,0.5); background: rgba(201,168,76,0.04); }
.doc-card.uploaded { border-style: solid; border-color: rgba(42,122,59,0.5); background: rgba(42,122,59,0.05); cursor: default; }
.doc-icon { font-size: 2rem; margin-bottom: 0.7rem; }
.doc-name { font-size: 0.78rem; font-weight: 600; color: var(--cream); margin-bottom: 0.3rem; }
.doc-status { font-size: 0.65rem; letter-spacing: 0.1em; text-transform: uppercase; }
.doc-status.pending { color: rgba(245,237,214,0.35); }
.doc-status.ok { color: var(--green-light); }
.doc-status.expired { color: #E8835A; }
.doc-expiry { font-size: 0.62rem; color: rgba(245,237,214,0.28); margin-top: 0.3rem; }
.doc-upload-btn {
  margin-top: 0.8rem; background: none; border: 1px solid rgba(201,168,76,0.3);
  color: rgba(201,168,76,0.7); font-family: 'Outfit', sans-serif; font-size: 0.68rem;
  font-weight: 600; letter-spacing: 0.1em; text-transform: uppercase; padding: 0.35rem 0.8rem;
  cursor: pointer; transition: all 0.2s;
}
.doc-upload-btn:hover { border-color: var(--gold); color: var(--gold); }
.doc-check { position: absolute; top: 0.6rem; right: 0.6rem; font-size: 0.8rem; color: var(--green-light); }

/* ============================================================
   VEHICLE PAGE
   ============================================================ */
.vehicle-display {
  background: var(--panel); border: 1px solid var(--border);
  padding: 2rem; text-align: center; margin-bottom: 1.5rem;
  position: relative;
}
.vehicle-emoji { font-size: 5rem; display: block; margin-bottom: 0.8rem; filter: drop-shadow(0 0 20px rgba(201,168,76,0.3)); }
.vehicle-plate {
  display: inline-block; background: #FFFDE7; color: #1A1209;
  font-family: 'Cinzel', serif; font-size: 1.5rem; font-weight: 900;
  padding: 0.4rem 1.2rem; letter-spacing: 0.25em; border: 3px solid #1A1209;
  margin: 0.5rem 0;
}
.vehicle-model { font-size: 0.9rem; color: rgba(245,237,214,0.55); margin-top: 0.4rem; }
.vehicle-info-grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(140px, 1fr)); gap: 0.8rem; margin-top: 1.2rem; }
.vehicle-info-item { text-align: center; padding: 0.7rem; background: rgba(255,255,255,0.025); border: 1px solid var(--border); }
.vehicle-info-val { font-family: 'Cinzel', serif; font-size: 0.95rem; font-weight: 700; color: var(--gold); }
.vehicle-info-label { font-size: 0.6rem; color: rgba(245,237,214,0.3); letter-spacing: 0.12em; text-transform: uppercase; margin-top: 0.2rem; }

/* ============================================================
   NOTIFICATIONS PAGE
   ============================================================ */
.notif-list { display: flex; flex-direction: column; gap: 0.6rem; }
.notif-item {
  display: flex; gap: 1rem; padding: 1rem 1.2rem;
  background: var(--panel); border: 1px solid var(--border);
  transition: all 0.2s; cursor: pointer; position: relative;
}
.notif-item:hover { border-color: rgba(201,168,76,0.3); }
.notif-item.unread { border-left: 3px solid var(--gold); background: rgba(201,168,76,0.05); }
.notif-item.unread::after { content:''; position:absolute; top:0.7rem; right:0.7rem; width:7px; height:7px; border-radius:50%; background:var(--gold); }
.notif-icon { font-size: 1.3rem; flex-shrink: 0; width: 36px; text-align: center; margin-top: 0.1rem; }
.notif-text { flex: 1; }
.notif-title { font-size: 0.85rem; font-weight: 600; color: var(--cream); margin-bottom: 0.25rem; }
.notif-body { font-size: 0.77rem; color: rgba(245,237,214,0.45); line-height: 1.55; }
.notif-time { font-size: 0.65rem; color: rgba(245,237,214,0.25); flex-shrink: 0; margin-top: 0.1rem; white-space: nowrap; }

/* ============================================================
   SUPPORT PAGE
   ============================================================ */
.support-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 1.3rem; margin-bottom: 2rem; }
.support-card {
  padding: 1.6rem; background: var(--panel); border: 1px solid var(--border);
  cursor: pointer; transition: all 0.3s; text-decoration: none;
  display: flex; align-items: flex-start; gap: 1.1rem;
}
.support-card:hover { border-color: var(--gold); background: rgba(201,168,76,0.06); transform: translateY(-2px); }
.support-card-icon { font-size: 2rem; flex-shrink: 0; }
.support-card-title { font-family: 'Cinzel', serif; font-size: 0.9rem; font-weight: 700; color: var(--cream); margin-bottom: 0.4rem; }
.support-card-desc { font-size: 0.78rem; color: rgba(245,237,214,0.42); line-height: 1.6; }

.faq-list { display: flex; flex-direction: column; gap: 0; }
.faq-item { border-bottom: 1px solid var(--border); }
.faq-q {
  padding: 1rem 0; font-size: 0.88rem; font-weight: 600; color: var(--cream);
  cursor: pointer; display: flex; justify-content: space-between; align-items: center;
  transition: color 0.2s;
}
.faq-q:hover { color: var(--gold); }
.faq-q-arrow { font-size: 0.7rem; color: rgba(245,237,214,0.35); transition: transform 0.3s; }
.faq-item.open .faq-q-arrow { transform: rotate(180deg); }
.faq-a { font-size: 0.8rem; color: rgba(245,237,214,0.5); line-height: 1.7; max-height: 0; overflow: hidden; transition: max-height 0.3s ease, padding 0.3s; }
.faq-item.open .faq-a { max-height: 200px; padding-bottom: 1rem; }

/* ============================================================
   MODAL
   ============================================================ */
.modal-overlay {
  position: fixed; inset: 0; z-index: 3000;
  background: rgba(0,0,0,0.75); backdrop-filter: blur(5px);
  display: flex; align-items: center; justify-content: center;
  opacity: 0; pointer-events: none; transition: opacity 0.3s;
}
.modal-overlay.visible { opacity: 1; pointer-events: all; }
.modal {
  background: #0F0A05; border: 1px solid var(--gold); width: 100%; max-width: 480px;
  padding: 2rem; position: relative; animation: modalIn 0.35s cubic-bezier(0.22,1,0.36,1);
}
@keyframes modalIn { from{transform:scale(0.92) translateY(20px);opacity:0;} to{transform:scale(1) translateY(0);opacity:1;} }
.modal::before { content:''; position:absolute; top:0; left:0; right:0; height:2px; background:var(--gold); }
.modal-title { font-family:'Cinzel',serif; font-size:1.1rem; font-weight:700; color:var(--gold); margin-bottom:1.4rem; letter-spacing:0.1em; }
.modal-close { position:absolute; top:0.8rem; right:0.9rem; background:none; border:none; color:rgba(245,237,214,0.35); font-size:1.2rem; cursor:pointer; transition:color 0.2s; }
.modal-close:hover { color:var(--gold); }
.modal-actions { display:flex; gap:0.8rem; margin-top:1.5rem; justify-content:flex-end; }
.modal-btn { padding:0.65rem 1.4rem; border:none; font-family:'Outfit',sans-serif; font-size:0.78rem; font-weight:700; letter-spacing:0.1em; text-transform:uppercase; cursor:pointer; transition:all 0.25s; }
.modal-btn-primary { background:var(--gold); color:var(--ink); }
.modal-btn-primary:hover { background:var(--gold-light); }
.modal-btn-secondary { background:none; border:1px solid var(--border); color:rgba(245,237,214,0.5); }
.modal-btn-secondary:hover { border-color:var(--gold); color:var(--gold); }

/* Toast */
.toast {
  position:fixed; bottom:2rem; right:2rem; z-index:9000;
  background:var(--ink-mid); border:1px solid var(--gold);
  padding:0.9rem 1.4rem; max-width:320px;
  display:flex; align-items:flex-start; gap:0.8rem;
  animation:toastIn 0.4s cubic-bezier(0.22,1,0.36,1);
  box-shadow:0 20px 50px rgba(0,0,0,0.4);
}
@keyframes toastIn { from{opacity:0;transform:translateX(30px);} to{opacity:1;transform:translateX(0);} }
.toast.exit { animation:toastOut 0.3s ease forwards; }
@keyframes toastOut { to{opacity:0;transform:translateX(30px);} }
.toast-icon { font-size:1.1rem; flex-shrink:0; margin-top:0.1rem; }
.toast-text { flex:1; }
.toast-title { font-size:0.82rem; font-weight:700; color:var(--cream); margin-bottom:0.2rem; }
.toast-msg { font-size:0.75rem; color:rgba(245,237,214,0.5); line-height:1.5; }

/* ============================================================
   MISC UTILITIES
   ============================================================ */
.divider { height:1px; background:var(--border); margin:1.5rem 0; }
.text-gold { color: var(--gold); }
.text-green { color: var(--green-light); }
.text-red { color: #E8835A; }
.text-muted { color: rgba(245,237,214,0.35); }
.flex-between { display:flex; justify-content:space-between; align-items:center; }
.mb-1 { margin-bottom:0.7rem; }
.mb-2 { margin-bottom:1.4rem; }
.mt-1 { margin-top:0.7rem; }
.fw-700 { font-weight:700; }

/* Scrollbar */
::-webkit-scrollbar { width:5px; height:5px; }
::-webkit-scrollbar-track { background:rgba(255,255,255,0.03); }
::-webkit-scrollbar-thumb { background:rgba(201,168,76,0.3); border-radius:3px; }
::-webkit-scrollbar-thumb:hover { background:rgba(201,168,76,0.55); }

/* Responsive */
@media (max-width: 1024px) {
  .grid-4 { grid-template-columns: repeat(2,1fr); }
  .support-grid { grid-template-columns: 1fr; }
}
@media (max-width: 768px) {
  .sidebar { transform: translateX(-260px); }
  .sidebar.open { transform: translateX(0); }
  .main-content { margin-left: 0; }
  .grid-2, .grid-3 { grid-template-columns: 1fr; }
  .page { padding: 1rem; }
  .trips-table { font-size: 0.78rem; }
  .trips-table th:nth-child(4), .trips-table td:nth-child(4) { display:none; }
}
</style>
</head>
<body>

<!-- ============================================================
     AUTH SCREEN
     ============================================================ -->
<div id="auth-screen">
  <div class="auth-stars" id="auth-stars"></div>
  <div class="auth-ring"></div>
  <div class="auth-ring"></div>

  <div class="auth-box">
    <!-- Logo -->
    <div class="auth-logo">
      <svg class="auth-logo-camel" viewBox="0 0 200 200" xmlns="http://www.w3.org/2000/svg">
        <ellipse cx="100" cy="140" rx="65" ry="30" fill="#C9A84C" opacity="0.15"/>
        <rect x="60" y="150" width="18" height="35" rx="5" fill="#C9A84C"/>
        <rect x="115" y="150" width="18" height="35" rx="5" fill="#C9A84C"/>
        <rect x="72" y="140" width="12" height="28" rx="4" fill="#C9A84C"/>
        <rect x="108" y="140" width="12" height="28" rx="4" fill="#C9A84C"/>
        <ellipse cx="100" cy="120" rx="52" ry="32" fill="#C9A84C"/>
        <ellipse cx="145" cy="90" rx="22" ry="28" fill="#C9A84C"/>
        <ellipse cx="100" cy="88" rx="28" ry="16" fill="#C9A84C"/>
        <ellipse cx="158" cy="70" rx="12" ry="20" fill="#C9A84C"/>
        <circle cx="163" cy="58" r="8" fill="#C9A84C"/>
        <ellipse cx="167" cy="53" rx="4" ry="3" fill="#C9A84C"/>
        <circle cx="159" cy="52" r="2" fill="#1A1209"/>
        <path d="M 162 56 Q 170 50 174 54" stroke="#8B6914" stroke-width="1.5" fill="none" stroke-linecap="round"/>
        <ellipse cx="72" cy="88" rx="18" ry="10" fill="#8B6914" opacity="0.4"/>
        <ellipse cx="128" cy="88" rx="18" ry="10" fill="#8B6914" opacity="0.4"/>
        <path d="M 48 120 Q 35 110 38 125" stroke="#8B6914" stroke-width="3" fill="none" stroke-linecap="round"/>
        <path d="M 38 125 Q 30 130 35 135" stroke="#8B6914" stroke-width="2" fill="none" stroke-linecap="round"/>
      </svg>
      <span class="auth-logo-name">KERVAN</span>
    </div>
    <div class="auth-role-badge">🚗 &nbsp; Driver Portal &nbsp; — &nbsp; Haydovchi Tizimi</div>

    <div class="auth-card">
      <div class="auth-tabs">
        <button class="auth-tab active" onclick="switchAuthTab('login')">Kirish</button>
        <button class="auth-tab" onclick="switchAuthTab('register')">Ro'yxatdan o'tish</button>
      </div>

      <!-- LOGIN -->
      <div class="auth-form-panel active" id="panel-login">
        <div class="auth-error" id="login-error">Telefon raqam yoki parol noto'g'ri.</div>
        <div class="field-group">
          <div class="field-label">Telefon raqam</div>
          <div class="auth-phone-group">
            <input class="auth-phone-prefix" value="+998" readonly>
            <input class="field-input" id="login-phone" type="tel" placeholder="90 123 45 67" maxlength="12">
          </div>
        </div>
        <div class="field-group">
          <div class="field-label">Parol</div>
          <input class="field-input" id="login-pass" type="password" placeholder="••••••••">
        </div>
        <div class="auth-forgot" onclick="showOtpPanel()">Parolni unutdingizmi? SMS orqali tiklash</div>
        <button class="auth-btn" onclick="doLogin()">Kirish →</button>
        <div class="auth-divider">YOKI</div>
        <div class="auth-terms">Demo uchun istalgan ma'lumot kiriting va <span style="color:var(--gold)">Kirish</span> tugmasini bosing</div>
      </div>

      <!-- OTP / forgot -->
      <div class="auth-form-panel" id="panel-otp">
        <div class="auth-success" id="otp-sent" style="display:block;">SMS kod yuborildi: +998 ** *** ** **</div>
        <div class="field-group">
          <div class="field-label" style="text-align:center;">SMS kodini kiriting</div>
          <div class="otp-inputs">
            <input class="otp-input" maxlength="1" oninput="otpMove(this,1)">
            <input class="otp-input" maxlength="1" oninput="otpMove(this,2)">
            <input class="otp-input" maxlength="1" oninput="otpMove(this,3)">
            <input class="otp-input" maxlength="1" oninput="otpMove(this,4)">
          </div>
        </div>
        <div class="otp-resend">Kodni olmadingizmi? <span onclick="showToast('📱','SMS qayta yuborildi','30 soniyada qayta so\'rang')">Qayta yuborish</span></div>
        <button class="auth-btn" onclick="doLogin()">Tasdiqlash →</button>
        <button class="auth-btn" style="background:none;border:1px solid var(--border);color:rgba(245,237,214,0.5);margin-top:0;" onclick="switchAuthTab('login')">← Orqaga</button>
      </div>

      <!-- REGISTER -->
      <div class="auth-form-panel" id="panel-register">
        <div class="auth-error" id="reg-error"></div>
        <div class="field-row">
          <div class="field-group">
            <div class="field-label">Ism</div>
            <input class="field-input" id="reg-fname" placeholder="Jasur">
          </div>
          <div class="field-group">
            <div class="field-label">Familya</div>
            <input class="field-input" id="reg-lname" placeholder="Toshmatov">
          </div>
        </div>
        <div class="field-group">
          <div class="field-label">Telefon raqam</div>
          <div class="auth-phone-group">
            <input class="auth-phone-prefix" value="+998" readonly>
            <input class="field-input" id="reg-phone" type="tel" placeholder="90 123 45 67">
          </div>
        </div>
        <div class="field-group">
          <div class="field-label">Parol</div>
          <input class="field-input" id="reg-pass" type="password" placeholder="Kamida 8 belgi">
        </div>
        <div class="field-group">
          <div class="field-label">Parolni tasdiqlang</div>
          <input class="field-input" id="reg-pass2" type="password" placeholder="••••••••">
        </div>
        <div class="field-group">
          <div class="field-label">Shahar</div>
          <select class="field-input" id="reg-city">
            <option value="">Shaharni tanlang...</option>
            <option>Toshkent</option><option>Samarqand</option><option>Buxoro</option>
            <option>Namangan</option><option>Andijon</option><option>Farg'ona</option>
            <option>Nukus</option><option>Urganch</option><option>Termiz</option>
          </select>
        </div>
        <button class="auth-btn" onclick="doRegister()">Ro'yxatdan o'tish →</button>
        <div class="auth-terms">Ro'yxatdan o'tish orqali siz Kervan <a>Foydalanish shartlari</a> va <a>Maxfiylik siyosatimiz</a>ga rozilik bildirasiz.</div>
      </div>
    </div><!-- /auth-card -->
  </div><!-- /auth-box -->
</div>

<!-- ============================================================
     MAIN APPLICATION
     ============================================================ -->
<div id="main-app">

  <!-- SIDEBAR -->
  <aside class="sidebar" id="sidebar">
    <a class="sidebar-logo" href="#">
      <svg class="sidebar-logo-camel" viewBox="0 0 200 200" xmlns="http://www.w3.org/2000/svg">
        <ellipse cx="100" cy="140" rx="65" ry="30" fill="#C9A84C" opacity="0.15"/>
        <rect x="60" y="150" width="18" height="35" rx="5" fill="#C9A84C"/>
        <rect x="115" y="150" width="18" height="35" rx="5" fill="#C9A84C"/>
        <rect x="72" y="140" width="12" height="28" rx="4" fill="#C9A84C"/>
        <rect x="108" y="140" width="12" height="28" rx="4" fill="#C9A84C"/>
        <ellipse cx="100" cy="120" rx="52" ry="32" fill="#C9A84C"/>
        <ellipse cx="145" cy="90" rx="22" ry="28" fill="#C9A84C"/>
        <ellipse cx="100" cy="88" rx="28" ry="16" fill="#C9A84C"/>
        <ellipse cx="158" cy="70" rx="12" ry="20" fill="#C9A84C"/>
        <circle cx="163" cy="58" r="8" fill="#C9A84C"/>
        <circle cx="159" cy="52" r="2" fill="#1A1209"/>
      </svg>
      <div>
        <div class="sidebar-logo-name">KERVAN</div>
        <div class="sidebar-logo-sub">Driver Portal</div>
      </div>
    </a>

    <!-- Driver mini profile -->
    <div class="sidebar-profile">
      <div class="sidebar-avatar" onclick="navigate('profile')">
        <span id="avatar-initials">JT</span>
        <div class="online-badge" id="online-badge"></div>
      </div>
      <div class="sidebar-profile-info">
        <div class="sidebar-profile-name" id="sidebar-driver-name">Jasur Toshmatov</div>
        <div class="sidebar-profile-id">ID: KRV-04821</div>
        <div class="sidebar-profile-status" id="status-label" onclick="toggleOnline()">Onlayn</div>
      </div>
    </div>

    <!-- Navigation -->
    <nav class="sidebar-nav">
      <div class="nav-section-label">Asosiy</div>
      <a class="nav-item active" id="nav-dashboard" onclick="navigate('dashboard')">
        <span class="nav-item-icon">🏠</span> Dashboard
      </a>
      <a class="nav-item" id="nav-trips" onclick="navigate('trips')">
        <span class="nav-item-icon">🗺️</span> Sayohatlar
        <span class="nav-badge gold" id="active-trip-badge" style="display:none">1</span>
      </a>
      <a class="nav-item" id="nav-earnings" onclick="navigate('earnings')">
        <span class="nav-item-icon">💰</span> Daromad
      </a>

      <div class="nav-section-label">Mening Hisobim</div>
      <a class="nav-item" id="nav-profile" onclick="navigate('profile')">
        <span class="nav-item-icon">👤</span> Profil
      </a>
      <a class="nav-item" id="nav-vehicle" onclick="navigate('vehicle')">
        <span class="nav-item-icon">🚗</span> Avtomobil
      </a>
      <a class="nav-item" id="nav-notifications" onclick="navigate('notifications')">
        <span class="nav-item-icon">🔔</span> Bildirishnomalar
        <span class="nav-badge" id="notif-count">3</span>
      </a>

      <div class="nav-section-label">Yordam</div>
      <a class="nav-item" id="nav-support" onclick="navigate('support')">
        <span class="nav-item-icon">❓</span> Yordam markazi
      </a>
      <a class="nav-item" onclick="showLogoutModal()">
        <span class="nav-item-icon">🚪</span> Chiqish
      </a>
    </nav>

    <div class="sidebar-bottom">
      <button class="sidebar-toggle-online" id="online-toggle" onclick="toggleOnline()">
        ONLAYN &nbsp; ●
      </button>
    </div>
  </aside>

  <!-- MAIN CONTENT -->
  <div class="main-content">

    <!-- Topbar -->
    <div class="topbar">
      <div class="topbar-left">
        <span class="page-title" id="topbar-title">Dashboard</span>
        <span class="page-breadcrumb" id="topbar-breadcrumb">Xush kelibsiz, Jasur!</span>
      </div>
      <div class="topbar-right">
        <button class="topbar-btn notif-dot" onclick="navigate('notifications')">🔔</button>
        <button class="topbar-btn" onclick="showRideRequest()">🧪 Yangi So'rov</button>
        <button class="topbar-btn primary" onclick="toggleOnline()" id="topbar-status-btn">● Onlayn</button>
      </div>
    </div>

    <!-- ====================================================
         PAGE: DASHBOARD
         ==================================================== -->
    <div class="page active" id="page-dashboard">

      <!-- Live status banner -->
      <div class="live-banner" id="live-banner">
        <div class="live-indicator">
          <div class="live-dot" id="live-dot"></div>
          <div>
            <div class="live-text" id="live-text">Faol — Buyurtmalar qabul qilinmoqda</div>
            <div class="live-since">Bugun soat 09:14 dan beri onlayn</div>
          </div>
        </div>
        <div style="text-align:right">
          <div style="font-size:0.65rem;color:rgba(245,237,214,0.35);letter-spacing:0.1em;text-transform:uppercase;">Faoliyat vaqti</div>
          <div class="live-timer" id="live-timer">2:47:33</div>
        </div>
      </div>

      <!-- KPI Cards -->
      <div class="grid-4 mb-2">
        <div class="card card-gold-top">
          <div class="card-label">Bugungi daromad</div>
          <div class="card-value gold" id="kpi-today">187,500</div>
          <div class="card-meta up">↑ +12% kecha</div>
          <div class="earnings-chart" id="weekly-bars"></div>
        </div>
        <div class="card card-gold-top">
          <div class="card-label">Bugungi sayohatlar</div>
          <div class="card-value" id="kpi-trips">8</div>
          <div class="card-meta">Maqsad: 12 ta</div>
          <div style="margin-top:1rem;background:rgba(255,255,255,0.06);height:5px;border-radius:3px;overflow:hidden;">
            <div style="width:66%;height:100%;background:var(--gold);border-radius:3px;transition:width 1s ease;"></div>
          </div>
        </div>
        <div class="card card-gold-top">
          <div class="card-label">Reyting</div>
          <div style="display:flex;align-items:center;gap:1rem;">
            <div class="rating-ring">
              <svg width="100" height="100" viewBox="0 0 100 100">
                <circle class="rating-ring-bg" cx="50" cy="50" r="42"/>
                <circle class="rating-ring-fill" cx="50" cy="50" r="42"
                  stroke-dasharray="263.9" stroke-dashoffset="26" id="rating-circle"/>
              </svg>
              <div class="rating-center">
                <div class="rating-num">4.9</div>
                <div class="rating-of">/ 5.0</div>
              </div>
            </div>
            <div>
              <div style="font-size:0.72rem;color:rgba(245,237,214,0.4);">Jami: <b style="color:var(--cream)">412</b> ta baho</div>
              <div style="font-size:0.72rem;color:rgba(245,237,214,0.4);margin-top:0.4rem;">⭐⭐⭐⭐⭐ — 94%</div>
            </div>
          </div>
        </div>
        <div class="card card-gold-top">
          <div class="card-label">Qabul qilish darajasi</div>
          <div class="card-value green">93%</div>
          <div class="card-meta up">↑ Zo'r!</div>
          <div style="margin-top:0.8rem;font-size:0.72rem;color:rgba(245,237,214,0.35);line-height:1.7;">
            Rad etilgan: <b style="color:var(--cream)">3</b><br>
            Bekor qilingan: <b style="color:var(--cream)">1</b>
          </div>
        </div>
      </div>

      <!-- Recent trips + Quick stats -->
      <div class="grid-2">
        <div class="card">
          <div class="flex-between mb-2">
            <div style="font-family:'Cinzel',serif;font-size:0.88rem;font-weight:700;color:var(--cream);letter-spacing:0.08em;">So'nggi sayohatlar</div>
            <button class="topbar-btn" onclick="navigate('trips')" style="font-size:0.65rem;padding:0.3rem 0.7rem;">Barchasi →</button>
          </div>
          <div class="trip-list" id="recent-trips-list"></div>
        </div>

        <div style="display:flex;flex-direction:column;gap:1.3rem;">
          <!-- Today goal -->
          <div class="card">
            <div class="card-label">Bugungi maqsad</div>
            <div style="display:flex;justify-content:space-between;align-items:flex-end;margin-bottom:0.7rem;">
              <span style="font-family:'Cinzel',serif;font-size:1.4rem;font-weight:700;color:var(--gold);">187,500 so'm</span>
              <span style="font-size:0.75rem;color:rgba(245,237,214,0.35);">Maqsad: 300,000</span>
            </div>
            <div style="background:rgba(255,255,255,0.06);height:8px;border-radius:4px;overflow:hidden;">
              <div style="width:62%;height:100%;background:linear-gradient(90deg,var(--gold-dark),var(--gold));border-radius:4px;transition:width 1.2s ease;"></div>
            </div>
            <div style="margin-top:0.7rem;font-size:0.72rem;color:rgba(245,237,214,0.35);">Yana 112,500 so'm kerak</div>
          </div>

          <!-- Bonus zone -->
          <div class="card" style="background:rgba(201,168,76,0.07);border-color:rgba(201,168,76,0.35);">
            <div class="card-label" style="color:var(--gold);">⚡ Bonus zonasi faol</div>
            <div style="font-size:0.88rem;color:var(--cream);font-weight:600;margin:0.5rem 0;">Toshkent — Samarqand yo'li</div>
            <div style="font-size:0.78rem;color:rgba(245,237,214,0.5);line-height:1.6;">
              Hozir bu yo'nalishda sayohat qilsangiz, tarifga <b style="color:var(--gold)">+20% bonus</b> qo'shiladi.<br>
              <span style="font-size:0.7rem;color:rgba(245,237,214,0.3);">Taklif tugaydi: 18:00 gacha</span>
            </div>
          </div>

          <!-- Weather/status -->
          <div class="card" style="background:rgba(26,107,107,0.08);border-color:rgba(26,107,107,0.3);">
            <div style="display:flex;justify-content:space-between;align-items:center;">
              <div>
                <div class="card-label" style="color:rgba(26,107,107,0.9);">Ob-havo — Toshkent</div>
                <div style="font-size:1.6rem;margin:0.3rem 0;">☀️ <span style="font-family:'Cinzel',serif;font-size:1.4rem;color:var(--cream);">28°C</span></div>
                <div style="font-size:0.75rem;color:rgba(245,237,214,0.4);">Quruq, yaxshi ko'rinish</div>
              </div>
              <div style="text-align:right;">
                <div style="font-size:0.7rem;color:rgba(245,237,214,0.3);margin-bottom:0.4rem;">Yo'lda talablar</div>
                <div style="font-family:'Cinzel',serif;font-size:1.5rem;color:var(--green-light);">Yuqori</div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>

    <!-- ====================================================
         PAGE: TRIPS
         ==================================================== -->
    <div class="page" id="page-trips">
      <div class="section-header">
        <div class="section-title">Sayohatlar tarixi</div>
        <div class="section-sub">Barcha qabul qilingan va bekor qilingan sayohatlar</div>
      </div>

      <div class="trips-filters">
        <button class="filter-btn active" onclick="filterTrips('all',this)">Barchasi</button>
        <button class="filter-btn" onclick="filterTrips('completed',this)">Bajarilgan</button>
        <button class="filter-btn" onclick="filterTrips('cancelled',this)">Bekor qilingan</button>
        <button class="filter-btn" onclick="filterTrips('active',this)">Faol</button>
        <input type="date" class="field-input" style="width:160px;padding:0.4rem 0.7rem;font-size:0.78rem;" id="trips-date-filter" onchange="renderTripsTable()">
      </div>

      <div class="card" style="overflow-x:auto;padding:0;">
        <table class="trips-table" id="trips-table">
          <thead>
            <tr>
              <th>ID</th>
              <th>Yo'nalish</th>
              <th>Masofa</th>
              <th>Vaqt</th>
              <th>Summa</th>
              <th>To'lov usuli</th>
              <th>Reyting</th>
              <th>Holat</th>
            </tr>
          </thead>
          <tbody id="trips-tbody"></tbody>
        </table>
      </div>

      <!-- Pagination -->
      <div style="display:flex;justify-content:space-between;align-items:center;margin-top:1.2rem;flex-wrap:wrap;gap:1rem;">
        <div style="font-size:0.78rem;color:rgba(245,237,214,0.35);" id="trips-count-label"></div>
        <div style="display:flex;gap:0.5rem;" id="pagination"></div>
      </div>
    </div>

    <!-- ====================================================
         PAGE: EARNINGS
         ==================================================== -->
    <div class="page" id="page-earnings">
      <div class="section-header">
        <div class="section-title">Daromad & To'lovlar</div>
        <div class="section-sub">Kunlik, haftalik va oylik statistika</div>
      </div>

      <div class="grid-4 mb-2">
        <div class="card card-gold-top">
          <div class="card-label">Bugun</div>
          <div class="card-value gold">187,500</div>
          <div class="card-meta">so'm</div>
        </div>
        <div class="card card-gold-top">
          <div class="card-label">Bu hafta</div>
          <div class="card-value gold">1,240,000</div>
          <div class="card-meta up">↑ +8%</div>
        </div>
        <div class="card card-gold-top">
          <div class="card-label">Bu oy</div>
          <div class="card-value gold">4,875,000</div>
          <div class="card-meta up">↑ +15%</div>
        </div>
        <div class="card card-gold-top">
          <div class="card-label">Umumiy (jami)</div>
          <div class="card-value">42.1M</div>
          <div class="card-meta">so'm — barcha vaqt</div>
        </div>
      </div>

      <!-- Chart -->
      <div class="card mb-2">
        <div class="flex-between mb-2">
          <div style="font-family:'Cinzel',serif;font-size:0.88rem;font-weight:700;color:var(--cream);letter-spacing:0.08em;">Daromad grafigi</div>
          <div class="earnings-period-selector">
            <button class="period-btn active" onclick="setPeriod('week',this)">Hafta</button>
            <button class="period-btn" onclick="setPeriod('month',this)">Oy</button>
            <button class="period-btn" onclick="setPeriod('year',this)">Yil</button>
          </div>
        </div>
        <div class="big-bar-chart" id="big-bar-chart"></div>
        <div class="chart-x-labels" id="chart-x-labels"></div>
      </div>

      <div class="grid-2 mb-2">
        <!-- Breakdown -->
        <div class="card">
          <div style="font-family:'Cinzel',serif;font-size:0.88rem;font-weight:700;color:var(--cream);letter-spacing:0.08em;margin-bottom:1.2rem;">Daromad taqsimoti</div>
          <div class="breakdown-list">
            <div class="breakdown-item">
              <span class="breakdown-label">Shaharlararo sayohatlar</span>
              <div class="breakdown-bar-wrap"><div class="breakdown-bar-fill" style="width:78%;"></div></div>
              <span class="breakdown-value">3,802,500 so'm</span>
            </div>
            <div class="breakdown-item">
              <span class="breakdown-label">Shahar ichida</span>
              <div class="breakdown-bar-wrap"><div class="breakdown-bar-fill" style="width:55%;background:var(--teal);"></div></div>
              <span class="breakdown-value">688,500 so'm</span>
            </div>
            <div class="breakdown-item">
              <span class="breakdown-label">Bonus va mukofotlar</span>
              <div class="breakdown-bar-wrap"><div class="breakdown-bar-fill" style="width:25%;background:var(--green-light);"></div></div>
              <span class="breakdown-value">243,750 so'm</span>
            </div>
            <div class="breakdown-item">
              <span class="breakdown-label">Kervan komissiyasi (−12%)</span>
              <div class="breakdown-bar-wrap"><div class="breakdown-bar-fill" style="width:12%;background:var(--rust);"></div></div>
              <span class="breakdown-value" style="color:#E8835A;">−585,000 so'm</span>
            </div>
          </div>
        </div>

        <!-- Payment methods breakdown -->
        <div class="card">
          <div style="font-family:'Cinzel',serif;font-size:0.88rem;font-weight:700;color:var(--cream);letter-spacing:0.08em;margin-bottom:1.2rem;">To'lov usullari</div>
          <div style="display:flex;flex-direction:column;gap:0.9rem;">
            <div style="display:flex;align-items:center;justify-content:space-between;padding:0.8rem;background:rgba(255,255,255,0.025);border:1px solid var(--border);">
              <div style="display:flex;align-items:center;gap:0.7rem;">
                <span style="font-size:1.3rem;">💵</span>
                <div>
                  <div style="font-size:0.82rem;font-weight:600;color:var(--cream);">Naqd pul</div>
                  <div style="font-size:0.68rem;color:rgba(245,237,214,0.35);">62% sayohatlar</div>
                </div>
              </div>
              <div style="font-family:'Cinzel',serif;font-size:0.95rem;font-weight:700;color:var(--gold);">3,022,500</div>
            </div>
            <div style="display:flex;align-items:center;justify-content:space-between;padding:0.8rem;background:rgba(255,255,255,0.025);border:1px solid var(--border);">
              <div style="display:flex;align-items:center;gap:0.7rem;">
                <span style="font-size:1.3rem;">💳</span>
                <div>
                  <div style="font-size:0.82rem;font-weight:600;color:var(--cream);">Karta (Click/Payme)</div>
                  <div style="font-size:0.68rem;color:rgba(245,237,214,0.35);">38% sayohatlar</div>
                </div>
              </div>
              <div style="font-family:'Cinzel',serif;font-size:0.95rem;font-weight:700;color:var(--gold);">1,852,500</div>
            </div>
          </div>
        </div>
      </div>

      <!-- Withdraw -->
      <div class="withdraw-section">
        <div class="flex-between mb-1" style="flex-wrap:wrap;gap:1rem;">
          <div class="withdraw-balance">
            <div class="withdraw-balance-label">Hisobdagi balans</div>
            <div class="withdraw-balance-amount">2,340,000 so'm</div>
            <div class="withdraw-balance-sub">Keyingi to'lov sanasi: 15-May</div>
          </div>
          <button class="withdraw-btn" onclick="showWithdrawModal()">Pul yechish →</button>
        </div>
        <div style="font-size:0.72rem;color:rgba(245,237,214,0.35);margin-bottom:0.8rem;letter-spacing:0.08em;text-transform:uppercase;">To'lov usulini tanlang</div>
        <div class="withdraw-methods">
          <div class="pay-method selected" onclick="selectPayMethod(this)"><span class="pay-method-icon">💳</span> Click</div>
          <div class="pay-method" onclick="selectPayMethod(this)"><span class="pay-method-icon">📱</span> Payme</div>
          <div class="pay-method" onclick="selectPayMethod(this)"><span class="pay-method-icon">🏦</span> Bank</div>
          <div class="pay-method" onclick="selectPayMethod(this)"><span class="pay-method-icon">💵</span> Naqd</div>
        </div>
      </div>
    </div>

    <!-- ====================================================
         PAGE: PROFILE
         ==================================================== -->
    <div class="page" id="page-profile">
      <!-- Profile hero -->
      <div class="profile-hero">
        <div class="profile-avatar-big">
          <span id="profile-initials">JT</span>
          <div class="profile-avatar-edit" onclick="showToast('📷','Rasm yuklash','Kamera yoki galereyadan tanlang')">✏️</div>
        </div>
        <div class="profile-main-info">
          <div class="profile-name" id="profile-full-name">Jasur Toshmatov</div>
          <div class="profile-id-row">
            <div class="profile-driver-id">Haydovchi ID: KRV-04821</div>
            <div class="profile-verified">Tasdiqlangan haydovchi</div>
          </div>
          <div class="profile-stats-row">
            <div class="profile-stat">
              <div class="profile-stat-val">412</div>
              <div class="profile-stat-label">Sayohat</div>
            </div>
            <div class="profile-stat">
              <div class="profile-stat-val">4.9 ⭐</div>
              <div class="profile-stat-label">Reyting</div>
            </div>
            <div class="profile-stat">
              <div class="profile-stat-val">2 yil</div>
              <div class="profile-stat-label">Tajriba</div>
            </div>
            <div class="profile-stat">
              <div class="profile-stat-val">93%</div>
              <div class="profile-stat-label">Qabul %</div>
            </div>
          </div>
        </div>
      </div>

      <!-- Personal info form -->
      <div class="card mb-2">
        <div class="form-section-title">👤 Shaxsiy ma'lumotlar</div>
        <div class="form-grid">
          <div class="field-group">
            <div class="field-label">Ism</div>
            <input class="field-input" id="pf-fname" value="Jasur">
          </div>
          <div class="field-group">
            <div class="field-label">Familya</div>
            <input class="field-input" id="pf-lname" value="Toshmatov">
          </div>
          <div class="field-group">
            <div class="field-label">Telefon</div>
            <input class="field-input" id="pf-phone" value="+998 90 123 45 67">
          </div>
          <div class="field-group">
            <div class="field-label">Elektron pochta</div>
            <input class="field-input" id="pf-email" type="email" value="jasur.toshmatov@gmail.com">
          </div>
          <div class="field-group">
            <div class="field-label">Shahar</div>
            <select class="field-input" id="pf-city">
              <option selected>Toshkent</option>
              <option>Samarqand</option><option>Buxoro</option><option>Namangan</option>
            </select>
          </div>
          <div class="field-group">
            <div class="field-label">Tug'ilgan sana</div>
            <input class="field-input" type="date" id="pf-dob" value="1992-05-18">
          </div>
        </div>
        <div style="margin-top:1.2rem;display:flex;gap:0.8rem;">
          <button class="save-btn" onclick="saveProfile()">Saqlash</button>
          <button class="save-btn" style="background:none;border:1px solid var(--border);color:rgba(245,237,214,0.5);" onclick="showChangePassModal()">Parolni o'zgartirish</button>
        </div>
      </div>

      <!-- Documents -->
      <div class="card mb-2">
        <div class="form-section-title">📄 Hujjatlar</div>
        <div class="doc-grid">
          <div class="doc-card uploaded">
            <div class="doc-check">✅</div>
            <div class="doc-icon">🪪</div>
            <div class="doc-name">Pasport</div>
            <div class="doc-status ok">Tasdiqlangan</div>
            <div class="doc-expiry">Amal qilish muddati: 2028-03-15</div>
          </div>
          <div class="doc-card uploaded">
            <div class="doc-check">✅</div>
            <div class="doc-icon">🚗</div>
            <div class="doc-name">Haydovchilik guvohnomasi</div>
            <div class="doc-status ok">Tasdiqlangan</div>
            <div class="doc-expiry">Muddati: 2026-08-20</div>
          </div>
          <div class="doc-card" onclick="uploadDoc(this,'texnik_ko_rik')">
            <div class="doc-icon">🔧</div>
            <div class="doc-name">Texnik ko'rik</div>
            <div class="doc-status expired">⚠ Muddati o'tgan!</div>
            <div class="doc-expiry">O'tgan: 2024-12-01</div>
            <button class="doc-upload-btn">Yangilash</button>
          </div>
          <div class="doc-card" onclick="uploadDoc(this,'sugurta')">
            <div class="doc-icon">📋</div>
            <div class="doc-name">Avtomobil sug'urtasi</div>
            <div class="doc-status pending">Yuklanmagan</div>
            <div class="doc-expiry">—</div>
            <button class="doc-upload-btn">Yuklash</button>
          </div>
          <div class="doc-card uploaded">
            <div class="doc-check">✅</div>
            <div class="doc-icon">📸</div>
            <div class="doc-name">Profil rasmi</div>
            <div class="doc-status ok">Tasdiqlangan</div>
            <div class="doc-expiry">—</div>
          </div>
          <div class="doc-card" onclick="uploadDoc(this,'tibbiy')">
            <div class="doc-icon">🏥</div>
            <div class="doc-name">Tibbiy ma'lumotnoma</div>
            <div class="doc-status pending">Ko'rib chiqilmoqda</div>
            <div class="doc-expiry">Yuborilgan: 2025-05-10</div>
          </div>
        </div>
      </div>

      <!-- Bank details -->
      <div class="card">
        <div class="form-section-title">🏦 Bank va to'lov</div>
        <div class="form-grid">
          <div class="field-group">
            <div class="field-label">Karta raqami</div>
            <input class="field-input" value="8600 **** **** 3421" readonly style="font-family:'Cinzel',serif;letter-spacing:0.1em;">
          </div>
          <div class="field-group">
            <div class="field-label">To'lov tizimi</div>
            <select class="field-input">
              <option>Click</option><option>Payme</option><option>Bank o'tkazmasi</option>
            </select>
          </div>
        </div>
        <button class="save-btn" style="margin-top:1.2rem;" onclick="showToast('🏦','Bank ma\'lumotlari','Muvaffaqiyatli saqlandi')">Saqlash</button>
      </div>
    </div>

    <!-- ====================================================
         PAGE: VEHICLE
         ==================================================== -->
    <div class="page" id="page-vehicle">
      <div class="section-header">
        <div class="section-title">Mening avtomobilim</div>
        <div class="section-sub">Avtomobil ma'lumotlari va texnik holati</div>
      </div>

      <div class="grid-2 mb-2">
        <div class="card" style="text-align:center;padding:2rem;">
          <span class="vehicle-emoji">🚙</span>
          <div class="vehicle-plate">01 B 123 AB</div>
          <div class="vehicle-model">Chevrolet Cobalt 2021 — Kumush</div>
          <div class="vehicle-info-grid">
            <div class="vehicle-info-item">
              <div class="vehicle-info-val">Cobalt</div>
              <div class="vehicle-info-label">Model</div>
            </div>
            <div class="vehicle-info-item">
              <div class="vehicle-info-val">2021</div>
              <div class="vehicle-info-label">Yil</div>
            </div>
            <div class="vehicle-info-item">
              <div class="vehicle-info-val">Kumush</div>
              <div class="vehicle-info-label">Rang</div>
            </div>
            <div class="vehicle-info-item">
              <div class="vehicle-info-val">4</div>
              <div class="vehicle-info-label">O'rindiqlar</div>
            </div>
            <div class="vehicle-info-item">
              <div class="vehicle-info-val">AC ✓</div>
              <div class="vehicle-info-label">Konditsioner</div>
            </div>
            <div class="vehicle-info-item">
              <div class="vehicle-info-val">87,420</div>
              <div class="vehicle-info-label">Km (yurgan)</div>
            </div>
          </div>
        </div>

        <div style="display:flex;flex-direction:column;gap:1rem;">
          <div class="card card-gold-top">
            <div class="card-label">Texnik holat</div>
            <div style="display:flex;flex-direction:column;gap:0.75rem;margin-top:0.5rem;">
              <div class="flex-between">
                <span style="font-size:0.82rem;color:rgba(245,237,214,0.6);">Moy almashtirish</span>
                <span style="font-size:0.8rem;color:var(--green-light);font-weight:600;">✅ OK</span>
              </div>
              <div class="flex-between">
                <span style="font-size:0.82rem;color:rgba(245,237,214,0.6);">Shina holati</span>
                <span style="font-size:0.8rem;color:var(--gold);font-weight:600;">⚠ Yaqin</span>
              </div>
              <div class="flex-between">
                <span style="font-size:0.82rem;color:rgba(245,237,214,0.6);">Aккumulyator</span>
                <span style="font-size:0.8rem;color:var(--green-light);font-weight:600;">✅ OK</span>
              </div>
              <div class="flex-between">
                <span style="font-size:0.82rem;color:rgba(245,237,214,0.6);">Tormozlar</span>
                <span style="font-size:0.8rem;color:var(--green-light);font-weight:600;">✅ OK</span>
              </div>
              <div class="flex-between">
                <span style="font-size:0.82rem;color:rgba(245,237,214,0.6);">Texnik ko'rik</span>
                <span style="font-size:0.8rem;color:#E8835A;font-weight:600;">❌ Yangilash kerak</span>
              </div>
            </div>
          </div>

          <div class="card">
            <div class="card-label">Avtomobil ma'lumotlarini o'zgartirish</div>
            <div style="display:flex;flex-direction:column;gap:0.8rem;margin-top:0.5rem;">
              <div class="field-group">
                <div class="field-label">Davlat raqami</div>
                <input class="field-input" value="01 B 123 AB">
              </div>
              <div class="field-group">
                <div class="field-label">Avtomobil modeli</div>
                <select class="field-input">
                  <option>Chevrolet Cobalt</option>
                  <option>Chevrolet Nexia</option>
                  <option>Chevrolet Spark</option>
                  <option>Chevrolet Lacetti</option>
                  <option>Chevrolet Malibu</option>
                  <option>Toyota Camry</option>
                  <option>Hyundai Accent</option>
                  <option>Daewoo Matiz</option>
                </select>
              </div>
              <button class="save-btn" style="margin-top:0.3rem;" onclick="showToast('🚗','Avtomobil','Ma\'lumotlar muvaffaqiyatli yangilandi')">Saqlash</button>
            </div>
          </div>
        </div>
      </div>

      <!-- Insurance & docs -->
      <div class="card">
        <div class="form-section-title">📋 Avtomobil hujjatlari</div>
        <div class="doc-grid">
          <div class="doc-card uploaded">
            <div class="doc-check">✅</div>
            <div class="doc-icon">🚙</div>
            <div class="doc-name">Texnik pasport</div>
            <div class="doc-status ok">Tasdiqlangan</div>
            <div class="doc-expiry">Berilgan: 2021-06-10</div>
          </div>
          <div class="doc-card" onclick="uploadDoc(this,'insurance')">
            <div class="doc-icon">🛡️</div>
            <div class="doc-name">OSAGO (sug'urta)</div>
            <div class="doc-status pending">Yuklanmagan</div>
            <button class="doc-upload-btn">Yuklash</button>
          </div>
          <div class="doc-card" onclick="uploadDoc(this,'texnik')">
            <div class="doc-icon">🔧</div>
            <div class="doc-name">Texnik ko'rik</div>
            <div class="doc-status expired">⚠ Muddati o'tgan</div>
            <div class="doc-expiry">Yangilash kerak!</div>
            <button class="doc-upload-btn">Yangilash</button>
          </div>
        </div>
      </div>
    </div>

    <!-- ====================================================
         PAGE: NOTIFICATIONS
         ==================================================== -->
    <div class="page" id="page-notifications">
      <div class="section-header flex-between">
        <div>
          <div class="section-title">Bildirishnomalar</div>
          <div class="section-sub">Barcha yangilik va ogohlantirishlar</div>
        </div>
        <button class="topbar-btn" onclick="markAllRead()">Barchasini o'qilgan deb belgilash</button>
      </div>
      <div class="notif-list" id="notif-list"></div>
    </div>

    <!-- ====================================================
         PAGE: SUPPORT
         ==================================================== -->
    <div class="page" id="page-support">
      <div class="section-header">
        <div class="section-title">Yordam markazi</div>
        <div class="section-sub">Muammo bormi? Biz yordam beramiz</div>
      </div>

      <div class="support-grid mb-2">
        <div class="support-card" onclick="showToast('📞','Qo\'ng\'iroq markazi','Operator sizga 2 daqiqada ulanadi')">
          <div class="support-card-icon">📞</div>
          <div>
            <div class="support-card-title">Telefon orqali yordam</div>
            <div class="support-card-desc">24/7 ishlaydi. +998 71 000 0000. Tez javob kafolatlangan.</div>
          </div>
        </div>
        <div class="support-card" onclick="showToast('💬','Live chat','Operator javob bermoqda...')">
          <div class="support-card-icon">💬</div>
          <div>
            <div class="support-card-title">Online chat</div>
            <div class="support-card-desc">Hozir 3 ta operator mavjud. Odatda 5 daqiqada javob beriladi.</div>
          </div>
        </div>
        <div class="support-card" onclick="showToast('📧','Email yuborildi','Javobi 24 soat ichida keladi')">
          <div class="support-card-icon">📧</div>
          <div>
            <div class="support-card-title">Email orqali murojaat</div>
            <div class="support-card-desc">support@kervan.uz — Murakkab muammolar uchun.</div>
          </div>
        </div>
        <div class="support-card" onclick="showToast('🚨','Favqulodda','Operator yo\'naltirilmoqda')">
          <div class="support-card-icon">🚨</div>
          <div>
            <div class="support-card-title">Favqulodda yordam</div>
            <div class="support-card-desc">Xavfli vaziyatda — darhol SOS tugmasini bosing.</div>
          </div>
        </div>
      </div>

      <!-- Ticket form -->
      <div class="card mb-2">
        <div class="form-section-title">📝 Murojaat yuborish</div>
        <div style="display:flex;flex-direction:column;gap:1rem;">
          <div class="field-group">
            <div class="field-label">Muammo turi</div>
            <select class="field-input">
              <option>To'lov muammosi</option>
              <option>Yomon baho</option>
              <option>Texnik nosozlik</option>
              <option>Hujjat masalasi</option>
              <option>Boshqa</option>
            </select>
          </div>
          <div class="field-group">
            <div class="field-label">Sayohat ID (ixtiyoriy)</div>
            <input class="field-input" placeholder="KRV-2025-XXXXX">
          </div>
          <div class="field-group">
            <div class="field-label">Xabar</div>
            <textarea class="field-input" rows="4" placeholder="Muammoni batafsil yozing..."></textarea>
          </div>
          <button class="save-btn" onclick="showToast('✅','Murojaat qabul qilindi','Raqam: TKT-00847. Javob 24 soat ichida')">Yuborish</button>
        </div>
      </div>

      <!-- FAQ -->
      <div class="card">
        <div class="form-section-title">❓ Ko'p beriladigan savollar</div>
        <div class="faq-list" id="faq-list"></div>
      </div>
    </div>

  </div><!-- /main-content -->
</div><!-- /main-app -->

<!-- RIDE REQUEST POPUP -->
<div class="ride-request-overlay" id="ride-request-overlay">
  <div class="ride-request-card">
    <div class="request-progress"><div class="request-progress-fill" id="request-progress-fill" style="width:100%"></div></div>
    <div class="request-header">
      <div>
        <div class="request-title">🔔 Yangi buyurtma!</div>
        <div style="font-size:0.7rem;color:rgba(245,237,214,0.35);margin-top:0.2rem;">Sizdan 2.3 km uzoqlikda</div>
      </div>
      <div>
        <div class="request-timer" id="request-timer">15</div>
        <div class="request-timer-label">soniya</div>
      </div>
    </div>

    <div class="request-passenger">
      <div class="request-pax-avatar">👤</div>
      <div>
        <div class="request-pax-name">Dilnoza Yusupova</div>
        <div class="request-pax-rating">⭐ <span>4.8</span></div>
        <div class="request-pax-trips">47 ta sayohat</div>
      </div>
    </div>

    <div class="request-route">
      <div class="request-route-item">
        <span class="request-route-icon" style="color:var(--rust);">📍</span>
        <div>
          <div class="request-route-label">Olish joyi</div>
          <div class="request-route-place">Yunusobod, 19-mavze, 12-uy</div>
        </div>
      </div>
      <div class="request-route-line" style="margin-left:9px;"></div>
      <div class="request-route-item">
        <span class="request-route-icon" style="color:var(--teal);">🏁</span>
        <div>
          <div class="request-route-label">Manzil</div>
          <div class="request-route-place">Toshkent — Samarqand (shaharlararo)</div>
        </div>
      </div>
    </div>

    <div class="request-stats">
      <div class="request-stat">
        <div class="request-stat-val">340 km</div>
        <div class="request-stat-label">Masofa</div>
      </div>
      <div class="request-stat">
        <div class="request-stat-val">~3h 45m</div>
        <div class="request-stat-label">Vaqt</div>
      </div>
      <div class="request-stat">
        <div class="request-stat-val" style="color:var(--green-light);">153,000</div>
        <div class="request-stat-label">So'm</div>
      </div>
    </div>

    <div class="request-actions">
      <button class="request-accept" onclick="acceptRide()">✓ Qabul qilish</button>
      <button class="request-decline" onclick="declineRide()">✕ Rad etish</button>
    </div>
  </div>
</div>

<!-- MODALS -->
<div class="modal-overlay" id="modal-overlay">
  <div class="modal">
    <button class="modal-close" onclick="closeModal()">✕</button>
    <div class="modal-title" id="modal-title">Modal</div>
    <div id="modal-body"></div>
    <div class="modal-actions" id="modal-actions"></div>
  </div>
</div>

<!-- TOAST CONTAINER -->
<div id="toast-container"></div>

<!-- ============================================================
     JAVASCRIPT
     ============================================================ -->
<script>
/* ======================================================
   AUTH STARS
   ====================================================== */
(function(){
  const c=document.getElementById('auth-stars');
  for(let i=0;i<70;i++){
    const s=document.createElement('div');s.className='star';
    s.style.cssText=`left:${Math.random()*100}%;top:${Math.random()*100}%;--dur:${2+Math.random()*3}s;--delay:${Math.random()*3}s;`;
    if(Math.random()>0.75){s.style.width=s.style.height='3px';}
    c.appendChild(s);
  }
})();

/* ======================================================
   AUTH TABS
   ====================================================== */
function switchAuthTab(tab){
  document.querySelectorAll('.auth-tab').forEach(t=>t.classList.remove('active'));
  document.querySelectorAll('.auth-form-panel').forEach(p=>p.classList.remove('active'));
  const tabs={login:0,register:1,otp:2};
  document.querySelectorAll('.auth-tab')[tabs[tab]||0]?.classList.add('active');
  document.getElementById('panel-'+(tab==='otp'?'otp':tab))?.classList.add('active');
}

function showOtpPanel(){
  document.querySelectorAll('.auth-tab').forEach(t=>t.classList.remove('active'));
  document.querySelectorAll('.auth-form-panel').forEach(p=>p.classList.remove('active'));
  document.getElementById('panel-otp').classList.add('active');
}

function otpMove(el, idx){
  if(el.value.length===1 && idx<4){
    document.querySelectorAll('.otp-input')[idx].focus();
  }
}

/* ======================================================
   LOGIN / REGISTER
   ====================================================== */
function doLogin(){
  document.getElementById('login-error').classList.remove('visible');
  setTimeout(()=>{
    launchApp('Jasur Toshmatov');
  }, 400);
}

function doRegister(){
  const fn=document.getElementById('reg-fname').value.trim();
  const ln=document.getElementById('reg-lname').value.trim();
  const ph=document.getElementById('reg-phone').value.trim();
  const p1=document.getElementById('reg-pass').value;
  const p2=document.getElementById('reg-pass2').value;
  const err=document.getElementById('reg-error');

  if(!fn||!ln||!ph){err.textContent="Iltimos, barcha majburiy maydonlarni to'ldiring.";err.classList.add('visible');return;}
  if(p1.length<8){err.textContent="Parol kamida 8 belgidan iborat bo'lishi kerak.";err.classList.add('visible');return;}
  if(p1!==p2){err.textContent="Parollar mos kelmadi.";err.classList.add('visible');return;}
  err.classList.remove('visible');

  // Show OTP verification
  showOtpPanel();
  document.getElementById('otp-sent').style.display='block';

  // Store name for later
  window._regName = fn + ' ' + ln;
}

function launchApp(name){
  const screen=document.getElementById('auth-screen');
  screen.classList.add('exit');
  setTimeout(()=>{
    screen.classList.add('gone');
    const app=document.getElementById('main-app');
    app.classList.add('visible');
    // Set name
    const n=window._regName||name;
    document.getElementById('sidebar-driver-name').textContent=n;
    document.getElementById('avatar-initials').textContent=n.split(' ').map(w=>w[0]||'').join('').toUpperCase().slice(0,2);
    document.getElementById('profile-initials').textContent=n.split(' ').map(w=>w[0]||'').join('').toUpperCase().slice(0,2);
    document.getElementById('profile-full-name').textContent=n;
    document.getElementById('topbar-breadcrumb').textContent='Xush kelibsiz, '+n.split(' ')[0]+'!';
    initApp();
  },750);
}

/* ======================================================
   NAVIGATION
   ====================================================== */
const pages={
  dashboard:{title:'Dashboard',breadcrumb:'Bugungi holat'},
  trips:{title:'Sayohatlar',breadcrumb:'Tarix va statistika'},
  earnings:{title:'Daromad',breadcrumb:'Moliyaviy hisobot'},
  profile:{title:'Mening profilim',breadcrumb:'Shaxsiy ma\'lumotlar'},
  vehicle:{title:'Avtomobil',breadcrumb:'Texnik holat va hujjatlar'},
  notifications:{title:'Bildirishnomalar',breadcrumb:'Yangiliklar'},
  support:{title:'Yordam markazi',breadcrumb:'Murojaat va ko\'mak'},
};

function navigate(page){
  // Hide all pages
  document.querySelectorAll('.page').forEach(p=>p.classList.remove('active'));
  document.querySelectorAll('.nav-item').forEach(n=>n.classList.remove('active'));

  document.getElementById('page-'+page)?.classList.add('active');
  document.getElementById('nav-'+page)?.classList.add('active');

  const info=pages[page]||{};
  document.getElementById('topbar-title').textContent=info.title||page;
  document.getElementById('topbar-breadcrumb').textContent=info.breadcrumb||'';

  if(page==='notifications') markAllRead();
}

/* ======================================================
   ONLINE / OFFLINE TOGGLE
   ====================================================== */
let isOnline=true;

function toggleOnline(){
  isOnline=!isOnline;
  const badge=document.getElementById('online-badge');
  const label=document.getElementById('status-label');
  const btn=document.getElementById('online-toggle');
  const topBtn=document.getElementById('topbar-status-btn');
  const banner=document.getElementById('live-banner');
  const dot=document.getElementById('live-dot');
  const liveText=document.getElementById('live-text');

  if(isOnline){
    badge.classList.remove('offline');
    label.className='sidebar-profile-status';
    label.textContent='Onlayn';
    btn.className='sidebar-toggle-online';
    btn.innerHTML='ONLAYN &nbsp; ●';
    topBtn.textContent='● Onlayn';
    banner.className='live-banner';
    dot.className='live-dot';
    liveText.className='live-text';
    liveText.textContent='Faol — Buyurtmalar qabul qilinmoqda';
    showToast('🟢','Siz onlayn bo\'ldingiz','Buyurtmalar qabul qilinmoqda');
  } else {
    badge.classList.add('offline');
    label.className='sidebar-profile-status offline';
    label.textContent='Oflayn';
    btn.className='sidebar-toggle-online offline';
    btn.innerHTML='OFLAYN &nbsp; ○';
    topBtn.textContent='○ Oflayn';
    banner.className='live-banner offline-banner';
    dot.className='live-dot off';
    liveText.className='live-text off';
    liveText.textContent='Oflayn — Buyurtmalar qabul qilinmayapti';
    showToast('🔴','Siz oflayn bo\'ldingiz','Buyurtmalar to\'xtatildi');
  }
}

/* ======================================================
   TIMER
   ====================================================== */
let startTime=Date.now()-((2*3600+47*60+33)*1000);
function updateTimer(){
  const elapsed=Math.floor((Date.now()-startTime)/1000);
  const h=Math.floor(elapsed/3600);
  const m=Math.floor((elapsed%3600)/60);
  const s=elapsed%60;
  document.getElementById('live-timer').textContent=
    `${h}:${String(m).padStart(2,'0')}:${String(s).padStart(2,'0')}`;
}
setInterval(updateTimer,1000);

/* ======================================================
   RECENT TRIPS DATA
   ====================================================== */
const tripsData=[
  {id:'KRV-2025-04821',from:'Toshkent',to:'Samarqand',dist:340,dur:'3h 45m',fare:153000,method:'Naqd',rating:5,status:'completed',time:'Bugun, 11:32'},
  {id:'KRV-2025-04820',from:'Toshkent',to:'Jizzax',dist:200,dur:'2h 10m',fare:90000,method:'Click',rating:5,status:'completed',time:'Bugun, 08:55'},
  {id:'KRV-2025-04819',from:'Samarqand',to:'Toshkent',dist:340,dur:'3h 50m',fare:150000,method:'Naqd',rating:4,status:'completed',time:'Kecha, 18:20'},
  {id:'KRV-2025-04818',from:'Toshkent',to:'Namangan',dist:310,dur:'3h 30m',fare:139500,method:'Payme',rating:5,status:'completed',time:'Kecha, 14:10'},
  {id:'KRV-2025-04817',from:'Jizzax',to:'Toshkent',dist:200,dur:'2h 15m',fare:90000,method:'Naqd',rating:3,status:'completed',time:'Kecha, 10:45'},
  {id:'KRV-2025-04816',from:'Toshkent',to:'Buxoro',dist:560,dur:'6h 00m',fare:252000,method:'Naqd',rating:5,status:'completed',time:'2-kun oldin, 09:00'},
  {id:'KRV-2025-04815',from:'Buxoro',to:'Samarqand',dist:270,dur:'3h 00m',fare:121500,method:'Click',rating:4,status:'completed',time:'3-kun oldin, 16:00'},
  {id:'KRV-2025-04814',from:'Toshkent',to:'Andijon',dist:380,dur:'4h 15m',fare:171000,method:'Naqd',rating:5,status:'completed',time:'4-kun oldin, 07:30'},
  {id:'KRV-2025-04813',from:'Andijon',to:'Toshkent',dist:380,dur:'4h 30m',fare:175000,method:'Payme',rating:4,status:'cancelled',time:'4-kun oldin, 13:00'},
  {id:'KRV-2025-04812',from:'Toshkent',to:'Farg\'ona',dist:420,dur:'4h 45m',fare:189000,method:'Naqd',rating:5,status:'completed',time:'5-kun oldin, 08:15'},
  {id:'KRV-2025-04811',from:'Navoiy',to:'Samarqand',dist:160,dur:'1h 50m',fare:72000,method:'Click',rating:5,status:'completed',time:'6-kun oldin, 12:00'},
  {id:'KRV-2025-04810',from:'Toshkent',to:'Guliston',dist:160,dur:'1h 45m',fare:72000,method:'Naqd',rating:4,status:'completed',time:'7-kun oldin, 10:30'},
];

let currentFilter='all';
let currentPage=1;
const perPage=8;

function renderRecentTrips(){
  const container=document.getElementById('recent-trips-list');
  const recent=tripsData.slice(0,5);
  container.innerHTML=recent.map(t=>`
    <div class="trip-item">
      <div class="trip-status-dot ${t.status}"></div>
      <div class="trip-info">
        <div class="trip-route">${t.from} → ${t.to}</div>
        <div class="trip-meta"><span>${t.time}</span><span>${t.dist} km</span></div>
      </div>
      <div class="trip-fare">${t.fare.toLocaleString()} so'm</div>
      <div class="trip-rating"><span>⭐</span>${t.rating}</div>
    </div>
  `).join('');
}

function renderTripsTable(){
  let data=tripsData;
  if(currentFilter!=='all') data=data.filter(t=>t.status===currentFilter);
  
  const total=data.length;
  const start=(currentPage-1)*perPage;
  const paginated=data.slice(start,start+perPage);

  document.getElementById('trips-count-label').textContent=`${total} ta sayohat (${start+1}–${Math.min(start+perPage,total)})`;

  document.getElementById('trips-tbody').innerHTML=paginated.map(t=>`
    <tr>
      <td style="font-size:0.7rem;color:rgba(245,237,214,0.35);font-family:'Cinzel',serif;">${t.id}</td>
      <td style="color:var(--cream);font-weight:600;">${t.from} <span style="color:var(--gold)">→</span> ${t.to}</td>
      <td>${t.dist} km</td>
      <td>${t.time}</td>
      <td style="font-family:'Cinzel',serif;color:var(--gold);font-weight:700;">${t.fare.toLocaleString()}</td>
      <td>${t.method}</td>
      <td>${t.status==='completed'?'⭐'.repeat(t.rating):'—'}</td>
      <td><span class="status-badge ${t.status}">${t.status==='completed'?'Bajarildi':t.status==='cancelled'?'Bekor':'Faol'}</span></td>
    </tr>
  `).join('');

  // Pagination
  const totalPages=Math.ceil(total/perPage);
  const pag=document.getElementById('pagination');
  pag.innerHTML='';
  for(let i=1;i<=totalPages;i++){
    const btn=document.createElement('button');
    btn.className='filter-btn'+(i===currentPage?' active':'');
    btn.textContent=i;
    btn.onclick=()=>{currentPage=i;renderTripsTable();};
    pag.appendChild(btn);
  }
}

function filterTrips(type,btn){
  document.querySelectorAll('.trips-filters .filter-btn').forEach(b=>b.classList.remove('active'));
  btn.classList.add('active');
  currentFilter=type;
  currentPage=1;
  renderTripsTable();
}

/* ======================================================
   EARNINGS CHART
   ====================================================== */
const weekData={
  week:{
    values:[145000,98000,220000,310000,187000,0,0],
    labels:['Dush','Sesh','Chor','Pay','Jum','Shan','Yak'],
    today:4
  },
  month:{
    values:[1200000,980000,1450000,1620000,1100000,1380000,1240000,1875000],
    labels:['1','5','10','15','20','25','30','Bun'],
    today:7
  },
  year:{
    values:[3200000,2800000,3900000,4200000,3600000,4100000,4500000,4800000,3900000,4200000,3800000,4875000],
    labels:['Yan','Fev','Mar','Apr','May','Iyn','Iyl','Avg','Sen','Okt','Noy','Dek'],
    today:11
  }
};

function renderWeeklyBars(){
  const c=document.getElementById('weekly-bars');
  if(!c)return;
  const data=[145000,98000,220000,310000,187000,0,0];
  const max=Math.max(...data)||1;
  const days=['D','S','C','P','J','S','Y'];
  c.innerHTML=data.map((v,i)=>`
    <div class="bar ${i===4?'today':''}" style="height:${(v/max*100)}%">
      <span class="bar-label">${days[i]}</span>
    </div>
  `).join('');
}

let currentPeriod='week';
function setPeriod(p,btn){
  document.querySelectorAll('.period-btn').forEach(b=>b.classList.remove('active'));
  btn.classList.add('active');
  currentPeriod=p;
  renderBigChart();
}

function renderBigChart(){
  const d=weekData[currentPeriod];
  const max=Math.max(...d.values)||1;
  const c=document.getElementById('big-bar-chart');
  const xl=document.getElementById('chart-x-labels');
  if(!c)return;

  c.innerHTML=d.values.map((v,i)=>`
    <div class="big-bar ${i===d.today?'highlight':''}" style="height:${(v/max*100)}%">
      <div class="big-bar-tooltip">${v.toLocaleString()} so'm</div>
      <span class="big-bar-label">${d.labels[i]}</span>
    </div>
  `).join('');
  xl.innerHTML=d.labels.map(l=>`<span>${l}</span>`).join('');
}

/* ======================================================
   NOTIFICATIONS DATA
   ====================================================== */
const notifData=[
  {icon:'💰',title:'To\'lov qabul qilindi',body:'2,340,000 so\'m hisobingizga o\'tkazildi. Click kartasiga.',time:'5 daqiqa oldin',unread:true},
  {icon:'⭐',title:'Yangi baho oldingiz',body:'Dilnoza Yusupova sizga 5 yulduz berdi va izoh qoldirdi: "Juda yaxshi haydovchi!"',time:'1 soat oldin',unread:true},
  {icon:'🔔',title:'Bonus zonasi faollashdi',body:'Toshkent-Samarqand yo\'nalishida hozir +20% bonus taklif! 18:00 gacha amal qiladi.',time:'2 soat oldin',unread:true},
  {icon:'⚠️',title:'Hujjat muddati tugayapti',body:'Texnik ko\'rik hujjatingiz muddati tugagan. Iltimos, yangilab, portalga yuklang.',time:'Kecha',unread:false},
  {icon:'🚗',title:'Avtomobil tekshiruvi eslatmasi',body:'Oylik texnik tekshiruvni o\'tkazishni unutmang. Xavfsizlik birinchi!',time:'2 kun oldin',unread:false},
  {icon:'📱',title:'Kervan ilovasi yangilandi',body:'Yangi versiyada tezkor navigatsiya va yaxshilangan yo\'l topish tizimi mavjud.',time:'3 kun oldin',unread:false},
  {icon:'🏆',title:'Oy yakuni — Top 10!',body:'Siz aprel oyida eng yaxshi 10 haydovchi orasiga kirdingiz! Maxsus bonus tayyor.',time:'7 kun oldin',unread:false},
];

function renderNotifications(){
  const c=document.getElementById('notif-list');
  c.innerHTML=notifData.map((n,i)=>`
    <div class="notif-item ${n.unread?'unread':''}" onclick="readNotif(${i})">
      <div class="notif-icon">${n.icon}</div>
      <div class="notif-text">
        <div class="notif-title">${n.title}</div>
        <div class="notif-body">${n.body}</div>
      </div>
      <div class="notif-time">${n.time}</div>
    </div>
  `).join('');
}

function readNotif(i){
  notifData[i].unread=false;
  renderNotifications();
  updateNotifBadge();
}

function markAllRead(){
  notifData.forEach(n=>n.unread=false);
  renderNotifications();
  updateNotifBadge();
}

function updateNotifBadge(){
  const count=notifData.filter(n=>n.unread).length;
  const badge=document.getElementById('notif-count');
  badge.textContent=count;
  badge.style.display=count>0?'inline-block':'none';
}

/* ======================================================
   FAQ DATA
   ====================================================== */
const faqData=[
  {q:"Daromadim qachon to'lanadi?",a:"Har hafta dushanba kuni avtomatik ravishda tanlangan to'lov usulingizga o'tkaziladi. Minimal yechib olish: 50,000 so'm."},
  {q:"Buyurtmani rad etish reytingga ta'sir qiladimi?",a:"Qabul qilish darajasi 85% dan yuqori bo'lsa, reyting ta'sirlanmaydi. Lekin tez-tez rad etish bonus imkoniyatlarini kamaytirishi mumkin."},
  {q:"Kervan komissiyasi qancha?",a:"Standart komissiya — 12%. Top-haydovchilar (reyting 4.8+) uchun bu 10% ga tushadi."},
  {q:"Passajir yomon baho qo'ysa nima qilaman?",a:"3 ish kuni ichida ilovadan yoki support@kervan.uz orqali murojaat qilishingiz mumkin. Asosli shikoyatlar ko'rib chiqiladi."},
  {q:"Yangi shaharlar qo'shish mumkinmi?",a:"Ha, profil sozlamalarida xizmat ko'rsatadigan viloyatlarni o'zgartirishingiz mumkin. O'zgarish 24 soat ichida kuchga kiradi."},
];

function renderFAQ(){
  const c=document.getElementById('faq-list');
  c.innerHTML=faqData.map((f,i)=>`
    <div class="faq-item" id="faq-${i}">
      <div class="faq-q" onclick="toggleFAQ(${i})">
        ${f.q}
        <span class="faq-q-arrow">▼</span>
      </div>
      <div class="faq-a">${f.a}</div>
    </div>
  `).join('');
}

function toggleFAQ(i){
  const item=document.getElementById('faq-'+i);
  item.classList.toggle('open');
}

/* ======================================================
   RIDE REQUEST
   ====================================================== */
let requestTimer=null;
let requestSeconds=15;

function showRideRequest(){
  if(!isOnline){showToast('🔴','Oflayn rejim','Buyurtma qabul qilish uchun onlayn bo\'ling');return;}
  requestSeconds=15;
  document.getElementById('request-timer').textContent=requestSeconds;
  document.getElementById('request-progress-fill').style.width='100%';
  document.getElementById('ride-request-overlay').classList.add('visible');
  
  requestTimer=setInterval(()=>{
    requestSeconds--;
    document.getElementById('request-timer').textContent=requestSeconds;
    document.getElementById('request-progress-fill').style.width=(requestSeconds/15*100)+'%';
    if(requestSeconds<=0){
      clearInterval(requestTimer);
      closeRideRequest();
      showToast('⏰','Vaqt tugadi','Buyurtma boshqa haydovchiga o\'tdi');
    }
  },1000);
}

function closeRideRequest(){
  clearInterval(requestTimer);
  document.getElementById('ride-request-overlay').classList.remove('visible');
}

function acceptRide(){
  closeRideRequest();
  document.getElementById('active-trip-badge').style.display='inline-block';
  showToast('✅','Buyurtma qabul qilindi!','Dilnoza Yusupova — Toshkent → Samarqand. Navigatsiya boshlanmoqda...');
}

function declineRide(){
  closeRideRequest();
  showToast('❌','Buyurtma rad etildi','Keyingi buyurtmani kutmoqda...');
}

/* ======================================================
   MODALS
   ====================================================== */
function openModal(title,body,actions){
  document.getElementById('modal-title').textContent=title;
  document.getElementById('modal-body').innerHTML=body;
  document.getElementById('modal-actions').innerHTML=actions;
  document.getElementById('modal-overlay').classList.add('visible');
}

function closeModal(){
  document.getElementById('modal-overlay').classList.remove('visible');
}
document.getElementById('modal-overlay').addEventListener('click',e=>{
  if(e.target===document.getElementById('modal-overlay')) closeModal();
});

function showLogoutModal(){
  openModal(
    'Chiqish',
    '<p style="font-size:0.9rem;color:rgba(245,237,214,0.6);line-height:1.7;">Haqiqatan ham tizimdan chiqmoqchimisiz? Aktiv sessiya tugaydi.</p>',
    `<button class="modal-btn modal-btn-secondary" onclick="closeModal()">Bekor qilish</button>
     <button class="modal-btn modal-btn-primary" onclick="doLogout()">Chiqish</button>`
  );
}

function doLogout(){
  closeModal();
  const app=document.getElementById('main-app');
  const auth=document.getElementById('auth-screen');
  app.classList.remove('visible');
  app.style.display='none';
  auth.classList.remove('exit','gone');
  auth.style.display='flex';
  // Reset form
  document.getElementById('panel-login').classList.add('active');
  document.getElementById('panel-register').classList.remove('active');
  document.getElementById('panel-otp').classList.remove('active');
  document.querySelectorAll('.auth-tab').forEach((t,i)=>t.classList.toggle('active',i===0));
}

function showWithdrawModal(){
  openModal(
    'Pul yechish',
    `<div style="display:flex;flex-direction:column;gap:1rem;">
      <div class="field-group">
        <div class="field-label">Yechib olinadigan miqdor (so'm)</div>
        <input class="field-input" id="withdraw-amount" placeholder="100,000" value="2,340,000">
      </div>
      <div style="font-size:0.78rem;color:rgba(245,237,214,0.4);">Tanlangan usul: Click · Karta: *3421</div>
      <div style="font-size:0.75rem;color:rgba(245,237,214,0.3);">Minimal: 50,000 so'm · Komissiya: bepul</div>
    </div>`,
    `<button class="modal-btn modal-btn-secondary" onclick="closeModal()">Bekor qilish</button>
     <button class="modal-btn modal-btn-primary" onclick="closeModal();showToast('💰','So\'rov yuborildi','1-3 ish kunida hisobingizga o\'tkaziladi')">Tasdiqlash</button>`
  );
}

function showChangePassModal(){
  openModal(
    'Parolni o\'zgartirish',
    `<div style="display:flex;flex-direction:column;gap:1rem;">
      <div class="field-group">
        <div class="field-label">Joriy parol</div>
        <input class="field-input" type="password" placeholder="••••••••">
      </div>
      <div class="field-group">
        <div class="field-label">Yangi parol</div>
        <input class="field-input" type="password" placeholder="Kamida 8 belgi">
      </div>
      <div class="field-group">
        <div class="field-label">Yangi parolni tasdiqlang</div>
        <input class="field-input" type="password" placeholder="••••••••">
      </div>
    </div>`,
    `<button class="modal-btn modal-btn-secondary" onclick="closeModal()">Bekor qilish</button>
     <button class="modal-btn modal-btn-primary" onclick="closeModal();showToast('🔐','Parol o\'zgartirildi','Keyingi kirishda yangi parol ishlaydi')">Saqlash</button>`
  );
}

/* ======================================================
   PROFILE SAVE
   ====================================================== */
function saveProfile(){
  const fn=document.getElementById('pf-fname').value;
  const ln=document.getElementById('pf-lname').value;
  const name=fn+' '+ln;
  document.getElementById('sidebar-driver-name').textContent=name;
  document.getElementById('profile-full-name').textContent=name;
  const initials=name.split(' ').map(w=>w[0]||'').join('').toUpperCase().slice(0,2);
  document.getElementById('avatar-initials').textContent=initials;
  document.getElementById('profile-initials').textContent=initials;
  showToast('✅','Profil yangilandi','Ma\'lumotlaringiz muvaffaqiyatli saqlandi');
}

/* ======================================================
   DOCUMENT UPLOAD
   ====================================================== */
function uploadDoc(card,type){
  // Simulate upload
  const input=document.createElement('input');
  input.type='file';
  input.accept='image/*,.pdf';
  input.onchange=()=>{
    card.classList.add('uploaded');
    card.innerHTML=`
      <div class="doc-check">✅</div>
      <div class="doc-icon">${card.querySelector('.doc-icon').textContent}</div>
      <div class="doc-name">${card.querySelector('.doc-name').textContent}</div>
      <div class="doc-status ok">Ko'rib chiqilmoqda</div>
      <div class="doc-expiry">Yuborildi: Bugun</div>
    `;
    showToast('📄','Hujjat yuklandi','Ko\'rib chiqish 1-2 ish kuni davom etadi');
  };
  input.click();
}

/* ======================================================
   PAYMENT METHOD SELECT
   ====================================================== */
function selectPayMethod(el){
  document.querySelectorAll('.pay-method').forEach(m=>m.classList.remove('selected'));
  el.classList.add('selected');
}

/* ======================================================
   TOAST
   ====================================================== */
function showToast(icon,title,msg){
  const c=document.getElementById('toast-container');
  const t=document.createElement('div');
  t.className='toast';
  t.innerHTML=`<div class="toast-icon">${icon}</div><div class="toast-text"><div class="toast-title">${title}</div><div class="toast-msg">${msg}</div></div>`;
  c.appendChild(t);
  setTimeout(()=>{
    t.classList.add('exit');
    setTimeout(()=>t.remove(),350);
  },3500);
}

/* ======================================================
   INIT
   ====================================================== */
function initApp(){
  renderRecentTrips();
  renderTripsTable();
  renderBigChart();
  renderWeeklyBars();
  renderNotifications();
  renderFAQ();
  updateNotifBadge();
  // Set today's date in filter
  document.getElementById('trips-date-filter').value=new Date().toISOString().split('T')[0];
  // Navigate to dashboard
  navigate('dashboard');
  // Welcome toast
  setTimeout(()=>{
    showToast('👋','Xush kelibsiz!','Bugun 8 ta sayohat bajarildi. Daromad: 187,500 so\'m');
  },1000);
  // Simulate incoming ride request after 5 seconds
  setTimeout(()=>{
    if(isOnline) showRideRequest();
  },5000);
}

// Keyboard shortcuts
document.addEventListener('keydown',e=>{
  if(e.key==='Escape'){closeRideRequest();closeModal();}
});
</script>
</body>
</html>
