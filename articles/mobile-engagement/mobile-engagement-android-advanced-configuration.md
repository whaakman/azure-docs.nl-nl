---
title: Geavanceerde configuratie voor Azure Mobile Engagement Android SDK
description: Beschrijving van de geavanceerde configuratieopties, met inbegrip van de Android Manifest met Azure Mobile Engagement Android SDK
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 37d2c09a-86fa-473d-8987-c7e35a0eb3e8
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: Java
ms.topic: article
ms.date: 10/04/2016
ms.author: piyushjo;ricksal
ms.openlocfilehash: 0301f71c76872714aa1bf727a6c21dd7a63db036
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2017
---
# <a name="advanced-configuration-for-azure-mobile-engagement-android-sdk"></a>Geavanceerde configuratie voor Azure Mobile Engagement Android SDK
> [!div class="op_single_selector"]
> * [Universeel Windows](mobile-engagement-windows-store-advanced-configuration.md)
> * [Windows Phone Silverlight](mobile-engagement-windows-phone-integrate-engagement.md)
> * [iOS](mobile-engagement-ios-integrate-engagement.md)
> * [Android](mobile-engagement-android-advanced-configuration.md)
>
>

Deze procedure wordt beschreven hoe verschillende configuratieopties voor Azure Mobile Engagement Android-apps configureren.

## <a name="prerequisites"></a>Vereisten
[!INCLUDE [Prereqs](../../includes/mobile-engagement-android-prereqs.md)]

## <a name="permission-requirements"></a>Machtigingsvereisten
Sommige opties vereisen specifieke machtigingen, die hier worden weergegeven voor de referentie- en in de regel in de specifieke functie. Deze machtigingen toevoegen aan de AndroidManifest.xml van uw project direct vóór of na de `<application>` label.

De machtiging-code moet eruitzien als het volgende, waarbij u de benodigde machtiging van de volgende tabel invullen.

    <uses-permission android:name="android.permission.[specific permission]"/>


| Machtiging | Wanneer gebruikt |
| --- | --- |
| INTERNET |Vereist. Voor eenvoudige rapportage |
| ACCESS_NETWORK_STATE |Vereist. Voor eenvoudige rapportage |
| RECEIVE_BOOT_COMPLETED |Vereist. Om weer te geven om het middelpunt meldingen na opnieuw opstarten van apparaat |
| WAKE_LOCK |Aanbevolen. Schakelt het verzamelen van gegevens bij gebruik van Wi-Fi of als het scherm is uitgeschakeld |
| TRILLEN |Optioneel. Trillingen ingeschakeld wanneer er worden meldingen ontvangen |
| DOWNLOAD_WITHOUT_NOTIFICATION |Optioneel. Hiermee kunt Android grote afbeelding melding |
| WRITE_EXTERNAL_STORAGE |Optioneel. Hiermee kunt Android grote afbeelding melding |
| ACCESS_COARSE_LOCATION |Optioneel. Rapportage van realtime locatie kunt |
| ACCESS_FINE_LOCATION |Optioneel. Schakelt op basis van een GPS-locatie rapportage |

Vanaf Android M [sommige machtigingen worden beheerd tijdens runtime](mobile-engagement-android-location-reporting.md#android-m-permissions).

Als u al ``ACCESS_FINE_LOCATION``, moet u niet ook gebruiken ``ACCESS_COARSE_LOCATION``.

## <a name="android-manifest-configuration-options"></a>Android Manifest configuratieopties
### <a name="crash-report"></a>Foutrapportage
Als u wilt uitschakelen foutenrapporten, voeg deze code tussen de `<application>` en `</application>` tags:

    <meta-data android:name="engagement:reportCrash" android:value="false"/>

### <a name="burst-threshold"></a>Burst drempelwaarde
Standaard wordt de Engagement-service-rapporten Logboeken in realtime. Als uw rapport toepassingslogboeken vaak verschillen, is het beter voor het bufferen van de logboeken en om te rapporteren ze allemaal tegelijk op een vaste tijd base ('burst modus' genoemd). Om dit te doen, voeg deze code tussen de `<application>` en `</application>` tags:

    <meta-data android:name="engagement:burstThreshold" android:value="{interval between too bursts (in milliseconds)}"/>

Burst-modus verhoogt de batterij enigszins, maar heeft een invloed op de Engagement-Monitor: alle sessies en taken duur zijn afgerond op de drempelwaarde burst (dus sessies en korter zijn dan de drempelwaarde burst is mogelijk niet zichtbaar taken). De drempelwaarde burst mag niet langer zijn dan 30000 (30s).

### <a name="session-timeout"></a>Sessietime-out
 U kunt een activiteit beëindigen door op de **Start** of **terug** sleutel, door de telefoon inactief of door te gaan naar een andere toepassing. Standaard wordt een sessie tien seconden na het einde van de laatste activiteit beëindigd. Zo voorkomt u een sessie splitsing telkens wanneer de gebruiker wordt afgesloten en retourneert snel aan de toepassing dit kan gebeuren wanneer de gebruiker een afbeelding opneemt controleert een melding, enzovoort. U wilt wijzigen van deze parameter. Om dit te doen, voeg deze code tussen de `<application>` en `</application>` tags:

    <meta-data android:name="engagement:sessionTimeout" android:value="{session timeout (in milliseconds)}"/>

## <a name="disable-log-reporting"></a>Melden van logboek uitschakelen
### <a name="using-a-method-call"></a>Met behulp van een methodeaanroep van
Als u stoppen met het verzenden van Logboeken Engagement wilt, kunt u bellen:

    EngagementAgent.getInstance(context).setEnabled(false);

Deze aanroep is permanent: een gedeelde voorkeuren-bestand wordt gebruikt.

Als Engagement actief is wanneer u deze functie aanroept, duurt een minuut voor de service te stoppen. Maar deze start de service helemaal de volgende keer dat Won't u de toepassing wordt gestart.

U kunt reporting opnieuw door het aanroepen van dezelfde functie met logboek inschakelen `true`.

### <a name="integration-in-your-own-preferenceactivity"></a>Integratie in uw eigen`PreferenceActivity`
In plaats van deze functie aanroept, kunt u deze instelling ook integreren rechtstreeks in uw bestaande `PreferenceActivity`.

U kunt Engagement om uw voorkeurenbestand (met de gewenste modus) te gebruiken kunt configureren in de `AndroidManifest.xml` het bestand met `application meta-data`:

* De `engagement:agent:settings:name` sleutel wordt gebruikt voor het definiëren van de naam van het bestand gedeelde voorkeuren.
* De `engagement:agent:settings:mode` sleutel wordt gebruikt voor het definiëren van de modus van het bestand gedeelde voorkeuren. Gebruik dezelfde modus als in uw `PreferenceActivity`. De modus moet worden doorgegeven als een getal: als u een combinatie van constante vlaggen in uw code gebruikt, controleert u de totale waarde.

Maakt gebruik van engagement altijd de `engagement:key` Booleaanse sleutel in het voorkeurenbestand voor het beheren van deze instelling.

Het volgende voorbeeld van `AndroidManifest.xml` ziet u de standaardwaarden:

    <application>
        [...]
        <meta-data
          android:name="engagement:agent:settings:name"
          android:value="engagement.agent" />
        <meta-data
          android:name="engagement:agent:settings:mode"
          android:value="0" />

Vervolgens kunt u toevoegen een `CheckBoxPreference` in de indeling van uw voorkeur zoals de volgende:

    <CheckBoxPreference
      android:key="engagement:enabled"
      android:defaultValue="true"
      android:title="Use Engagement"
      android:summaryOn="Engagement is enabled."
      android:summaryOff="Engagement is disabled." />
