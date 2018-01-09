---
title: Het gebruik van de invoegtoepassing Azure slave met Hudson continue integratie | Microsoft Docs
description: Beschrijft hoe u de invoegtoepassing Azure slave met Hudson continue integratie.
services: virtual-machines-linux
documentationcenter: 
author: rmcmurray
manager: wpickett
editor: 
ms.assetid: b2083d1c-4de8-4a19-a615-ccc9d9b6e1d9
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-multiple
ms.devlang: java
ms.topic: article
ms.date: 04/25/2017
ms.author: robmcm
ms.openlocfilehash: c11b59f8ea432075b147a391de4b7bd3331e639e
ms.sourcegitcommit: 9a8b9a24d67ba7b779fa34e67d7f2b45c941785e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/08/2018
---
# <a name="how-to-use-the-azure-slave-plug-in-with-hudson-continuous-integration"></a>Het gebruik van de invoegtoepassing Azure slave met Hudson continue integratie
De invoegtoepassing voor Hudson Azure slave kunt u knooppunten van de slave op Azure inrichten bij het uitvoeren van gedistribueerde bouwt.

## <a name="install-the-azure-slave-plug-in"></a>De Azure-Slave invoegtoepassing installeren
1. Klik in het dashboard Hudson **Hudson beheren**.
2. In de **beheren Hudson** pagina, klikt u op **invoegtoepassingen beheren**.
3. Klik op de **beschikbaar** tabblad.
4. Klik op **Search** en het type **Azure** te beperken van de lijst om de relevante invoegtoepassingen.
   
    Als u ervoor kiezen om te bladeren door de lijst met beschikbare invoegtoepassingen, vindt u de Azure slave invoegtoepassing onder de **Clusterbeheer en gedistribueerd bouwen** sectie het **anderen** tabblad.
5. Schakel het selectievakje voor **Azure Slave invoegtoepassing**.
6. Klik op **Install**.
7. Opnieuw opstarten Hudson.

Nu dat de invoegtoepassing is geïnstalleerd, worden de volgende stappen voor het configureren van de invoegtoepassing met het profiel van uw Azure-abonnement en een sjabloon die wordt gebruikt bij het maken van de virtuele machine voor het knooppunt slave maken.

## <a name="configure-the-azure-slave-plug-in-with-your-subscription-profile"></a>De Azure-Slave invoegtoepassing configureren met het profiel van uw abonnement
Een abonnement-profiel, ook bekend als de publicatie-instellingen, is een XML-bestand met beveiligde referenties en wat extra informatie die u wilt werken met Azure in uw ontwikkelomgeving. Voor het configureren van de invoegtoepassing Azure slave, hebt u het volgende nodig:

* Uw abonnements-id
* Een beheercertificaat voor uw abonnement

Deze vindt u in uw [abonnement profiel]. Hieronder volgt een voorbeeld van een profiel voor een abonnement.

    <?xml version="1.0" encoding="utf-8"?>

        <PublishData>

          <PublishProfile SchemaVersion="2.0" PublishMethod="AzureServiceManagementAPI">

        <Subscription

              ServiceManagementUrl="https://management.core.windows.net"

              Id="<Subscription ID>"

              Name="Pay-As-You-Go"
            ManagementCertificate="<Management certificate value>" />

          </PublishProfile>

    </PublishData>

Zodra u het profiel van uw abonnement hebt, volg deze stappen voor het configureren van de invoegtoepassing Azure slave.

1. Klik in het dashboard Hudson **Hudson beheren**.
2. Klik op **systeem configureren**.
3. Schuif omlaag in de pagina naar de **Cloud** sectie.
4. Klik op **toevoegen van nieuwe cloud > Microsoft Azure**.
   
    ![toevoegen van nieuwe cloud][add new cloud]
   
    Hier ziet de velden waarop u moet uw abonnementsgegevens invoeren.
   
    ![profiel configureren][configure profile]
5. Het abonnement-id en het beheer certificaat uit het profiel van uw abonnement Kopieer en plak deze in de juiste velden.
   
    Bij het kopiëren van het abonnement-id en de management-certificaat **niet** de aanhalingstekens die plaatst u de waarden.
6. Klik op **controleren configuratie**.
7. Wanneer de configuratie is geverifieerd, klikt u op **opslaan**.

## <a name="set-up-a-virtual-machine-template-for-the-azure-slave-plug-in"></a>Een virtuele machine-sjabloon instellen voor de Azure-Slave invoegtoepassing
Een VM-sjabloon definieert de parameters die de invoegtoepassing wordt gebruikt voor het maken van een knooppunt slave op Azure. In de volgende stappen maakt we sjabloon voor een Ubuntu VM.

1. Klik in het dashboard Hudson **Hudson beheren**.
2. Klik op **systeem configureren**.
3. Schuif omlaag in de pagina naar de **Cloud** sectie.
4. Binnen de **Cloud** sectie, zoeken **Azure virtuele Machine-sjabloon toevoegen** en klik op de **toevoegen** knop.
   
    ![vm-sjabloon toevoegen][add vm template]
5. Geef de naam van een cloudservice in de **naam** veld. Als de opgegeven naam naar een bestaande cloudservice verwijst, dat de virtuele machine wordt ingericht in die service. Azure wordt anders maakt u een nieuwe.
6. In de **beschrijving** en voer de tekst die beschrijft de sjabloon die u maakt. Deze informatie wordt alleen gebruikt voor administratieve doeleinden en wordt niet gebruikt in een VM-inrichting.
7. In de **Labels** veld **linux**. Dit label wordt gebruikt voor het identificeren van de sjabloon die u maakt en vervolgens worden gebruikt om te verwijzen naar de sjabloon bij het maken van een taak Hudson.
8. Selecteer een regio waar de virtuele machine wordt gemaakt.
9. Selecteer de relevante VM-grootte.
10. Geef een opslagaccount waar de virtuele machine wordt gemaakt. Zorg ervoor dat deze zich in dezelfde regio bevinden als de cloudservice die u wilt gebruiken. Als u wilt dat nieuwe opslag worden gemaakt, kunt u dit veld leeg laten.
11. Bewaartijd geeft het aantal minuten voordat Hudson wordt een niet-actieve slave verwijderd. Dit laat de standaardwaarde van 60.
12. In **gebruik**, selecteert u de juiste voorwaarde wanneer dit knooppunt slave wordt gebruikt. Selecteer nu **gebruikmaken van dit knooppunt zo veel mogelijk**.
    
     Op dit moment eruit uw formulier enigszins vergelijkbaar met het volgende:
    
     ![De sjabloonconfiguratie][template config]
13. In **installatiekopie familie-Id of** u moet opgeven welke installatiekopie wordt geïnstalleerd op de virtuele machine. U kunt selecteren in een lijst van families van de afbeelding of geef een aangepaste installatiekopie.
    
     Als u selecteren in een lijst van families van de installatiekopie wilt, voert u het eerste teken (hoofdlettergevoelig) van de naam van de installatiekopie-serie. Voor het exemplaar typen **U** verschijnt nu een lijst met Ubuntu Server families. Wanneer u in de lijst selecteert, wordt Jenkins de nieuwste versie van deze installatiekopie van die familie gebruiken bij het inrichten van uw virtuele machine.
    
     ![Lijst met familie van besturingssystemen][OS family list]
    
     Als u een aangepaste installatiekopie die u wilt gebruiken in plaats daarvan hebt, voer de naam van de aangepaste installatiekopie. Afbeelding van aangepaste namen worden niet weergegeven in een lijst zodat u ervoor te zorgen dat de naam correct worden ingevoerd.    
    
     Typ voor deze zelfstudie **U** online zetten van een lijst met afbeeldingen Ubuntu en selecteer **Ubuntu Server 14.04 TNS**.
14. Voor **methode starten**, selecteer **SSH**.
15. Het onderstaande script kopiëren en plakken in de **Init script** veld.
    
         # Install Java
    
         sudo apt-get -y update
    
         sudo apt-get install -y openjdk-7-jdk
    
         sudo apt-get -y update --fix-missing
    
         sudo apt-get install -y openjdk-7-jdk
    
         # Install git
    
         sudo apt-get install -y git
    
         #Install ant
    
         sudo apt-get install -y ant
    
         sudo apt-get -y update --fix-missing
    
         sudo apt-get install -y ant
    
     De **Init script** worden uitgevoerd nadat de virtuele machine is gemaakt. In dit voorbeeld installeert het script Java, git en ant.
16. In de **gebruikersnaam** en **wachtwoord** velden, voert u de gewenste waarden voor de administrator-account dat wordt gemaakt op de virtuele machine.
17. Klik op **sjabloon controleren** om te controleren of de parameters die u hebt opgegeven ongeldig zijn.
18. Klik op **Opslaan**.

## <a name="create-a-hudson-job-that-runs-on-a-slave-node-on-azure"></a>Een taak Hudson die wordt uitgevoerd op een lager niveau bevindende-knooppunt in Azure maken
In deze sectie maakt u een Hudson-taak die wordt uitgevoerd op een knooppunt slave op Azure.

1. Klik in het dashboard Hudson **nieuwe taak**.
2. Voer een naam voor de taak die u maakt.
3. Selecteer voor het taaktype **bouwen van een taak vrije-stijl software**.
4. Klik op **OK**.
5. Selecteer in de configuratiepagina van de taak **beperken waar dit project kan worden uitgevoerd**.
6. Selecteer **knooppunt en label menu** en selecteer **linux** (we opgegeven dit label bij het maken van de virtuele machine-sjabloon in de vorige sectie).
7. In de **bouwen** sectie, klikt u op **toevoegen build stap** en selecteer **shell uitvoeren**.
8. Bewerken van het volgende script, vervangen **{uw github-accountnaam}**, **{projectnaam van uw}**, en **{uw projectmap}** met waarden nodig en plak het bewerkte het script in het tekstgebied die wordt weergegeven.
   
        # Clone from git repo
   
        currentDir="$PWD"
   
        if [ -e {your project directory} ]; then
   
              cd {your project directory}
   
              git pull origin master
   
        else
   
              git clone https://github.com/{your github account name}/{your project name}.git
   
        fi
   
        # change directory to project
   
        cd $currentDir/{your project directory}
   
        #Execute build task
   
        ant
9. Klik op **Opslaan**.
10. Zoek in het dashboard Hudson de taak die u zojuist hebt gemaakt en klik op de **plannen van een build** pictogram.

Hudson vervolgens maakt u een lager niveau bevindende knooppunt met behulp van de sjabloon in de vorige sectie hebt gemaakt en voer het script dat u hebt opgegeven in de stap build voor deze taak.

## <a name="next-steps"></a>Volgende stappen
In het [Azure Java Developer Center] vindt u meer informatie over het gebruik van Azure met Java.

<!-- URL List -->

[Azure Java Developer Center]: https://azure.microsoft.com/develop/java/
[abonnement profiel]: http://go.microsoft.com/fwlink/?LinkID=396395

<!-- IMG List -->

[add new cloud]: ./media/virtual-machines-azure-slave-plugin-for-hudson/hudson-setup-addcloud.png
[configure profile]: ./media/virtual-machines-azure-slave-plugin-for-hudson/hudson-setup-configureprofile.png
[add vm template]: ./media/virtual-machines-azure-slave-plugin-for-hudson/hudson-setup-addnewvmtemplate.png
[template config]: ./media/virtual-machines-azure-slave-plugin-for-hudson/hudson-setup-templateconfig1-withdata.png
[OS family list]: ./media/virtual-machines-azure-slave-plugin-for-hudson/hudson-oslist.png

