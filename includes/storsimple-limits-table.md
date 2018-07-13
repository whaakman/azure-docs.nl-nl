<!--author=alkohli last changed: 12/15/15-->

| Limiet-id | Limiet | Opmerkingen |
| --- | --- | --- |
| Maximum aantal opslagaccountreferenties |64 | |
| Maximum aantal volumecontainers |64 | |
| Maximale aantal volumes |255 | |
| Maximum aantal planningen per bandbreedtesjabloon |168 |Een planning voor elk uur, elke dag van de week (24 uur per dag * 7). |
| Maximale grootte van een gelaagd volume op fysieke apparaten |64 TB voor 8100- en 8600 |8100- en 8600 zijn fysieke apparaten. |
| Maximale grootte van een gelaagd volume op de virtuele apparaten in Azure |30 TB voor 8010 <br></br> 64 TB voor de 8020 |8010 en 8020 zijn virtuele apparaten in Azure die respectievelijk gebruikmaken van Standard Storage en Premium-opslag. |
| Maximale grootte van een lokaal vastgemaakt volume op fysieke apparaten |9 TB voor 8100 <br></br> 24 TB voor 8600 |8100- en 8600 zijn fysieke apparaten. |
| Maximum aantal iSCSI-verbindingen |512 | |
| Maximum aantal verbindingen iSCSI-initiators |512 | |
| Maximum aantal access control records per apparaat |64 | |
| Maximale aantal volumes per back-upbeleid |24 | |
| Maximum aantal per back-upbeleid bewaarde back-ups |64 | |
| Maximum aantal planningen per back-upbeleid |10 | |
| Maximum aantal momentopnamen van elk type per volume kunnen worden bewaard |256 |Dit omvat lokale momentopnamen en cloud-momentopnamen. |
| Maximum aantal momentopnamen die gebruikt in elk apparaat worden kunnen |10.000 | |
| Maximale aantal volumes dat voor back-up en herstel parallel kunnen worden verwerkt of klonen |16 |<ul><li>Als er meer dan 16 volumes, worden ze achter elkaar verwerkt zodra verwerking sleuven beschikbaar.</li><li>Nieuwe back-ups van een gekloonde of een herstelde gelaagd volume is niet mogelijk totdat de bewerking is voltooid. Back-ups zijn echter voor een lokaal volume nadat het volume online is toegestaan.</li></ul> |
| Terugzetten en herstellen van tijd voor gelaagde volumes klonen |< 2 minuten |<ul><li>Het volume wordt beschikbaar gesteld binnen twee minuten van herstel- of kloon bewerking, ongeacht de grootte van het volume.</li><li>In eerste instantie de volume prestaties mogelijk langzamer dan normaal omdat de meeste van de gegevens en metagegevens is nog steeds bevindt zich in de cloud. Prestaties verhogen als de gegevensstromen vanuit de cloud met de StorSimple-apparaat.</li><li>De totale tijd voor het downloaden van metagegevens is afhankelijk van de grootte van het toegewezen. Metagegevens is automatisch in het apparaat op de achtergrond met de snelheid van 5 minuten per TB aan gegevens toegewezen volume gebracht. Deze frequentie kan worden beïnvloed door internetbandbreedte naar de cloud.</li><li>Het terugzetten of de kloonbewerking is voltooid wanneer alle metagegevens op het apparaat is.</li><li>Back-upbewerkingen kunnen niet worden uitgevoerd totdat het herstel of de kloonbewerking is volledig is voltooid. |
| Herstellen van tijd voor lokaal vastgemaakte volumes herstellen |< 2 minuten |<ul><li>Het volume wordt beschikbaar gesteld binnen twee minuten van de herstelbewerking, ongeacht de grootte van het volume.</li><li>In eerste instantie de volume prestaties mogelijk langzamer dan normaal omdat de meeste van de gegevens en metagegevens is nog steeds bevindt zich in de cloud. Prestaties verhogen als de gegevensstromen vanuit de cloud met de StorSimple-apparaat.</li><li>De totale tijd voor het downloaden van metagegevens is afhankelijk van de grootte van het toegewezen. Metagegevens is automatisch in het apparaat op de achtergrond met de snelheid van 5 minuten per TB aan gegevens toegewezen volume gebracht. Deze frequentie kan worden beïnvloed door internetbandbreedte naar de cloud.</li><li>In tegenstelling tot gelaagde volumes, in het geval van lokaal vastgemaakte volumes, wordt ook lokaal gegevens op het volume gedownload op het apparaat. De herstelbewerking is voltooid wanneer alle gegevens op het volume is ingesteld op het apparaat.</li><li>De bewerkingen voor het herstellen mogelijk lang en de totale tijd voor de terugzetbewerking niet voltooien is afhankelijk van de grootte van de ingerichte lokaal volume, de Internet-bandbreedte en de bestaande gegevens op het apparaat. Back-upbewerkingen op de lokaal vastgemaakte volume zijn toegestaan terwijl de herstelbewerking uitgevoerd wordt. |
| Beschikbaarheid van thin-herstel |Laatste failover | |
| Maximum aantal clients lezen/schrijven-doorvoer (wanneer worden aangeleverd vanuit de SSD-laag) * |920/720 MB/s met een enkele 10GbE netwerkinterface |Maximaal 2 x met MPIO- en twee netwerkinterfaces. |
| Maximum aantal clients lezen/schrijven-doorvoer (wanneer worden aangeleverd vanuit de HDD-laag) * |120/250 MB/s | |
| Maximum aantal clients lezen/schrijven-doorvoer (wanneer worden aangeleverd vanuit de cloudlaag) * |11/41 MB/s |Lezen doorvoer is afhankelijk van clients genereren en beheren van voldoende i/o-wachtrijdiepte. |

&#42;Maximale doorvoer per i/o-type is gemeten met 100 procent lezen en schrijven van 100 procent scenario's. Werkelijke doorvoer mogelijk lager en is afhankelijk van i/o-netwerk van de voorwaarden en combineren.

