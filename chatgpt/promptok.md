1. Rendszer előkészítése

Prompt:
„Hozz létre egy új Linux felhasználót devops néven, add hozzá a docker csoporthoz, majd ellenőrizd, hogy sudo joggal képes Docker parancsot futtatni.”

2. Docker telepítése és ellenőrzése

Prompt:
„Telepítsd a Docker Engine-t Ubuntu rendszeren, indítsd el a szolgáltatást, majd futtass egy hello-world konténert a telepítés ellenőrzéséhez.”

3. Saját Dockerfile – alkalmazás konténerezése

Prompt:
„Írj egy Dockerfile-t, amely egy egyszerű Python alkalmazást futtat, a konténer a 5000-es porton figyeljen, majd buildeld és indítsd el az image-et.”

4. Több konténer Docker Compose-szal

Prompt:
„Hozz létre egy docker-compose.yml fájlt egy webalkalmazással és egy PostgreSQL adatbázissal, majd indítsd el a szolgáltatásokat és ellenőrizd a futásukat.”

5. Prometheus futtatása és target hozzáadása

Prompt:
„Indítsd el a Prometheus szervert Dockerrel, add hozzá egy futó konténer metrikáit targetként, majd ellenőrizd, hogy a metrikák megjelennek-e.”

6. Logok gyűjtése Filebeat segítségével

Prompt:
„Konfiguráld a Filebeat-et úgy, hogy Docker konténerek logjait olvassa, és továbbítsa azokat egy OpenSearch szerver felé.”

7. cAdvisor konténer monitorozás

Prompt:
„Indítsd el a cAdvisor-t Docker konténerben, majd ellenőrizd egy böngészőből, hogy a konténer CPU és memória használata megjelenik.”

8. Grafana dashboard létrehozása

Prompt:
„Telepítsd a Grafanát Dockerrel, add hozzá a Prometheust adatforrásként, majd hozz létre egy dashboardot konténer CPU használat megjelenítésére.”

9. OpenSearch stack indítása

Prompt:
„Indíts el OpenSearch és OpenSearch Dashboards szolgáltatásokat Docker Compose segítségével, majd ellenőrizd az elérhetőséget böngészőből.”

10. GitLab Runner regisztrálása

Prompt:
„Telepíts egy GitLab Runner-t Docker executorral, regisztráld egy projekthez, és ellenőrizd, hogy aktív állapotba kerül.”

11. CI pipeline – Docker build & push

Prompt:
„Készíts egy .gitlab-ci.yml fájlt, amely buildel egy Docker image-et és feltölti a GitLab Container Registry-be.”

12. Token használat CI-ben

Prompt:
„Állíts be GitLab CI változót Personal Access Token használatával, majd használd azt docker login művelethez a pipeline során.”

🔧 A. Rendszer / Linux / Alapok

A1.
„Ellenőrizd, hogy egy szolgáltatás fut-e Linuxon, indítsd el ha nem fut, és állítsd be automatikus indulásra.”

A2.
„Hozz létre egy könyvtárat, állíts be tulajdonost, jogosultságokat, és írd ki az aktuális effektív jogosultságokat.”

A3.
„Derítsd ki, hogy melyik folyamat használja a 3000-es portot, majd állítsd le.”

🐳 B. Docker – alap, haladó, hibák

B1. Telepítés / ellenőrzés
„Telepítsd a Docker Engine-t, indítsd el, majd ellenőrizd a verziót és a futó konténereket.”

B2. Konténer indítás
„Indíts egy nginx konténert háttérben, mapeld a 8080-as portra, és ellenőrizd böngészőből.”

B3. Dockerfile készítés
„Írj Dockerfile-t Node/Python alkalmazáshoz, amely külön build és runtime réteget használ.”

B4. Hibás Dockerfile javítása
„A Docker image nem buildel — javítsd ki a Dockerfile hibáit.”

B5. Volume és logok
„Indíts konténert úgy, hogy a logok host gépre kerüljenek.”

📦 C. Docker Compose – MINDEN

C1. Alap compose
„Készíts docker-compose.yml fájlt legalább 2 szolgáltatással és közös networkkel.”

C2. Környezeti változók
„Add át adatbázis jelszót biztonságosan environment változóval.”

C3. Volume + DB
„Biztosítsd, hogy az adatbázis újraindítás után is megőrizze az adatokat.”

C4. Hibakeresés
„A compose stack nem indul — derítsd ki a hibát és javítsd.”

📊 D. Prometheus / Monitoring

D1. Prometheus indítás
„Indíts Prometheus szervert Dockerrel konfigurációs fájlból.”

D2. Target hozzáadás
„Adj hozzá új scrape targetet Prometheushoz.”

D3. Hibaelhárítás
„A target DOWN állapotú — derítsd ki miért.”

📁 E. Filebeat / Loggyűjtés

E1. Filebeat Docker logokra
„Konfiguráld a Filebeat-et Docker konténerek logjainak olvasására.”

E2. Output hibák
„A logok nem érkeznek meg OpenSearchbe — javítsd a konfigurációt.”

📈 F. cAdvisor / Grafana

F1. cAdvisor indítás
„Indíts cAdvisor-t úgy, hogy a host Docker metrikák láthatók legyenek.”

F2. Grafana datasource
„Add hozzá a Prometheust Grafanához adatforrásként.”

F3. Dashboard
„Készíts dashboardot CPU és memória monitorozásra.”

🔍 G. OpenSearch

G1. Stack indítás
„Indíts OpenSearch + Dashboards stacket Docker Compose-szal.”

G2. Index ellenőrzése
„Ellenőrizd, hogy létrejött-e index logok számára.”

G3. Kapcsolati hiba
„A Dashboards nem tud csatlakozni az OpenSearchhez — javítsd.”

🦊 H. GitLab – Runner / Registry / CI

H1. Registry login
„Jelentkezz be GitLab Container Registry-be CLI-ről.”

H2. Runner telepítés
„Telepíts és regisztrálj GitLab Runner-t Docker executorral.”

H3. Runner hibák
„A pipeline pending állapotban van — derítsd ki az okát.”

⚙️ I. CI/CD Pipeline – gyakorlat

I1. Alap pipeline
„Írj .gitlab-ci.yml fájlt Docker image buildelésre.”

I2. Push registrybe
„Állítsd be, hogy a buildelt image feltöltésre kerüljön.”

I3. Token használat
„Használj CI változót authentikációra.”

I4. Pipeline hiba
„A pipeline build stage-nél elhasal — javítsd.”

🚨 J. ZH-TIPIKUS ‘CSAVAROS’ FELADATOK

J1.
„Egy konfiguráció majdnem jó — találd meg az egyetlen hibás sort.”

J2.
„Valami fut, de nem elérhető — hálózat, port, tűzfal hiba.”

J3.
„Szolgáltatások nem látják egymást Compose-ban — oldd meg.”