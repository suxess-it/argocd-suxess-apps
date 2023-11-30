# suxess-apps

das repo wo wir deklarativ unsere apps über argocd deployen

# Konventionen

## Staging

Aktuell sind noch keine Staging-Konventionen implementiert.
Da wir hauptsächlich nur Plattform-Services betreiben, die es nur einmal im Cluster gibt, ist das aktuell auch noch nicht so dringend.
Will man eine Applikation in mehreren Stages deployen, muss man dafür getrennte Application-Defintions anlegen und sich um die Promotion selber kümmern.

## Application Definition

Aktuell wird für jede Applikation ein eigenes Application-Definition-YAML angelegt. ApplicationSets werden noch nicht verwendet weil wir noch keinen generischen Ansatz wissen, der für alle unsere Applikationen passt.
In weiterer Folge könnte es sein dass wir ApplicationSets einführen, sobald wir wissen wie so eine generische Application-Definition aussehen könnte.

### Helm-Charts



### Values-Files


### Wichtige Defaults

Um den gitops-Prinzipien treu zu bleiben sollte jede Applikation folgende Attribute gesetzt haben, außer es gibt gute Gründe dagegen:

```
metadata:
  finalizers:
  - resources-finalizer.argocd.argoproj.io
  [...]
spec:
  project: default
  [...]
  syncPolicy:
    automated:
      prune: true
      selfHeal: true
    syncOptions:
    - CreateNamespace=true
```

### Offene Fragen

1. Helm-Chart-Versionen direkt in AppDef angeben --> dann muss immer Parent-App gesynct werden wenn man neues Helm-Chart in App verwenden will

2. Values direkt in AppDef angeben --> dann muss immer Parent-App gesynct werden wenn man neues Helm-Chart in App verwenden will

3. Wenn man nur AppDef hat und kein eigenes gitops-Repo, dann gibts auch keine catalog-info.yaml und docs für die unterschiedlichen AppDefs

4. sx- Prefix bei Appnamen? notwendig oder nicht?

5. pro app ein eigenes gitops-Repo oder alles in https://github.com/suxess-it/charts ?


## Applikationen deaktivieren

Will man Applikationen temporär nicht deployen, können sie einfach mit dem Suffix ".yaml.disabled" umbenannt werden. Somit werden sie automatisch von ArgoCD ignoriert.




