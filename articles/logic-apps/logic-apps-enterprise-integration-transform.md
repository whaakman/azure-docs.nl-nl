---
title: XML transformeren tussen notaties - Azure Logic Apps | Microsoft Docs
description: Transformaties of kaarten die XML tussen indelingen in Azure Logic Apps met Enterprise Integration Pack converteren maken
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, LADocs
ms.topic: article
ms.assetid: add01429-21bc-4bab-8b23-bc76ba7d0bde
ms.date: 07/08/2016
ms.openlocfilehash: 9dd471f70407191734b4c5a3aa84d5365a7beab8
ms.sourcegitcommit: 2ad510772e28f5eddd15ba265746c368356244ae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/28/2018
ms.locfileid: "43125292"
---
# <a name="create-maps-that-transform-xml-between-formats-in-azure-logic-apps-with-enterprise-integration-pack"></a>Kaarten die XML tussen notaties in Azure Logic Apps met Enterprise Integration Pack transformeren maken

De Enterprise integration transformatie-connector worden gegevens van de ene indeling geconverteerd naar een andere indeling. Bijvoorbeeld, wellicht u een binnenkomend bericht met de huidige datum in de indeling YearMonthDay. U kunt een transformatie te formatteren van de datum moet de indeling MonthDayYear gebruiken.

## <a name="what-does-a-transform-do"></a>Wat is een transformatie?
Een transformatie, die ook wel bekend als een kaart, bestaat uit een bron-XML-schema (invoer) en een doel-XML-schema (uitvoer). U kunt verschillende ingebouwde functies gebruiken om te bewerken of beheren van de gegevens, waaronder tekenreeksmanipulatie, voorwaardelijke toewijzingen, rekenkundige bewerkingen, datum tijd bevatten, en zelfs lussen constructies.

## <a name="how-to-create-a-transform"></a>Het maken van een transformatie?
U kunt een transformatie/kaart maken met behulp van de Visual Studio [Enterprise Integration SDK](https://aka.ms/vsmapsandschemas). Wanneer u klaar bent met het maken en testen van de transformatie, uploadt u de transformatie naar uw integratie-account. 

## <a name="how-to-use-a-transform"></a>Het gebruik van een transformatie
Nadat u de transformatie/kaart naar uw integratieaccount uploaden, kunt u het maken van een logische app. De logische app wordt uw transformaties uitgevoerd wanneer de logische app wordt geactiveerd (en er is invoer inhoud die moet worden getransformeerd).

**Hier volgen de stappen voor het gebruik van een transformatie**:

### <a name="prerequisites"></a>Vereisten

* Een integratieaccount maakt en een kaart aan toe te voegen  

Nu dat u hebt afgehandeld van de vereisten, is het tijd om uw logische app maken:  

1. Een logische app maken en [koppelen aan uw integratieaccount](../logic-apps/logic-apps-enterprise-integration-accounts.md "Leer hoe u een integratieaccount koppelt aan een logische app") die de kaart bevat.
2. Voeg een **aanvragen** trigger aan uw logische app  
   ![](./media/logic-apps-enterprise-integration-transforms/transform-1.png)    
3. Toevoegen de **XML transformeren** actie door eerst te selecteren **een actie toevoegen**   
   ![](./media/logic-apps-enterprise-integration-transforms/transform-2.png)   
4. Voert u het woord *transformeren* in het zoekvak om te filteren van alle acties in de map die u wilt gebruiken  
   ![](./media/logic-apps-enterprise-integration-transforms/transform-3.png)  
5. Selecteer de **XML transformeren** actie   
6. Het XML-bestand toevoegen **inhoud** die u transformeren. U kunt een XML-gegevens die u in de HTTP-aanvraag als ontvangt de **inhoud**. Selecteer in dit voorbeeld wordt de hoofdtekst van de HTTP-aanvraag die de logische app geactiveerd.

   > [!NOTE]
   > Zorg ervoor dat de inhoud voor de **XML transformeren** XML is. Als de inhoud niet in XML is of base64-gecodeerd, moet u een expressie waarmee de inhoud wordt verwerkt. Bijvoorbeeld, kunt u [functies](logic-apps-workflow-definition-language.md#functions), bijvoorbeeld ```@base64ToBinary``` voor het decoderen van inhoud of ```@xml``` voor het verwerken van de XML-inhoud.
 

7. Selecteer de naam van de **kaart** dat u gebruiken wilt voor het uitvoeren van de transformatie. De kaart moet al in uw integratie-account. In een eerdere stap heeft u al uw logische app-toegang in uw integratieaccount die de kaart bevat.      
   ![](./media/logic-apps-enterprise-integration-transforms/transform-4.png) 
8. Sla uw werk  
    ![](./media/logic-apps-enterprise-integration-transforms/transform-5.png) 

Op dit moment bent u klaar instellen van uw kaart. In een werkelijke toepassing, kunt u de getransformeerde gegevens opslaan in een LOB-toepassing zoals SalesForce. U kunt eenvoudig als een actie voor het verzenden van de uitvoer van de transformatie met Salesforce. 

U kunt nu de transformatie testen door een aanvraag naar de HTTP-eindpunt.  


## <a name="features-and-use-cases"></a>Functies en use-cases
* De transformatie in een kaart gemaakt is eenvoudig, zoals het kopiëren van een naam en het adres van een document naar de andere. Of u kunt meer complexe transformaties met behulp van de bewerkingen out-of-the-box-map maken.  
* Verschillende bewerkingen van de kaart of functies zijn direct beschikbaar, waaronder tekenreeksen, tijdfuncties datum, enzovoort.  
* U kunt een kopie van de directe gegevens tussen de schema's kunt doen. In het toewijzen van de opgenomen in de SDK, is dit net zo eenvoudig als een lijn die verbinding de elementen in het schema van de gegevensbron met hun collega's in het doelschema maakt wordt getekend.  
* Als het maken van een kaart, bekijkt u een grafische weergave van de kaart, waarin de relaties en koppelingen die u maakt.
* Gebruik de functie Test-kaart toe te voegen een bericht van de XML-voorbeeld. U kunt met een simpele klik testen van de kaart die u hebt gemaakt en de gegenereerde uitvoer weergegeven.  
* Bestaande maps uploaden  
* Biedt ondersteuning voor de XML-indeling.

## <a name="advanced-features"></a>Geavanceerde functies

### <a name="reference-assembly-or-custom-code-from-maps"></a>Naslaginformatie over assembly of aangepaste code uit kaarten 
De transformatie-actie biedt ondersteuning voor kaarten ook of met een verwijzing naar externe assembly worden getransformeerd. Op deze manier kunt aanroepen naar aangepaste .NET-code rechtstreeks vanuit de XSLT-toewijzingen. Hier volgen de vereisten voor het gebruik van de assembly van maps.

* De kaart en de assembly waarnaar wordt verwezen in de kaart moet worden [geüpload naar het integratieaccount](./logic-apps-enterprise-integration-maps.md). 

  > [!NOTE]
  > Kaart en de assembly zijn vereist om te worden geüpload in een specifieke volgorde. Voordat u de toewijzing die verwijst naar de assembly uploadt, moet u de assembly uploaden.

* De kaart moet bovendien hebben deze kenmerken en een CDATA-sectie gevonden die de aanroep van de assembly-code bevat:

    * **naam** is de aangepaste assemblynaam.
    * **naamruimte** is de naamruimte in de assembly die de aangepaste code bevat.

  In dit voorbeeld ziet u een kaart die verwijst naar een assembly met de naam 'XslUtilitiesLib' en roept de `circumreference` methode van de assembly.

  ````xml
  <?xml version="1.0" encoding="UTF-8"?>
  <xsl:stylesheet version="1.0" xmlns:xsl="http://www.w3.org/1999/XSL/Transform" xmlns:msxsl="urn:schemas-microsoft-com:xslt" xmlns:user="urn:my-scripts">
  <msxsl:script language="C#" implements-prefix="user">
    <msxsl:assembly name="XsltHelperLib"/>
    <msxsl:using namespace="XsltHelpers"/>
    <![CDATA[public double circumference(int radius){ XsltHelper helper = new XsltHelper(); return helper.circumference(radius); }]]>
  </msxsl:script>
  <xsl:template match="data">
     <circles>
        <xsl:for-each select="circle">
            <circle>
                <xsl:copy-of select="node()"/>
                    <circumference>
                        <xsl:value-of select="user:circumference(radius)"/>
                    </circumference>
            </circle>
        </xsl:for-each>
     </circles>
    </xsl:template>
    </xsl:stylesheet>
  ````


### <a name="byte-order-mark"></a>Bytevolgordemarkering
Het antwoord van de transformatie wordt standaard gestart met de Byte Order Mark (BOM). U kunt toegang tot deze functionaliteit alleen tijdens het werken in de weergave van Code-editor. Geef deze als functionaliteit wilt uitschakelen, `disableByteOrderMark` voor de `transformOptions` eigenschap:

````json
"Transform_XML": {
    "inputs": {
        "content": "@{triggerBody()}",
        "integrationAccount": {
            "map": {
                "name": "TestMap"
            }
        },
        "transformOptions": "disableByteOrderMark"
    },
    "runAfter": {},
    "type": "Xslt"
}
````





## <a name="learn-more"></a>Meer informatie
* [Meer informatie over het Enterprise Integration Pack](../logic-apps/logic-apps-enterprise-integration-overview.md "meer informatie over Enterprise Integration Pack")  
* [Meer informatie over maps](../logic-apps/logic-apps-enterprise-integration-maps.md "meer informatie over enterprise integration-kaarten")  

