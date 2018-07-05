---
title: On-premises gegevensgateway installeren | Microsoft Docs
description: Informatie over het installeren en configureren van een On-premises gegevensgateway.
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 07/03/2018
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 59dc2dad103de60cb7d4b6a44a6d6f7271368391
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37444936"
---
# <a name="install-and-configure-an-on-premises-data-gateway"></a>Installeren en configureren van een on-premises gegevensgateway
Een on-premises gegevensgateway is vereist wanneer een of meer Azure Analysis Services-servers in dezelfde regio verbinding met on-premises gegevensbronnen maken. Zie voor meer informatie over de gateway, [On-premises gegevensgateway](analysis-services-gateway.md).

## <a name="prerequisites"></a>Vereisten
**Minimale vereisten:**

* .NET 4.5 framework
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
* Aanmelden bij Azure met een account in Azure AD voor dezelfde [tenant](https://msdn.microsoft.com/library/azure/jj573650.aspx#BKMK_WhatIsAnAzureADTenant) als het abonnement registreert u de gateway in. Azure B2B (Gast)-accounts worden niet ondersteund bij het installeren en registreren van een gateway.
* De hier beschreven, (geïntegreerde) gateway wordt niet ondersteund in Azure Government, Azure Duitsland en Azure China soevereine regio's. Gebruik **toegewezen On-premises gateway voor Azure Analysis Services**geïnstalleerd vanuit van uw server **Quick Start** in de portal. 


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

1. Klik in Azure portal op **maken van een nieuwe service** > **bedrijfsintegratie** > **On-premises gegevensgateway**  >   **Maak**.

   ![Een gateway-resource maken](media/analysis-services-gateway-install/aas-gateway-new-azure-resource.png)

2. In **verbindingsgateway maken**, voer de volgende instellingen:

    * **Naam**: Voer een naam in voor uw gateway. 

    * **Abonnement**: Selecteer het Azure-abonnement koppelen aan uw gateway-resource. 
   
      Het standaardabonnement is gebaseerd op het Azure-account waarmee u zich aanmeldt.

    * **Resourcegroep**: maak een resourcegroep of selecteer een bestaande resourcegroep.

    * **Locatie**: Selecteer de regio waarin u uw gateway in geregistreerd.

    * **De Installatienaam van de**: als uw gatewayinstallatie is niet nog is geselecteerd, selecteert u de gateway die is geregistreerd. 

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
