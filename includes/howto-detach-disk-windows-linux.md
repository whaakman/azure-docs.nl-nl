Wanneer u een gegevensschijf die is gekoppeld aan een virtuele machine niet meer nodig hebt, kunt u deze eenvoudig loskoppelen. Als u een schijf loskoppelt, wordt de schijf van de virtuele machine verwijderd, maar wordt de schijf niet verwijderd uit het Azure-opslagaccount.

Als u de bestaande gegevens op de schijf opnieuw wilt gebruiken, kunt u de schijf opnieuw koppelen aan dezelfde of een andere virtuele machine.  

> [!NOTE]
> Als u de schijf van een besturingssysteem wilt loskoppelen, moet u eerst de virtuele machine verwijderen.
>

## <a name="find-the-disk"></a>De schijf vinden
Als u de naam van de schijf niet weet of de schijf wilt controleren voordat u deze loskoppelt, volgt u deze stappen.

1. Meld u aan bij de [Azure-portal](https://portal.azure.com).

2. Klik op **Virtuele machines** en selecteer vervolgens de toepasselijke VM.

3. Klik onder **Instellingen** op **Schijven** aan de linkerkant van het dashboard van de virtuele machine.

 Het dashboard van de virtuele machine bevat de naam en het type van alle gekoppelde schijven. Op dit scherm wordt bijvoorbeeld een virtuele machine weergegeven met een besturingssysteemschijf en een gegevensschijf:

    ![Gegevensschijf vinden](./media/howto-detach-disk-windows-linux/vmwithdisklist.png)

## <a name="detach-the-disk"></a>De schijf loskoppelen
1. Klik in Azure Portal op **Virtuele machines** en klik vervolgens op de naam van de virtuele machine die de gegevensschijf heeft die u wilt loskoppelen.

2. Klik onder **Instellingen** op **Schijven** aan de linkerkant van het dashboard van de virtuele machine.

3. Klik op de schijf die u wilt loskoppelen.

  ![De schijf zoeken die u wilt loskoppelen](./media/howto-detach-disk-windows-linux/disklist.png)

4. Klik vanuit de opdrachtbalk op **Loskoppelen**.

  ![De opdracht Loskoppelen vinden](./media/howto-detach-disk-windows-linux/diskdetachcommand.png)

5. Klik in het bevestigingsvenster op **Ja** om de schijf los te koppelen.

  ![Het loskoppelen van de schijf bevestigen](./media/howto-detach-disk-windows-linux/confirmdetach.png)

De schijf blijft in de opslag, maar is niet meer gekoppeld aan een virtuele machine.
