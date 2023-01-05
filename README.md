# cast-radio-kanaler
Cast radiokanaler til højttalere/Cast radio channels to speakers

Jeg har en del Google Nest minis og JBL Flip, som jeg gerne vil bruge til afspilning af musik.
Jeg prøvede at bruge integration: Radio Browser, som glemmer alverdens radio kanaler under medier. 
Den anvender formatet mpeg, som normalt bruges til digital video og lyd, altså video/film.
Derfor begyndte jeg at kigge efter mp3 og streams, og fandt denne hjemmeside, Danish Radio Streams: https://www.astra2sat.com/danish-radio-streams/. 
Der er en fejl for DR P1, men den reference findes på DRs egen hjemmeside: https://dr.custhelp.com/app/answers/detail/a_id/1666/~/hvor-finder-jeg-de-direkte-links-til-drs-radiokanaler%3F

Jeg har dette kort, hvor jeg har muligheden for at vælge kanal, vælg lokation og sætte lydstyrken + tænd og sluk.
![image](https://user-images.githubusercontent.com/103023823/210777360-8eb1d308-b47c-459b-bb06-586d5f4f94dd.png)

Det virker med mp3 links, og er langt hurtigere end at anvende referencerne til mpeg fra Radio Browser under medier.

Forberedringer:
- Lydstyrke ændringen viker ved at trykke "Spil radio", altså samme kanal og anden lydniveau.
- I stedet for "Spil radio" og "Stop afspilning" med tilhørende "kør"-knapper, så skal der findes en layout som ligner en normal mediaplayer.
- Sexy er den ikke, men den virker. Hvordan den bliver sexy og med ikon for stationerne svæver ude i fremtiden... :)

Implementering:

Jeg smed følgende inputs i configuration.yaml
input_select:
  radio_station:
    name: 'Vælg radiokanal:'
    options:
      - DR P1
      - DR P2
      - DR P3
      - DR P4 Fyn
      - DR P5 Fyn
      - DR P6
      - DR P8
    initial: DR P4 Fyn
    icon: mdi:radio
  chromecast_radio:
    name: 'Vælg højttaler:'
    options:
      - Køkken
      - Stue
      - Kontor
    initial: Kontor
    icon: mdi:speaker-wireless

input_number: 
  volume_radio:
    name: Lydstyrke
    icon: mdi:volume-high
    initial: 0.5
    min: 0
    max: 1
    step: 0.1

og lavede følgende script, som jeg gemte i scripts.yaml
play_radio_channel:
  alias: Spil radio
  sequence:
    - service: media_player.volume_set
      data_template:
        entity_id: >
          {% if is_state("input_select.chromecast_radio", "Køkken") %} media_player.kitchen
          {% elif is_state("input_select.chromecast_radio", "Stue") %} media_player.living_room
          {% elif is_state("input_select.chromecast_radio", "Kontor") %} media_player.office
          {% endif %}
        volume_level: '{{  states.input_number.volume_radio.state  }}'
    - service: media_player.play_media
      data_template:
        entity_id: >
          {% if is_state("input_select.chromecast_radio", "Køkken") %} media_player.kitchen
          {% elif is_state("input_select.chromecast_radio", "Stue") %} media_player.living_room
          {% elif is_state("input_select.chromecast_radio", "Kontor") %} media_player.office
          {% endif %}
        media_content_id: >
          {% if is_state("input_select.radio_station", "DR P1") %} 	http://live-icy.dr.dk/A/A03H.mp3
          {% elif is_state("input_select.radio_station", "DR P2") %} 	http://live-icy.dr.dk/A/A04H.mp3
          {% elif is_state("input_select.radio_station", "DR P3") %} http://live-icy.dr.dk/A/A05H.mp3
          {% elif is_state("input_select.radio_station", "DR P4 Fyn") %} https://live-icy.dr.dk/A/A07H.mp3
          {% elif is_state("input_select.radio_station", "DR P5 Fyn") %} https://live-icy.dr.dk/A/A02H.mp3
          {% elif is_state("input_select.radio_station", "DR P6") %} http://live-icy.dr.dk/A/A29H.mp3
          {% elif is_state("input_select.radio_station", "DR P8") %} http://live-icy.dr.dk/A/A22H.mp3
          {% endif %}
        media_content_type: audio/mp3

stop_playing:
  alias: Stop afspilning
  sequence:
    - service: media_player.media_stop
      data_template:
        entity_id: >
          {% if is_state("input_select.chromecast_radio", "Køkken") %} media_player.kitchen
          {% elif is_state("input_select.chromecast_radio", "Stue") %} media_player.living_room
          {% elif is_state("input_select.chromecast_radio", "Kontor") %} media_player.office
          {% endif %}

Det non-sexy korts yaml ser sådan ud:
type: entities
entities:
  - entity: input_select.radio_station
  - entity: input_select.chromecast_radio
  - entity: input_number.volume_radio
  - entity: script.play_radio_channel
    icon: mdi:play
  - entity: script.stop_playing
    icon: mdi:stop
title: Danmarks Radio
state_color: false

Genstart scripts og input under udviklingsværktøjer > YAML eller genstart kunfiguration
