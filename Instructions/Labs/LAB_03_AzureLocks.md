---
lab:
  title: '03: Azure Resource Manager-Sperren'
  module: Module 01 - Manage Identity and Access
ms.openlocfilehash: 54375454646bdcf0586b249f65349691c3a3b9c3
ms.sourcegitcommit: a8470295248a6363987bd5ea47154fe39f8535c3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/09/2022
ms.locfileid: "139703533"
---
# <a name="lab-03-resource-manager-locks"></a>Lab 03: Resource Manager-Sperren
# <a name="student-lab-manual"></a>Lab-Handbuch für Kursteilnehmer

## <a name="lab-scenario"></a>Labszenario 

Sie wurden aufgefordert, einen Proof of Concept zu erstellen, der zeigt, wie Ressourcensperren verwendet werden können, um versehentliches Löschen oder versehentliche Änderungen zu verhindern. Insbesondere ist Folgendes erforderlich:

- Erstellen einer ReadOnly-Sperre

- Erstellen einer Löschsperre

> Für alle Ressourcen in diesem Lab verwenden wir die Region **USA, Osten**. Vergewissern Sie sich bei Ihrem Kursleiter, dass dies die Region ist, die für den Kurs verwendet werden soll. 
 
## <a name="lab-objectives"></a>Labziele

In diesem Lab führen Sie die folgende Übung aus:

- Übung 1: Resource Manager-Sperren

## <a name="resource-manager-locks-diagram"></a>Diagramm zu Resource Manager-Sperren

![image](https://user-images.githubusercontent.com/91347931/157514986-1bf6a9ea-4c7f-4487-bcd7-542648f8dc95.png)

## <a name="instructions"></a>Anweisungen

### <a name="exercise-1-resource-manager-locks"></a>Übung 1: Resource Manager-Sperren

#### <a name="estimated-timing-20-minutes"></a>Geschätzte Zeit: 20 Minuten

In dieser Übung führen Sie die folgenden Aufgaben aus:

- Aufgabe 1: Erstellen einer Ressourcengruppe mit einem Speicherkonto.
- Aufgabe 2: Hinzufügen einer ReadOnly-Sperre für das Speicherkonto. 
- Aufgabe 3: Testen der ReadOnly-Sperre. 
- Aufgabe 4: Entfernen der ReadOnly-Sperre und Erstellen einer Löschsperre.
- Aufgabe 5: Testen der Löschsperre.

#### <a name="task-1-create-a-resource-group-with-a-storage-account"></a>Aufgabe 1: Erstellen einer Ressourcengruppe mit einem Speicherkonto.

In dieser Aufgabe erstellen Sie eine Ressourcengruppe und ein Speicherkonto für das Lab. 

1. Melden Sie sich am Azure-Portal ( **`https://portal.azure.com/`** ) an.

    >**Hinweis**: Melden Sie sich am Azure-Portal mit einem Konto an, das über die Rolle „Besitzer“ oder „Mitwirkender“ in dem Azure-Abonnement verfügt, das Sie für dieses Lab nutzen.

1. Öffnen Sie Cloud Shell, indem Sie oben rechts im Azure-Portal auf das erste Symbol klicken. Wenn Sie dazu aufgefordert werden, wählen Sie **PowerShell** und dann **Speicher erstellen** aus.

1. Stellen Sie sicher, dass oben links im Cloud Shell-Bereich im Dropdownmenü der Eintrag **PowerShell** ausgewählt ist.

1. Führen Sie in der PowerShell-Sitzung im Cloud Shell-Bereich Folgendes aus, um eine Ressourcengruppe zu erstellen (klären Sie den Wert des location-Parameters mit Ihrem Dozenten ab):

    ```powershell
    New-AzResourceGroup -Name AZ500LAB03 -Location 'EastUS'
    ```

1. Führen Sie in der PowerShell-Sitzung im Cloud Shell-Bereich Folgendes aus, um ein Speicherkonto in der neu erstellten Ressourcengruppe zu erstellen:
    
    ```powershell
    New-AzStorageAccount -ResourceGroupName AZ500LAB03 -Name (Get-Random -Maximum 999999999999999) -Location  EastUS -SkuName Standard_LRS -Kind StorageV2 
    ```

   >**Hinweis**: Warten Sie, bis das Speicherkonto erstellt wurde. Dieser Vorgang kann einige Minuten in Anspruch nehmen. 

1. Schließen Sie den Cloud Shell-Bereich.

#### <a name="task-2-add-a-readonly-lock-on-the-storage-account"></a>Aufgabe 2: Hinzufügen einer ReadOnly-Sperre für das Speicherkonto. 

In dieser Aufgabe fügen Sie dem Speicherkonto eine Schreibschutzsperre hinzu. Dadurch wird die Ressource vor versehentlichem Löschen oder Ändern geschützt. 

1. Geben Sie im Azure-Portal oben auf der Azure-Portalseite im Textfeld **Nach Ressourcen, Diensten und Dokumenten suchen** den Begriff **Ressourcengruppen** ein, und drücken Sie die **EINGABETASTE**.

1. Wählen Sie auf dem Blatt **Ressourcengruppen** den Ressourcengruppeneintrag **AZ500LAB03** aus.

1. Wählen Sie auf dem Blatt der Ressourcengruppe **AZ500LAB03** in der Liste der Ressourcen das neue Speicherkonto aus. 

1. Klicken Sie im Abschnitt **Einstellungen** auf das Symbol „Sperren“.

1. Klicken Sie auf **+ Hinzufügen**, und geben Sie die folgenden Einstellungen an:

   |Einstellung|Wert|
   |---|---|
   |Sperrenname|**ReadOnly Lock**|
   |Sperrtyp|**Schreibgeschützt**|

1. Klicken Sie auf **OK**. 

   >**Hinweis**: Das Speicherkonto ist jetzt vor versehentlichem Löschen und Ändern geschützt.

#### <a name="task-3-test-the-readonly-lock"></a>Aufgabe 3: Testen der ReadOnly-Sperre 

1. Klicken Sie im Abschnitt **Einstellungen** des Blatts „Speicherkonto“ auf **Konfiguration**.

1. Legen Sie die Option **Sichere Übertragung erforderlich** auf **Deaktiviert** fest, und klicken Sie dann auf **Speichern**.

1. Sie sollten eine Benachrichtigung mit der Meldung **Fehler beim Aktualisieren des Speicherkontos** erhalten.

1. Klicken Sie oben im Azure-Portal auf das Symbol **Benachrichtigungen**, und überprüfen Sie die Benachrichtigung, die dem folgenden Text ähnelt: 

    > **Fehler beim Aktualisieren des Speicherkontos "xxxxxxxx". Fehler: Der Bereich "xxxxxxxx" kann keinen Schreibvorgang ausführen, da folgende Bereiche gesperrt sind: '/subscriptions/xxxxx-xxx-xxxx-xxxx-xxxxxxxx/resourceGroups/AZ500LAB03/providers/Microsoft. Storage/storageAccounts/xxxxxxx'. Entfernen Sie die Sperre, und versuchen Sie es erneut.**

1. Kehren Sie zum Blatt **Konfiguration** des Speicherkontos zurück, und klicken Sie auf **Verwerfen**. 

1. Wählen Sie auf dem Blatt „Speicherkonto“ die Option **Übersicht** aus, und klicken Sie auf dem Blatt **Übersicht** auf **Löschen**.

1. Geben Sie auf dem Blatt **Speicherkonto löschen** den Namen des Speicherkontos ein, um zu bestätigen, dass Sie fortfahren möchten, und klicken Sie dann auf **Löschen**.

1. Überprüfen Sie die neu generierte Benachrichtigung, die dem folgenden Text ähnelt: 

    > **Fehler beim Löschen des Speicherkontos "xxxxxxx". Fehler: Der Bereich "xxxxxxx" kann keinen Löschvorgang ausführen, da folgende Bereiche gesperrt sind: '/subscriptions/xxxx-xxxx-xxxx-xxxx-xxxxxx/resourceGroups/AZ500LAB03/providers/Microsoft. Storage/storageAccounts/xxxxxxx'. Entfernen Sie die Sperre, und versuchen Sie es erneut.**

   >**Hinweis**: Sie haben nun sichergestellt, dass eine ReadOnly-Sperre das versehentliche Löschen und Ändern einer Ressource verhindert.

#### <a name="task-4-remove-the-readonly-lock-and-create-a-delete-lock"></a>Aufgabe 4: Entfernen der ReadOnly-Sperre und Erstellen einer Löschsperre.

In dieser Aufgabe entfernen Sie die ReadOnly-Sperre aus dem Speicherkonto und erstellen eine Löschsperre. 

1. Navigieren Sie im Azure-Portal zurück zum Blatt, das die Eigenschaften des neu erstellten Speicherkontos anzeigt.

1. Wählen Sie im Abschnitt **Einstellungen** die Option **Sperren** aus.  

1. Klicken Sie auf dem Blatt **Sperren** ganz rechts neben dem Eintrag **ReadOnly-Sperre** auf das **Löschsymbol**.

1. Klicken Sie auf **+ Hinzufügen**, und geben Sie die folgenden Einstellungen an:

   |Einstellung|Wert|
   |---|---|
   |Sperrenname|**Löschsperre**|
   |Sperrtyp|**Löschen**|

1. Klicken Sie auf **OK**. 

#### <a name="task-5-test-the-delete-lock"></a>Aufgabe 5: Testen der Löschsperre.

In dieser Aufgabe testen Sie die Löschsperre. Sie sollten das Speicherkonto ändern, aber nicht löschen können. 

1. Klicken Sie im Abschnitt **Einstellungen** des Blatts „Speicherkonto“ auf **Konfiguration**.

1. Legen Sie die Option **Sichere Übertragung erforderlich** auf **Deaktiviert** fest, und klicken Sie dann auf **Speichern**.

   >**Hinweis**: Dieses Mal sollte die Änderung erfolgreich sein.

1. Wählen Sie auf dem Blatt „Speicherkonto“ die Option **Übersicht** aus, und klicken Sie auf dem Blatt **Übersicht** auf **Löschen**.

1. Geben Sie auf dem Blatt **Speicherkonto löschen** den Namen des Speicherkontos ein, um zu bestätigen, dass Sie fortfahren möchten, und klicken Sie dann auf **Löschen**.

1. Überprüfen Sie die Benachrichtigung, die dem folgenden Text ähnelt: 

    > **'xxxxxx' kann nicht gelöscht werden, weil diese Ressource oder das übergeordnete Element eine Löschsperre aufweist. Die Sperren müssen entfernt werden, damit diese Ressource gelöscht werden kann.**

   >**Hinweis**: Sie haben nun sichergestellt, dass eine **Löschsperre** Konfigurationsänderungen zulässt, aber versehentliches Löschen verhindert.

   >**Hinweis**: Mithilfe von Ressourcensperren können Sie eine zusätzliche Schutzmaßnahme gegen versehentliche oder böswillige Änderungen und/oder das Löschen der wichtigsten Ressourcen implementieren. Ressourcensperren können von jedem Benutzer mit der Rolle **Besitzer** entfernt werden, aber dies erfordert eine bewusste Anstrengung. Sperren ergänzen die rollenbasierte Zugriffssteuerung. 

> Ergebnisse: In dieser Übung haben Sie gelernt, Resource Manager-Sperren zu verwenden, um Ressourcen vor Änderungen und versehentlichem Löschen zu schützen.

**Bereinigen von Ressourcen**

> Denken Sie daran, alle neu erstellten Azure-Ressourcen zu entfernen, die Sie nicht mehr verwenden. Durch das Entfernen nicht verwendeter Ressourcen wird sichergestellt, dass keine unerwarteten Kosten anfallen.

1. Öffnen Sie im Azure-Portal Cloud Shell, indem Sie oben rechts im Azure-Portal auf das erste Symbol klicken. Wenn Sie dazu aufgefordert werden, klicken Sie auf **Verbindung wiederherstellen**.

1. Führen Sie in der PowerShell-Sitzung im Cloud Shell-Bereich Folgendes aus, um die Löschsperre zu entfernen:

    ```powershell
    $storageaccountname = (Get-AzStorageAccount -ResourceGroupName AZ500LAB03).StorageAccountName

    $lockName = (Get-AzResourceLock -ResourceGroupName AZ500LAB03 -ResourceName $storageAccountName -ResourceType Microsoft.Storage/storageAccounts).Name

    Remove-AzResourceLock -LockName $lockName -ResourceName $storageAccountName  -ResourceGroupName AZ500LAB03 -ResourceType Microsoft.Storage/storageAccounts -Force
    ```

1.  Führen Sie in der PowerShell-Sitzung im Cloud Shell-Bereich Folgendes aus, um die Ressourcengruppe zu entfernen:

    ```powershell
    Remove-AzResourceGroup -Name "AZ500LAB03" -Force -AsJob
    ```

1.  Schließen Sie den **Cloud Shell**-Bereich. 
