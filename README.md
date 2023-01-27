# cast-radio-kanaler
Cast radiokanaler til højttalere/Cast radio channels to speakers

<img src="https://user-images.githubusercontent.com/103023823/211158774-eb0bd03a-c189-4688-a58b-732c480302dd.png" width="300" height="300">
Jeg har en del Google Nest minis, JBL Flip og Samsung TV, som jeg gerne vil bruge til afspilning af musik (JBL Flip og Samsung TV skal være tændt på forhånd).
Jeg prøvede at bruge integration: Radio Browser, som gemmer alverdens radiokanaler under medier. 
Den anvender formatet MPEG, som normalt bruges til digital video og lyd, altså video/film.
Derfor begyndte jeg at kigge efter MP3 og streams, og fandt denne hjemmeside, Danish Radio Streams: https://www.astra2sat.com/danish-radio-streams/. 
Der er en fejl for DR P1, men den reference findes på DRs egen hjemmeside: https://dr.custhelp.com/app/answers/detail/a_id/1666/~/hvor-finder-jeg-de-direkte-links-til-drs-radiokanaler%3F. Den side refererer til et kvalitativt bedre format end MP3, nemlig AAC.

Jeg har dette kort, hvor jeg har muligheden for at vælge kanal, vælge højttalere og sætte lydstyrken + spil og stop. Bemærk at nederste linje som forestiller en "musicplayer" kræver man installere en HACS frontend: multiple-entity-row og card-mod.

<img src="https://user-images.githubusercontent.com/103023823/210971298-c5cc1ff0-f076-4392-ae7e-8813df0d3f55.png" width="200" height="200"><img src="https://user-images.githubusercontent.com/103023823/210972145-5c3402a2-ef5c-4eec-aa70-6a257fd420cf.png" width="200" height="200"><img src="https://user-images.githubusercontent.com/103023823/210972232-73a97786-0600-41b0-afa5-3e36ee3fc8d7.png" width="200" height="200"><img src="https://user-images.githubusercontent.com/103023823/210968251-46572a87-5ce8-4a8d-8936-0e941711ac6b.png" width="200" height="200"><img src="https://user-images.githubusercontent.com/103023823/210972372-612b0e9f-aed3-4ed5-a66a-ee1b2351e5b5.png" width="200" height="200"><img src="https://user-images.githubusercontent.com/103023823/210971717-2767165a-d77e-44b6-950c-92f71744364e.png" width="200" height="200"><img src="https://user-images.githubusercontent.com/103023823/210971791-0254de52-9b83-4b05-a777-d4ddeecf4359.png" width="200" height="200">

Det virker med MP3 links, og er langt hurtigere end at anvende referencerne til MPEG fra Radio Browser under medier. Jeg endte med at vælge AAC formatet, da kvaliteten er bedre på AAC end MP3.

Forbedringer:
- <del>Lydstyrke ændringen virker ved at trykke "Spil radio", altså samme kanal og anden lydniveau.</del>
- <del>I stedet for "Spil radio" og "Stop afspilning" med tilhørende "kør"-knapper, så skal der findes et layout som ligner en normal mediaplayer.</del>
- <del>Formatet AAC har en bedre lydkvalitet end MP3, men understøtter HA dette!?</del>
- <del>Sexy er den ikke, men den virker. Hvordan den bliver sexy og med ikon for stationerne svæver ude i fremtiden... :)</del>
- <del>Lydstyrke skal virke straks ved at bruge "slider" og burde være i intervallet 0 .. 100</del>
- <del>Der skal gøres noget ved farverne på tekst og ikoner. Jeg drømmer om en "tåget baggrund" med klar tekst, men "opacity = 30%" tåger både bag- og forgrund.</del>

Implementering:
Jeg lavede en gruppe med alle mine højttalere under Indstillinger > Enheder og tjenester > Hjælpere - hvor jeg valgte en gruppe fra "Media player gruppe" og tilføjede enhederne.

Jeg smed følgende inputs i configuration.yaml
input_select og input_number er formateret korrekt her: https://github.com/MaximusClavius/cast-radio-kanaler/blob/main/inputs
Jeg smed følgende inputs i configuration.yaml

og lavede følgende script til at starte og afslutte afspilning, som jeg gemte i scripts.yaml
https://github.com/MaximusClavius/cast-radio-kanaler/blob/main/script

<p>og følgende automation for at kunne ændre lydstyrke ved direkte berøring.</p>

<p>Dette korts yaml ser sådan ud: https://github.com/MaximusClavius/cast-radio-kanaler/blob/main/card-entilities</p>

<p>Ønsker man at baggrunden skal være anderledes, så kan man ændre disse to CSS attributer:
background-color: rgba(255, 255, 255, 0.3); <- alfa: 0.3 giver størst effekt.
background-blend-mode: lighten; <- alternativer: normal | multiply | screen | overlay | darken | lighten | color-dodge | color-burn | hard-light | soft-light | difference | exclusion | hue |saturation | color | luminosity</p>

Tilføje eller fjerne radiokanaler skal ske i "input_select: radio_station", og i script: play_radio_channel under "media_content_id:" med relevant link fra Danish Radio Streams. Husk korrekt formatering!
Samme metode for flere eller færre højttalere.

Genstart scripts og input under udviklingsværktøjer > YAML eller genstart konfiguration

<a href="https://www.paypal.com/donate/?hosted_button_id=NNUF56TVFMJXY"><img src="https://www.paypalobjects.com/da_DK/DK/i/btn/btn_donateCC_LG.gif" alt="HTML tutorial"></a>
