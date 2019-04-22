---
title: Azure SQL Database Managed Instance-tijdzone | Microsoft-Docs'
description: Meer informatie over de tijdzone-details van Azure SQL Database Managed Instance
services: sql-database
ms.service: sql-database
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: MladjoA
ms.author: mlandzic
ms.reviewer: ''
manager: craigg
ms.date: 04/10/2019
ms.openlocfilehash: 23314e97051da95ab164baeab6e9d089f486351a
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/18/2019
ms.locfileid: "59489687"
---
# <a name="time-zone-in-azure-sql-database-managed-instance-preview"></a>Tijdzone in beheerd exemplaar voor Azure SQL Database (Preview)

Met behulp van Coordinated Universal Time (UTC) is een aanbevolen procedure voor de gegevenslaag van oplossingen voor cloudopslag, Azure SQL Database Managed Instance biedt een keuze uit tijdzone om te voldoen aan de behoeften van de bestaande toepassingen die opslaan van waarden voor datum en tijd en datum gesprek en de tijdfuncties bij een impliciete context van een bepaalde tijdzone.

T-SQL-functies, zoals [GETDATE()](https://docs.microsoft.com/sql/t-sql/functions/getdate-transact-sql) of bekijk de tijdzone instellen op het exemplaar, CLR-code. SQL Agent-taken ook volgen plannen op basis van de tijdzone van het exemplaar.

  >[!NOTE]
  > Managed Instance is de enige Implementatieoptie van Azure SQL Database die ondersteuning biedt voor tijdzone-instelling. Andere implementatie-opties Volg altijd UTC.
Gebruik [AT TIME ZONE](https://docs.microsoft.com/sql/t-sql/queries/at-time-zone-transact-sql) in één en gegroepeerde SQL databases als u nodig hebt met het interpreteren van de datum en tijd waarop gegevens in niet-UTC-tijdzone.

## <a name="supported-time-zones"></a>Ondersteunde tijdzones

Een set ondersteunde tijdzones is overgenomen van het onderliggende besturingssysteem van het beheerde exemplaar en het regelmatig wordt bijgewerkt om nieuwe definities van de tijdzone en wijzigingen in de bestaande.

Een lijst met namen van de ondersteunde tijdzones is toegankelijk via de [sys.time_zone_info](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-time-zone-info-transact-sql) door het systeemweergave.

## <a name="setting-time-zone"></a>Tijdzone instellen

Een tijdzone van de beheerde instantie kan tijdens het maken van exemplaar alleen worden ingesteld. De standaardtijdzone is Coordinated Universal Time (UTC).

  >[!NOTE]
  > De tijdzone van een bestaande beheerde exemplaar kan niet worden gewijzigd.

### <a name="setting-the-time-zone-through-azure-portal"></a>De tijdzone via Azure-portal instellen

Bij het invoeren van parameters voor het nieuwe exemplaar, selecteer een tijdzone in de lijst van ondersteunde tijdzones:
  
![Tijdzone instellen tijdens het maken van exemplaar](media/sql-database-managed-instance-timezone/01-setting_timezone-during-instance-creation.png)

### <a name="azure-resource-manager-template"></a>Azure Resource Manager-sjabloon

Geef de eigenschap timezoneId in uw [Resource Manager-sjabloon](https://aka.ms/sql-mi-create-arm-posh) de tijdzone instellen tijdens het maken van het exemplaar.

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

Lijst met ondersteunde waarden voor de eigenschap timezoneId vindt aan het einde van dit artikel.

Indien niet opgegeven, wordt de tijdzone worden ingesteld op UTC.

## <a name="checking-the-time-zone-of-instance"></a>Controle van de tijdzone van exemplaar

[CURRENT_TIMEZONE](https://docs.microsoft.com/sql/t-sql/functions/current-timezone-transact-sql) functie geeft als resultaat een weergavenaam van de tijdzone van het exemplaar.

## <a name="cross-feature-considerations"></a>Overwegingen voor cross-functie

### <a name="restore-and-import"></a>Terugzetten en importeren

U kunt herstellen van back-upbestand of gegevens importeren naar beheerd exemplaar van een exemplaar of een server met een andere tijdzone-instellingen. Echter, zorg ervoor dat u dit doen met de waarschuwing en voor het analyseren van de werking van de toepassing en de resultaten van de query's en rapporten, net als bij de overdracht van gegevens tussen twee SQL Server-exemplaren met andere tijdzone-instellingen.

### <a name="point-in-time-restore"></a>Terugzetten naar eerder tijdstip

Bij het uitvoeren van point-in-time-restore, wordt de tijd om naar te herstellen wordt geïnterpreteerd als UTC-tijd om te voorkomen dat elke ambiguïteit vanwege wintertijd en de mogelijke wijzigingen.

### <a name="auto-failover-groups"></a>Automatische failover-groepen

Met behulp van dezelfde tijdzone voor de primaire en secundaire exemplaar in failover-groep wordt niet afgedwongen, maar het wordt sterk aanbevolen.
  >[!IMPORTANT]
  > Er zijn geldige scenario's voor een andere tijdzone uit bij geo-secundaire exemplaar gebruikt voor alleen leesbewerkingen, houd er rekening mee dat in het geval van handmatige of automatische failover naar de secundaire exemplaar het, behouden de oorspronkelijke tijdzone.

## <a name="limitations"></a>Beperkingen

- Tijdzone van de bestaande beheerde instantie kan niet worden gewijzigd.
- Tijdzone van het exemplaar naleven niet, externe processen gestart vanuit de SQL Agent-taken.
- Beheerd exemplaar van de systeemeigen [New-AzSqlInstance](https://docs.microsoft.com/powershell/module/az.sql/new-azsqlinstance) PowerShell-cmdlet biedt geen ondersteuning voor doorgeven tijdzone parameter nog. Gebruik PowerShell-wrapper met [Resource Manager-sjabloon](https://aka.ms/sql-mi-create-arm-posh) in plaats daarvan.
- CLI-opdracht [az sql mi maken](https://docs.microsoft.com/cli/azure/sql/mi?view=azure-cli-latest#az-sql-mi-create) biedt geen ondersteuning voor de tijdzoneparameter nog.

## <a name="list-of-supported-time-zones"></a>Lijst met ondersteunde tijdzones

| **Tijdzone-ID** | **Weergavenaam van de tijdzone** |
| --- | --- |
| Datumgrens (standaardtijd) | (UTC-12:00) Internationale Datumgrens West |
| UTC-11 | (UTC-11:00) Coordinated Universal Time-11 |
| Aleoeten (standaardtijd) | (UTC-10:00) Aleoeten |
| Hawaï (standaardtijd) | (UTC-10:00) Hawaï |
| Marquesas (standaardtijd) | (UTC-09:30) Marquesaseilanden |
| Alaska (standaardtijd) | (UTC-09:00) Alaska |
| UTC-09 | (UTC-09:00) Coordinated Universal Time-09 |
| Pacific (standaardtijd, Mexico) | (UTC-08:00) Baja California |
| UTC-08 | (UTC-08:00) Coordinated Universal Time-08 |
| Pacific (standaardtijd) | (UTC-08:00) Pacific Time (VS & Canada) |
| US Mountain (standaardtijd) | (UTC-07:00) Arizona |
| Mountain (standaardtijd, Mexico) | (UTC-07:00) Chihuahua, La Paz, Mazatlan |
| Mountain (standaardtijd) | (UTC-07:00) Mountain Time (VS & Canada) |
| Centraal-Amerika (stnd. tijd) | (UTC-06:00) Centraal-Amerika |
| Central (standaardtijd) | (UTC-06:00) Central Time (US & Canada) |
| Paaseiland (standaardtijd) | (UTC-06:00) Paaseiland |
| Central (standaardtijd, Mexico) | (UTC-06:00) Guadalajara, Mexico-stad, Monterrey |
| Canada - centraal (standaardtijd) | (UTC-06:00) Saskatchewan |
| SA Pacific (standaardtijd) | (UTC-05:00) Bogota, Lima, Quito, Rio Branco |
| Eastern (standaardtijd, Mexico) | (UTC-05:00) Chetumal |
| Eastern (standaardtijd) | (UTC-05:00) Eastern Time (VS & Canada) |
| Haïti (standaardtijd) | (UTC-05:00) Haïti |
| Cuba (standaardtijd) | (UTC-05:00) Havana |
| US - oost (standaardtijd) | (UTC-05:00) Indiana (Oost) |
| Turks- en Caicoseilanden (standaardtijd) | (UTC-05:00) Turks- en Caicoseilanden |
| Paraguay (standaardtijd) | (UTC-04:00) Asuncion |
| Atlantic (standaardtijd) | (UTC-04:00) Atlantic Time (Canada) |
| Venezuela (standaardtijd) | (UTC-04:00) Caracas |
| Centraal Brazilië (standaardtijd) | (UTC-04:00) Cuiaba |
| SA Western (standaardtijd) | (UTC-04:00) Georgetown, La Paz, Manaus, San Juan |
| Pacific SA (standaardtijd) | (UTC-04:00) Santiago |
| Newfoundland (standaardtijd) | (UTC-03:30) Newfoundland |
| Tocantins (standaardtijd) | (UTC-03:00) Araguaina |
| E. Zuid-Amerika (standaardtijd) | (UTC-03:00) Brasilia |
| SA Eastern (standaardtijd) | (UTC-03:00) Cayenne, Fortaleza |
| Argentinië (standaardtijd) | (UTC-03:00) Buenos Aires |
| Groenland (standaardtijd) | (UTC-03:00) Groenland |
| Montevideo (standaardtijd) | (UTC-03:00) Montevideo |
| Magallanes (standaardtijd) | (UTC-03:00) Punta Arenas |
| Saint-Pierre (standaardtijd) | (UTC-03:00) Saint-Pierre en Miquelon |
| Bahia standaardtijd | (UTC-03:00) Salvador |
| UTC-02 | (UTC-02:00) Coordinated Universal Time-02 |
| Mid-Atlantic (standaardtijd) | (UTC-02:00) Mid-Atlantic - Oud |
| Azoren (standaardtijd) | (UTC-01:00) Azoren |
| Cape Verde Standard Time | (UTC-01:00) Cabo Verde |
| UTC | (UTC) Coordinated Universal Time |
| GMT (standaardtijd) | (UTC+00:00) Dublin, Edinburgh, Lissabon, Londen |
| Greenwich (standaardtijd) | (UTC+00:00) Monrovia, Reykjavik |
| W. Europa (standaardtijd) | (UTC+01:00) Amsterdam, Berlijn, Bern, Rome, Stockholm, Wenen |
| Centraal-Europa (standaardtijd) | (UTC+01:00) Belgrado, Bratislava, Boedapest, Ljubljana, Praag |
| Romance (standaardtijd) | (UTC+01:00) Brussel, Kopenhagen, Madrid, Parijs |
| Marokko (standaardtijd) | (UTC +01:00) Casablanca |
| Standaardtijd van Sao Tomé | (UTC+01:00) Sao Tomé |
| Europa centraal (standaardtijd) | (UTC+01:00) Sarajevo, Skopje, Warschau, Zagreb |
| W. Centraal-Afrika (standaardtijd) | (UTC+01:00) Centraal-Afrika - West |
| Jordanië (standaardtijd) | (UTC+02:00) Amman |
| GTB (standaardtijd) | (UTC+02:00) Athene, Boekarest |
| Midden-Oosten (standaardtijd) | (UTC+02:00) Beiroet |
| Egypte (standaardtijd) | (UTC+02:00) Caïro |
| E. Europa (standaardtijd) | (UTC+02:00) Chisinau |
| Syrië standaardtijd | (UTC+02:00) Damascus |
| Westelijke Jordaanoever (standaardtijd) | (UTC+02:00) Gaza, Hebron |
| Zuid-Afrika (standaardtijd) | (UTC+02:00) Harare, Pretoria |
| FLE (standaardtijd) | (UTC+02:00) Helsinki, Kiev, Riga, Sofia, Tallinn, Vilnius |
| Israël (standaardtijd) | (UTC+02:00) Jeruzalem |
| Kaliningrad Standard Time | (UTC+02:00) Kaliningrad |
| Soedan (standaardtijd) | (UTC + 02:00) Khartoem |
| Libië standaardtijd | (UTC+02:00) Tripoli |
| Namibië (standaardtijd) | (UTC + 02:00) Windhoek |
| Arab. schiereiland (st. tijd) | (UTC+03:00) Bagdad |
| Turkije standaardtijd | (UTC+03:00) Istanboel |
| Arabisch schiereiland (st.tijd) | (UTC+03:00) Koeweit, Riyad |
| Belarus (standaardtijd) | (UTC+03:00) Minsk |
| Rusland (standaardtijd) | (UTC+03:00) Moskou, Sint-Petersburg |
| E. Afrika (standaardtijd) | (UTC+03:00) Nairobi |
| Iran (standaardtijd) | (UTC+03:30) Teheran |
| Arabisch schiereiland (st.tijd) | (UTC+04:00) Abu Dhabi, Muscat |
| Astrachan (standaardtijd) | (UTC+04:00) Astrachan, Oeljanovsk |
| Azerbeidzjan (standaardtijd) | (UTC+04:00) Bakoe |
| Rusland Time Zone 3 | (UTC+04:00) Izjevsk, Samara |
| Mauritius (standaardtijd) | (UTC+04:00) Port Louis |
| Saratov (standaardtijd) | (UTC+04:00) Saratov |
| Georgië (standaardtijd) | (UTC+04:00) Tbilisi |
| Volgograd (standaardtijd) | (UTC+04:00) Volgograd |
| Kaukasus (standaardtijd) | (UTC+04:00) Jerevan |
| Afghanistan (standaardtijd) | (UTC+04:30) Kabul |
| West Asia (standaardtijd) | (UTC+05:00) Asjchabad, Tasjkent |
| Ekaterinburg (standaardtijd) | (UTC+05:00) Ekaterinenburg |
| Pakistan (standaardtijd) | (UTC+05:00) Islamabad, Karachi |
| India (standaardtijd) | (UTC+05:30) Chennai, Kolkata, Mumbai, New Delhi |
| Sri Lanka (standaardtijd) | (UTC+05:30) Sri Jayawardenepura |
| Nepal (standaardtijd) | (UTC+05:45) Kathmandu |
| Central Asia (standaardtijd) | (UTC+06:00) Astana |
| Bangladesh standaardtijd | (UTC+06:00) Dhaka |
| Omsk (standaardtijd) | (UTC+06:00) Omsk |
| Myanmar (standaardtijd) | (UTC+06:30) Yangon (Rangoon) |
| Zuidoost-Azië (standaardtijd) | (UTC+07:00) Bangkok, Hanoi, Jakarta |
| Altaj (standaardtijd) | (UTC+07:00) Barnaul, Gorno-Altajsk |
| W. Mongolia Standard Time | (UTC+07:00) Hovd |
| North Asia (standaardtijd) | (UTC+07:00) Krasnoyarsk |
| N. Central Asia (standaardtijd) | (UTC+07:00) Novosibirsk |
| Tomsk (standaardtijd) | (UTC+07:00) Tomsk |
| China (standaardtijd) | (UTC+08:00) Beijing, Chongqing, Hongkong, Urumqi |
| North Asia East (standaardtijd) | (UTC+08:00) Irkoetsk |
| Singapore (standaardtijd) | (UTC+08:00) Kuala Lumpur, Singapore |
| W. Australië (standaardtijd) | (UTC+08:00) Perth |
| Taipei (standaardtijd) | (UTC+08:00) Taipei |
| Ulaanbaatar standaardtijd | (UTC+08:00) Ulaanbaatar |
| Australië - centraal-west (standaardtijd) | (UTC+08:45) Eucla |
| Transbaikal (standaardtijd) | (UTC+09:00) Chita |
| Tokio (standaardtijd) | (UTC+09:00) Osaka, Sapporo, Tokio |
| Noord-Korea (standaardtijd) | (UTC + 09:00) Pyongyang |
| Korea (standaardtijd) | (UTC+09:00) Seoul |
| Yakutsk Standard Time | (UTC+09:00) Jakoetsk |
| Cen. Australië (standaardtijd) | (UTC+09:30) Adelaide |
| Australië - centraal (standaardtijd) | (UTC+09:30) Darwin |
| E. Australië (standaardtijd) | (UTC+10:00) Brisbane |
| Australië - oost (standaardtijd) | (UTC+10:00) Canberra, Melbourne, Sydney |
| West Pacific (standaardtijd) | (UTC+10:00) Guam, Port Moresby |
| Tasmanië (standaardtijd) | (UTC+10:00) Hobart |
| Wladiwostok (standaardtijd) | (UTC+10:00) Vladivostok |
| Lord Howe Island (standaardtijd) | (UTC+10:30) Lord Howe Island |
| Bougainville (standaardtijd) | (UTC+11:00) Bougainville |
| Rusland-tijdzone 10 | (UTC+11:00) Chokurdakh |
| Magadan standaardtijd | (UTC+11:00) Magadan |
| Norfolk (standaardtijd) | (UTC+11:00) Norfolk |
| Sachalin (standaardtijd) | (UTC+11:00) Sachalin |
| Central Pacific (standaardtijd) | (UTC+11:00) Salomonseilanden, Nieuw-Caledonië |
| Rusland-tijdzone 11 | (UTC+12:00) Anadyr, Petropavlovsk-Kamtsjatski |
| Nieuw-Zeeland (standaardtijd) | (UTC+12:00) Auckland, Wellington |
| UTC+12 | (UTC+12:00) Coordinated Universal Time+12 |
| Fiji (standaardtijd) | (UTC+12:00) Fiji |
| Kamtsjatka (standaardtijd) | (UTC+12:00) Petropavlovsk-Kamtsjatski - Oud |
| Chathameilanden (standaardtijd) | (UTC+12:45) Chathameilanden |
| UTC+13 | (UTC+13:00) Coordinated Universal Time+13 |
| Tonga (standaardtijd) | (UTC+13:00) Nuku'alofa |
| Samoa (standaardtijd) | (UTC+13:00) Samoa |
| Line-eilanden (standaardtijd) | (UTC+14:00) Kiritimati |

## <a name="see-also"></a>Zie ook

- [CURRENT_TIMEZONE (Transact-SQL)](https://docs.microsoft.com/sql/t-sql/functions/current-timezone-transact-sql)
- [AT TIME ZONE (Transact-SQL)](https://docs.microsoft.com/sql/t-sql/queries/at-time-zone-transact-sql)
- [sys.time_zone_info (Transact-SQL)](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-time-zone-info-transact-sql)