# Breek het web

## Opdracht

Je hebt 2 ‘enhancements’ onderzocht door features van het platform bewust uit te zetten.

Het doel is je te laten beseffen hoeveel je nog niet weet van het Web, erachter komen dat je misschien aannames hebt die niet kloppen, en om je je in te laten leven in de eindgebruiker.

### Custom web fonts

#### Welke problemen kunnen de features veroorzaken (benoem cijfers, meningen, bronnen)

At first glance, web fonts do not appear to cause much problems for users. I tried several search queries (among others 'problems website fonts', 'web fonts issues', 'web fonts problems'). However, they are liable to do so. Web fonts potentially slow down page load speed and performance.

More problematic is the way browsers handle custom webfonts. Most browsers today (this is slowly changing for the better), make text invisible until the custom font is loaded, with a maximum wait time of usually 3 seconds. Some browsers however, wait *forever*. In these browsers, your pretty custom font is a single point of failure.

In addition to these problems, if a web font does work normally, it causes the perceived page load speed to lower considerably because of FOIT (Flash of Invisible Text), depending on browser. The content is invisible until the font loads, after which the entire layout might shift if the x-height of the font differs from that of the fallback.

#### Hoe je de features hebt kunnen testen (met een tool, met een plugin, met plakband?)

- Font CDN's blokkeren
- Network throttling

#### Een aantal sites zien waar je de problemen bent tegengekomen

- bramstein.com (FOIT)
- nytimes.com (FOUT, wel async loading!)
- washingtonpost.com (fonts als base64 in CSS, niet blokkeerbaar zonder CSS te blokkeren)
- volkskrant.nl (heeft een back-up CDN)

#### Hoe je dit probleem op de betreffende site kan verbeteren (PE)

- [https://www.filamentgroup.com/lab/font-loading.html](https://www.filamentgroup.com/lab/font-loading.html)
- [https://www.smashingmagazine.com/2016/02/preload-what-is-it-good-for/](https://www.smashingmagazine.com/2016/02/preload-what-is-it-good-for/)
- [http://www.bramstein.com/writing/web-font-loading-patterns.html](http://www.bramstein.com/writing/web-font-loading-patterns.html)
- [https://css-tricks.com/preventing-the-performance-hit-from-custom-fonts/](https://css-tricks.com/preventing-the-performance-hit-from-custom-fonts/)

#### Bronnen

- https://www.filamentgroup.com/lab/font-loading.html

### LocalStorage

Most browsers treat LocalStorage as a type of cookies that are never sent back to the server. Websites can use it to remember data, content and settings on a users computer.

Opera Mini is the only current browser that does not support LocalStorage. In Firefox it's possible to disable LocalStorage through a flag (dom.storage.enabled). In other browsers it's only possible to complete disable cookies as well as LocalStorage and SessionStorage.

As they serve a slightly different purpose, I opted to only test websites in Firefox, where I can granularly disable LocalStorage.

#### Welke problemen kunnen de features veroorzaken (benoem cijfers, meningen, bronnen)

Cijfers, meningen en bronnen hierover zijn niet te vinden. Dit zijn de dingen waarvan _ik_ denk dat ze een effect kunnen zijn van het uitzetten van LocalStorage.

User settings may not be remembered until the next session. If sites only try to store user settings in LocalStorage and not in cookies, the user might loose their settings (like UI state) every time.

Usually things like being logged in are not stored in LocalStorage because the data in it is not sent to the server. This means the server has no way of checking if the user is logged in. Cookies are used for this purpose.

Content and/or website data might not be able to be cached if LocalStorage is used for that purpose. This might lead to higher data usage on a users' device.

#### Hoe je de features hebt kunnen testen (met een tool, met een plugin, met plakband?)

Door in Firefox de flag `dom.storage.enabled` op `false` te zetten.

#### Een aantal sites zien waar je de problemen bent tegengekomen

Geen sites waarop ik user facing problemen heb kunnen ontdekken. Enkele sites laten wel `console` errors zien, maar functioneren prima.

- Safari (private mode)

#### Hoe je dit probleem op de betreffende site kan verbeteren (PE)

Detect support for LocalStorage first. Then see if it has been disabled. If so, use cookies. If not, use LocalStorage. The most important thing would be not to make a session or LocalStorage required at all for using your website, just use it as a way to store small amounts of data or user preferences that are not vital to the functioning of your application. These should be stored on the server.
