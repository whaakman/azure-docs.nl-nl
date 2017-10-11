#### <a name="to-create-a-new-service"></a>Een nieuwe service maken

1.  Meld u met uw Microsoft-accountreferenties aan bij Azure Portal met deze URL: <https://portal.azure.com/>. Als het apparaat in de portal van de overheid implementeert, aanmelden op: <https://portal.azure.us/>

2.  Klik in de Azure-portal op **+ nieuw** &gt; **opslag** &gt; **virtuele StorSimple-reeks**.

    ![Nieuwe service maken](./media/storsimple-virtual-array-create-new-service/createnewservice2.png) 

3.  In de **StorSimple Apparaatbeheer** blade die wordt geopend, het volgende doen:

    1.  Geef een unieke **resourcenaam** voor uw service op. De resourcenaam is een beschrijvende naam die kan worden gebruikt om de service te identificeren. De naam kan tussen 2 en 50 tekens bevatten (letters, cijfers en afbreekstreepjes). De naam moet beginnen en eindigen met een letter of cijfer.

    2.  Kies een **abonnement** in de vervolgkeuzelijst. Het abonnement is gekoppeld aan uw factureringsrekening. Dit veld wordt niet weergegeven als u slechts één abonnement hebt.

    3.  Voor **resourcegroep**, selecteer een bestaande of maak een nieuwe groep. Zie [Azure-resourcegroepen](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-infrastructure-resource-groups-guidelines/) voor meer informatie.

    4.  Geef een **locatie** voor uw service op. Zie [Azure-gebieden](https://azure.microsoft.com/regions/#services) voor meer informatie over welke services beschikbaar in welke regio zijn. Kies in het algemeen een **locatie** die het dichtst bij de geografische regio waar u het apparaat implementeert. U kunt ook rekening houden met het volgende:

        -   Als u bestaande workloads in Azure die u ook wilt implementeren met uw StorSimple-apparaat hebt, raden wij u dat datacenter gebruiken.

        -   Uw StorSimple-Apparaatbeheer en Azure-opslag kan bestaan uit twee verschillende locaties. In dat geval moet u de StorSimple--apparaatbeheerfunctie en het Azure Storage-account afzonderlijk maken. U maakt een Azure Storage-account door naar de Azure Storage-service in Azure Portal te gaan en de stappen in [Een Azure Storage-account maken](https://azure.microsoft.com/documentation/articles/storage-create-storage-account/#create-a-storage-account) uit te voeren. Nadat u dit account hebt gemaakt, voegt u het toe aan de StorSimple-apparaatbeheerfunctie met de stappen in [Een nieuw opslagaccount voor de service maken](https://azure.microsoft.com/en-us/documentation/articles/storsimple-deployment-walkthrough/#configure-a-new-storage-account-for-the-service).

        -   Als het virtuele apparaat in de Portal Government implementeert, is de service Manager voor StorSimple-apparaat beschikbaar op ons Iowa en ons Virginia locaties.

    5.  Selecteer **maken van een nieuwe Azure-opslagaccount** automatisch een opslagaccount maken met de service. Geef een **opslagaccountnaam**. Als u een andere locatie voor uw gegevens wilt kiezen, schakelt u dit vakje uit.

    6.  Schakel **Aan dashboard vastmaken** in als u een snelkoppeling naar deze service op uw dashboard wilt.

    7.  Klik op **Maken** om de StorSimple-apparaatbeheerfunctie te maken.

        ![Nieuwe service maken](./media/storsimple-virtual-array-create-new-service/createnewservice4.png)  

U wordt omgeleid naar de **Service** startpagina. Het maken van de service duurt enkele minuten. Nadat de service is gemaakt, krijgt u hierover een melding en wordt de status van de service gewijzigd in **Actief**.


