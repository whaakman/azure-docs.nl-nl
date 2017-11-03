---
title: Uw aangepaste connectors - Azure Logic Apps certificeren | Microsoft Docs
description: Connectors beschikbaar maken voor alle gebruikers in Azure Logic Apps, Microsoft Flow en Microsoft PowerApps
author: ecfan
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: 
ms.service: logic-apps
ms.workload: logic-apps
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/22/2017
ms.author: LADocs; estfan
ms.openlocfilehash: 9bcb138c181df0c12548cc5dc721542fd9c9ba68
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="submit-your-connectors-for-microsoft-certification"></a>De connectors voor Microsoft-certificeringsinstantie indienen

Als u aangepaste connectors openbaar beschikbaar voor alle gebruikers in Azure Logic Apps, Microsoft Flow en Microsoft PowerApps, indienen uw connector naar Microsoft voor beoordeling en validatie goedkeuring voor publicatie. 

## <a name="certification-criteria"></a>Certificeringsinstantie criteria

| Mogelijkheid | Details | Noodzakelijk of aanbevolen |
|------------|---------|-------------------------|
| Software-as-a-Service (SaaS)-app | Voldoet aan een scenario voor gebruikers die past bij de goed met Logic Apps, stroom en PowerApps. | Vereist |
| Verificatietype | Uw API moet ondersteunen OAuth2, API-sleutel of basisverificatie. | Vereist | 
| Ondersteuning | U moet een ondersteuning contact op met opgeven zodat klanten kunnen hulp krijgen. | Vereist | 
| Beschikbaarheid en bedrijfstijd | Uw app heeft minimaal 99,9% beschikbaarheid. | Aanbevolen | 
|||| 

Ook als u niet een Microsoft-partner of Independent Software Vendors (ISV's bent) en u wilt certificeren en openbaar release een connector, moet u ofwel eigenaar van de onderliggende service of aanwezig expliciete rechten voor het gebruik van de API.

## <a name="validation-phases"></a>Validatie fasen

Microsoft gebruikt deze validatie fasen voor het evalueren van de connector:

| Validatiefase | Beschrijving | 
| ----- | ----------- |
| Functionaliteit | Microsoft test uw connector met Logic Apps, stroom en PowerApps voor deze fouten: <p>-Ongeldige OpenAPI (Swagger) of JSON fouten die worden weergegeven in de sectie definitie van de aangepaste connector-wizard <p>-Runtime en schema validatiefouten die worden weergegeven in de Test-sectie van de aangepaste connector-wizard | 
| Inhoud | Microsoft controleert of de connector beschrijvende namen en beschrijvingen voor elke entiteit gebruikt. Zorg ervoor dat elke bewerking, invoerparameter en antwoord-kenmerk in de connector Swagger deze elementen heeft: <p>- [Samenvatting](../logic-apps/custom-connector-openapi-extensions.md#summary) <br>- [Beschrijving](../logic-apps/custom-connector-openapi-extensions.md#description) </br>- [Zichtbaarheid informatie](../logic-apps/custom-connector-openapi-extensions.md#visibility) | 
||| 

## <a name="nominate-and-submit-your-connector-to-microsoft-for-certification"></a>Benoemen en het verzenden van de connector voor Microsoft voor certificering

Als u wilt toepassen voor certificering, de volgende stappen uit:

1. **Benoemen**

   1. [Verzenden van uw benoeming](https://go.microsoft.com/fwlink/?linkid=848754).

      1. Kies aan de onderkant van de pagina **contact met ons opnemen**.
      2. Vul het formulier en selecteer **vragen over het ISV-connector programma en mede marketing**.
      3. Kies **indienen**.

   2. Meld u aan de wederzijdse vrijgeven van Non-overeenkomst en de overeenkomst voor partners die u ontvangt. 

      Microsoft vereist deze ondertekende contracten voordat u doorgaat. 
      We kunnen vervolgens controleren of de connector voldoet aan de criteria van de certificeringsinstantie. 
   
   3. Als de connector is goedgekeurd, bericht Microsoft met instructies voor het voorbereiden.
    
2. **Beoordelen**

   1. Deze gegevens worden verzonden naar uw contactpersoon benoeming ter controle:

      * Het bestand OpenAPI waarmee uw API wordt beschreven
      * Het pictogrambestand (PNG- of jpg) die de connector vertegenwoordigt 
      
        Het pictogram moet een logo ~ 160 pixels in een vierkant 230 pixel hebben. 
        Een witte logo op een gekleurde achtergrond verdient de voorkeur.
      
      * Het pictogram merk kleur die in de indeling moet overeenkomen met de gekleurde achtergrond in het pictogrambestand hexadecimale

      * Een testaccount voor validatie
      * Er is een Neem contact op met ondersteuning

   2. Als we meer gegevens nodig, er wordt contact met u.

3. **Publiceren**

    Nadat we de functionaliteit en de inhoud van de connector valideren, fase we de connector voor de implementatie voor alle producten en regio's.
    
    Standaard worden alle connectors gepubliceerd als 'premium'-connectors. 
    Als u uw app met Azure hebt gemaakt, kunt u voor het weergeven van de connector als een 'standaard' connector die beschikbaar is voor alle gebruikers met Office 365 Enterprise plannen toepassen. 
    Vraag uw contactpersoon benoeming voor meer informatie.

## <a name="next-steps"></a>Volgende stappen

* [Veelgestelde vragen over aangepaste connector](../logic-apps/custom-connector-faq.md)
* [Overzicht van aangepaste connector](../logic-apps/custom-connector-overview.md)