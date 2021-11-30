---
lab:
  title: 14 – Azure Security Center
  module: Module 04 - Manage Security Operations
ms.openlocfilehash: 33134a5d1f7f5138083c1889c04f3296c3f05586
ms.sourcegitcommit: 2eb153f2856445e5afaa218a012cb92e3d48f24b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/16/2021
ms.locfileid: "132625671"
---
# <a name="lab-14-azure-security-center"></a>Lab 14: Azure Security Center
# <a name="student-lab-manual"></a>Lab-Handbuch für Kursteilnehmer

## <a name="lab-scenario"></a>Labszenario

Sie wurden gebeten, einen Proof of Concept für eine Security Center-basierte Umgebung zu erstellen. Insbesondere geht es Ihnen um Folgendes:

- Konfigurieren von Security Center zum Überwachen eines virtuellen Computers.
- Überprüfen der Security Center-Empfehlungen für den virtuellen Computer.
- Implementieren der Empfehlungen für die Gastkonfiguration und den Just-in-Time-VM-Zugriff. 
- Überprüfen, wie die Sicherheitsbewertung verwendet werden kann, um den Fortschritt bei der Erstellung einer sichereren Infrastruktur zu bestimmen.

> Für alle Ressourcen in diesem Lab verwenden wir die Region **USA, Osten**. Vergewissern Sie sich bei Ihrem Kursleiter, dass dies die Region ist, die für den Kurs verwendet werden soll. 

## <a name="lab-objectives"></a>Ziele des Labs

In diesem Lab führen Sie die folgende Übung aus:

- Übung 1: Implementieren von Security Center

### <a name="exercise-1-implement-security-center"></a>Übung 1: Implementieren von Security Center

In dieser Übung führen Sie die folgenden Aufgaben aus:

- Aufgabe 1: Konfigurieren von Security Center
- Aufgabe 2: Überprüfen der Security Center-Empfehlungen
- Aufgabe 3: Implementieren der Security Center-Empfehlungen zum Aktivieren des Just-in-Time-VM-Zugriffs

#### <a name="task-1-configure-security-center"></a>Aufgabe 1: Konfigurieren von Security Center

In dieser Aufgabe integriert und konfigurieren Sie Security Center.

1. Melden Sie sich beim Azure-Portal ( **`https://portal.azure.com/`** ) an.

    >**Hinweis**: Melden Sie sich beim Azure-Portal mit einem Konto an, das über die Rolle „Besitzer“ oder „Mitwirkender“ in dem Azure-Abonnement verfügt, das Sie für dieses Lab verwenden.

1. Geben Sie im Azure-Portal oben auf der Azure-Portalseite im Textfeld **Nach Ressourcen, Diensten und Dokumenten suchen** den Begriff **Security Center** ein, und drücken Sie die **EINGABETASTE**.

1. Klicken Sie auf dem Blatt **Security Center \| Erste Schritte** auf **Upgrade** und dann auf **Agents installieren**.
     
1. Klicken Sie auf dem Blatt **Security Center \| Erste Schritte** im vertikalen Menü auf der linken Seite im Abschnitt **Verwaltung** auf **Preise und Einstellungen**.

1. Klicken Sie auf dem Blatt **Security Center \| Preise und Einstellungen** auf den Eintrag, der Ihr Abonnement darstellt, und stellen Sie sicher, dass auf dem Blatt **Einstellungen \| Azure Defender-Pläne** die Option **Azure Defender** aktiviert ist. 

    >**Hinweis**: Überprüfen Sie alle Features, die als Teil der Azure Defender-Ebene verfügbar sind, und stellen Sie sicher, dass Azure Defender für jeden Ressourcentyp aktiviert ist. 

1. Wenn Sie Änderungen vorgenommen haben, klicken Sie auf **Speichern**.

1. Wählen Sie auf dem Blatt **Einstellungen \| Azure Defender-Pläne** die Option **Alle aktivieren** aus, und klicken Sie auf **Speichern**.

1. Klicken Sie auf dem Blatt **Einstellungen \| Azure Defender-Pläne** auf der vertikalen Menüleiste auf der linken Seite auf **Automatische Bereitstellung**.

1. Stellen Sie auf dem Blatt **Einstellungen \| Automatische Bereitstellung** sicher, dass die **automatische Bereitstellung** für das erste Element **Log Analytics-Agent für Azure-VMs** auf **Ein** festgelegt ist. 

1. Klicken Sie auf dem Blatt **Einstellungen \| Automatische Bereitstellung** auf der vertikalen Menüleiste auf der linken Seite auf **Workflowautomatisierung**.

1. Klicken Sie auf dem Blatt **Einstellungen \| Workflowautomatisierung** auf **+ Workflowautomatisierung hinzufügen**.

1. Überprüfen Sie auf dem Blatt **Workflowautomatisierung hinzufügen** die verfügbaren Einstellungen. 

    >**Hinweis**: Sie können auf Aktionen basierende Warnungen zur Bedrohungserkennung und Security Center-Empfehlungen auslösen. Sie können auch eine Aktion basierend auf Logic Apps konfigurieren. 

1. Klicken Sie auf dem Blatt **Workflowautomatisierung hinzufügen** auf **Abbrechen**.

    >**Hinweis**: Security Center bietet viele Einblicke in virtuelle Computer, einschließlich Systemupdatestatus, Betriebssystem-Sicherheitskonfigurationen und Endpunktschutz.

1. Navigieren Sie zurück zum Blatt **Security Center \| Preise und Einstellungen**, und klicken Sie auf den Eintrag, der den Log Analytics-Arbeitsbereich darstellt, den Sie im vorherigen Lab erstellt haben.

1. Stellen Sie auf dem Blatt **Einstellungen \| Azure Defender-Pläne** sicher, dass **Azure Defender** aktiviert ist, und klicken Sie auf **Speichern**.


#### <a name="task-2-review-the-security-center-recommendation"></a>Aufgabe 2: Überprüfen der Security Center-Empfehlung

In dieser Aufgabe überprüfen Sie die Security Center-Empfehlungen. 

1. Navigieren Sie im Azure-Portal zurück zum Blatt **Security Center \| Übersicht**. 

1. Überprüfen Sie auf dem Blatt **Security Center \| Übersicht** die Kachel **Sicherheitsbewertung**.

    >**Hinweis**: Notieren Sie sich die aktuelle Bewertung, sofern verfügbar.

1. Navigieren Sie zurück zum Blatt **Security Center \| Übersicht**, und wählen Sie **Bewertete Ressourcen** aus.

1. Wählen Sie auf dem Blatt **Bestand** den Eintrag **myVM** aus.

    >**Hinweis**: Möglicherweise müssen Sie einige Minuten warten und die Browserseite aktualisieren, bis der Eintrag angezeigt wird.
    
1. Überprüfen Sie auf dem Blatt **Ressourcenzustand** auf der Registerkarte **Empfehlungen** die Liste der Empfehlungen für **myVM**.


#### <a name="task-3-implement-the-security-center-recommendation-to-enable-just-in-time-vm-access"></a>Aufgabe 3: Implementieren der Security Center-Empfehlungen zum Aktivieren des Just-in-Time-VM-Zugriffs

In dieser Aufgabe implementieren Sie die Security Center-Empfehlungen zum Aktivieren des Just-in-Time-VM-Zugriffs auf dem virtuellen Computer. 

1. Navigieren Sie im Azure-Portal zurück zum Blatt **Security Center \| Übersicht**, und wählen Sie die Kachel **Azure Defender** aus.

1. Klicken Sie auf dem **Azure Defender** im Abschnitt **Erweiterter Schutz** auf die Kachel **JIT-VM-Zugriff**, und klicken Sie auf dem Blatt **JIT-VM-Zugriff** auf **JIT-VM-Zugriff testen**.

1. Wählen Sie unter **JIT-VM-Zugriff** die Option **Nicht konfiguriert** aus, und klicken Sie dann auf den Eintrag **myVM**.

    >**Hinweis**: Möglicherweise müssen Sie einige Minuten warten, bis der Eintrag **myVM** verfügbar wird.

1. Wählen Sie **JIT auf 1 VM aktivieren** aus.

1. Klicken Sie auf dem Blatt **Konfiguration des JIT-VM-Zugriffs** ganz rechts in der Zeile, die auf Port **22** verweist, auf die Schaltfläche mit den Auslassungspunkten und dann auf **Löschen**.

1. Klicken Sie auf dem Blatt **Konfiguration des JIT-VM-Zugriffs** auf **Speichern**.

    >**Hinweis**: Überwachen Sie den Status der Konfiguration, indem Sie auf der Symbolleiste auf das Symbol **Benachrichtigungen** klicken, um das Blatt **Benachrichtigungen** anzuzeigen. 

    >**Hinweis**: Es kann einige Zeit dauern, bis die Implementierung von Empfehlungen in diesem Lab durch die Sicherheitsbewertung widergespiegelt wird. Überprüfen Sie regelmäßig die Sicherheitsbewertung, um die Auswirkungen der Implementierung dieser Features zu ermitteln. 

> Ergebnisse: Sie haben Security Center integriert und Empfehlungen für virtuelle Computer implementiert. 


>**Hinweis**: Entfernen Sie nicht die Ressourcen aus diesem Lab, da sie für das Azure Sentinel-Lab benötigt werden.
