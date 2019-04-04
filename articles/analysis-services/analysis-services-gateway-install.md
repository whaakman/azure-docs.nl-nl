---
title: On-premises gegevensgateway installeren | Microsoft Docs
description: Informatie over het installeren en configureren van een On-premises gegevensgateway.
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 01/08/2019
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 629a97048ceba4ac02e3aa1dd59310980e5a0c95
ms.sourcegitcommit: 0a3efe5dcf56498010f4733a1600c8fe51eb7701
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2019
ms.locfileid: "58894163"
---
# <a name="install-and-configure-an-on-premises-data-gateway"></a>Een on-premises gegevensgateway installeren en configureren

Een on-premises gegevensgateway is vereist wanneer een of meer Azure Analysis Services-servers in dezelfde regio verbinding met on-premises gegevensbronnen maken. Zie voor meer informatie over de gateway, [On-premises gegevensgateway](analysis-services-gateway.md).

## <a name="prerequisites"></a>Vereisten

**Minimale vereisten:**

* .NET 4.5 Framework
* 64-bits versie van Windows 7 / Windows Server 2008 R2 (of hoger)

**Aanbevolen:**

* 8-core CPU
* 8 GB geheugen
* 64-bits versie van Windows 2012 R2 (of hoger)

**Belangrijke overwegingen:**

* Tijdens de installatie bij het registreren van uw gateway met Azure, is de standaardregio voor uw abonnement geselecteerd. U kunt een andere regio. Als u servers in meer dan één regio hebt, moet u een gateway voor elke regio. 
* De gateway kan niet worden geïnstalleerd op een domeincontroller.
* Slechts één gateway kan worden geïnstalleerd op een enkele computer.
* De gateway installeren op een computer die blijft op en gaat niet naar de slaapstand.
* Moet u de gateway niet installeren op een computer draadloos zijn verbonden met uw netwerk. Prestaties kan worden verminderd.
* Wanneer u de gateway installeert, moet het gebruikersaccount dat u bent aangemeld bij de computer met logboekbestand op als service bevoegdheden hebben. Wanneer de installatie is voltooid, de On-premises data gateway-service maakt gebruik van de account NT SERVICE\PBIEgwService als een service aan te melden. Een ander account kan alleen worden opgegeven tijdens de installatie van of in Services nadat setup voltooid is. Zorg ervoor dat instellingen voor Groepsbeleid kunnen het account dat u bent aangemeld bij het installeren van zowel het serviceaccount dat u hebt logboek op als service bevoegdheden.
* Aanmelden bij Azure met een account in Azure AD voor dezelfde [tenant](/previous-versions/azure/azure-services/jj573650(v=azure.100)#BKMK_WhatIsAnAzureADTenant) als het abonnement registreert u de gateway in. Azure B2B (Gast)-accounts worden niet ondersteund bij het installeren en registreren van een gateway.
* Als de gegevensbronnen zijn in een Azure-netwerk (VNet), moet u configureren de [AlwaysUseGateway](analysis-services-vnet-gateway.md) servereigenschap.
* De hier beschreven, (geïntegreerde) gateway wordt niet ondersteund in Azure Duitsland-regio's. In plaats daarvan gebruik **toegewezen On-premises gateway voor Azure Analysis Services**geïnstalleerd vanuit van uw server **Quick Start** in de portal. 


## <a name="download"></a>Downloaden

 [De gateway downloaden](https://aka.ms/azureasgateway)

## <a name="install"></a>Installeren

1. Voer setup uit.

2. Selecteer een locatie, de voorwaarden accepteren en klik vervolgens op **installeren**.

   ![Installeren van de locatie en de licentievoorwaarden](media/analysis-services-gateway-install/aas-gateway-installer-accept.png)

3. Meld u aan bij Azure. Het account moet zich in van uw tenant Azure Active Directory. Dit account wordt gebruikt voor de gatewaybeheerder. Azure B2B (Gast)-accounts worden niet ondersteund bij het installeren en registreren van de gateway.

   ![Aanmelden bij Azure](media/analysis-services-gateway-install/aas-gateway-installer-account.png)

   > [!NOTE]
   > Als u zich met een domeinaccount aanmeldt, wordt deze toegewezen aan uw organisatieaccount in Azure AD. Uw organisatie-account wordt gebruikt als de gatewaybeheerder van de.

## <a name="register"></a>Registreren

Als u wilt een gateway-resource maken in Azure, moet u het lokale exemplaar dat u hebt geïnstalleerd met de Gateway-Cloudservice registreren. 

1.  Selecteer **een nieuwe gateway registreren op deze computer**.

    ![Registreren](media/analysis-services-gateway-install/aas-gateway-register-new.png)

2. Typ een naam en herstel-sleutel voor uw gateway. De gateway gebruikt standaard de standaardregio van uw abonnement. Als u een andere regio selecteren wilt, selecteert u **regio wijzigen**.

    > [!IMPORTANT]
    > Sla de herstelsleutel op een veilige plaats. De herstelsleutel is vereist om hiernaar overnemen, migreren of terugzetten van een gateway. 

   ![Registreren](media/analysis-services-gateway-install/aas-gateway-register-name.png)


## <a name="create-resource"></a>Maak een Azure-gateway-resource

Nadat u hebt geïnstalleerd en geregistreerd van de gateway, moet u een gateway-resource maken in uw Azure-abonnement. Meld u bij Azure met hetzelfde account dat u gebruikt bij het registreren van de gateway.

1. Klik in Azure portal op **een resource maken** > **integratie** > **On-premises gegevensgateway**.

   ![Een gateway-resource maken](media/analysis-services-gateway-install/aas-gateway-new-azure-resource.png)

2. In **verbindingsgateway maken**, voer de volgende instellingen:

   * **Naam**: Voer een naam voor uw gateway. 

   * **Abonnement**: Selecteer het Azure-abonnement koppelen aan uw gateway-resource. 
   
     Het standaardabonnement is gebaseerd op het Azure-account waarmee u zich aanmeldt.

   * **Resourcegroep**: Maak een resourcegroep of selecteer een bestaande resourcegroep.

   * **Locatie**: Selecteer de regio die u hebt uw gateway in geregistreerd.

   * **De Installatienaam van de**: Als de gatewayinstallatie is niet nog is geselecteerd, selecteert u de gateway die is geregistreerd. 

     Wanneer u klaar bent, klikt u op **maken**.

## <a name="connect-servers"></a>Servers verbinden met de gateway-resource

1. Klik in het overzicht van uw Azure Analysis Services-server op **On-Premises Data Gateway**.

   ![Server-gateway verbinding maken](media/analysis-services-gateway-install/aas-gateway-connect-server.png)

2. In **kiezen van een On-Premises gegevensgateway om verbinding te**, selecteert u uw gateway-resource en klik vervolgens op **geselecteerde gateway verbinden**.

   ![Verbinding maken met server gateway-resource](media/analysis-services-gateway-install/aas-gateway-connect-resource.png)

    > [!NOTE]
    > Als uw gateway niet wordt weergegeven in de lijst, wordt de server is waarschijnlijk dat niet in dezelfde regio als de regio u is opgegeven bij het registreren van de gateway. 

Dat is alles. Als u wilt openen van poorten of problemen oplost, moet u uitchecken [On-premises gegevensgateway](analysis-services-gateway.md).

## <a name="next-steps"></a>Volgende stappen

* [Analyseservices beheren](analysis-services-manage.md)   
* [Gegevens ophalen uit Azure Analysis Services](analysis-services-connect.md)   
* [Gateway gebruiken voor gegevensbronnen in een virtueel Azure-netwerk](analysis-services-vnet-gateway.md)
