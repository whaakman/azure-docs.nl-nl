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
ms.openlocfilehash: e144214a58f9fe383cf4edd878554792d9d6a6f9
ms.sourcegitcommit: ed66a704d8e2990df8aa160921b9b69d65c1d887
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/30/2019
ms.locfileid: "64947169"
---
# <a name="rewrite-http-request-and-response-headers-with-azure-application-gateway---azure-portal"></a>Herschrijf de HTTP-aanvraag- en reactieheaders met Azure Application Gateway - Azure portal

In dit artikel wordt beschreven hoe u de Azure portal gebruiken voor het configureren van een [Application Gateway v2 SKU](<https://docs.microsoft.com/azure/application-gateway/application-gateway-autoscaling-zone-redundant>) exemplaar te herschrijven van de HTTP-headers in aanvragen en antwoorden.

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="before-you-begin"></a>Voordat u begint

U moet een Application Gateway v2 SKU-exemplaar om de stappen in dit artikel te voltooien. Headers herschrijven wordt niet in de v1-SKU ondersteund. Als u de v2-SKU hebt, maakt u een [Application Gateway v2 SKU](https://docs.microsoft.com/azure/application-gateway/tutorial-autoscale-ps) exemplaar voordat u begint.

## <a name="create-required-objects"></a>Vereiste objecten maken

Voor het configureren van HTTP-header herschrijven, moet u deze stappen hebt voltooid.

1. Maak de objecten die vereist voor HTTP-header herschrijven zijn:

   - **Actie schrijven**: Hiermee geeft de aanvraag en aanvraagheader-velden die u van plan bent om te schrijven en de nieuwe waarde voor de headers. U kunt een koppelen of meer voorwaarden met een actie herschrijven herschrijven.

   - **Herschrijf de voorwaarde**: Een optionele configuratie. Herschrijven voorwaarden evalueren de inhoud van HTTP (S) aanvragen en antwoorden. De actie herschrijven treedt op als de HTTP (S)-aanvraag of antwoord komt overeen met de voorwaarde herschrijven.

     Als u meer dan één voorwaarde aan een actie koppelen, wordt de actie optreedt, alleen wanneer alle voorwaarden wordt voldaan. De bewerking is met andere woorden, een logische AND-bewerking.

   - **Regel herschrijven**: Bevat meerdere herschrijven actie / Herschrijf de voorwaarde combinaties.

   - **Reeks regel**: Helpt u bepalen de volgorde waarin de herschrijvingsregels uitvoeren. Deze configuratie is handig wanneer u meerdere herschrijvingsregels in een set herschrijven hebt. Eerste een herschrijvingsregel voor met een lagere waarde van de regel-reeks wordt gestart. Als u dezelfde regel reekswaarde aan twee herschrijvingsregels toewijzen, is de volgorde van de uitvoering van niet-deterministisch.

   - **Herschrijf de set**: Bevat meerdere herschrijvingsregels dat gekoppeld aan een regel voor het doorsturen van aanvragen is.

2. Koppel het herschrijven ingesteld op een regel voor doorsturen. De configuratie opnieuw schrijven is gekoppeld aan de bron-listener via de regel voor doorsturen. Wanneer u een regel voor het doorsturen van basic gebruikt, wordt de configuratie van de herschrijven header is gekoppeld aan de listener van een bron en is een algemene header herschrijven. Wanneer u een pad gebaseerde routering regel gebruikt, wordt de configuratie van de koptekst opnieuw schrijven is gedefinieerd in de URL-path-map. In dat geval van de toepassing alleen op het padgebied van het specifieke van een site.

U kunt meerdere sets van HTTP-header herschrijven maken en toepassen van elke opnieuw ingesteld op meerdere listeners. Maar u kunt alleen een is ingesteld op een specifieke listener opnieuw toepassen.

## <a name="sign-in-to-azure"></a>Aanmelden bij Azure

Meld u met uw Azure-account aan bij [Azure Portal](https://portal.azure.com/).

## <a name="configure-header-rewrite"></a>Koptekst herschrijven configureren

In dit voorbeeld wijzigen we een Omleidings-URL door de location-header in het HTTP-antwoord verzonden door een back-endtoepassing herschrijven.

1. Selecteer **alle resources**, en selecteer vervolgens uw application gateway.

2. Selecteer **herschrijft** in het linkerdeelvenster.

3. Selecteer **herschrijven set**:

   ![Herschrijven set toevoegen](media/rewrite-http-headers-portal/add-rewrite-set.png)

4. Geef een naam op voor de set herschrijven en koppel deze aan een regel voor doorsturen:

   - Voer de naam in voor het herschrijven instellen in de **naam** vak.
   - Selecteer een of meer van de regels die worden vermeld in de **die zijn gekoppeld routeringsregels** lijst. U kunt alleen de regels die nog niet gekoppeld aan andere sets herschrijven is selecteren. De regels die al gekoppeld aan andere sets herschrijven zijn worden grijs weergegeven.
   - Selecteer **Volgende**.
   
     ![Naam en koppeling toevoegen](media/rewrite-http-headers-portal/name-and-association.png)

5. Maak een herschrijvingsregel voor:

   - Selecteer **toevoegen herschrijvingsregel**.

     ![Herschrijvingsregel voor het toevoegen](media/rewrite-http-headers-portal/add-rewrite-rule.png)

   - Voer een naam voor de herschrijvingsregel in de **naam voor de regel herschrijven** vak. Voer een getal in de **reeks regel** vak.

     ![De naam van de regel herschrijven toevoegen](media/rewrite-http-headers-portal/rule-name.png)

6. In dit voorbeeld hebt we de location-header herschrijven alleen wanneer deze een verwijzing naar azurewebsites.net bevat. Om dit te doen, moet u een voorwaarde om te beoordelen of de location-header in het antwoord azurewebsites.net bevat toevoegen:

   - Selecteer **voorwaarde toevoegen** en selecteer vervolgens het vak waarin de **als** instructies uit te vouwen.

     ![Een voorwaarde toevoegen](media/rewrite-http-headers-portal/add-condition.png)

   - In de **Type variabele om te controleren op** in de lijst met **HTTP-header**.

   - In de **Header-type** in de lijst met **antwoord**.

   - Omdat in dit voorbeeld wordt de location-header een algemene header is, evalueert selecteren **algemene header** onder **headernaam**.

   - In de **algemene header** in de lijst met **locatie**.

   - Onder **hoofdlettergevoelig**, selecteer **Nee**.

   - In de **Operator** in de lijst met **gelijkteken (=)**.

   - Voer een reguliere-expressiepatroon. In dit voorbeeld gebruiken we het patroon `(https?):\/\/.*azurewebsites\.net(.*)$`.

   - Selecteer **OK**.

     ![Configureren van een als voorwaarde](media/rewrite-http-headers-portal/condition.png)

7. Voeg een actie voor het herschrijven van de location-header:

   - In de **actietype** in de lijst met **ingesteld**.

   - In de **Header-type** in de lijst met **antwoord**.

   - Onder **headernaam**, selecteer **algemene header**.

   - In de **algemene header** in de lijst met **locatie**.

   - Voer de headerwaarde. In dit voorbeeld gebruiken we `{http_resp_Location_1}://contoso.com{http_resp_Location_2}` als waarde van de header. Deze waarde wordt vervangen door *azurewebsites.net* met *contoso.com* in de location-header.

   - Selecteer **OK**.

     ![Een actie toevoegen](media/rewrite-http-headers-portal/action.png)

8. Selecteer **maken** om te maken van het herschrijven:

   ![Selecteer Maken](media/rewrite-http-headers-portal/create.png)

9. De weergave van de set herschrijven wordt geopend. Controleer of de opnieuw ingesteld dat u hebt gemaakt in de lijst met herschrijven sets is:

   ![Herschrijf de set weergeven](media/rewrite-http-headers-portal/rewrite-set-list.png)

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over het instellen van enkele veelvoorkomende use cases, [algemene header scenario's herschrijven](https://docs.microsoft.com/azure/application-gateway/rewrite-http-headers).