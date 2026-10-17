# HeliumOS

Extensie de browser (Manifest V3) care transformă **tab-ul nou** într-un mic sistem de operare cu temă **Aero (Windows 7)**: desktop, ferestre, sistem de fișiere, terminal, editor de cod, vizualizator de imagini, player audio/video și multe altele.

Construită pentru [Helium](https://github.com/imputnet/helium), dar funcționează în orice browser bazat pe Chromium (Chrome, Edge, Brave etc.). Fără framework-uri, fără dependențe: doar HTML, CSS și JavaScript.

> **Stadiu:** experimental (v0.7). Nu este un OS real: rulează în sandbox-ul browserului.

<!-- Adaugă aici capturi de ecran: docs/screenshot-desktop.png, docs/screenshot-files.png -->

## Funcții

**Desktop și ferestre**
- Ferestre care se mută, se redimensionează, se minimizează, se maximizează și se închid, cu animații
- Aero Snap: trage o fereastră la marginea stângă/dreaptă (jumătate de ecran) sau sus (maximizat)
- Bară de activități cu butoane grupate pe aplicație, aplicații fixate, previzualizare la hover, calendar și „Arată desktopul”
- Meniu Start în două coloane, cu căutare de programe și fișiere
- Pictograme mutabile pe desktop: trage-le pe o grilă, selectează mai multe cu dreptunghi sau Ctrl+click, trage fișiere de pe desktop peste Coș sau peste un folder
- Animație de pornire, ecran de login cu parolă (opțional) și ecran de blocare
- Desktopuri virtuale (până la 6) cu Vizualizare activități (butonul ▦ de lângă Start)
- Notificări: baloane pe desktop, centru de notificări (🔔) cu istoric, „Nu deranja”, sunet opțional și mementouri
- Meniuri contextuale (click dreapta) pe desktop, în Fișiere și în bara de activități

**Aplicații incluse**

| Aplicație | Ce face |
|---|---|
| Fișiere | Explorer cu panou de navigare, înapoi/înainte, căutare, coloane sortabile, drag & drop, import/export |
| Notepad | Editor de text simplu (Ctrl+S) |
| Cod | Editor cu evidențierea sintaxei (JS, CSS, HTML, Python), rulare `.js` și previzualizare `.html` într-un sandbox izolat |
| Terminal | `ls`, `cd`, `cat`, `echo > fișier`, `mkdir`, `rm`, `mv`, `cp`, `open`, `edit` și altele (`help`) |
| Imagini | Navigare între imagini, zoom, „Setează ca fundal”, „Editează în Paint” |
| Paint | Creion, pensulă, radieră, spray, linie, dreptunghi, elipsă, umplere, pipetă, text; anulare/refacere, palete de culori, salvare PNG/JPG în `/Pictures` |
| Solitaire | Klondike cu extragere 1 sau 3, tragere cu mouse-ul, dublu-click spre fundații, anulare, auto-completare, punctaj, cronometru și statistici |
| Coș de reciclare | Ștergerile se mută aici; restaurare în locul inițial sau ștergere definitivă |
| Manager de activități | Lista ferestrelor (cu „Închide fereastra”) și grafic de performanță |
| Muzică | Audio și video cu playlist și vizualizator de spectru |
| Browser | Pagini web în iframe |
| Calculator | Calculator simplu |
| Setări | Temă, fundal, pictograme, cont și parolă, notificări, copii de siguranță |

**Personalizare**
- Fundal propriu (umple, potrivește, întinde, mozaic, centrat) sau din imaginile tale
- Pictograme proprii pentru fiecare aplicație (imagine sau emoji), pe desktop și în bara de activități
- Culoarea ferestrelor, transparență (sticlă Aero) sau tema „Plat”, nume și poză de utilizator
- Parolă de utilizator (hash PBKDF2, doar acces obișnuit, nu criptare) și animație de pornire configurabilă

## Desktopuri virtuale

- Butonul ▦ din bara de activități (sau Alt+Shift+↑) deschide **Vizualizare activități**: miniaturi ale desktopurilor, ferestrele desktopului curent și „+ Desktop nou” (maxim 6).
- Click pe un desktop îl activează. Dublu-click pe nume îl redenumește, iar ✕ îl elimină (ferestrele lui trec pe desktopul vecin).
- Muți o ferestră trăgând-o peste un desktop sau din click dreapta pe bara ei de titlu.
- Taste: **Alt+Shift+←/→** sau **Ctrl+Alt+←/→** trec între desktopuri, iar **1–6** schimbă desktopul cât timp Vizualizarea e deschisă. Unele sisteme rezervă aceste combinații.
- Terminal: `desk` (listă), `desk 2` (schimbă), `desk new` (creează).
- Pictogramele de pe desktop sunt comune tuturor desktopurilor, iar bara de activități arată ferestrele desktopului curent.

## Notificări

- Se afișează ca baloane în dreapta jos și se păstrează în centrul de notificări (🔔 din bara de activități).
- Evenimente care generează notificări: importuri, mutare în Coș (cu buton „Anulează”), imagini salvate din Paint, schimbarea parolei, avertisment de spațiu și memento de copie de siguranță.
- **Mementouri:** din panoul 🔔 sau din Terminal: `remind 10m scoate pâinea`. Sună cât timp HeliumOS e deschis într-un tab; cele ratate apar la următoarea deschidere.
- **Din cod:** în aplicația Cod, un fișier `.js` poate apela `notify("Titlu", "Mesaj")`.
- **Notificări de sistem** (opțional): în Setări → Notificări, cu permisiunea opțională „notificări”, apar și când tab-ul nu e în față.

## Instalare

1. Descarcă sau clonează repository-ul.
2. Deschide pagina de extensii (`helium://extensions` sau `chrome://extensions`).
3. Activează **Developer mode**.
4. Apasă **Load unpacked** și alege folderul proiectului.
5. Deschide un tab nou.

## Cum se folosesc fișierele

- Fișierele sunt stocate local, în browser (**IndexedDB**). Nu se trimite nimic pe internet.
- Poți trage fișiere din calculator pe desktop (ajung în `/Desktop`) sau într-un folder din aplicația Fișiere.
- Limita este de 20 MB per fișier. Fișierele mari fac salvările mai lente.
- Dacă ștergi datele browserului, fișierele dispar. Folosește **Setări → Sistem → Exportă** pentru copii de siguranță.

## Permisiuni

| Permisiune | Motiv |
|---|---|
| `chrome_url_overrides.newtab` | Înlocuiește pagina de tab nou |
| `storage` | Reține per sesiune de browser dacă te-ai autentificat și dacă animația de pornire a rulat |
| `notifications` (opțională) | Cerută doar dacă activezi notificările de sistem |
| `declarativeNetRequest` + `<all_urls>` | Elimină antetele `X-Frame-Options` / `Content-Security-Policy` **doar** pentru cadrele inițiate de extensie, ca aplicația Browser să poată afișa site-uri în iframe |
| Pagina `sandbox.html` | Rulează cod și previzualizări HTML într-o origine izolată, fără acces la extensie |

Regula de rețea se aplică numai ferestrelor din HeliumOS, nu restului navigării.

## Structura proiectului

```
manifest.json     configurare extensie (MV3)
newtab.html       pagina desktopului
os.css            stiluri (tema Plat + tema Aero)
os.js             sistemul: ferestre, fișiere, aplicații, Start
background.js     regula de rețea pentru iframe-uri
sandbox.html/js   mediu izolat pentru „Rulează” în editorul de cod
```

## Limitări cunoscute

- Unele site-uri pot refuza să se încarce în aplicația Browser chiar și cu regula de mai sus.
- Nu are procese reale, rețea sau acces la fișierele calculatorului în afara importului/exportului.
- Parola nu criptează fișierele: cine are acces la profilul browserului o poate ocoli. Dacă o uiți, singura cale e resetarea.

## Idei pentru viitor

- Minesweeper și Solitaire
- Minesweeper
- Fundal diferit pe fiecare desktop virtual
- Tapet personalizat pe ecranul de blocare

## Contribuții

Sunt binevenite probleme (issues) și pull request-uri. Codul este scris în JavaScript simplu, fără pas de build, deci poți edita fișierele direct și apăsa „Reload” pe extensie.

## Licență

Alege o licență (de exemplu MIT) și adaug-o într-un fișier `LICENSE`.
