---
title: SAP S/4HANA of BW/4HANA op een virtuele machine in Azure implementeren | Microsoft Docs
description: SAP S/4HANA of BW/4HANA op een virtuele machine in Azure implementeren
services: virtual-machines-linux
documentationcenter: 
author: hermanndms
manager: timlt
editor: 
tags: azure-resource-manager
keywords: 
ms.assetid: 44bbd2b6-a376-4b5c-b824-e76917117fa9
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 09/15/2016
ms.author: hermannd
ms.openlocfilehash: 342bd20466cdeb0b9df1cdacd4664fa4e3e0c604
ms.sourcegitcommit: c7215d71e1cdeab731dd923a9b6b6643cee6eb04
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/17/2017
---
# <a name="deploy-sap-s4hana-or-bw4hana-on-azure"></a>SAP S/4HANA of BW/4HANA in Azure implementeren
In dit artikel wordt beschreven hoe S/4HANA in Azure implementeren met behulp van de bibliotheek voor SAP Cloud toestel (SAP CAL) 3.0. Volg dezelfde stappen voor het implementeren van andere SAP HANA-oplossingen, zoals BW/4HANA.

> [!NOTE]
Voor meer informatie over de CAL SAP, gaat u naar de [SAP-Cloudbibliotheek toestel](https://cal.sap.com/) website. SAP heeft ook een blog over de [SAP Cloud toestel bibliotheek 3.0](http://scn.sap.com/community/cloud-appliance-library/blog/2016/05/27/sap-cloud-appliance-library-30-came-with-a-new-user-experience).

> [!NOTE]
Vanaf 29 mei 2017, kunt u het Azure Resource Manager-implementatiemodel naast het klassieke implementatiemodel minder voorkeur gebruikt voor het implementeren van de CAL SAP. U wordt aangeraden dat u de nieuwe Resource Manager-implementatiemodel gebruiken en het klassieke implementatiemodel negeren.

## <a name="step-by-step-process-to-deploy-the-solution"></a>Stapsgewijze procedure voor het implementeren van de oplossing

De volgende reeks schermafbeeldingen ziet u hoe S/4HANA in Azure implementeren met behulp van de CAL SAP. Het proces werkt op dezelfde manier voor andere oplossingen, zoals BW/4HANA.

De **oplossingen** pagina bevat enkele van de oplossingen op basis van een SAP CAL HANA op Azure. **SAP S 4HANA 1610 FPS01, Fully-Activated toestel** is in de middelste rij:

![SAP CAL oplossingen](./media/cal-s4h/s4h-pic-1c.png)

### <a name="create-an-account-in-the-sap-cal"></a>Een account maken in de SAP CAL
1. Voor het eerst aanmelden bij de CAL SAP, gebruikt u uw SAP-gebruiker of een andere gebruiker geregistreerd met SAP. Definieer een SAP CAL-account dat wordt gebruikt door de SAP-CAL voor het implementeren van toepassingen in Azure. In de definitie van de account moet u:

    a. Selecteer het implementatiemodel in Azure (Resource Manager of klassiek).

    b. Voer uw Azure-abonnement. Een SAP CAL-account kan worden toegewezen aan slechts één abonnement. Als u meer dan één abonnement nodig hebt, moet u een ander SAP CAL-account maken.

    c. De SAP CAL machtigen om op te implementeren in uw Azure-abonnement.

    > [!NOTE]
    De volgende stappen laten zien hoe een SAP CAL-account voor implementaties van Resource Manager wilt maken. Als u al een SAP CAL-account dat is gekoppeld aan het klassieke implementatiemodel hebt u *moet* om deze stappen om een nieuwe SAP CAL-account maken. De nieuwe SAP CAL-account moet implementeren in het Resource Manager-model.

2. Maak een nieuwe SAP CAL-account. De **Accounts** pagina ziet u drie opties voor Azure: 

    a. **Microsoft Azure (klassiek)** is van het klassieke implementatiemodel en is niet langer voorkeur.

    b. **Microsoft Azure** is de nieuwe Resource Manager-implementatiemodel.

    c. **Windows Azure worden beheerd door 21Vianet** is een optie in China die gebruikmaakt van het klassieke implementatiemodel.

    Selecteer wilt implementeren in het Resource Manager-model **Microsoft Azure**.

    ![Details van SAP CAL-Account](./media/cal-s4h/s4h-pic-2a.png)

3. Voer de Azure **abonnements-ID** die kunnen worden gevonden in de Azure portal.

   ![SAP CAL Accounts](./media/cal-s4h/s4h-pic3c.png)

4. Voor het autoriseren van de CAL SAP implementeren in de Azure-abonnement u hebt gedefinieerd, klikt u op **autoriseren**. De volgende pagina wordt weergegeven in het browsertabblad:

   ![Internet Explorer cloud services-aanmeldingspagina](./media/cal-s4h/s4h-pic4c.png)

5. Als meer dan één gebruiker wordt weergegeven, kiest u de Microsoft-account dat is gekoppeld, worden de CO-beheerder van het Azure-abonnement dat u hebt geselecteerd. De volgende pagina wordt weergegeven in het browsertabblad:

   ![Bevestiging van Internet Explorer cloud-services](./media/cal-s4h/s4h-pic5a.png)

6. Klik op **accepteren**. Als de autorisatie geslaagd is, wordt de definitie van de SAP CAL-account opnieuw weergegeven. Na korte tijd een bericht wordt bevestigd dat het verificatieproces voltooid is.

7. De zojuist gemaakte SAP CAL-account om aan te wijzen van uw gebruikers, Voer uw **gebruikers-ID** in het tekstvak aan de rechterkant en klikt u op **toevoegen**.

   ![Account toe aan de koppeling met gebruiker](./media/cal-s4h/s4h-pic8a.png)

8. Als u wilt uw account koppelen aan de gebruiker die u aan te melden bij de CAL SAP gebruiken, klikt u op **revisie**. 
 
9. Klik op de koppeling tussen uw gebruiker en de zojuist gemaakte SAP CAL-account om **maken**.

   ![Gebruiker niet aan SAP CAL-account](./media/cal-s4h/s4h-pic9b.png)

Een SAP CAL-account dat kan worden gemaakt:

- Gebruik het Resource Manager-implementatiemodel.
- SAP-systemen implementeren in uw Azure-abonnement.

U kunt nu beginnen met het S/4HANA implementeren in uw gebruikerabonnement in Azure.

> [!NOTE]
Voordat u doorgaat, moet u bepalen of er Azure vCPU quota's voor virtuele machines van Azure H-serie. Op dit moment de CAL SAP H-serie virtuele machines van Azure gebruikt voor het implementeren van enkele van de SAP HANA-oplossingen. Uw Azure-abonnement mogelijk geen quota's vCPU H-serie voor H-serie. Als dit het geval is, moet u mogelijk contact opnemen met ondersteuning van Azure als u een quotum van ten minste 16 H-serie vcpu's.

> [!NOTE]
Wanneer u een oplossing op Azure in de CAL SAP implementeert, kan het gebeuren dat u slechts één Azure-regio kunt. Als u wilt implementeren in Azure-regio's dan die wordt voorgesteld door de CAL SAP, moet u een CAL-abonnement aanschaffen van SAP. Ook is het mogelijk voor het openen van een bericht met SAP hebben uw CAL-account te leveren in Azure-regio's dan degene die in eerste instantie aanbevolen ingeschakeld.

### <a name="deploy-a-solution"></a>Een oplossing implementeren

We implementeren van een oplossing van de **oplossingen** pagina van de CAL SAP. De CAL SAP heeft twee reeksen om te implementeren:

- Een eenvoudige reeks die gebruikmaakt van één pagina voor het definiëren van het systeem om te worden geïmplementeerd
- Een geavanceerde reeks waarmee u bepaalde keuzes op VM-grootten 

Het basic pad naar de implementatie Hier ziet.

1. Op de **accountdetails** pagina, moet u:

    a. Een SAP CAL-account selecteren. (Gebruik een account dat is gekoppeld om te implementeren met het implementatiemodel van Resource Manager.)

    b. Geef het exemplaar van een **naam**.

    c. Selecteer een Azure **regio**. De CAL SAP stelt voor een regio. Als u een andere Azure-regio moet en u een SAP CAL-abonnement hebt, moet u een abonnement CAL met SAP rangschikken.

    d. Voer een model **wachtwoord** voor de oplossing van acht of negen tekens. Het wachtwoord wordt gebruikt voor de beheerders van de verschillende onderdelen.

   ![SAP CAL-standaardmodus: Exemplaar maken](./media/cal-s4h/s4h-pic10a.png)

2. Klik op **maken**, en klik in het berichtvenster op **OK**.

   ![SAP CAL ondersteund VM-grootten](./media/cal-s4h/s4h-pic10b.png)

3. In de **persoonlijke sleutel** in het dialoogvenster, klikt u op **opslaan** voor het opslaan van de persoonlijke sleutel in de CAL SAP. Met wachtwoordbeveiliging voor de persoonlijke sleutel, klikt u op **downloaden**. 

   ![SAP CAL persoonlijke sleutel](./media/cal-s4h/s4h-pic10c.png)

4. Lees de CAL SAP **waarschuwing** bericht en op **OK**.

   ![SAP CAL waarschuwing](./media/cal-s4h/s4h-pic10d.png)

    Nu de implementatie uitgevoerd wordt. Na enige tijd, afhankelijk van de omvang en complexiteit van de oplossing (de CAL SAP biedt een schatting), wordt de status weergegeven als active en klaar voor gebruik.

5. Als u de virtuele machines die zijn verzameld met de bijbehorende bronnen in één resourcegroep zoekt, gaat u naar de Azure-portal: 

   ![SAP CAL-objecten die zijn geïmplementeerd in de nieuwe portal](./media/cal-s4h/sapcaldeplyment_portalview.png)

6. In de portal SAP CAL wordt de status weergegeven als **Active**. Voor verbinding met de oplossing, klikt u op **Connect**. Verschillende opties voor het verbinding maken met de verschillende onderdelen worden geïmplementeerd in deze oplossing.

   ![SAP CAL exemplaren](./media/cal-s4h/active_solution.png)

7. Voordat u een van de opties kunt verbinding maken met de geïmplementeerde systemen, klikt u op **Getting Started Guide**. 

   ![Verbinding met het exemplaar](./media/cal-s4h/connect_to_solution.png)

    De documentatie van de naam van de gebruikers voor elk van de methoden connectiviteit. De wachtwoorden voor deze gebruikers zijn ingesteld op het hoofdwachtwoord die u aan het begin van het implementatieproces gedefinieerd. In de documentatie worden andere meer functionaliteit gebruikers weergegeven met hun wachtwoorden, die u aan te melden bij het geïmplementeerde systeem kunt gebruiken. 

    Bijvoorbeeld, als u de SAP-gebruikersinterface die vooraf geïnstalleerd op de extern bureaublad van Windows-machine, het systeem S/4 als volgt uitzien:

   ![SM50 in de vooraf geïnstalleerde SAP-gebruikersinterface](./media/cal-s4h/gui_sm50.png)

    Of als u de DBACockpit gebruikt, het exemplaar als volgt uitzien:

   ![SM50 in de gebruikersinterface van de SAP DBACockpit](./media/cal-s4h/dbacockpit.png)

Binnen een paar uur wordt een gezonde SAP S/4 toestel geïmplementeerd in Azure.

Als u een SAP CAL-abonnement hebt gekocht, ondersteund SAP implementaties via de CAL SAP volledig in Azure. De Ondersteuningswachtrij is BC-VCM-CAL.







