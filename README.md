# cast-radio-kanaler
Cast radiokanaler til højttalere/Cast radio channels to speakers

Jeg har en del Google Nest minis og JBL Flip, som jeg gerne vil bruge til afspilning af musik.
Jeg prøvede at bruge integration: Radio Browser, som glemmer alverdens radiokanaler under medier. 
Den anvender formatet mpeg, som normalt bruges til digital video og lyd, altså video/film.
Derfor begyndte jeg at kigge efter mp3 og streams, og fandt denne hjemmeside, Danish Radio Streams: https://www.astra2sat.com/danish-radio-streams/. 
Der er en fejl for DR P1, men den reference findes på DRs egen hjemmeside: https://dr.custhelp.com/app/answers/detail/a_id/1666/~/hvor-finder-jeg-de-direkte-links-til-drs-radiokanaler%3F

Jeg har dette kort, hvor jeg har muligheden for at vælge kanal, vælg lokation og sætte lydstyrken + tænd og sluk.
![image](https://user-images.githubusercontent.com/103023823/210777360-8eb1d308-b47c-459b-bb06-586d5f4f94dd.png)

Det virker med mp3 links, og er langt hurtigere end at anvende referencerne til mpeg fra Radio Browser under medier.

Forbedringer:
- Lydstyrke ændringen virker ved at trykke "Spil radio", altså samme kanal og anden lydniveau.
- I stedet for "Spil radio" og "Stop afspilning" med tilhørende "kør"-knapper, så skal der findes et layout som ligner en normal mediaplayer.
- Formatet AAC forbedrer lydkvaliteten af mp3, men understøtter HA dette!?
- Sexy er den ikke, men den virker. Hvordan den bliver sexy og med ikon for stationerne svæver ude i fremtiden... :)

Implementering:
Jeg smed følgende inputs i configuration.yaml
input_select og input_number er formateret korrekt her: https://github.com/MaximusClavius/cast-radio-kanaler/blob/main/inputs
Jeg smed følgende inputs i configuration.yaml

og lavede følgende script til at starte og afslutte afspilning, som jeg gemte i scripts.yaml
https://github.com/MaximusClavius/cast-radio-kanaler/blob/main/script

Det non-sexy korts yaml ser sådan ud: https://github.com/MaximusClavius/cast-radio-kanaler/blob/main/card-entilities

Tilføje eller fjerne radiokanaler skal ske i "input_select: radio_station", og i script: play_radio_channel under "media_content_id:" med relevant link fra Danish Radio Streams. Husk korrekt formatering!
Samme metode for flere eller færre højttalere.

Genstart scripts og input under udviklingsværktøjer > YAML eller genstart kunfiguration
