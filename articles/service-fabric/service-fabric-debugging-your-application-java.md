---
title: Debuggen Ihrer Anwendung in Eclipse
description: Verbessern Sie die Zuverlässigkeit und Leistung Ihrer Dienste, indem Sie sie in Eclipse in einem lokalen Entwicklungscluster entwickeln und debuggen.
author: suhuruli
ms.topic: conceptual
ms.date: 11/02/2017
ms.author: suhuruli
ms.openlocfilehash: 6f2361bf76bd4f9d297fbe541b950840f13966cc
ms.sourcegitcommit: dabd9eb9925308d3c2404c3957e5c921408089da
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2020
ms.locfileid: "86246400"
---
# <a name="debug-your-java-service-fabric-application-using-eclipse"></a>Debuggen der Service Fabric-Anwendung in Java mithilfe von Eclipse
> [!div class="op_single_selector"]
> * [Visual Studio/CSharp](service-fabric-debugging-your-application.md) 
> * [Eclipse/Java](service-fabric-debugging-your-application-java.md)
> 

1. Um einen lokalen Cluster für die Entwicklung zu erstellen, folgen Sie den Schritten unter [Einrichten der Service Fabric-Entwicklungsumgebung](service-fabric-get-started-linux.md).

2. Aktualisieren Sie „entryPoint.sh“ für den Dienst, den Sie debuggen möchten, sodass sie den Java-Prozess mit Remotedebugparametern startet. Sie finden diese Datei am folgenden Speicherort: `ApplicationName\ServiceNamePkg\Code\entrypoint.sh`. In diesem Beispiel wurde für das Debuggen Port 8001 festgelegt.

    ```sh
    java -Xdebug -Xrunjdwp:transport=dt_socket,address=8001,server=y,suspend=n -Djava.library.path=$LD_LIBRARY_PATH -jar myapp.jar
    ```
3. Aktualisieren Sie das Anwendungsmanifest, indem die Instanz- oder Replikatanzahl für den zu debuggenden Dienst auf 1 festlegen. Diese Einstellung vermiedet Konflikte an dem Port, der für das Debuggen verwendet wird. Legen Sie für zustandslose Dienste z.B. `InstanceCount="1"` und für zustandsbehaftete Dienste die Ziel- und Mindestgröße für Replikatgruppen wie folgt auf 1 fest: `TargetReplicaSetSize="1" MinReplicaSetSize="1"`.

4. Stellen Sie die Anwendung bereit.

5. Wählen Sie in der Eclipse-IDE **Run -> Debug Configurations -> Remote Java Application and input connection properties** (Ausführen > Debugkonfiguration > Eigenschaften der Remote-Java-Anwendung und Eingangsverbindung) aus, und legen Sie die Eigenschaften wie folgt fest:

   ```
   Host: ipaddress
   Port: 8001
   ```
6.  Legen Sie an den gewünschten Stellen Haltepunkte fest, und debuggen Sie die Anwendung.

Wenn die Anwendung abstürzt, können Sie auch Coredumps aktivieren. Führen Sie `ulimit -c` in einer Shell aus. Wenn dabei 0 zurückgegeben wird, sind Coredumps nicht aktiviert. Um unbegrenzte Coredumps zu aktivieren, führen Sie den folgenden Befehl aus: `ulimit -c unlimited`. Sie können den Status auch mit dem Befehl `ulimit -a` überprüfen.  Wenn Sie den Pfad für die Coredump-Generierung ändern möchten, führen Sie `echo '/tmp/core_%e.%p' | sudo tee /proc/sys/kernel/core_pattern` aus. 

### <a name="next-steps"></a>Nächste Schritte

* [Sammeln von Protokollen mit Azure-Diagnose unter Linux](./service-fabric-diagnostics-event-aggregation-lad.md)
* [Lokales Überwachen und Diagnostizieren von Diensten](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally-linux.md)
