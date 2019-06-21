---
title: Hulpprogramma - Azure implementeren
description: Het installeren van een gebruikersinterfaceprogramma voor het beheren van virtuele Windows-bureaublad preview resources.
services: virtual-desktop
author: ChJenk
ms.service: virtual-desktop
ms.topic: tutorial
ms.date: 06/04/2019
ms.author: v-chjenk
ms.openlocfilehash: 275fec5fb696a7e1352bbddccd288863e984b796
ms.sourcegitcommit: 82efacfaffbb051ab6dc73d9fe78c74f96f549c2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/20/2019
ms.locfileid: "67304558"
---
# <a name="tutorial-deploy-a-management-tool"></a>Zelfstudie: Een hulpprogramma voor beheer implementeren

Het hulpprogramma biedt een gebruikersinterface (UI) voor het beheren van resources van Microsoft Virtual Desktop Preview. In deze zelfstudie leert u over het implementeren en verbinding maken met het beheerprogramma.

>[!NOTE]
>Deze instructies zijn voor een virtuele Windows-bureaublad Preview-specifieke configuratie die kan worden gebruikt met de bestaande processen van uw organisatie.

## <a name="important-considerations"></a>Belangrijke overwegingen

Omdat de app toestemming nodig om te communiceren met virtuele Windows-bureaublad, dit hulpprogramma biedt geen ondersteuning voor scenario's voor Business-to-Business (B2B). Van iedere tenant Azure Active Directory (AAD)-abonnement moet een eigen afzonderlijke implementatie van het beheerprogramma.

Dit hulpprogramma is een voorbeeld. Microsoft biedt belangrijke beveiligings- en kwaliteitsupdates zijn. De [broncode is beschikbaar in GitHub](https://github.com/Azure/RDS-Templates/tree/master/wvd-templates/wvd-management-ux/deploy). Klanten en partners worden aangemoedigd om het hulpprogramma voor aanpassen aan de zakelijke behoeften aanpassen.

## <a name="what-you-need-to-run-the-azure-resource-manager-template"></a>Wat u nodig hebt om uit te voeren van de Azure Resource Manager-sjabloon

Voordat u de Azure Resource Manager-sjabloon implementeert, moet u een Azure Active Directory-gebruiker om de management-gebruikersinterface te implementeren. Deze gebruiker moet het volgende doen:

- Hebt u Azure multi-factor Authentication (MFA) is uitgeschakeld
- Machtiging voor het maken van resources in uw Azure-abonnement hebt
- Machtiging voor het maken van een Azure AD-toepassing hebben. Volg deze stappen om te controleren of de gebruiker heeft de [vereiste machtigingen](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#required-permissions).

Na de implementatie van de Azure Resource Manager-sjabloon, moet u start de beheersgebruikersinterface om te valideren. Deze gebruiker moet het volgende doen:
- Beschikt over een roltoewijzing weergeven of bewerken van uw tenant virtuele Windows-bureaublad

## <a name="run-the-azure-resource-manager-template-to-provision-the-management-ui"></a>Uitvoeren van de Azure Resource Manager-sjabloon voor het inrichten van de gebruikersinterface voor beheer

Voordat u begint, zorg ervoor dat de server en client-apps hebben toestemming door naar de pagina de [Windows virtuele bureaublad toestemming geven pagina](https://rdweb.wvd.microsoft.com) voor de Azure Active Directory (AAD) weergegeven.

Volg deze instructies voor het implementeren van de Azure Resource Manager-sjabloon:

1. Ga naar de [Azure RDS-sjablonen in de GitHub-pagina](https://github.com/Azure/RDS-Templates/tree/master/wvd-templates/wvd-management-ux/deploy).
2. De sjabloon implementeren in Azure.
    - Als u in een Enterprise-abonnement implementeert, schuif naar beneden en selecteer **implementeren in Azure**. Zie [richtlijnen voor Sjabloonparameters](#guidance-for-template-parameters).
    - Als u in een Cloud Solution Provider-abonnement implementeert, volgt u deze instructies voor het implementeren in Azure:
        1. Schuif naar beneden en met de rechtermuisknop op **implementeren in Azure**en selecteer vervolgens **kopie koppelingslocatie**.
        2. Open een teksteditor zoals Kladblok en plak er de koppeling.
        3. Direct na het <https://portal.azure.com/> en voordat de hashtag (#), voer een apenstaartje (@) gevolgd door de naam van de tenant-domein. Hier volgt een voorbeeld van de indeling: <https://portal.azure.com/@Contoso.onmicrosoft.com#create/>.
        4. Meld u aan de Azure-portal aan als een gebruiker met Administrator-/ Inzenderrechten machtigingen voor de Cloud Solution Provider-abonnement.
        5. Plak de koppeling die u hebt gekopieerd naar de teksteditor in de adresbalk.

### <a name="guidance-for-template-parameters"></a>Richtlijnen voor Sjabloonparameters
Dit is hoe u parameters opgeven voor het configureren van het hulpprogramma:

- Dit is de URL van de broker extern bureaublad:  <https://rdbroker.wvd.microsoft.com/>
- Dit is de bron-URL:  <https://mrs-prod.ame.gbl/mrs-RDInfra-prod>
- AAD-referenties met uitgeschakeld om te melden bij Azure MFA gebruiken. Zie [wat u nodig hebt om uit te voeren van de Azure Resource Manager-sjabloon](#what-you-need-to-run-the-azure-resource-manager-template).
- Gebruik een unieke naam voor de toepassing die wordt geregistreerd in uw Azure Active Directory voor het beheerprogramma; bijvoorbeeld: Apr3UX.

## <a name="provide-consent-for-the-management-tool"></a>Geef toestemming voor het beheerprogramma

Na de GitHub Azure Resource Manager sjabloon is voltooid, vindt u een resourcegroep die twee app-services samen met een app service-plan in de Azure-portal.

Voordat u zich aanmelden en het hulpprogramma gebruikt, moet u toestemming geven voor de nieuwe Azure Active Directory-toepassing die is gekoppeld aan het beheerprogramma. Door te geven toestemming kunt toestaat u het beheerprogramma voor virtuele Windows-bureaublad management aanroepen namens de gebruiker die aangemeld bij het hulpprogramma.

![Een schermafbeelding van de machtigingen worden opgegeven wanneer u toestemming voor het beheerprogramma van de gebruikersinterface.](media/management-ui-delegated-permissions.png)

Om te bepalen welke gebruiker kunt u zich aanmeldt bij het hulpprogramma, gaat u naar uw [instellingenpagina voor Azure Active Directory-gebruiker](https://portal.azure.com/#blade/Microsoft_AAD_IAM/StartboardApplicationsMenuBlade/UserSettings/menuId/) en noteer de waarde voor **gebruikers toestemming kunnen geven voor apps die toegang tot bedrijfsgegevens namens hen** .

![Een schermopname als gebruikers toestemming voor toepassingen voor alleen de gebruiker kunnen verlenen.](media/management-ui-user-consent-allowed.png)

- Als de waarde is ingesteld op **Ja**, kunt u zich aanmelden met een gebruikersaccount in de Azure Active Directory en toestemming geven voor alleen die gebruiker. Als u zich aanmeldt bij het hulpprogramma met een andere gebruiker later, moet u opnieuw een dezelfde toestemming heeft gegeven uitvoeren.
- Als de waarde is ingesteld op **Nee**, moet u zich aanmeldt als een globale beheerder in de Azure Active Directory en toestemming van een beheerder bieden voor alle gebruikers in de map. Er zijn geen andere gebruikers wordt geconfronteerd met een toestemmingsprompt.


Als u welke gebruiker toestemming geven om u gebruikt besluit, volgt u deze instructies te geven van toestemming voor het hulpprogramma:

1. Ga naar uw Azure-resources, selecteert u de Azure App Services-resource met de naam die u hebt opgegeven in de sjabloon (bijvoorbeeld Apr3UX) en navigeer naar de URL die is gekoppeld. bijvoorbeeld, <https://rdmimgmtweb-210520190304.azurewebsites.net>.
2. Meld u aan met het juiste Azure Active Directory-gebruikersaccount.
3. Als u met een globale beheerder geverifieerd, kunt u nu het selectievakje in om te selecteren **toestemming namens uw organisatie**. Selecteer **accepteren** toestemming geven om.
   
   ![Een schermafbeelding van de volledige instemmingspagina dat de gebruiker of beheerder ziet.](media/management-ui-consent-page.png)

Dit nu gaat u naar het beheerprogramma.

## <a name="use-the-management-tool"></a>Gebruik het hulpprogramma

Nadat u toestemming voor de organisatie of voor een opgegeven gebruiker, hebt u toegang tot het hulpprogramma op elk gewenst moment.

Volg deze instructies om het hulpprogramma te starten:

1. Selecteer de Azure App Services-resource met de naam die u hebt opgegeven in de sjabloon (bijvoorbeeld Apr3UX) en navigeer naar de URL die is gekoppeld. bijvoorbeeld, <https://rdmimgmtweb-210520190304.azurewebsites.net>.
2. Meld u aan met uw virtueel bureaublad van Windows-referenties.
3. Wanneer u hierom wordt gevraagd om een Tenant-groep te kiezen, selecteert u **standaard Tenant groep** uit de vervolgkeuzelijst.

> [!NOTE]
> Als u een aangepaste groep van de Tenant hebt, voer de naam handmatig in plaats van te kiezen uit de vervolgkeuzelijst.

## <a name="next-steps"></a>Volgende stappen

Nu dat u hebt geleerd over het implementeren en verbinding maken met het beheerprogramma, leert u hoe u Azure Service Health om problemen met de service en statusadvies te bewaken.

> [!div class="nextstepaction"]
> [Zelfstudie over service-waarschuwingen instellen](./set-up-service-alerts.md)
