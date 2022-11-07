# TP4 : TCP, UDP et services réseau

🌞 Déterminez, pour ces 5 applications, si c'est du TCP ou de l'UDP

- avec Wireshark, on va faire les chirurgiens réseau

    - opera(vidéo youtube)

         IP et port du serveur `142.250.178.142:443`

         Port local `50638`

            [opera.exe]
            UDP    [::]:5353              *:* 
        
         [wireshark Opera](./Opera.pcapng)

    - spotify(musique)

         IP et port du serveur `35.186.224.25:443`

         Port local `57479`

          [Spotify.exe]
           UDP    10.3.1.15:57479          *:*

         [wireshark Spotify](./Spotify.pcapng)

    - Valorant(partie entrainement)

         IP et port du serveur `162.249.72.1:7428`

         Port local `64627`

          [RiotClientServices.exe]
          UDP    0.0.0.0:64627          *:*
         [wireshark Valorant](./Valorant.pcapng)

    - discord

         IP et port du serveur `162.159.136.234:443`

         Port local `57734`

           [Discord.exe]
            TCP    10.33.16.244:57734     162.159.136.234:443    ESTABLISHED

         [wireshark Discord](./Discord.pcapng)
    - epic game

         IP et port du serveur `52.73.231.66:443`

         Port local `61895`

          [EpicGamesLauncher.exe]
          TCP    10.33.16.244:61895     52.73.231.66:443

         [wireshark Epic games](./Epic%20game.pcapng)


