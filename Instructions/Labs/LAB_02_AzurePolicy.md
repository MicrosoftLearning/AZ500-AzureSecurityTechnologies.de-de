---
lab:
  title: '02: Azure Policy'
  module: Module 01 - Manage Identity and Access
ms.openlocfilehash: cde2b0c001dedef2b29c2da320e2952f9766b069
ms.sourcegitcommit: 2eb153f2856445e5afaa218a012cb92e3d48f24b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/16/2021
ms.locfileid: "132625653"
---
# <a name="lab-02-azure-policy"></a>Lab 02: Azure Policy
# <a name="student-lab-manual"></a>Lab-Handbuch für Kursteilnehmer

## <a name="lab-scenario"></a>Labszenario

Sie wurden aufgefordert, einen Proof of Concept zu erstellen, der zeigt, wie Azure Policy verwendet werden kann. Insbesondere ist Folgendes erforderlich:

- Erstellen einer Richtlinie „Zulässige Standorte“, die sicherstellt, dass Ressourcen nur in einer bestimmten Region erstellt werden.
- Sicherstellen durch Tests, dass Ressourcen nur am zulässigen Standort erstellt werden können

> Für alle Ressourcen in diesem Lab verwenden wir die Region **USA, Osten**. Vergewissern Sie sich bei Ihrem Kursleiter, dass dies die Region ist, die für den Kurs verwendet werden soll. 

## <a name="lab-objectives"></a>Ziele des Labs

In diesem Lab führen Sie die folgenden Aufgaben aus:

- Übung 1: Implementieren von Azure Policy. 

### <a name="exercise-1-implement-azure-policy"></a>Übung 1: Implementieren von Azure Policy

#### <a name="estimated-timing-20-minutes"></a>Geschätzte Zeit: 20 Minuten

In dieser Übung führen Sie die folgenden Aufgaben aus:

- Aufgabe 1: Erstellen einer Azure-Ressourcengruppe. 
- Aufgabe 2: Erstellen einer Richtlinienzuweisung „Zulässige Standorte“.
- Aufgabe 3: Überprüfen, ob die Richtlinienzuweisung „Zulässige Standorte“ funktioniert. 

#### <a name="task-1-create-a-resource-group-for-the-lab"></a>Aufgabe 1: Erstellen einer Ressourcengruppe für das Lab. 

In dieser Aufgabe erstellen Sie eine Ressourcengruppe für das Lab. 

1. Melden Sie sich am Azure-Portal ( **`https://portal.azure.com/`** ) an.

    >**Hinweis**: Melden Sie sich am Azure-Portal mit einem Konto an, das über die Rolle „Besitzer“ oder „Mitwirkender“ in dem Azure-Abonnement verfügt, das Sie für dieses Lab verwenden.

1. Öffnen Sie Cloud Shell, indem Sie oben rechts im Azure-Portal auf das erste Symbol klicken. Wenn Sie dazu aufgefordert werden, wählen Sie **PowerShell** und dann **Speicher erstellen** aus.

1. Stellen Sie sicher, dass oben links im Cloud Shell-Bereich im Dropdownmenü der Eintrag **PowerShell** ausgewählt ist.

1. Führen Sie in der PowerShell-Sitzung im Cloud Shell-Bereich Folgendes aus, um eine Ressourcengruppe zu erstellen (klären Sie den Wert des location-Parameters mit Ihrem Dozenten ab):

    ```powershell
    New-AzResourceGroup -Name AZ500LAB02 -Location 'East US'
    ```

1. Führen Sie in der PowerShell-Sitzung im Cloud Shell-Bereich Folgendes aus, um Ressourcengruppen aufzulisten und zu bestätigen, dass die neue Ressourcengruppe erstellt wurde:

    ```powershell
    Get-AzResourceGroup | format-table
    ```

1. Schließen Sie **Cloud Shell**.

#### <a name="task-2-create-an-allowed-locations-policy-assignment"></a>Aufgabe 2: Erstellen einer Richtlinienzuweisung „Zulässige Standorte“.

In dieser Aufgabe erstellen Sie eine Richtlinienzuweisung „Zulässige Standorte“ und geben an, welche Azure-Regionen die Richtlinie verwenden kann. 

1. Geben Sie im Azure-Portal oben auf der Azure-Portalseite im Textfeld **Nach Ressourcen, Diensten und Dokumenten suchen** den Begriff **Ressourcen** ein, und drücken Sie die **EINGABETASTE**.

1. Wählen Sie auf dem Blatt **Richtlinie** im Abschnitt **Erstellung** die Option **Definitionen** aus.

1. Nehmen Sie sich eine Minute Zeit, um die integrierten Definitionen zu durchsuchen. Verwenden Sie die Dropdownliste **Kategorie**, um die Liste der Richtlinien zu filtern.

1. Geben Sie im Textfeld **Suchen** die Anhabe **Zulässige Speicherorte** ein. 

   >**Hinweis**: Mit der Richtlinie **Zulässige Standorte** können Sie den Standort von Ressourcen (nicht von Ressourcengruppen) einschränken. Um Standorte von Ressourcengruppen einzuschränken, können Sie die Richtlinie **Zulässige Standorte für Ressourcengruppen** verwenden.

1. Klicken Sie auf die Richtliniendefinition **Zulässige Standorte**, um die zugehörigen Details anzuzeigen. 

   >**Hinweis**: Diese Richtliniendefinition verwendet ein Array von Standorten als Parameter. Eine Richtlinienregel ist eine Wenn-dann-Anweisung (if-then). Die if-Klausel überprüft, ob der Ressourcenstandort in der Parameterliste enthalten ist. Wenn dies nicht der Fall ist, verweigert die then-Klausel die Ressourcenerstellung oder markiert vorhandene Ressourcen als nicht konform.

1. Klicken Sie auf dem Blatt **Zulässige Standorte** auf **Zuweisen**.

1. Klicken Sie auf der Registerkarte **Grundeinstellungen** des Blatts **Zulässige Standorte** neben dem Textfeld **Bereich** auf die Schaltfläche mit den Auslassungszeichen (...), und geben Sie auf dem Blatt **Bereich** die folgenden Einstellungen an:

   |Einstellung|Wert|
   |---|---|
   |Subscription|Der Name Ihres Azure-Abonnements.|
   |Resource group|**AZ500LAB02**|

1. Klicken Sie auf **Auswählen**.

1. Geben Sie auf dem Blatt **Zulässige Standorte** auf der Registerkarte **Grundeinstellungen** die folgenden Einstellungen an (übernehmen Sie die Standardwerte für die anderen Einstellungen):

   |Einstellung|Wert|
   |---|---|
   |Zuweisungsname|**„Vereinigtes Königreich, Süden“ für AZ500LAB02 zulassen**|
   |BESCHREIBUNG|**Erstellen von Ressourcen in „Vereinigtes Königreich, Süden“ nur für AZ500LAB02 zulassen**|
   |Durchsetzung von Richtlinien|**Aktiviert**|

1. Klicken Sie auf **Weiter**.

1. Wählen Sie auf der Registerkarte **Parameter** des Blatts **Zulässige Standorte** in der Dropdownliste **Zulässige Standorte** die Option **Vereinigtes Königreich, Süden** als einzigen zulässigen Standort aus. 

   >**Hinweis**: Sie können mehrere Standorte auswählen. Wenn die Richtlinie einen anderen Satz von Parametern erfordern würde, würde diese Registerkarte deren Auswahl bereitstellen. 

1. Klicken Sie auf **Überprüfen und erstellen** und dann auf **Erstellen**, um die Richtlinienzuweisung zu erstellen. 

   >**Hinweis**: Es wird eine Benachrichtigung angezeigt, dass die Zuweisung erfolgreich war und ungefähr 30 Minuten bis zur Fertigstellung in Anspruch nehmen kann.

   >**Hinweis**: Der Grund, warum es bis zu 30 Minuten dauern kann, bis die Azure-Richtlinienzuweisung wirksam wird, liegt in ihrer globalen Replikation. Dies dauert in der Regel nur wenige Minuten.  Wenn die nächste Aufgabe fehlschlägt, warten Sie einfach einige Minuten, und versuchen Sie die entsprechenden Schritte dann erneut.

#### <a name="task-3-test-the-allowed-locations-policy-assignment"></a>Aufgabe 3: Testen der Richtlinienzuweisung „Zulässige Standorte“

In dieser Aufgabe  testen Sie die Richtlinienzuweisung „Zulässige Standorte“. 

1. Geben Sie im Azure-Portal oben auf der Azure-Portalseite im Textfeld **Nach Ressourcen, Diensten und Dokumenten suchen** den Begriff **Virtuelle Netzwerke** ein, und drücken Sie die **EINGABETASTE**.

1. Klicken Sie auf dem Blatt **Virtuelle Netzwerke** auf **+ Erstellen**.

   >**Hinweis**: Zunächst versuchen Sie, ein virtuelles Netzwerk in „USA, Osten“ zu erstellen. Da dies kein zulässiger Standort ist, sollte die Anforderung blockiert werden. 

1. Geben Sie auf der Registerkarte **Grundeinstellungen** des Blatts **Neues Netzwerk erstellen** die folgenden Einstellungen an (übernehmen Sie die Standardwerte für andere Einstellungen):

    |Einstellung|Wert|
    |---|---|
    |Resource group|**AZ500LAB02**|
    |Name|**myVnet**|
    |Region|**(USA) USA, Osten**|

1. Klicken Sie auf **Überprüfen + erstellen**. 

1. Beachten Sie auf der Registerkarte **Überprüfen und erstellen** des Blatts **Virtuelles Netzwerk erstellen** die Meldung **Überprüfungsfehler**. 

    > **Hinweis**: Wenn die Warnung **Überprüfungsfehler** nicht angezeigt wird, klicken Sie auf **Zurück**, und warten Sie einige Minuten.

1. Klicken Sie auf die Fehlermeldung, um das Blatt **Fehler** zu öffnen. Es wird eine ausführliche Fehlermeldung angezeigt, die besagt, dass die Bereitstellung der Ressource **myVnet** aufgrund der Richtlinie nicht zulässig war.

1. Schließen Sie das Blatt **Fehler**, klicken Sie auf dem Blatt **Virtuelles Netzwerk erstellen** auf die Registerkarte **Grundlagen**, und wählen Sie dann in der Dropdownliste **Region** die Option **(Europa) Vereinigtes Königreich, Süden** aus.

1. Klicken Sie auf **Überprüfen und erstellen**, bestätigen Sie, dass die Überprüfung erfolgreich war, klicken Sie auf **Erstellen**, und bestätigen Sie, dass das virtuelle Netzwerk erfolgreich erstellt wurde. 

> Ergebnisse der Übung: In dieser Übung haben Sie gelernt, eine Azure-Richtlinie anzuwenden, indem Sie eine integrierte Richtliniendefinition ausgewählt und sie einer Ressourcengruppe zugewiesen haben.

**Bereinigen von Ressourcen**

> Denken Sie daran, alle neu erstellten Azure-Ressourcen zu entfernen, die Sie nicht mehr verwenden. Durch das Entfernen nicht verwendeter Ressourcen wird sichergestellt, dass keine unerwarteten Kosten anfallen.

1. Öffnen Sie im Azure-Portal Cloud Shell, indem Sie oben rechts im Azure-Portal auf das erste Symbol klicken. Wenn Sie dazu aufgefordert werden, klicken Sie auf **Verbindung wiederherstellen**.

1. Führen Sie im Cloud Shell-Bereich in der PowerShell-Sitzung den folgenden Code aus, um die Ressourcengruppe zu entfernen, die Sie in diesem Lab erstellt haben:
  
    ```powershell
    Remove-AzResourceGroup -Name "AZ500LAB02" -Force -AsJob
    ```

1.  Schließen Sie den **Cloud Shell**-Bereich. 
