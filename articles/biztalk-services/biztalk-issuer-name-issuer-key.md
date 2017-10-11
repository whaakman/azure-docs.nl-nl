---
title: Naam van verlener en sleutel van verlener in BizTalk Services | Microsoft Docs
description: Informatie over het ophalen van de naam van de verlener en sleutel van verlener voor Service Bus of Access Control (ACS) in BizTalk Services. MABS, WABS
services: biztalk-services
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
ms.assetid: 067fe356-d1aa-420f-b2f2-1a418686470a
ms.service: biztalk-services
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/07/2016
ms.author: mandia
ms.openlocfilehash: b9fd985c23558596408e78eadae00dd0f95c4214
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="biztalk-services-issuer-name-and-issuer-key"></a>BizTalk Services: naam en sleutel van verlener

> [!INCLUDE [BizTalk Services is being retired, and replaced with Azure Logic Apps](../../includes/biztalk-services-retirement.md)]

Azure BizTalk Services maakt gebruik van de naam van de Service Bus-verlener en sleutel van verlener en de naam van de certificaatverlener Access Control en sleutel van verlener. Specifiek:

| Taak | Welke naam van verlener en sleutel van verlener |
| --- | --- |
| Implementeren van uw toepassing vanuit Visual Studio |Toegang tot de naam van de certificaatverlener besturingselement en de sleutel van verlener |
| De Azure BizTalk Services-Portal configureren |Toegang tot de naam van de certificaatverlener besturingselement en de sleutel van verlener |
| Relays LOB maken met de BizTalk Adapter Services in Visual Studio |Naam van Service Bus verlener en sleutel van verlener |

Dit onderwerp worden de stappen voor het ophalen van de naam van verlener en sleutel van verlener. 

## <a name="access-control-issuer-name-and-issuer-key"></a>Toegang tot de naam van de certificaatverlener besturingselement en de sleutel van verlener
De naam van de certificaatverlener Access Control en sleutel van verlener worden gebruikt door het volgende:

* Uw Azure BizTalk Service-toepassing in Visual Studio gemaakt: als u wilt uw BizTalk Service-toepassing in Visual Studio implementeren naar Azure, u de naam van de certificaatverlener Access Control en sleutel van verlener invoeren. 
* De Azure BizTalk Services-Portal: Wanneer u een BizTalk Service maakt en de BizTalk Services-Portal te openen, uw naam van de certificaatverlener Access Control en sleutel van verlener worden automatisch geregistreerd voor uw implementaties met dezelfde Access Control-waarden.

### <a name="get-the-access-control-issuer-name-and-issuer-key"></a>De naam van de certificaatverlener Access Control en sleutel van verlener

Om ACS gebruiken voor verificatie en krijgt u de naam van verlener en sleutel van verlener waarden, worden de algemene stappen omvatten:

1. Installeer de [Azure Powershell-cmdlets](https://azure.microsoft.com/documentation/articles/powershell-install-configure/).
2. Uw Azure-account toevoegen:`Add-AzureAccount`
3. Retourneert de abonnementsnaam van uw:`get-azuresubscription`
4. Selecteer uw abonnement:`select-azuresubscription <name of your subscription>` 
5. Maak een nieuwe naamruimte:`new-azuresbnamespace <name for the service bus> "Location" -CreateACSNamespace $true -NamespaceType Messaging`

    Voorbeeld:`new-azuresbnamespace biztalksbnamespace "South Central US" -CreateACSNamespace $true -NamespaceType Messaging`
      
5. Wanneer de nieuwe ACS-naamruimte wordt gemaakt (dit kan enkele minuten duren), worden de naam van verlener en sleutel van verlener waarden staan in de verbindingsreeks: 

    ```
    Name                  : biztalksbnamespace
    Region                : South Central US
    DefaultKey            : abcdefghijklmnopqrstuvwxyz
    Status                : Active
    CreatedAt             : 10/18/2016 9:36:30 PM
    AcsManagementEndpoint : https://biztalksbnamespace-sb.accesscontrol.windows.net/
    ServiceBusEndpoint    : https://biztalksbnamespace.servicebus.windows.net/
    ConnectionString      : Endpoint=sb://biztalksbnamespace.servicebus.windows.net/;SharedSecretIssuer=owner;SharedSecretValue=abcdefghijklmnopqrstuvwxyz
    NamespaceType         : Messaging
    ```

Samengevat:  
De naam van certificaatverlener SharedSecretIssuer =  
Sleutel van verlener SharedSecretKey =

Meer op de [nieuw AzureSBNamespace](https://msdn.microsoft.com/library/dn495165.aspx) cmdlet. 

## <a name="service-bus-issuer-name-and-issuer-key"></a>Naam van Service Bus verlener en sleutel van verlener
Naam van Service Bus verlener en sleutel van verlener worden gebruikt door de BizTalk Adapter Services. In uw BizTalk Services-project in Visual Studio gebruikt u de BizTalk Adapter Services verbinding maken met een on-premises Line-of-Business (LOB)-systeem. Als u wilt verbinden, de Relay LOB maken en voer de details van uw LOB-systeem. Wanneer u dit doet, typt u ook de naam van Service Bus verlener en sleutel van verlener.

### <a name="to-retrieve-the-service-bus-issuer-name-and-issuer-key"></a>Voor het ophalen van de naam van Service Bus verlener en sleutel van verlener
1. Meld u aan bij de [klassieke Azure-portal](http://go.microsoft.com/fwlink/p/?LinkID=213885).
2. Selecteer in het navigatiedeelvenster links **Service Bus**.
3. Selecteer de naamruimte. Selecteer in de taakbalk **verbindingsgegevens**. U ziet nu de **standaard verlener** (naam van de certificaatverlener) en **standaard sleutel** (sleutel van verlener). De waarden kunnen worden gekopieerd.  

Samengevat:  
De naam van certificaatverlener = standaard certificaatverlener  
Sleutel van verlener standaardsleutel =

## <a name="next"></a>Volgende
Aanvullende onderwerpen voor Azure BizTalk Services:

* [De Azure BizTalk Services SDK installeren](http://go.microsoft.com/fwlink/p/?LinkID=241589)<br/>
* [Zelfstudies: Azure BizTalk Services](http://go.microsoft.com/fwlink/p/?LinkID=236944)<br/>
* [De Azure BizTalk Services SDK gaan gebruiken](http://go.microsoft.com/fwlink/p/?LinkID=302335)<br/>
* [Azure BizTalk Services](http://go.microsoft.com/fwlink/p/?LinkID=303664)<br/>

## <a name="see-also"></a>Zie ook
* [How to: ACS-Management-Service gebruiken voor het configureren van de Service-identiteiten](http://go.microsoft.com/fwlink/p/?LinkID=303942)<br/>
* [BizTalk Services: Developer, Basic, Standard en Premium-edities grafiek](http://go.microsoft.com/fwlink/p/?LinkID=302279)<br/>
* [BizTalk Services: Inrichten met behulp van Azure classic portal](http://go.microsoft.com/fwlink/p/?LinkID=302280)<br/>
* [BizTalk Services: statusgrafiek voor de inrichting](http://go.microsoft.com/fwlink/p/?LinkID=329870)<br/>
* [BizTalk Services: de tabbladen Dashboard, Bewaken en Schalen](http://go.microsoft.com/fwlink/p/?LinkID=302281)<br/>
* [BizTalk Services: back-ups maken en herstellen](http://go.microsoft.com/fwlink/p/?LinkID=329873)<br/>
* [BizTalk Services: beperking](http://go.microsoft.com/fwlink/p/?LinkID=302282)<br/>

