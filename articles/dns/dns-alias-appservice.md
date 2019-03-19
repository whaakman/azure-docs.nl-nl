---
title: Met load balancing Azure-web-apps hosten in de apex van de zone
description: Een Azure DNS-alias-record met load balancing web om apps te hosten in de apex van de zone gebruiken
services: dns
author: vhorne
ms.service: dns
ms.topic: article
ms.date: 11/3/2018
ms.author: victorh
ms.openlocfilehash: b08eae072c2fbe420401424baf97a25b4cbbe87b
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58086323"
---
# <a name="host-load-balanced-azure-web-apps-at-the-zone-apex"></a>Met load balancing Azure-web-apps hosten in de apex van de zone

Het DNS-protocol wordt voorkomen dat de toewijzing van iets anders dan een A of AAAA-record in de apex van de zone. Het toppunt van een voorbeeld van de zone is contoso.com. Deze beperking geeft een probleem voor toepassingseigenaren van een die beschikken over Netwerktaakverdeling toepassingen achter Traffic Manager. Het is niet mogelijk om te verwijzen naar de Traffic Manager-profiel van de zone apexrecord. Als gevolg hiervan moeten toepassingseigenaren een tijdelijke oplossing gebruiken. Een omleiding op het niveau van de toepassing moet van het toppunt van de zone omleiden naar een ander domein. Een voorbeeld is een omleiding van contoso.com naar www\.contoso.com. Deze overeenkomst geeft een single point of failure voor de omleidingsfunctie.

Dit probleem is met aliasrecords, niet meer bestaat. Toepassingseigenaren van kunnen nu hun apexrecord zone verwijzen naar een Traffic Manager-profiel met externe eindpunten. Toepassingseigenaren van de kunnen verwijzen naar dezelfde Traffic Manager-profiel dat wordt gebruikt voor andere domeinen in hun DNS-zone.

Bijvoorbeeld: contoso.com en een www\.contoso.com kan verwijzen naar dezelfde Traffic Manager-profiel. Dit geldt zolang het Traffic Manager-profiel alleen externe eindpunten zijn geconfigureerd heeft.

In dit artikel leert u hoe u een alias-record voor het toppunt van uw domein maken en configureren van de eindpunten van uw Traffic Manager-profiel voor uw web-apps.

Als u nog geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="prerequisites"></a>Vereisten

U moet een beschikbare domeinnaam hebben die u in Azure DNS kunt hosten om te testen. U moet het volledige beheer over dit domein hebben. Volledig beheer betekent ook de mogelijkheid om naamserverrecords (NS) voor het domein in te stellen.

Zie voor instructies voor het hosten van uw domein in Azure DNS [zelfstudie: Uw domein hosten in Azure DNS](dns-delegate-domain-azure-dns.md) voor meer informatie.

Het voorbeelddomein dat wordt gebruikt voor deze zelfstudie is contoso.com, maar u moet uw eigen domeinnaam gebruiken.

## <a name="create-a-resource-group"></a>Een resourcegroep maken

Maak een resourcegroep voor het opslaan van alle resources in dit artikel gebruikt.

## <a name="create-app-service-plans"></a>Maken van App Service-plannen

Maak twee Web App Service-plannen in de resourcegroep met behulp van de volgende tabel voor informatie over de configuratie. Zie voor meer informatie over het maken van een App Service-plan [beheren van een App Service-plan in Azure](../app-service/app-service-plan-manage.md).


|Name  |Besturingssysteem  |Locatie  |Prijscategorie  |
|---------|---------|---------|---------|
|ASP-01     |Windows|US - oost|Dev/Test D1 gedeeld|
|ASP-02     |Windows|US - centraal|Dev/Test D1 gedeeld|

## <a name="create-app-services"></a>App-Services maken

Maak twee web-apps, één in elke App Service-plan.

1. In de linkerbovenhoek van de Azure portal-pagina, klikt u op **een resource maken**.
2. Type **Web-app** in de zoekbalk en druk op Enter.
3. Klik op **Web-App**.
4. Klik op **Create**.
5. Accepteer de standaardinstellingen en gebruik de volgende tabel om te configureren van de twee web-apps:

   |Name<br>(moet uniek zijn binnen. azurewebsites.net)|Resourcegroep |App Service-Plan /-locatie
   |---------|---------|---------|
   |App-01|Bestaande gebruiken<br>Uw resourcegroep selecteren|ASP-01(East US)|
   |App-02|Bestaande gebruiken<br>Uw resourcegroep selecteren|ASP-02(Central US)|

### <a name="gather-some-details"></a>Sommige gegevens verzamelen

Nu moet u Noteer de naam van de IP-adres en de hostnaam voor de apps.

1. Open de resourcegroep en klik op uw eerste app (**App-01** in dit voorbeeld).
2. Klik in de linkerkolom op **eigenschappen**.
3. Noteer het adres onder **URL**, en klikt u onder **uitgaande IP-adressen** Let op het eerste IP-adres in de lijst. U gebruikt deze gegevens later bij het configureren van uw Traffic Manager-eindpunten.
4. Herhaal dit voor **App-02**.

## <a name="create-a-traffic-manager-profile"></a>Een Traffic Manager-profiel maken

Een Traffic Manager-profiel maken in de resourcegroep. Gebruik de standaardinstellingen en typ een unieke naam binnen de naamruimte trafficmanager.net.

Zie voor meer informatie over het maken van een Traffic Manager-profiel [Quick Start: Maken van een Traffic Manager-profiel voor een maximaal beschikbare webtoepassing](../traffic-manager/quickstart-create-traffic-manager-profile.md).

### <a name="create-endpoints"></a>Eindpunten maken

U kunt nu de eindpunten voor de twee web-apps maken.

1. Open de resourcegroep en klikt u op uw Traffic Manager-profiel.
2. Klik in de linkerkolom op **eindpunten**.
3. Klik op **Add**.
4. Gebruik de volgende tabel de eindpunten configureren:

   |Type  |Name  |Doel  |Locatie  |Instellingen voor aangepaste headers|
   |---------|---------|---------|---------|---------|
   |Extern eindpunt     |End-01|IP-adres dat u hebt genoteerd voor App-01|US - oost|host:\<de URL die u hebt genoteerd voor App-01\><br>Voorbeeld: **host: de app-01.azurewebsites.net**|
   |Extern eindpunt     |End-02|U hebt genoteerd voor App-02 IP-adres|US - centraal|host:\<de URL die u hebt genoteerd voor App-02\><br>Voorbeeld: **host: de app-02.azurewebsites.net**

## <a name="create-dns-zone"></a>DNS-zone maken

U kunt een bestaande DNS-zone gebruiken voor het testen of u kunt een nieuwe zone maken. Als u wilt maken en delegeren van een nieuwe DNS-zone in Azure, Zie [zelfstudie: Uw domein hosten in Azure DNS](dns-delegate-domain-azure-dns.md) voor meer informatie.

### <a name="add-the-alias-record-set"></a>De alias-recordset toevoegen

Als uw DNS-zone klaar is, kunt u een alias-record voor het toppunt van de zone toevoegen.

1. Open de resourcegroep en klik op de DNS-zone.
2. Klik op **Recordset**.
3. Toevoegen van de recordset met behulp van de volgende tabel:

   |Name  |Type  |De recordset alias  |Aliastype  |Azure-resource|
   |---------|---------|---------|---------|-----|
   |@     |A|Ja|Azure-resource|Traffic Manager - uw profiel|

## <a name="add-custom-hostnames"></a>Aangepaste hostnamen toevoegen

Een aangepaste hostnaam toevoegen aan beide web-apps.

1. Open de resourcegroep en klikt u op uw eerste web-app.
2. Klik in de linkerkolom op **aangepaste domeinen**.
3. Klik op **hostnaam toevoegen**.
4. Typ onder de hostnaam, de domeinnaam van uw. Bijvoorbeeld contoso.com.

   Uw domein moet worden gevalideerd en groen vinkje weergegeven naast weergeven **beschikbaarheid van hostnaam** en **domeineigendom**.
5. Klik op **hostnaam toevoegen**.
6. Om te zien van de nieuwe hostnaam onder **hostnamen toegewezen aan site**, vernieuw de browser. Het vernieuwen op de pagina komt niet altijd meteen wijzigingen.
7. Herhaal deze procedure voor uw tweede web-app.

## <a name="test-your-web-apps"></a>Testen van uw web-apps

U kunt nu testen om ervoor te zorgen dat u uw web-app kunt bereiken en dat het wordt geladen met gelijke taakverdeling.

1. Open een webbrowser en Ga naar uw domein. Bijvoorbeeld contoso.com. Hier ziet u de standaardpagina van de web-app.
2. Uw eerste web-app stoppen.
3. Sluit uw internetbrowser en wacht een paar minuten.
4. Start uw webbrowser en Ga naar uw domein. Nog steeds ziet u de standaardpagina van de web-app.
5. Uw tweede web-app stoppen.
6. Sluit uw internetbrowser en wacht een paar minuten.
7. Start uw webbrowser en Ga naar uw domein. Hier ziet u fout 403, waarmee wordt aangegeven dat de web-app is gestopt.
8. Uw tweede WebApp te starten.
9. Sluit uw internetbrowser en wacht een paar minuten.
10. Start uw webbrowser en Ga naar uw domein. U ziet de standaardpagina van de web-app opnieuw.

## <a name="next-steps"></a>Volgende stappen

Voor meer informatie over de aliasrecords, raadpleegt u de volgende artikelen:

- [Zelfstudie: Een alias-record om te verwijzen naar een openbaar IP-adres van Azure configureren](tutorial-alias-pip.md)
- [Zelfstudie: Een alias-record ter ondersteuning van het toppunt van domeinnamen met Traffic Manager configureren](tutorial-alias-tm.md)
- [Veelgestelde vragen over DNS](https://docs.microsoft.com/azure/dns/dns-faq#alias-records)
