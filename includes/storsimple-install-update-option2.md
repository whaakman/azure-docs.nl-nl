<!--author=SharS last changed: 03/17/2016-->

#### <a name="to-install-update-12-from-the-azure-classic-portal"></a>1.2 Update installeren via de klassieke Azure portal
1. In de klassieke Azure portal, gaat u naar de **apparaten** pagina en selecteer het apparaat.
2. Navigeer naar **apparaten** > **configureren**.
3. Onder **netwerkinterfaces**, eerst controleren of er ten minste één netwerkinterface die iSCSI is ingeschakeld. Ga vervolgens naar de netwerkinterface (behalve DATA 0) die een gateway die is toegewezen.
4. Schakel de netwerkinterface die een gateway is toegewezen en sla de gewijzigde configuratie. Let op de netwerkinterface-instellingen behouden en dus wanneer u opnieuw deze netwerkinterface later inschakelt, de portal wordt teruggezet naar de oorspronkelijke instellingen.
5. U kunt nu [de klassieke Azure portal gebruiken voor het installeren van Update 1.2](#install-update-12-via-the-azure-classic-portal). Volg de instructies vanaf stap 3 van deze procedure. Nadat u de updates hebt geïnstalleerd, kunt u de netwerkinterface die u hebt uitgeschakeld opnieuw inschakelen.

