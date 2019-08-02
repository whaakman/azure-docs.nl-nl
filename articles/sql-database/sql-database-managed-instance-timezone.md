---
title: Tijd zones van beheerde instantie Azure SQL Database | Microsoft Docs "
description: Meer informatie over de tijdzone specificaties van Azure SQL Database beheerde instantie
services: sql-database
ms.service: sql-database
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: MladjoA
ms.author: mlandzic
ms.reviewer: ''
ms.date: 07/05/2019
ms.openlocfilehash: 33c844374d6d2b8e64cde6c7c9633e54a292d95f
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/26/2019
ms.locfileid: "68567289"
---
# <a name="time-zones-in-azure-sql-database-managed-instance"></a>Tijd zones in Azure SQL Database beheerde instantie

Coordinated Universal Time (UTC) is de aanbevolen tijd zone voor de gegevenslaag van cloud oplossingen. Azure SQL Database Managed instance biedt ook een keuze aan tijd zones om te voldoen aan de behoeften van bestaande toepassingen die datum-en tijd waarden opslaan en functies voor datum en tijd aanroepen met een impliciete context van een specifieke tijd zone.

T-SQL-functies [, zoals getdate ()](https://docs.microsoft.com/sql/t-sql/functions/getdate-transact-sql) of CLR-code, observeren de tijd zone die is ingesteld op instantie niveau. SQL Server Agent taken volgen ook schema's volgens de tijd zone van het exemplaar.

  >[!NOTE]
  > Het beheerde exemplaar is de enige implementatie optie van Azure SQL Database die de tijd zone-instelling ondersteunt. Andere implementatie opties volgen altijd UTC.
Gebruik [op tijd zone](https://docs.microsoft.com/sql/t-sql/queries/at-time-zone-transact-sql) in één en gepoolde SQL-Data Base als u datum-en tijd gegevens wilt interpreteren in een niet-UTC-tijd zone.

## <a name="supported-time-zones"></a>Ondersteunde tijd zones

Een set ondersteunde tijd zones wordt overgenomen van het onderliggende besturings systeem van het beheerde exemplaar. Het wordt regel matig bijgewerkt om nieuwe tijdzone definities op te halen en wijzigingen aan te brengen in de bestaande.

Met het [beleid voor zomer tijd/tijd zone wijzigingen](https://aka.ms/time) wordt de historische nauw keurigheid van 2010 voorwaarts gegarandeerd.

Een lijst met namen van de ondersteunde tijd zones wordt weer gegeven in de systeem weergave [sys. time_zone_info](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-time-zone-info-transact-sql) .

## <a name="set-a-time-zone"></a>Een tijd zone instellen

Een tijd zone van een beheerd exemplaar kan alleen worden ingesteld tijdens het maken van een exemplaar. De standaardtijd zone is UTC.

  >[!NOTE]
  > De tijd zone van een bestaand beheerd exemplaar kan niet worden gewijzigd.

### <a name="set-the-time-zone-through-the-azure-portal"></a>Stel de tijd zone in met behulp van de Azure Portal

Wanneer u para meters voor een nieuw exemplaar opgeeft, selecteert u een tijd zone in de lijst met ondersteunde tijd zones.
  
![Een tijd zone instellen tijdens het maken van een instantie](media/sql-database-managed-instance-timezone/01-setting_timezone-during-instance-creation.png)

### <a name="azure-resource-manager-template"></a>Azure Resource Manager-sjabloon

Geef de eigenschap timezoneId op in uw [Resource Manager-sjabloon](https://aka.ms/sql-mi-create-arm-posh) om de tijd zone in te stellen tijdens het maken van een exemplaar.

```json
"properties": {
                "administratorLogin": "[parameters('user')]",
                "administratorLoginPassword": "[parameters('pwd')]",
                "subnetId": "[parameters('subnetId')]",
                "storageSizeInGB": 256,
                "vCores": 8,
                "licenseType": "LicenseIncluded",
                "hardwareFamily": "Gen5",
                "collation": "Serbian_Cyrillic_100_CS_AS",
                "timezoneId": "Central European Standard Time"
            },

```

Aan het einde van dit artikel vindt u een lijst met ondersteunde waarden voor de eigenschap timezoneId.

Als u niets opgeeft, wordt de tijd zone ingesteld op UTC.

## <a name="check-the-time-zone-of-an-instance"></a>De tijd zone van een exemplaar controleren

De functie [CURRENT_TIMEZONE](https://docs.microsoft.com/sql/t-sql/functions/current-timezone-transact-sql) retourneert een weergave naam van de tijd zone van het exemplaar.

## <a name="cross-feature-considerations"></a>Overwegingen voor meerdere functies

### <a name="restore-and-import"></a>Herstellen en importeren

U kunt een back-upbestand herstellen of gegevens importeren naar een beheerd exemplaar vanuit een exemplaar of een server met verschillende instellingen voor de tijd zone. Zorg er daarom voor dat u dit doet. Analyseer het toepassings gedrag en de resultaten van de query's en rapporten, net als bij het overdragen van gegevens tussen twee SQL Server exemplaren met verschillende instellingen voor de tijd zone.

### <a name="point-in-time-restore"></a>Terugzetten naar eerder tijdstip

<del>Wanneer u een herstel naar een bepaald tijdstip uitvoert, wordt de tijd om te herstellen, geïnterpreteerd als UTC-tijd. Deze instelling vermijdt Verwar ring door zomer tijd en de mogelijke wijzigingen.<del>

 >[!WARNING]
  > Het huidige gedrag is niet in overeenstemming met de instructie hierboven en de tijd om te herstellen naar wordt geïnterpreteerd als de tijd zone van het beheerde exemplaar van de bron waarvoor automatische database back-ups worden gemaakt. Er wordt aan gewerkt om dit gedrag te corrigeren om het gegeven punt in de tijd te interpreteren als UTC-tijd. Zie [bekende problemen](sql-database-managed-instance-timezone.md#known-issues) voor meer informatie.

### <a name="auto-failover-groups"></a>Automatische failover-groepen

Het gebruik van dezelfde tijd zone in een primair en secundair exemplaar in een failovergroep wordt niet afgedwongen, maar we raden dit ten zeerste aan.

  >[!WARNING]
  > We raden u ten zeerste aan dezelfde tijd zone te gebruiken voor het primaire en secundaire exemplaar in een failovergroep. Vanwege sommige zeldzame scenario's wordt de dezelfde tijd zone op de primaire en secundaire instanties niet afgedwongen. Het is belang rijk om te begrijpen dat in het geval van een hand matige of automatische failover de oorspronkelijke tijd zone wordt bewaard.

## <a name="limitations"></a>Beperkingen

- De tijd zone van het bestaande beheerde exemplaar kan niet worden gewijzigd.
- Externe processen die vanuit de SQL Server Agent taken worden gestart, hebben geen tijd zone van het exemplaar.

## <a name="known-issues"></a>Bekende problemen

Wanneer een PITR-bewerking (Point-in-time Restore) wordt uitgevoerd, wordt de tijd die moet worden teruggezet, geïnterpreteerd als per tijd zone die is ingesteld op het beheerde exemplaar van automatische database back-ups, zelfs hoewel de portal pagina voor PITR aangeeft dat de tijd wordt geïnterpreteerd als UTC.

Voorbeeld:

Stel dat er voor de automatische back-ups van een tijd zoneset Eastern (standaard tijd) (UTC-5) is ingesteld.
De portal pagina voor herstel naar een bepaald tijdstip geeft aan dat de tijd die u wilt herstellen, UTC is:

![PITR met de lokale tijd met behulp van portal](media/sql-database-managed-instance-timezone/02-pitr-with-nonutc-timezone.png)

De tijd voor het herstellen naar wordt echter in werkelijkheid geïnterpreteerd als Eastern (standaard tijd). in deze specifieke voorbeeld database wordt de status op 9 AM Eastern (standaard tijd) en niet UTC-tijd hersteld.

Als u naar een bepaald punt in UTC-tijd wilt herstellen, moet u eerst de overeenkomende tijd in de tijd zone van het bron exemplaar berekenen en die tijd gebruiken in het portal-of Power shell/CLI-script.

## <a name="list-of-supported-time-zones"></a>Lijst met ondersteunde tijd zones

| **Tijd zone-ID** | **Weergave naam van de tijd zone** |
| --- | --- |
| Datumgrens (standaardtijd) | (UTC-12:00) Internationale Datumgrens West |
| UTC-11 | (UTC-11:00) Coordinated Universal Time-11 |
| Aleoeten (standaardtijd) | (UTC-10:00) Aleoeten |
| Hawaï (standaardtijd) | (UTC-10:00) Hawaii |
| Marquesas Standard Time | (UTC-09:30) Marquesaseilanden |
| Alaska (standaardtijd) | (UTC-09:00) Alaska |
| UTC-09 | (UTC-09:00) Coordinated Universal Time-09 |
| Pacific (standaardtijd, Mexico) | (UTC-08:00) Baja California |
| UTC-08 | (UTC-08:00) Coordinated Universal Time-08 |
| Pacific (standaardtijd) | (UTC-08:00) Pacific Time (VS en Canada) |
| US Mountain (standaardtijd) | (UTC-07:00) Arizona |
| Mountain (standaardtijd, Mexico) | (UTC-07:00) Chihuahua, La Paz, Mazatlan |
| Mountain (standaardtijd) | (UTC-07:00) Mountain Time (VS en Canada) |
| Centraal-Amerika (stnd. tijd) | (UTC-06:00) Centraal-Amerika |
| Central (standaardtijd) | (UTC-06:00) Central Time (VS en Canada) |
| Paaseiland (standaardtijd) | (UTC-06:00) Paaseiland |
| Central (standaardtijd, Mexico) | (UTC-06:00) Guadalajara, Mexico, Monterrey |
| Canada - centraal (standaardtijd) | (UTC-06:00) Saskatchewan |
| SA Pacific (standaardtijd) | (UTC-05:00) Bogota, Lima, Quito, Rio Branco |
| Eastern (standaardtijd, Mexico) | (UTC-05:00) Chetumal |
| Eastern (standaardtijd) | (UTC-05:00) Eastern Time (VS en Canada) |
| Haïti (standaardtijd) | (UTC-05:00) Haïti |
| Cuba (standaardtijd) | (UTC-05:00) Havana |
| US - oost (standaardtijd) | (UTC-05:00) Indiana (East) |
| Turks-en Caicoseilanden (standaard tijd) | (UTC-05:00) Turks- en Caicoseilanden |
| Paraguay (standaardtijd) | (UTC-04:00) Asuncion |
| Atlantic (standaardtijd) | (UTC-04:00) Atlantic Time (Canada) |
| Venezuela Standard Time | (UTC-04:00) Caracas |
| Centraal Brazilië (standaardtijd) | (UTC-04:00) Cuiaba |
| SA Western (standaardtijd) | (UTC-04:00) Georgetown, La Paz, Manaus, San Juan |
| Pacific SA (standaardtijd) | (UTC-04:00) Santiago |
| Newfoundland (standaardtijd) | (UTC-03:30) Newfoundland |
| Tocantins (standaardtijd) | (UTC-03:00) Araguaina |
| E. Zuid-Amerika (standaard tijd) | (UTC-03:00) Brasilia |
| SA Eastern (standaardtijd) | (UTC-03:00) Cayenne, Fortaleza |
| Argentinië (standaardtijd) | (UTC-03:00) Buenos Aires |
| Groenland (standaardtijd) | (UTC-03:00) Groenland |
| Montevideo (standaardtijd) | (UTC-03:00) Montevideo |
| Magallanes Standard Time | (UTC-03:00) Punta Arenas |
| Saint-Pierre (standaardtijd) | (UTC-03:00) Saint-Pierre en Miquelon |
| Bahia standaardtijd | (UTC-03:00) Salvador |
| UTC-02 | (UTC-02:00) Coordinated Universal Time-02 |
| Mid-Atlantic (standaardtijd) | (UTC-02:00) Mid-Atlantic - verouderd |
| Azoren (standaardtijd) | (UTC-01:00) Azoren |
| Cabo Verde (standaard tijd) | (UTC-01:00) Cabo Verde |
| UTC | (UTC) Coordinated Universal Time |
| GMT (standaardtijd) | (UTC+00:00) Dublin, Edinburgh, Lissabon, Londen |
| Greenwich (standaardtijd) | (UTC+00:00) Monrovia, Reykjavik |
| W. Europa (standaard tijd) | (UTC+01:00) Amsterdam, Berlijn, Bern, Rome, Stockholm, Wenen |
| Centraal-Europa (standaardtijd) | (UTC+01:00) Belgrado, Bratislava, Boedapest, Ljubljana, Praag |
| Romance (standaardtijd) | (UTC+01:00) Brussel, Kopenhagen, Madrid, Parijs |
| Marokko (standaardtijd) | (UTC +01:00) Casablanca |
| Standaardtijd van Sao Tomé | (UTC+01:00) Sao Tomé |
| Europa centraal (standaardtijd) | (UTC+01:00) Sarajevo, Skopje, Warschau, Zagreb |
| W. Centraal-Afrika (standaard tijd) | (UTC+01:00) West Central Africa |
| Jordanië (standaardtijd) | (UTC+02:00) Amman |
| GTB (standaardtijd) | (UTC+02:00) Athene, Boekarest |
| Midden-Oosten (standaardtijd) | (UTC+02:00) Beiroet |
| Egypte (standaardtijd) | (UTC+02:00) Caïro |
| E. Europa (standaard tijd) | (UTC+02:00) Chisinau |
| Syrië standaardtijd | (UTC+02:00) Damascus |
| West Bank (standaard tijd) | (UTC+02:00) Gaza, Hebron |
| Zuid-Afrika (standaardtijd) | (UTC+02:00) Harare, Pretoria |
| FLE (standaardtijd) | (UTC+02:00) Helsinki, Kiev, Riga, Sofia, Tallinn, Vilnius |
| Israël (standaard tijd) | (UTC+02:00) Jeruzalem |
| Kaliningrad (standaard tijd) | (UTC+02:00) Kaliningrad |
| Soedan (standaardtijd) | (UTC + 02:00) Khartoem |
| Libië standaardtijd | (UTC+02:00) Tripoli |
| Namibië (standaardtijd) | (UTC + 02:00) Windhoek |
| Arab. schiereiland (st. tijd) | (UTC+03:00) Bagdad |
| Turkije standaardtijd | (UTC+03:00) Istanboel |
| Arabisch schiereiland (st.tijd) | (UTC+03:00) Koeweit, Riyad |
| Belarus (standaardtijd) | (UTC+03:00) Minsk |
| Rusland (standaard tijd) | (UTC+03:00) Moskou, Sint-Petersburg |
| E. Afrika (standaard tijd) | (UTC+03:00) Nairobi |
| Iran (standaardtijd) | (UTC+03:30) Teheran |
| Arabisch schiereiland (st.tijd) | (UTC+04:00) Abu Dhabi, Muscat |
| Astrachan (standaardtijd) | (UTC+04:00) Astrachan, Oeljanovsk |
| Azerbeidzjan (standaardtijd) | (UTC+04:00) Bakoe |
| Rusland-tijd Zone 3 | (UTC+04:00) Izhevsk, Samara |
| Mauritius (standaardtijd) | (UTC+04:00) Port Louis |
| Saratov (standaardtijd) | (UTC+04:00) Saratov |
| Georgië (standaardtijd) | (UTC+04:00) Tbilisi |
| Volgograd (standaard tijd) | (UTC+04:00) Volgograd |
| Kaukasus (standaardtijd) | (UTC+04:00) Jerevan |
| Afghanistan (standaardtijd) | (UTC+04:30) Kaboel |
| West Asia (standaardtijd) | (UTC+05:00) Asjchabad, Tasjkent |
| Jekaterinenburg standaard tijd) | (UTC+05:00) Jekaterinenburg |
| Pakistan (standaardtijd) | (UTC+05:00) Islamabad, Karachi |
| India (standaardtijd) | (UTC+05:30) Chennai, Kolkata, Mumbai, New Delhi |
| Sri Lanka (standaardtijd) | (UTC+05:30) Sri Jayawardenepura |
| Nepal (standaardtijd) | (UTC+05:45) Kathmandu |
| Central Asia (standaardtijd) | (UTC+06:00) Astana |
| Bangladesh standaardtijd | (UTC+06:00) Dhaka |
| Omsk (standaardtijd) | (UTC+06:00) Omsk |
| Myanmar (standaardtijd) | (UTC+06:30) Yangon (Rangoon) |
| Zuidoost-Azië (standaardtijd) | (UTC+07:00) Bangkok, Hanoi, Jakarta |
| Altaj (standaardtijd) | (UTC+07:00) Barnaoel, Gorno-Altajsk |
| W. Mongolië (standaard tijd) | (UTC+07:00) Hovd |
| North Asia (standaard tijd) | (UTC+07:00) Krasnoyarsk |
| NVT. Central Asia (standaardtijd) | (UTC+07:00) Novosibirsk |
| Tomsk (standaardtijd) | (UTC+07:00) Tomsk |
| China (standaardtijd) | (UTC+08:00) Beijing, Chongqing, Hongkong SAR, Urumqi |
| Noord-Azië Oost (standaard tijd) | (UTC+08:00) Irkoetsk |
| Singapore (standaard tijd) | (UTC+08:00) Kuala Lumpur, Singapore |
| W. Australië (standaard tijd) | (UTC+08:00) Perth |
| Taipei (standaardtijd) | (UTC+08:00) Taipei |
| Ulaanbaatar standaardtijd | (UTC+08:00) Ulaanbaatar |
| Australië - centraal-west (standaardtijd) | (UTC+08:45) Eucla |
| Transbaikal (standaardtijd) | (UTC+09:00) Chita |
| Tokio (standaardtijd) | (UTC+09:00) Osaka, Sapporo, Tokio |
| Noord-Korea (standaardtijd) | (UTC+09:00) Pyongyang |
| Korea (standaardtijd) | (UTC+09:00) Seoul |
| Jakoetsk standaard tijd) | (UTC+09:00) Jakoetsk |
| Centraal. Australië (standaard tijd) | (UTC+09:30) Adelaide |
| Australië - centraal (standaardtijd) | (UTC+09:30) Darwin |
| E. Australië (standaard tijd) | (UTC+10:00) Brisbane |
| Australië - oost (standaardtijd) | (UTC+10:00) Canberra, Melbourne, Sydney |
| West Pacific (standaardtijd) | (UTC+10:00) Guam, Port Moresby |
| Tasmanië (standaardtijd) | (UTC+10:00) Hobart |
| Wladiwostok (standaard tijd) | (UTC+10:00) Vladivostok |
| Lord Howe Island (standaardtijd) | (UTC+10:30) Lord Howe Island |
| Bougainville (standaardtijd) | (UTC+11:00) Bougainville |
| Rusland-tijd zone 10 | (UTC+11:00) Chokurdakh |
| Magadan standaardtijd | (UTC+10:00) Magadan |
| Norfolk (standaardtijd) | (UTC+11:00) Norfolk |
| Sachalin (standaardtijd) | (UTC+10:00) Sakhalin |
| Central Pacific (standaardtijd) | (UTC+11:00) Salomonseilanden, Nieuw-Caledonië |
| Rusland-tijd zone 11 | (UTC+12:00) Anadyr, Petropavlovsk-Kamtsjatski |
| Nieuw-Zeeland (standaardtijd) | (UTC+12:00) Auckland, Wellington |
| UTC+12 | (UTC+12:00) Coordinated Universal Time+12 |
| Fiji (standaardtijd) | (UTC+12:00) Fiji |
| Kamtsjatka (standaardtijd) | (UTC+12:00) Petropavlovsk-Kamtsjatski - verouderd |
| Chathameilanden (standaardtijd) | (UTC+12:45) Chathameilanden |
| UTC+13 | (UTC+13:00) Coordinated Universal Time+13 |
| Tonga (standaardtijd) | (UTC+13:00) Nuku'alofa |
| Samoa (standaardtijd) | (UTC+13:00) Samoa |
| Line-eilanden (standaardtijd) | (UTC+14:00) Kiritimati-eiland |

## <a name="see-also"></a>Zie ook 

- [CURRENT_TIMEZONE (Transact-SQL)](https://docs.microsoft.com/sql/t-sql/functions/current-timezone-transact-sql)
- [OP tijd ZONE (Transact-SQL)](https://docs.microsoft.com/sql/t-sql/queries/at-time-zone-transact-sql)
- [sys.time_zone_info (Transact-SQL)](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-time-zone-info-transact-sql)
