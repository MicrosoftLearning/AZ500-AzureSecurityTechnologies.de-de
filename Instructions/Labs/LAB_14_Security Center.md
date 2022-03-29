---
lab:
  title: 14 – Microsoft Defender für Cloud
  module: Module 04 - Microsoft Defender for Cloud
ms.openlocfilehash: 6ec274b75692321577c8966e07349211209eaa02
ms.sourcegitcommit: a8470295248a6363987bd5ea47154fe39f8535c3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/09/2022
ms.locfileid: "139703587"
---
# <a name="lab-14-microsoft-defender-for-cloud"></a>Lab 14: Microsoft Defender für Cloud
# <a name="student-lab-manual"></a>Lab-Handbuch für Kursteilnehmer

## <a name="lab-scenario"></a>Labszenario

Sie wurden gebeten, einen Proof of Concept der Microsoft Defender für Cloud-basierten Umgebung zu erstellen. Insbesondere geht es Ihnen um Folgendes:

- Sie sollen Microsoft Defender für Cloud für die Überwachung eines virtuellen Computers konfigurieren.
- Sehen Sie sich die Empfehlungen von Microsoft Defender für Cloud für den virtuellen Computer an.
- Implementieren der Empfehlungen für die Gastkonfiguration und den Just-in-Time-VM-Zugriff. 
- Überprüfen, wie die Sicherheitsbewertung verwendet werden kann, um den Fortschritt bei der Erstellung einer sichereren Infrastruktur zu bestimmen.

> Für alle Ressourcen in diesem Lab verwenden wir die Region **USA, Osten**. Vergewissern Sie sich bei Ihrem Kursleiter, dass dies die Region ist, die für den Kurs verwendet werden soll. 

## <a name="lab-objectives"></a>Labziele

In diesem Lab führen Sie die folgende Übung aus:

- Übung 1: Implementieren von Microsoft Defender für Cloud

## <a name="microsoft-defender-for-cloud-diagram"></a>Microsoft Defender für Cloud-Diagramm

![image](https://user-images.githubusercontent.com/91347931/157537800-94a64b6e-026c-41b2-970e-f8554ce1e0ab.png)

## <a name="instructions"></a>Anweisungen

### <a name="exercise-1-implement-microsoft-defender-for-cloud"></a>Übung 1: Implementieren von Microsoft Defender für Cloud

In dieser Übung führen Sie die folgenden Aufgaben aus:

- Aufgabe 1: Konfigurieren von Microsoft Defender für Cloud
- Aufgabe 2: Überprüfen der Empfehlungen von Microsoft Defender für Cloud
- Aufgabe 3: Implementieren der Empfehlung von Microsoft Defender für Cloud zum Aktivieren des Just-in-Time-VM-Zugriffs

#### <a name="task-1-configure-microsoft-defender-for-cloud"></a>Aufgabe 1: Konfigurieren von Microsoft Defender für Cloud

In dieser Aufgabe werden Sie Microsoft Defender für Cloud einführen und konfigurieren.

1. Melden Sie sich beim Azure-Portal ( **`https://portal.azure.com/`** ) an.

    >**Hinweis**: Melden Sie sich beim Azure-Portal mit einem Konto an, dem in dem Azure-Abonnement, das Sie für dieses Lab verwenden, die Rolle „Besitzer“ oder „Mitwirkender“ zugewiesen ist.

2. Geben Sie im Azure-Portal oben im Textfeld **Ressourcen, Dienste und Dokumente durchsuchen** **Microsoft Defender für Cloud** ein, und drücken Sie die **EINGABETASTE**.

3. Klicken Sie auf dem Blatt **Microsoft Defender für Cloud \| Erste Schritte** auf **Upgrade**, wenn dieses noch nicht abgeschlossen wurde.
     
4. Scrollen Sie auf dem Blatt **Microsoft Defender für Cloud \| Erste Schritte** zur Registerkarte **Agents installieren** herunter, und klicken Sie auf **Agents installieren**, wenn dieser Vorgang noch nicht abgeschlossen wurde.

5. Aktivieren Sie auf dem Blatt **Microsoft Defender für Cloud \| Erste Schritte** auf der Registerkarte **Upgrade** >> im Abschnitt **Select workspaces with enhanced security features** (Arbeitsbereiche mit erweiterten Sicherheitsfeatures auswählen) >> **Microsoft Defender plan** (Microsoft Defender-Tarif), indem Sie Ihren Log Analytics-Arbeitsbereich auswählen. 

    >**Hinweis**: Überprüfen Sie alle Features, die als Teil von Microsoft Defender-Tarifen verfügbar sind. 

6. Wählen Sie auf dem Blatt **Defender plans** die Option **Enable all Microsoft Defender for Cloud Plans** (Alle Microsoft Defender für Cloud-Tarife aktivieren) aus, und klicken Sie auf **Speichern**.

7. Navigieren Sie zu **Microsoft Defender für Cloud**, und klicken Sie unter den Verwaltungseinstellungen in der vertikalen Menüleiste auf der linken Seite auf **Umgebungseinstellungen**.

8. Klicken Sie auf dem Blatt **Microsoft Defender für Cloud | Umgebungseinstellungen** auf das entsprechende Abonnement. 

9. Klicken Sie auf dem Blatt **Einstellungen | Defender plans (Defender-Tarife)** auf der vertikalen Menüleiste auf der linken Seite auf **Automatische Bereitstellung**.

10. Stellen Sie auf dem Blatt **Einstellungen | Automatische Bereitstellung** sicher, dass die automatische Bereitstellung für das erste Element **Log Analytics agent for Azure VMs** (Log Analytics-Agent für Azure-VMs) auf **Ein** festgelegt ist.

11. Klicken Sie auf dem Blatt **Einstellungen \| Workflowautomatisierung** auf **+ Workflowautomatisierung hinzufügen**.

12. Überprüfen Sie auf dem Blatt **Workflowautomatisierung hinzufügen** die verfügbaren Einstellungen. 

    >**Hinweis**: Sie können Aktionen auf der Grundlage von Warnungen zur Bedrohungserkennung und Empfehlungen von Microsoft Defender für Cloud auslösen. Sie können auch eine Aktion basierend auf Logic Apps konfigurieren. 

13. Klicken Sie auf dem Blatt **Workflowautomatisierung hinzufügen** auf **Abbrechen**.

    >**Hinweis**: Microsoft Defender für Cloud liefert viele Einsichten in virtuelle Computer, darunter den Status von Systemupdates, die Konfiguration der Betriebssystemsicherheit und den Endpunktschutz.

14. Navigieren Sie zurück zu dem Blatt **Microsoft Defender für Cloud \| Umgebungseinstellungen**, erweitern Sie Ihr Abonnement, und klicken Sie auf den Eintrag, der den Log Analytics-Arbeitsbereich darstellt, den Sie im vorherigen Lab erstellt haben.

15. Stellen Sie sicher, dass die Option **Enable all Microsoft Defender for Cloud plans** (Alle Microsoft Defender für Cloud-Tarife aktivieren) auf dem Blatt **Einstellungen \| Defender plans (Defender-Tarife)** ausgewählt ist, und klicken Sie auf **Speichern**.

16. Wählen Sie auf dem Blatt **Microsoft Defender für Cloud**  Einstellungen **die Option \|Datensammlung** aus. Wählen Sie **Alle Ereignisse** und **Speichern** aus.


#### <a name="task-2-review-the-microsoft-defender-for-cloud-recommendation"></a>Aufgabe 2: Überprüfen der Empfehlung von Microsoft Defender für Cloud

In dieser Aufgabe überprüfen Sie die Empfehlungen von Microsoft Defender für Cloud. 

1. Navigieren Sie im Azure-Portal zurück zum Blatt **Microsoft Defender für Cloud \| Übersicht**. 

2. Überprüfen Sie auf dem Blatt **Microsoft Defender für Cloud \| Übersicht** die Kachel **Sicherheitsbewertung**.

    >**Hinweis**: Notieren Sie sich die aktuelle Bewertung, sofern verfügbar.

3. Navigieren Sie zurück zum Blatt **Microsoft Defender für Cloud \| Übersicht**, und wählen Sie **Assessed resources** (Bewertete Ressourcen) aus.

4. Wählen Sie auf dem Blatt **Bestand** den Eintrag **myVM** aus.

    >**Hinweis**: Möglicherweise müssen Sie einige Minuten warten und die Browserseite aktualisieren, bis der Eintrag angezeigt wird.
    
5. Überprüfen Sie auf dem Blatt **Ressourcenzustand** auf der Registerkarte **Empfehlungen** die Liste der Empfehlungen für **myVM**.


#### <a name="task-3-implement-the-microsoft-defender-for-cloud-recommendation-to-enable-just-in-time-vm-access"></a>Aufgabe 3: Implementieren der Empfehlung von Microsoft Defender für Cloud zum Aktivieren des Just-in-Time-VM-Zugriffs

In dieser Aufgabe implementieren Sie die Empfehlung von Microsoft Defender für Cloud, um den Just-in-Time-VM-Zugriff zu aktivieren. 

1. Navigieren Sie im Azure-Portal zurück zum Blatt **Microsoft Defender für Cloud \| Übersicht**, und wählen Sie unter der Kachel **Cloud Security** (Cloudsicherheit) die Option **Workload protections** (Workloadschutz) aus.

2. Klicken Sie auf dem Blatt **Workload protections** im Bereich **Erweiterter Schutz** auf die Kachel **Just-in-time VM access** (Just-In-Time-VM-Zugriff), und klicken Sie auf dem Blatt **Just-in-time VM access** auf **Try Just in time VM access** (Just-in-Time-VM-Zugriff versuchen).

    >**Hinweis**: Wenn die VMs nicht aufgeführt sind, navigieren Sie zum Blatt **Virtueller Computer**, und klicken Sie auf **Konfiguration**. Klicken Sie auf die Option **Enable the Just-in-time VMs** (Just-In-Time-VMs aktivieren) unter **Just-in-time Vm's access** (Zugriff auf Just-In-Time-VMs). Wiederholen Sie den obigen Schritt, um zurück zu **Microsoft Defender für Cloud** zu navigieren. Aktualisieren Sie die Seite, um die VM anzuzeigen.

3. Wählen Sie unter **JIT-VM-Zugriff** die Option **Nicht konfiguriert** aus, und klicken Sie dann auf den Eintrag **myVM**.

    >**Hinweis**: Möglicherweise müssen Sie einige Minuten warten, bis der Eintrag **myVM** verfügbar wird.

4. Wählen Sie **JIT auf 1 VM aktivieren** aus.

5. Klicken Sie auf dem Blatt **Konfiguration des JIT-VM-Zugriffs** ganz rechts in der Zeile, die auf Port **22** verweist, auf die Schaltfläche mit den Auslassungspunkten und dann auf **Löschen**.

6. Klicken Sie auf dem Blatt **Konfiguration des JIT-VM-Zugriffs** auf **Speichern**.

    >**Hinweis**: Überwachen Sie den Status der Konfiguration, indem Sie auf der Symbolleiste auf das Symbol **Benachrichtigungen** klicken, um das Blatt **Benachrichtigungen** anzuzeigen. 

    >**Hinweis**: Es kann einige Zeit dauern, bis die Implementierung von Empfehlungen in diesem Lab durch die Sicherheitsbewertung widergespiegelt wird. Überprüfen Sie regelmäßig die Sicherheitsbewertung, um die Auswirkungen der Implementierung dieser Features zu ermitteln. 

> Ergebnisse: Sie haben Microsoft Defender für Cloud eingeführt und Empfehlungen für virtuelle Computer implementiert. 

    >**Note**: Do not remove the resources from this lab as they are needed for the Azure Sentinel lab.
