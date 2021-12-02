<!DOCTYPE html>
<html lang=en>
    <head>
        <meta charset="UTF-8" />
        <meta name="viewport" content="width=\, initial scale=1.0" />
        <meta http-equiv="X-UA-Compatible" content="ie=edge" />
        <link 
        rel="stylesheet" 
        href="https://unpkg.com/leaflet@1.7.1/dist/leaflet.css"
   integrity="sha512-xodZBNTC5n17Xt2atTPuE1HxjVMSvLVW9ocqUKLsCC5CXdbqCmblAshOMAS6/keqq/sMZMZ19scR4PsZChSR7A=="
   crossorigin=""
   />
   <script 
   src="https://unpkg.com/leaflet@1.7.1/dist/leaflet.js"
   integrity="sha512-XQoYMqMTK8LvdxXYG3nZ448hOEQiglfqkJs1NOQV44cWnUrBc8PkAOcXy20w0vlaXaVUearIOBhiXZ5V3ynxwA=="
   crossorigin=""
   ></script>
   <style>
   #issMap { 
       height: 500px;
    }  
   </style>
        <title>NeXT Assessment Nur Qamarina Ruslan</title>
        <script>
            function displayDate() {
                document.getElementById("showDate").innerHTML = 
                Date();
            }
        </script>
        <style>
            .social-icon {
                color: #aaaaaa;
                transition: color 0.2s;
                text-decoration: none;
                margin: 0 03px;
                font-size: 140%;
            }

            .social-icon:hover {
                color: rgb(0, 0, 0)
            }
        </style>
    </head>
    <body>
        <h1>Where is the International Space Station (ISS)?</h1>

        <a class="social-icon" href="https://www.facebook.com/ISS/" target="_blank">
            <ion-icon name="logo-facebook"></ion-icon>
            </a>

        <a class="social-icon" href="https://twitter.com/Space_Station" target="_blank">
        <ion-icon name="logo-twitter"></ion-icon>
        </a>

        <a class="social-icon" href="https://www.youtube.com/channel/UCLA_DiR1FfKNvjuUpBHmylQ" target="_blank">
            <ion-icon name="logo-youtube"></ion-icon>
            </a>

        <script type="module" src="https://unpkg.com/ionicons@5.5.2/dist/ionicons/ionicons.esm.js"></script>
        <script nomodule src="https://unpkg.com/ionicons@5.5.2/dist/ionicons/ionicons.js"></script>

       <br />
       <br />

       <button type="button" onclick="displayDate()">Show Date and Time</button>
       <p id="showDate"></p>
    <p>
        latitude: <span id="lat"></span><br />
        longitude: <span id="lon"></span>
    </p>

    <div id ="issMap"></div>

        <script>
            //Making a map and tiles
            const mymap = L.map('issMap').setView([0, 0], 1);
            const attribution =
            '&copy; <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a> contributors';

            const tileUrl = 'https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png';
            const tiles = L.tileLayer(tileUrl, { attribution });
            tiles.addTo(mymap);

            //Making a marker with a custom icon
            const issIcon = L.icon({
    iconUrl: 'iss.png',
    iconSize: [50, 32],
    iconAnchor: [25, 16]
});
            const marker =  L.marker([0, 0], { icon: issIcon }).addTo(mymap);

            const api_url = 'https://api.wheretheiss.at/v1/satellites/25544';

            let firstTime = true;

            async function getISS() {
                const response = await fetch(api_url);
                const data = await response.json();
                const { latitude, longitude } = data;

                marker.setLatLng([latitude, longitude]);
                if (firstTime) {
                mymap.setView([latitude, longitude], 2)
                firstTime = false;
                }
                document.getElementById('lat').textContent = latitude.toFixed(5);
                document.getElementById('lon').textContent = longitude.toFixed(5);
            }

            getISS();

            setInterval(getISS, 1000);
        </script>
    </body>
</html>
