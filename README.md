# gusdiraj.github.io
<!DOCTYPE html>
<html lang="id" manifest="../cache.appcache">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width,initial-scale=1">
<title>Johan Game</title>
<style>
  :root {
    --bg: #180d2b;
    --panel: rgba(25, 12, 48, .82);
    --line: rgba(255, 105, 180, .45);
    --pink: #ff55a8;
    --orange: #ff9b42;
    --gold: #ffe08a;
    --text: #f4dff0;
  }

  * { box-sizing: border-box; }

  html, body {
    margin: 0;
    min-height: 100%;
    background:
      linear-gradient(to bottom, rgba(255, 88, 168, .15), transparent 40%),
      radial-gradient(circle at 50% 18%, #ff8a61 0, #e94c91 18%, #632c83 46%, #180d2b 78%);
    color: var(--text);
    font: 15px/1.45 "Segoe UI", system-ui, sans-serif;
    overflow-x: hidden;
  }

  body {
    position: relative;
  }

  body::before {
    content: "";
    position: fixed;
    inset: 42% -20% -12%;
    z-index: -2;
    pointer-events: none;
    opacity: .48;
    background-image:
      linear-gradient(rgba(255, 180, 224, .35) 1px, transparent 1px),
      linear-gradient(90deg, rgba(255, 180, 224, .35) 1px, transparent 1px);
    background-size: 42px 28px;
    transform: perspective(420px) rotateX(60deg) scale(1.45);
    transform-origin: center bottom;
  }

  body::after {
    content: "";
    position: fixed;
    width: 420px;
    height: 420px;
    left: 50%;
    top: 23%;
    z-index: -1;
    pointer-events: none;
    border-radius: 50%;
    background:
      radial-gradient(circle, rgba(255, 205, 104, .95) 0 8%, rgba(255, 120, 160, .5) 25%, transparent 68%);
    filter: blur(18px);
    transform: translate(-50%, -50%);
    opacity: .72;
  }

  @keyframes sunsetGlow {
    0%, 100% { opacity: .55; transform: translate(-50%, -50%) scale(.94); }
    50% { opacity: .9; transform: translate(-50%, -50%) scale(1.08); }
  }

  #brand {
    position: relative;
    text-align: center;
    padding: 16px 10px;
    background: rgba(24, 8, 40, .76);
    border-bottom: 1px solid var(--line);
    box-shadow: 0 4px 28px rgba(255, 55, 150, .3);
    backdrop-filter: blur(10px);
  }

  #brand::after {
    content: "";
    position: absolute;
    left: 15%;
    right: 15%;
    bottom: -1px;
    height: 3px;
    background: linear-gradient(90deg, transparent, var(--orange), var(--pink), transparent);
    box-shadow: 0 0 15px var(--pink);
  }

  #brand svg {
    width: 48px;
    height: 32px;
    margin-right: 8px;
    vertical-align: middle;
    filter: drop-shadow(0 0 8px rgba(255, 187, 93, .75));
  }

  #brand b {
    font-family: Georgia, "Times New Roman", serif;
    font-size: 16px;
    font-weight: 600;
    letter-spacing: .22em;
    color: var(--gold);
    text-shadow:
      0 1px 8px rgba(255, 180, 92, .75),
      2px 2px 0 rgba(255, 70, 150, .35);
    vertical-align: middle;
  }

  #wrap {
    max-width: 680px;
    margin: 34px auto;
    padding: 24px 22px;
    background: var(--panel);
    border: 1px solid var(--line);
    border-radius: 14px;
    box-shadow:
      0 0 0 1px rgba(255,255,255,.04) inset,
      0 14px 45px rgba(25, 0, 35, .6),
      0 0 30px rgba(255, 70, 160, .2);
    backdrop-filter: blur(12px);
  }

  #state {
    font-size: 22px;
    font-weight: 700;
    margin: 0 0 8px;
    text-shadow: 0 0 14px currentColor;
  }

  #det {
    font-size: 13px;
    color: #d4aeca;
    margin: 0 0 6px;
  }

  #det b {
    color: #ffe8f6;
    font-weight: 600;
  }

  #cache {
    font-size: 12px;
    color: #b487ad;
    margin: 0;
    min-height: 14px;
  }

  #cache.cacheok { color: #9df0be; }

  .ok { color: #9df0be; }
  .bad { color: #ff817e; }
  .warn { color: #ffd76a; }

  @media (max-width: 600px) {
    #wrap {
      margin: 22px 12px;
      padding: 20px 16px;
    }

    #state {
      font-size: 19px;
    }

    #brand b {
      font-size: 14px;
    }
  }
</style>
</head>
<body>
<div id="brand">
  <svg viewBox="0 0 120 80" role="img" aria-label="Stik PS4">
    <path
      d="M30 22C19 22 13 31 10 48L7 63C6 69 12 73 17 70L35 59H85L103 70C108 73 114 69 113 63L110 48C107 31 101 22 90 22L75 29H45Z"
      fill="#d8b45c"
      stroke="#f0d98a"
      stroke-width="3"
      stroke-linejoin="round"/>
    <path d="M31 39H45M38 32V46" stroke="#0b0d10" stroke-width="4" stroke-linecap="round"/>
    <circle cx="80" cy="37" r="4" fill="#0b0d10"/>
    <circle cx="92" cy="45" r="4" fill="#0b0d10"/>
    <circle cx="60" cy="39" r="5" fill="#0b0d10"/>
    <circle cx="67" cy="39" r="5" fill="#0b0d10"/>
  </svg>
  <b>JOHAN GAME</b>
</div>

<div id="wrap">
  <div id="state">mendeteksi firmware...</div>
  <div id="det"></div>
  <div id="cache"></div>
</div>

<script>
(function () {
    var stateEl = document.getElementById("state");
    var detEl = document.getElementById("det");
    var cacheEl = document.getElementById("cache");

    function setState(t, c) {
        stateEl.textContent = t;
        stateEl.className = c || "";
    }

    function setCache(t, ok) {
        cacheEl.textContent = t;
        cacheEl.className = ok ? "cacheok" : "";
    }

    var params = new URLSearchParams(location.search);
    var key = null, fwnum = null;
    var m = /PlayStation\s+4[\/ ](\d+)\.(\d+)/.exec(navigator.userAgent);

    if (m) {
        var minor = parseInt(m[2], 16);
        var ms = minor.toString(16);
        if (ms.length < 2) ms = "0" + ms;
        key = m[1] + "." + ms;
        fwnum = parseInt(m[1], 10) * 100 + parseInt(ms, 10);
    }

    var bug = null, why = "";
    var forced = params.get("bug");

    if (forced === "lapse" || forced === "poops") {
        bug = forced;
        why = "dipaksa melalui ?bug=" + forced;
    } else if (!key) {
        why = "perangkat bukan PlayStation 4";
    } else if (fwnum <= 1202) {
        bug = "lapse";
        why = key + " <= 12.02";
    } else if (fwnum >= 1250) {
        bug = "poops";
        why = key + " >= 12.50";
    } else {
        why = key + " -- tidak ada bug yang berfungsi (lapse berakhir di 12.02, poops memerlukan 12.50)";
    }

    detEl.innerHTML = "firmware <b>" + (key || "tidak diketahui") + "</b>"
        + " &middot; bug <b>" + (bug || "tidak ada") + "</b> &middot; " + why;

    if (!bug) {
        setState("FIRMWARE TIDAK DIDUKUNG", "bad");
        return;
    }

    var target = (bug === "lapse" ? "../run_lapse.html" : "../run_poops.html")
        + location.search;

    function go() {
        setState("memuat rangkaian " + bug + "...", "warn");
        location.replace(target);
    }

    function waitForTap(msg, action) {
        setState(msg, "warn");

        function fire() {
            window.removeEventListener("click", fire);
            window.removeEventListener("keydown", fire);
            action();
        }

        window.addEventListener("click", fire);
        window.addEventListener("keydown", fire);
    }

    var ac = window.applicationCache;

    if (!ac || !document.documentElement.hasAttribute("manifest")) {
        go();
    } else if (!navigator.onLine) {
        setCache("offline -- menggunakan cache", true);
        go();
    } else if (ac.status === ac.IDLE) {
        setCache("tersimpan di cache", true);
        go();
    } else if (ac.status === ac.UPDATEREADY) {
        try { ac.swapCache(); } catch (e) {}
        setCache("pembaruan selesai diunduh", true);
        waitForTap(
            "PEMBARUAN SIAP -- tekan X atau ketuk untuk memuat ulang dan menjalankan",
            function () { location.reload(); }
        );
    } else {
        setCache("memeriksa cache...");

        ac.addEventListener("downloading", function () {
            setCache("menyimpan cache untuk penggunaan offline...");
        }, false);

        ac.addEventListener("progress", function (e) {
            if (e && e.total) {
                setCache("menyimpan cache " + Math.round((e.loaded / e.total) * 100) + "%");
            }
        }, false);

        ac.addEventListener("cached", function () {
            setCache("tersimpan untuk penggunaan offline", true);
            waitForTap(
                "TERSIMPAN (pertama kali) -- tekan X atau ketuk untuk menjalankan",
                go
            );
        }, false);

        ac.addEventListener("updateready", function () {
            try { ac.swapCache(); } catch (e) {}
            setCache("pembaruan selesai diunduh", true);
            waitForTap(
                "PEMBARUAN SIAP -- tekan X atau ketuk untuk memuat ulang dan menjalankan",
                function () { location.reload(); }
            );
        }, false);

        ac.addEventListener("noupdate", function () {
            setCache("cache siap digunakan secara offline", true);
            go();
        }, false);

        ac.addEventListener("error", function () {
            setCache("cache tidak tersedia");
            waitForTap(
                "CACHE GAGAL -- tekan X atau ketuk untuk tetap menjalankan",
                go
            );
        }, false);

        ac.addEventListener("obsolete", function () {
            setCache("cache sudah tidak berlaku");
            waitForTap(
                "CACHE KEDALUWARSA -- tekan X atau ketuk untuk menjalankan",
                go
            );
        }, false);
    }
})();
</script>
</body>
</html>
