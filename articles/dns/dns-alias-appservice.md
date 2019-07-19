---
title: Host load balanced Azure web apps op de zone Apex
description: Een Azure DNS alias record gebruiken om de web-apps met taak verdeling te hosten op de zone Apex
services: dns
author: vhorne
ms.service: dns
ms.topic: article
ms.date: 7/13/2019
ms.author: victorh
ms.openlocfilehash: 7d20ef750aa4556a73852982631423d3d08271f5
ms.sourcegitcommit: 470041c681719df2d4ee9b81c9be6104befffcea
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/12/2019
ms.locfileid: "67854117"
---
# <a name="host-load-balanced-azure-web-apps-at-the-zone-apex"></a>Host load balanced Azure web apps op de zone Apex

Het DNS-protocol voor komt dat de toewijzing van iets anders dan een A-of AAAA-record bij de zone Apex. Een voor beeld van een zone Apex is contoso.com. Deze beperking geeft een probleem met de eigen aren van toepassingen die achter Traffic Manager toepassingen met taak verdeling hebben. Het is niet mogelijk om te verwijzen naar het Traffic Manager profiel in de zone Apex-record. Als gevolg hiervan moeten toepassings eigenaren gebruikmaken van een tijdelijke oplossing. Een omleiding op de toepassingslaag moet worden omgeleid van de zone naar een ander domein. Een voor beeld is een omleiding van contoso.com\.naar www contoso.com. Deze indeling bevat een Single Point of Failure voor de omleidings functie.

Met alias records bestaat dit probleem niet meer. Toepassings eigenaren kunnen nu de Apex-record van de zone naar een Traffic Manager profiel met externe eind punten aanwijzen. Toepassings eigenaren kunnen verwijzen naar hetzelfde Traffic Manager-profiel dat wordt gebruikt voor elk ander domein binnen hun DNS-zone.

Contoso.com en www\.contoso.com kunnen bijvoorbeeld verwijzen naar hetzelfde Traffic Manager-profiel. Dit is het geval zolang het Traffic Manager profiel alleen externe eind punten heeft geconfigureerd.

In dit artikel leert u hoe u een alias record kunt maken voor uw domein Apex en hoe u de eind punten van uw Traffic Manager-profiel kunt configureren voor uw web-apps.

Als u nog geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="prerequisites"></a>Vereisten

U moet een beschikbare domeinnaam hebben die u in Azure DNS kunt hosten om te testen. U moet het volledige beheer over dit domein hebben. Volledig beheer betekent ook de mogelijkheid om naamserverrecords (NS) voor het domein in te stellen.

Zie [zelf studie voor instructies voor het hosten van uw domein in azure DNS. Uw domein hosten in Azure DNS](dns-delegate-domain-azure-dns.md) voor meer informatie.

Het voorbeelddomein dat wordt gebruikt voor deze zelfstudie is contoso.com, maar u moet uw eigen domeinnaam gebruiken.

## <a name="create-a-resource-group"></a>Een resourcegroep maken

Een resource groep maken voor het opslaan van alle resources die in dit artikel worden gebruikt.

## <a name="create-app-service-plans"></a>App Service-abonnementen maken

Maak twee Web App Service-abonnementen in uw resource groep met behulp van de volgende tabel voor configuratie-informatie. Zie [een app service-abonnement beheren in azure](../app-service/app-service-plan-manage.md)voor meer informatie over het maken van een app service-abonnement.


|Name  |Besturingssysteem  |Location  |Prijscategorie  |
|---------|---------|---------|---------|
|ASP-01     |Windows|East US|Dev/test D1-gedeeld|
|ASP-02     |Windows|US - centraal|Dev/test D1-gedeeld|

## <a name="create-app-services"></a>App Services maken

Maak twee web-apps, één in elk App Service-abonnement.

1. Klik in de linkerbovenhoek van de pagina Azure Portal op **een resource maken**.
2. Typ **Web-app** in de zoek balk en druk op ENTER.
3. Klik op **Web-app**.
4. Klik op **Create**.
5. Accepteer de standaard waarden en gebruik de volgende tabel om de twee web-apps te configureren:

   |Name<br>(moet uniek zijn binnen. azurewebsites.net)|Resourcegroep |Abonnement/locatie App Service
   |---------|---------|---------|
   |App-01|Bestaande gebruiken<br>Uw resourcegroep selecteren|ASP-01 (VS-Oost)|
   |App-02|Bestaande gebruiken<br>Uw resourcegroep selecteren|ASP-02 (VS-Centraal)|

### <a name="gather-some-details"></a>Enkele details verzamelen

Nu moet u het IP-adres en de hostnaam voor de apps noteren.

1. Open de resource groep en klik op uw eerste app (**app-01** in dit voor beeld).
2. Klik in de linkerkolom op **Eigenschappen**.
3. Noteer het adres onder **URL**en noteer het eerste IP-adres in de lijst onder **uitgaande IP-adressen** . U gebruikt deze informatie later wanneer u uw Traffic Manager-eind punten configureert.
4. Herhaal dit voor **app-02**.

## <a name="create-a-traffic-manager-profile"></a>Een Traffic Manager-profiel maken

Maak een Traffic Manager-profiel in de resource groep. Gebruik de standaard waarden en typ een unieke naam binnen de naam ruimte trafficmanager.net.

Voor informatie over het maken van een Traffic Manager profiel [raadpleegt u Quick Start: Een Traffic Manager-profiel maken voor een Maxi maal beschik bare webtoepassing](../traffic-manager/quickstart-create-traffic-manager-profile.md).

### <a name="create-endpoints"></a>Eindpunten maken

U kunt nu de eind punten voor de twee web-apps maken.

1. Open de resource groep en klik op uw Traffic Manager profiel.
2. Klik in de linkerkolom op **eind punten**.
3. Klik op **Toevoegen**.
4. Gebruik de volgende tabel om de eind punten te configureren:

   |type  |Name  |Doel  |Location  |Instellingen voor aangepaste header|
   |---------|---------|---------|---------|---------|
   |Extern eind punt     |End-01|IP-adres dat u hebt genoteerd voor app-01|East US|host:\<de URL die u hebt vastgelegd voor app-01\><br>Voor beeld: **host: app-01.azurewebsites.net**|
   |Extern eind punt     |End-02|IP-adres dat u hebt vastgelegd voor app-02|US - centraal|host:\<de URL die u voor app-02 hebt vastgelegd\><br>Voor beeld: **host: app-02.azurewebsites.net**

## <a name="create-dns-zone"></a>DNS-zone maken

U kunt een bestaande DNS-zone gebruiken om te testen of u kunt een nieuwe zone maken. Zie [zelf studie voor het maken en delegeren van een nieuwe DNS-zone in Azure: Uw domein hosten in Azure DNS](dns-delegate-domain-azure-dns.md) voor meer informatie.

### <a name="add-the-alias-record-set"></a>De set met alias records toevoegen

Wanneer uw DNS-zone klaar is, kunt u een alias record voor de zone Apex toevoegen.

1. Open de resource groep en klik op de DNS-zone.
2. Klik op **Recordset**.
3. Voeg de recordset toe met behulp van de volgende tabel:

   |Name  |type  |Alias records instellen  |Alias type  |Azure-resource|
   |---------|---------|---------|---------|-----|
   |@     |A|Ja|Azure-resource|Traffic Manager-uw profiel|

## <a name="add-custom-hostnames"></a>Aangepaste hostnamen toevoegen

Voeg een aangepaste hostnaam toe aan beide web-apps.

1. Open de resource groep en klik op uw eerste web-app.
2. Klik in de linkerkolom op **aangepaste domeinen**.
3. Klik op **hostnaam toevoegen**.
4. Typ onder hostnaam de naam van uw domein. Bijvoorbeeld contoso.com.

   Uw domein moet validatie door geven en groene vinkjes weer geven naast **hostnamen Beschik baarheid** en **domein eigendom**.
5. Klik op **hostnaam toevoegen**.
6. Vernieuw uw browser om de nieuwe hostnaam te bekijken onder **hostnamen die zijn toegewezen aan de site**. Op de pagina vernieuwen worden niet altijd de wijzigingen meteen weer gegeven.
7. Herhaal deze procedure voor uw tweede web-app.

## <a name="test-your-web-apps"></a>Uw Web-Apps testen

U kunt nu testen om er zeker van te zijn dat u uw web-app kunt bereiken en dat de taak verdeling wordt gebalanceerd.

1. Open een webbrowser en blader naar uw domein. Bijvoorbeeld contoso.com. De pagina standaard web-app wordt weer geven.
2. Stop uw eerste web-app.
3. Sluit de webbrowser en wacht enkele minuten.
4. Start uw webbrowser en blader naar uw domein. De standaard pagina voor web-apps moet nog steeds worden weer geven.
5. Stop uw tweede web-app.
6. Sluit de webbrowser en wacht enkele minuten.
7. Start uw webbrowser en blader naar uw domein. U ziet fout 403 om aan te geven dat de web-app is gestopt.
8. Start uw tweede web-app.
9. Sluit de webbrowser en wacht enkele minuten.
10. Start uw webbrowser en blader naar uw domein. De pagina standaard web-app wordt opnieuw weer geven.

## <a name="next-steps"></a>Volgende stappen

Raadpleeg de volgende artikelen voor meer informatie over alias records:

- [Zelfstudie: Een alias record configureren om te verwijzen naar een openbaar IP-adres van Azure](tutorial-alias-pip.md)
- [Zelfstudie: Een alias record configureren ter ondersteuning van Apex-domein namen met Traffic Manager](tutorial-alias-tm.md)
- [Veelgestelde vragen over DNS](https://docs.microsoft.com/azure/dns/dns-faq#alias-records)

Zie [een actieve DNS-naam migreren naar Azure app service](../app-service/manage-custom-dns-migrate-domain.md)voor meer informatie over het migreren van een actieve DNS-naam.
