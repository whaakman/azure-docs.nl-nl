---
title: Herschrijf de HTTP-aanvraag- en reactieheaders met Azure Application Gateway - Azure portal | Microsoft Docs
description: Informatie over het gebruik van de Azure-portal een Azure Application Gateway voor het herschrijven van de HTTP-headers in de aanvragen en antwoorden te geven via de gateway configureren
services: application-gateway
author: abshamsft
ms.service: application-gateway
ms.topic: article
ms.date: 04/10/2019
ms.author: absha
ms.custom: mvc
ms.openlocfilehash: 6afc07f98905469b06622e7829ec4a215b94845e
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/22/2019
ms.locfileid: "59994602"
---
# <a name="rewrite-http-request-and-response-headers-with-azure-application-gateway---azure-portal"></a>Herschrijf de HTTP-aanvraag- en reactieheaders met Azure Application Gateway - Azure portal

Dit artikel leest u hoe de Azure-portal gebruiken om te configureren een [Application Gateway v2 SKU](<https://docs.microsoft.com/azure/application-gateway/application-gateway-autoscaling-zone-redundant>) te herschrijven van de HTTP-headers in de aanvragen en antwoorden.

> [!IMPORTANT]
> De SKU met automatisch schalen en zone-redundantie voor toepassingsgateways is momenteel in openbare preview. Deze preview wordt aangeboden zonder service level agreement en wordt niet aanbevolen voor productieworkloads. De reden hiervoor is dat bepaalde functies mogelijk niet worden ondersteund of beperkte mogelijkheden hebben. Raadpleeg voor meer informatie de [aanvullende gebruiksrechtovereenkomst voor Microsoft Azure-previews](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="before-you-begin"></a>Voordat u begint

U moet een Application Gateway-v2 SKU sinds de mogelijkheid tot herschrijven van de koptekst wordt niet ondersteund voor de v1-SKU. Als u de v2-SKU hebt, maakt u een [Application Gateway v2 SKU](https://docs.microsoft.com/azure/application-gateway/tutorial-autoscale-ps) voordat u begint.

## <a name="what-is-required-to-rewrite-a-header"></a>Wat is vereist voor het herschrijven van een koptekst

Voor het configureren van HTTP-header herschrijven, moet u naar:

1. De nieuwe objecten die zijn vereist voor het herschrijven van de http-headers maken:

   - **Actie schrijven**: Hiermee geeft u de aanvraag en aanvraagheader-velden die u van plan bent om te schrijven en de nieuwe waarde die de oorspronkelijke kopteksten worden herschreven moeten aan. U kunt een of meer herschrijven voorwaarde koppelen met een actie herschrijven.

   - **Herschrijf de voorwaarde**: Het is een optionele configuratie. Als een voorwaarde herschrijven wordt toegevoegd, wordt de inhoud van het HTTP (S) aanvragen en antwoorden geëvalueerd. De beslissing om uit te voeren van de herschrijven actie die is gekoppeld aan de voorwaarde herschrijven zijn gebaseerd of het HTTP (S)-aanvraag of antwoord met de voorwaarde herschrijven overeenkomt. 

     Als meer dan één voorwaarden zijn gekoppeld aan een actie, en vervolgens de actie wordt uitgevoerd alleen als alle voorwaarden wordt voldaan, dat wil zeggen, een logische AND-bewerking wordt uitgevoerd.

   - **Regel herschrijven**: herschrijvingsregel bevat meerdere herschrijven actie - voorwaarde combinaties herschrijven.

   - **Reeks regel**: helpt u bepalen de volgorde waarin de verschillende herschrijven regels worden uitgevoerd. Dit is handig wanneer er meerdere herschrijvingsregels in een set herschrijven. De herschrijvingsregel met minder regel reekswaarde wordt eerst uitgevoerd. Als u de dezelfde volgorde van de regel aan twee herschrijvingsregels wordt de volgorde van de uitvoering van niet-deterministisch zijn.

   - **Herschrijf de Set**: meerdere herschrijvingsregels die wordt gekoppeld aan een regel voor het doorsturen van aanvraag bevat.

2. U moet het opnieuw instellen met een regel voor doorsturen koppelen. Dit is omdat de configuratie opnieuw schrijven is gekoppeld aan de bron-listener via de regel voor doorsturen. Wanneer u een basisregel voor routering, wordt de configuratie van de herschrijven header is gekoppeld aan de listener van een bron en is een algemene header herschrijven. Wanneer een regel op pad gebaseerde routering wordt gebruikt, wordt de configuratie van de koptekst opnieuw schrijven is gedefinieerd in de URL-path-map. Dus dit alleen van toepassing op het padgebied van het specifieke van een site.

U kunt meerdere sets van http-header herschrijven maken en elke set herschrijven kan worden toegepast op meerdere listeners. U kunt echter alleen een is ingesteld op een specifieke listener opnieuw toepassen.

## <a name="sign-in-to-azure"></a>Aanmelden bij Azure

Meld u met uw Azure-account aan bij [Azure Portal](https://portal.azure.com/).

## <a name="configure-header-rewrite"></a>Koptekst herschrijven configureren

In dit voorbeeld wijzigen we de omleidings-URL door het herschrijven van de location-header in het http-antwoord verzonden door de back endtoepassing. 

1. Selecteer **alle resources**, en selecteer vervolgens uw application gateway.

2. Selecteer **herschrijft** in het menu links.

3. Klik op **+ herschrijven set**. 

   ![Herschrijven set toevoegen](media/rewrite-http-headers-portal/add-rewrite-set.png)

4. Geef naam op voor het opnieuw instellen en deze koppelen aan een regel voor doorsturen:

   - Voer de naam van het opnieuw instellen in de **naam** tekstvak.
   - Selecteer een of meer regels die worden vermeld in de **die zijn gekoppeld routeringsregels** lijst. U kunt alleen de regels die niet gekoppeld aan andere sets herschrijven zijn selecteren. De regels die al gekoppeld aan andere sets herschrijven zijn worden lichter gekleurd weergegeven.
   - Klik op volgende.
   
     ![Naam en koppeling toevoegen](media/rewrite-http-headers-portal/name-and-association.png)

5. Maak een herschrijvingsregel voor:

   - Klik op **+ toevoegen herschrijvingsregel**.![ Herschrijvingsregel voor het toevoegen](media/rewrite-http-headers-portal/add-rewrite-rule.png)
   - Geef een naam op voor de herschrijvingsregel in het tekstvak voor de regel herschrijven en geef een regelreeks.![De naam van regel toevoegen](media/rewrite-http-headers-portal/rule-name.png)

6. In dit voorbeeld zullen we de location-header herschrijven alleen wanneer deze een verwijzing naar 'azurewebsites.net bevat'. Om dit te doen, moet u een voorwaarde om te beoordelen of de location-header in het antwoord azurewebsites.net bevat toevoegen:

   - Klik op **+ voorwaarde toevoegen** en klik vervolgens op de sectie met de **als** instructies voor het uitbreiden van deze![ De naam van regel toevoegen](media/rewrite-http-headers-portal/add-condition.png)

   - Selecteer **HTTP-header** uit de **Type variabele om te controleren** vervolgkeuzelijst. 

   - Selecteer **Header-type** als **antwoord**.

   - Omdat in dit voorbeeld zijn we de location-header die wordt uitgevoerd om te worden van een algemene header, selecteer evalueren **algemene header** keuzerondje als de **headernaam**.

   - Selecteer **locatie** uit de **algemene header** vervolgkeuzelijst.

   - Selecteer **Nee** als de **hoofdlettergevoelig** instelling.

   - Selecteer **gelijkteken (=)** uit de **Operator** vervolgkeuzelijst.

   - Voer de reguliere-expressiepatroon. In dit voorbeeld gebruiken we het patroon `(https?):\/\/.*azurewebsites\.net(.*)$` .

   - Klik op **OK**.

     ![Location-header wijzigen](media/rewrite-http-headers-portal/condition.png)

7. Voeg een actie voor het herschrijven van de location-header:

   - Selecteer **ingesteld** als de **actietype**.

   - Selecteer **antwoord** als de **Header-type**.

   - Selecteer **algemene header** als de **headernaam**.

   - Selecteer **locatie** uit de **algemene header** vervolgkeuzelijst.

   - Voer de headerwaarde. In dit voorbeeld gebruiken we `{http_resp_Location_1}://contoso.com{http_resp_Location_2}` als waarde van de header. Dit wordt vervangen door *azurewebsites.net* met *contoso.com* in de location-header.

   - Klik op **OK**.

     ![Location-header wijzigen](media/rewrite-http-headers-portal/action.png)

8. Klik op **maken** om te maken van het herschrijven.

   ![Location-header wijzigen](media/rewrite-http-headers-portal/create.png)

9. U gaat naar de weergave van de set herschrijven. Controleer of de opnieuw ingesteld dat u hierboven hebt gemaakt is aanwezig in de lijst opnieuw ingesteld.

   ![Location-header wijzigen](media/rewrite-http-headers-portal/rewrite-set-list.png)

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over de configuratie is vereist om uit te voeren sommige van de algemene gebruiksvoorbeelden [algemene header scenario's herschrijven](https://docs.microsoft.com/azure/application-gateway/rewrite-http-headers).

   
