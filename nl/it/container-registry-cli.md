---

copyright:
  years: 2017, 2019
lastupdated: "2019-01-24"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:download: .download}

# CLI {{site.data.keyword.registrylong_notm}}
{: #containerregcli}

Puoi utilizzare la CLI {{site.data.keyword.registrylong}}, che viene fornita nel plug-in della CLI `container-registry`, per gestire il tuo registro e le sue risorse per il tuo account {{site.data.keyword.Bluemix_notm}}.
{: shortdesc}

**Prerequisiti**

* Installa la CLI [{{site.data.keyword.Bluemix_notm}} ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](/docs/cli/index.html#overview). Il prefisso per eseguire i comandi utilizzando la CLI {{site.data.keyword.Bluemix_notm}} è `ibmcloud`.

* Prima di eseguire i comandi di registro, esegui l'accesso a {{site.data.keyword.Bluemix_notm}} con il comando `ibmcloud login` per generare un token di accesso e autenticare la tua sessione.

Nella riga di comando, vieni informato quando sono disponibili degli aggiornamenti ai plug-in delle CLI `ibmcloud` e `container-registry`. Assicurati di tenere la tua CLI aggiornata in modo da poter utilizzare tutti i comandi e gli indicatori disponibili.

Se vuoi visualizzare la versione corrente del tuo plug-in della CLI `container-registry`, esegui `ibmcloud plugin list`.

Per informazioni su come utilizzare la CLI {{site.data.keyword.registrylong_notm}}, vedi [Introduzione a {{site.data.keyword.registrylong_notm}}](/docs/services/Registry/index.html#index).

Per ulteriori informazioni sulla piattaforma IAM e sui ruoli di accesso al servizio necessari per alcuni comandi, vedi [Gestione dell'accesso utente con Identity and Access Management](/docs/services/Registry/iam.html#iam).

Non inserire informazioni personali nelle immagini del contenitore, nei nomi degli spazi dei nomi, nei campi di descrizione (ad esempio, nei token di registro) o in qualsiasi dato di configurazione dell'immagine (ad esempio, nomi o etichette dell'immagine).
{:tip}

## `ibmcloud cr api`
{: #bx_cr_api}

Restituisce i dettagli sull'endpoint dell'API del registro su cui vengono eseguiti i comandi.

```
ibmcloud cr api
```
{: codeblock}

**Prerequisiti**

Nessuno

## `ibmcloud cr build`
{: #bx_cr_build}

Crea un'immagine Docker in {{site.data.keyword.registrylong_notm}}.

```
ibmcloud cr build [--no-cache] [--pull] [--quiet | -q] [--build-arg KEY=VALUE ...] [--file FILE | -f FILE] --tag TAG DIRECTORY
```
{: codeblock}

**Prerequisiti**

Per ulteriori informazioni sulle autorizzazioni necessarie, vedi [Ruoli di accesso per l'utilizzo di {{site.data.keyword.registrylong_notm}}](/docs/services/Registry/iam.html#access_roles_using)..

**Opzioni di comando**
<dl>
<dt>`DIRECTORY`</dt>
<dd>L'ubicazione del tuo contesto di compilazione, che contiene i tuoi file Dockerfile e prerequisiti. Se esegui il comando quando la tua directory di lavoro è impostata sulla posizione in cui è memorizzato il contesto di compilazione, puoi sostituire `DIRECTORY` con un punto (.)..</dd>
<dt>`--no-cache`</dt>
<dd>(Facoltativo)  Se specificato, i livelli di immagine memorizzati in cache dalle compilazioni precedenti non vengono utilizzati in questa compilazione.</dd>
<dt>`--pull`</dt>
<dd>(Facoltativo) Se specificato, le immagini di base vengono estratte anche se sull'host di compilazione esiste già un'immagine con una tag corrispondente.</dd>
<dt>`--quiet`, `-q`</dt>
<dd>(Facoltativo) Se specificato, l'output di compilazione viene eliminato a meno che non si verifichi un errore.</dd>
<dt>`--build-arg KEY=VALUE`</dt>
<dd>(Facoltativo) Specifica un argomento di compilazione aggiuntivo nel formato `'KEY=VALUE'`. È possibile specificare più argomenti di compilazione includendo questo parametro più volte. Il valore di ogni argomento di compilazione è disponibile come una variabile di ambiente quando specifichi una riga ARG che corrisponde alla chiave nel tuo Dockerfile.</dd>
<dt>`--file FILE`, `-f FILE`</dt>
<dd>(Facoltativo) Se utilizzi gli stessi file per più compilazioni, puoi scegliere un percorso a un Dockerfile diverso. Specifica la posizione del Dockerfile in relazione al contesto di compilazione. Se non specificato, il valore predefinito è `PATH/Dockerfile`, dove PATH è la root del contesto di compilazione.</dd>
<dt>`--tag TAG`, `-t TAG`</dt>
<dd>Il nome completo dell'immagine che desideri creare, che include lo spazio dei nomi e l'URL del registro.</dd>
</dl>

**Esempio**

Crea un'immagine Docker che non utilizza una cache di compilazione da compilazioni precedenti, dove l'output di compilazione è soppresso, la tag è *`registry.ng.bluemix.net/birds/bluebird:1`* e la directory è la tua directory di lavoro.

```
ibmcloud cr build --no-cache --quiet --tag registry.ng.bluemix.net/birds/bluebird:1 .
```
{: pre}

## `ibmcloud cr exemption-add`
{: #bx_cr_exemption_add}

Creare un'esenzione per un problema di sicurezza. Puoi creare un'esenzione per un problema di sicurezza che si applica ad ambiti differenti. L'ambito può essere l'account, lo spazio dei nomi, il repository o la tag.

```
ibmcloud cr exemption-add --scope SCOPE --issue-type ISSUE_TYPE --issue-id ISSUE_ID
```
{: codeblock}

**Prerequisiti**

Per ulteriori informazioni sulle autorizzazioni necessarie, vedi [Ruoli di accesso per la configurazione di {{site.data.keyword.registrylong_notm}}](/docs/services/Registry/iam.html#access_roles_configure)..

**Opzioni di comando**
<dl>
<dt>`--scope SCOPE`</dt>
<dd>Per impostare il tuo account come ambito, utilizza `"*"` come valore. Per impostare uno spazio dei nomi, un repository o una tag come ambito, immetti il valore in uno dei seguenti formati: `namespace`, `namespace/repository`, `namespace/repository:tag`
</dd>
<dt>`--issue-type ISSUE_TYPE`</dt>
<dd>Il tipo di problema di sicurezza che desideri esentare. Per trovare i tipi di problema validi, esegui `ibmcloud cr exemption-types`.
</dd>
<dt>`--issue-id ISSUE_ID`</dt>
<dd>L'ID del problema di sicurezza che desideri esentare. Per trovare un ID problema, esegui `ibmcloud cr va<image>`, dove *&lt;image&gt;* è il nome della tua immagine, e usa il valore pertinente dalla colonna **Vulnerability ID** o **Configuration Issue ID**.
</dd>
</dl>

**Esempi**

Crea un'esenzione CVE per CVE con ID `CVE-2018-17929` per tutte le immagini nel repository `registry.ng.bluemix.net/birds/bluebird`.

```
ibmcloud cr exemption-add --scope registry.ng.bluemix.net/birds/bluebird --issue-type cve --issue-id CVE-2018-17929
```
{: pre}

Crea un'esenzione CVE al livello dell'account per CVE con ID `CVE-2018-17929`.

```
ibmcloud cr exemption-add --scope "*" --issue-type cve --issue-id CVE-2018-17929
```
{: pre}

Crea un'esenzione del problema di configurazione per il problema `application_configuration:nginx.ssl_protocols` per una singola immagine con la tag `registry.ng.bluemix.net/birds/bluebird:1`.

```
ibmcloud cr exemption-add --scope registry.ng.bluemix.net/birds/bluebird:1 --issue-type configuration --issue-id application_configuration:nginx.ssl_protocols
```
{: pre}

## `ibmcloud cr exemption-list` (`ibmcloud cr exemptions`)
{: #bx_cr_exemption_list}

Elenca le tue esenzioni per i problemi di sicurezza.

```
ibmcloud cr exemption-list [--scope SCOPE]
```
{: codeblock}

**Prerequisiti**

Per ulteriori informazioni sulle autorizzazioni necessarie, vedi [Ruoli di accesso per la configurazione di {{site.data.keyword.registrylong_notm}}](/docs/services/Registry/iam.html#access_roles_configure)..

**Opzioni di comando**
<dl>
<dt>`--scope SCOPE`</dt>
<dd>(Facoltativo) Elenca solo le esenzioni che si applicano a questo ambito. Per impostare uno spazio dei nomi, un repository o una tag come ambito, immetti il valore in uno dei seguenti formati: `namespace`, `namespace/repository`, `namespace/repository:tag`
</dd>
</dl>

**Esempio**

Elenca tutte le tue esenzioni per i problemi di sicurezza che si applicano alle immagini nel repository *`birds/bluebird`*. L'output include le esenzioni al livello dell'account, le esenzioni che hanno come ambito lo spazio dei nomi *`birds`* e le esenzioni che hanno come ambito il repository *`birds/bluebird`* ma non quelle che hanno come ambito specifiche tag all'interno del repository *`birds/bluebird`*.

```
ibmcloud cr exemption-list --scope birds/bluebird
```
{: pre}

## `ibmcloud cr exemption-rm`
{: #bx_cr_exemption_rm}

Elimina un'esenzione per un problema di sicurezza. Per visualizzare le tue esenzioni esistenti, esegui `ibmcloud cr exemption-list`.

```
ibmcloud cr exemption-rm --scope SCOPE --issue-type ISSUE_TYPE --issue-id ISSUE_ID
```
{: codeblock}

**Prerequisiti**

Per ulteriori informazioni sulle autorizzazioni necessarie, vedi [Ruoli di accesso per la configurazione di {{site.data.keyword.registrylong_notm}}](/docs/services/Registry/iam.html#access_roles_configure)..

**Opzioni di comando**
<dl>
<dt>`--scope SCOPE`</dt>
<dd>Per impostare il tuo account come ambito, utilizza `"*"` come valore. Per impostare uno spazio dei nomi, un repository o una tag come ambito, immetti il valore in uno dei seguenti formati: `namespace`, `namespace/repository`, `namespace/repository:tag`
</dd>
<dt>`--issue-type ISSUE_TYPE`</dt>
<dd>Il tipo di problema dell'esenzione del problema di sicurezza che desideri rimuovere. Per trovare i tipi di problema per le tue esenzioni, esegui `ibmcloud cr exemption-list`.
</dd>
<dt>`--issue-id ISSUE_ID`</dt>
<dd>L'ID dell'esenzione per il problema di sicurezza che desideri rimuovere. Per trovare gli ID problema per le tue esenzioni, esegui `ibmcloud cr exemption-list`.
</dd>
</dl>

**Esempi**

Elimina un'esenzione CVE per CVE con ID `CVE-2018-17929` per tutte le immagini nel repository `registry.ng.bluemix.net/birds/bluebird`.

```
ibmcloud cr exemption-rm --scope registry.ng.bluemix.net/birds/bluebird --issue-type cve --issue-id CVE-2018-17929
```
{: pre}

Elimina un'esenzione CVE al livello dell'account per CVE con ID `CVE-2018-17929`.

```
ibmcloud cr exemption-rm --scope "*" --issue-type cve --issue-id CVE-2018-17929
```
{: pre}

Elimina un'esenzione del problema di configurazione per il problema `application_configuration:nginx.ssl_protocols` per una singola immagine con la tag `registry.ng.bluemix.net/birds/bluebird:1`.

```
ibmcloud cr exemption-rm --scope registry.ng.bluemix.net/birds/bluebird:1 --issue-type configuration --issue-id application_configuration:nginx.ssl_protocols
```
{: pre}

## `ibmcloud cr exemption-types`
{: #bx_cr_exemption_types}

Elenca i tipi di problemi di sicurezza che puoi esentare.

```
ibmcloud cr exemption-types
```
{: codeblock}

**Prerequisiti**

Per ulteriori informazioni sulle autorizzazioni necessarie, vedi [Ruoli di accesso per la configurazione di {{site.data.keyword.registrylong_notm}}](/docs/services/Registry/iam.html#access_roles_configure)..

## `ibmcloud cr iam-policies-enable`
{: #bx_cr_iam_policies_enable}

Se stai utilizzando l'autenticazione IAM, questo comando abilita l'autorizzazione granulare. Per ulteriori informazioni, vedi [Gestione dell'accesso utente con Identity and Access Management](/docs/services/Registry/iam.html#iam) e [Definizione delle politiche del ruolo di accesso utente](/docs/services/Registry/registry_users.html#user).

```
ibmcloud cr iam-policies-enable
```
{: codeblock}

**Prerequisiti**

Per ulteriori informazioni sulle autorizzazioni necessarie, vedi [Ruoli di accesso per la configurazione di {{site.data.keyword.registrylong_notm}}](/docs/services/Registry/iam.html#access_roles_configure)..

## `ibmcloud cr image-inspect`
{: #bx_cr_image_inspect}

Visualizza i dettagli su una specifica immagine.

```
ibmcloud cr image-inspect [--format FORMAT] IMAGE [IMAGE...]
```
{: codeblock}

**Prerequisiti**

Per ulteriori informazioni sulle autorizzazioni necessarie, vedi [Ruoli di accesso per l'utilizzo di {{site.data.keyword.registrylong_notm}}](/docs/services/Registry/iam.html#access_roles_using)..

**Opzioni di comando**
<dl>
<dt>`--format FORMAT`</dt>
<dd>(Facoltativo) Formatta gli elementi di output utilizzando un modello Go.

Per ulteriori informazioni, vedi [Formattazione e filtro dell'output della CLI per i comandi {{site.data.keyword.registrylong_notm}}](/docs/services/Registry/registry_cli_reference.html#registry_cli_listing).

</dd>
<dt>`IMAGE`</dt>
<dd>Il nome dell'immagine per cui desideri ottenere un report. Puoi analizzare più immagini elencando ogni immagine nel comando con uno spazio tra ciascun nome.

<p>Per trovare i nomi delle tue immagini, esegui `ibmcloud cr image-list`. Combina il contenuto delle colonne **Repository** e **Tag** per creare il nome dell'immagine nel formato `repository:tag`. Se nel nome immagine non è specificata una tag, viene analizzata l'immagine contrassegnata con `latest`.</p>

</dd>
</dl>

**Esempio**

Visualizza i dettagli sulle porte esposte per l'immagine *`registry.ng.bluemix.net/birds/bluebird:1`* utilizzando la direttiva di formattazione *`"{{ .Config.ExposedPorts }}"`*.

```
ibmcloud cr image-inspect  --format "{{ .Config.ExposedPorts }}" registry.ng.bluemix.net/birds/bluebird:1
```
{: pre}

## `ibmcloud cr image-list` (`ibmcloud cr images`)
{: #bx_cr_image_list}

Visualizza tutte le immagini nel tuo account {{site.data.keyword.Bluemix_notm}}.

Il nome dell'immagine è la combinazione del contenuto delle colonne **Repository** e **Tag** nel formato: `repository:tag`
{:tip}

```
ibmcloud cr image-list [--no-trunc] [--format FORMAT] [-q, --quiet] [--restrict RESTRICTION] [--include-ibm]
```
{: codeblock}

**Prerequisiti**

Per ulteriori informazioni sulle autorizzazioni necessarie, vedi [Ruoli di accesso per l'utilizzo di {{site.data.keyword.registrylong_notm}}](/docs/services/Registry/iam.html#access_roles_using)..

**Opzioni di comando**
<dl>
<dt>`--no-trunc`</dt>
<dd>(Facoltativo) Non troncare i digest immagine.</dd>
<dt>`--format FORMAT`</dt>
<dd>(Facoltativo) Formatta gli elementi di output utilizzando un modello Go.

Per ulteriori informazioni, vedi [Formattazione e filtro dell'output della CLI per i comandi {{site.data.keyword.registrylong_notm}}](/docs/services/Registry/registry_cli_reference.html#registry_cli_listing).

</dd>
<dt>`-q`, `--quiet`</dt>
<dd>(Facoltativo) Ogni immagine viene elencata nel formato: `repository:tag`</dd>
<dt>`--restrict RESTRICTION`</dt>
<dd>(Facoltativo) Limita l'output per visualizzare solo le immagini nello spazio dei nomi o nello spazio dei nomi e nel repository specificati.</dd>
<dt>`--include-ibm`</dt>
<dd>(Facoltativo) Include le immagini pubbliche fornite da {{site.data.keyword.IBM_notm}} nell'output. Senza questa opzione, per impostazione predefinita vengono elencate solo le immagini private.</dd>
</dl>

**Esempio**

Visualizza le immagini nello spazio dei nomi *`birds`* nel formato `repository:tag`, senza troncare i digest immagine.

```
ibmcloud cr image-list --restrict birds --quiet --no-trunc
```
{: pre}

## `ibmcloud cr image-rm`
{: #bx_cr_image_rm}

Elimina una o più immagini specificate da {{site.data.keyword.registrylong_notm}}.

```
ibmcloud cr image-rm IMAGE [IMAGE...]
```
{: codeblock}

**Prerequisiti**

Per ulteriori informazioni sulle autorizzazioni necessarie, vedi [Ruoli di accesso per l'utilizzo di {{site.data.keyword.registrylong_notm}}](/docs/services/Registry/iam.html#access_roles_using)..

**Opzioni di comando**

<dl>
<dt>`IMAGE`</dt>
<dd>Il nome dell'immagine che desideri eliminare. Puoi eliminare più immagini contemporaneamente elencando ogni immagine nel comando con uno spazio tra ciascun nome. `IMMAGINE` deve essere nel formato `repository:tag`, ad esempio: `registry.ng.bluemix.net/namespace/image:latest`

<p>Per trovare i nomi delle tue immagini, esegui `ibmcloud cr image-list`. Combina il contenuto delle colonne **Repository** e **Tag** per creare il nome dell'immagine nel formato `repository:tag`. Se nel nome immagine non è specificata una tag, viene eliminata per impostazione predefinita l'immagine contrassegnata con `latest`.</p>

</dd>
</dl>

**Esempio**
Elimina l'immagine *`registry.ng.bluemix.net/birds/bluebird:1`*.

```
ibmcloud cr image-rm registry.ng.bluemix.net/birds/bluebird:1
```
{: pre}

## `ibmcloud cr image-tag`
{: #bx_cr_image_tag}

Crea una nuova immagine, TARGET_IMAGE, che fa riferimento a un'immagine di origine, SOURCE_IMAGE, in {{site.data.keyword.registrylong_notm}}. Le immagini di origine e di destinazione devono essere nella stessa regione.

Per trovare i nomi delle tue immagini, esegui `ibmcloud cr image-list`. Combina il contenuto delle colonne **Repository** e **Tag** per creare il nome dell'immagine nel formato `repository:tag`.
{: tip}

```
ibmcloud cr image-tag [SOURCE_IMAGE] [TARGET_IMAGE]
```
{: codeblock}

**Prerequisiti**

Per ulteriori informazioni sulle autorizzazioni necessarie, vedi [Ruoli di accesso per l'utilizzo di {{site.data.keyword.registrylong_notm}}](/docs/services/Registry/iam.html#access_roles_using)..

**Opzioni di comando**
<dl>
<dt>`SOURCE_IMAGE`</dt>
<dd>Il nome dell'immagine di origine. `SOURCE_IMAGE` deve essere nel formato `repository:tag`, ad esempio: `registry.ng.bluemix.net/namespace/image:latest`

</dd>
<dt>`TARGET_IMAGE`</dt>
<dd>Il nome dell'immagine di destinazione. `TARGET_IMAGE` deve essere nel formato `repository:tag`, ad esempio: `registry.ng.bluemix.net/namespace/image:latest`

</dd>
</dl>

**Esempi**

Aggiungi un altro riferimento tag, `latest`, all'immagine *`registry.ng.bluemix.net/birds/bluebird:1`*.

```
ibmcloud cr image-tag  registry.ng.bluemix.net/birds/bluebird:1 registry.ng.bluemix.net/birds/bluebird:latest
```
{: pre}

Copia l'immagine `registry.ng.bluemix.net/birds/bluebird:peck` in un altro repository nello stesso spazio dei nomi `birds/pigeon`.

```
ibmcloud cr image-tag registry.ng.bluemix.net/birds/bluebird:peck registry.ng.bluemix.net/birds/pigeon:peck
```
{: pre}

Copia l'immagine `registry.ng.bluemix.net/birds/bluebird:peck` in un altro spazio dei nomi `animals` a cui hai accesso.

```
ibmcloud cr image-tag registry.ng.bluemix.net/birds/bluebird:peck registry.ng.bluemix.net/animals/dog:bark
```
{: pre}

## `ibmcloud cr info`
{: #bx_cr_info}

Visualizza il nome e l'account del registro a cui hai eseguito l'accesso.

```
ibmcloud cr info
```
{: codeblock}

**Prerequisiti**

Nessuno

## `ibmcloud cr login`
{: #bx_cr_login}

Questo comando esegue il comando `docker login` sul registro. Il comando `docker login` è obbligatorio per abilitare l'esecuzione dei comandi `docker push` o `docker pull` per il registro. Questo comando non è obbligatorio per eseguire altri comandi `ibmcloud cr`. Se Docker non è installato, questo comando restituisce un messaggio di errore.

```
ibmcloud cr login
```
{: codeblock}

**Prerequisiti**

Nessuno

## `ibmcloud cr namespace-add`
{: #bx_cr_namespace_add}

Scegli un nome per il tuo spazio dei nomi e aggiungilo al tuo account {{site.data.keyword.Bluemix_notm}}.

```
ibmcloud cr namespace-add NAMESPACE
```
{: codeblock}

**Prerequisiti**

Per ulteriori informazioni sulle autorizzazioni necessarie, vedi [Ruoli di accesso per l'utilizzo di {{site.data.keyword.registrylong_notm}}](/docs/services/Registry/iam.html#access_roles_using)..

**Opzioni di comando**
<dl>
<dt>`NAMESPACE`</dt>
<dd>Lo spazio dei nomi che desideri aggiungere. Lo spazio dei nomi deve essere univoco in tutti gli account {{site.data.keyword.Bluemix_notm}} nella stessa regione. Gli spazi dei nomi devono avere tra i 4 e i 30 caratteri e contenere solo lettere minuscole, numeri, trattini e caratteri di sottolineatura. Gli spazi dei nomi devono iniziare e finire con una lettera o un numero.
  
<p>  
<strong>Suggerimento</strong> non inserire informazioni personali nei tuoi nomi dello spazio dei nomi.
</p>
  
</dd>
</dl>

**Esempio**

Crea uno spazio dei nomi con il nome *`birds`*.

```
ibmcloud cr namespace-add birds
```
{: pre}

## `ibmcloud cr namespace-list` (`ibmcloud cr namespaces`)
{: #bx_cr_namespace_list}

Visualizza tutti gli spazi dei nomi che appartengono al tuo account {{site.data.keyword.Bluemix_notm}}.

```
ibmcloud cr namespace-list
```
{: codeblock}

**Prerequisiti**

Per ulteriori informazioni sulle autorizzazioni necessarie, vedi [Ruoli di accesso per l'utilizzo di {{site.data.keyword.registrylong_notm}}](/docs/services/Registry/iam.html#access_roles_using)..

## `ibmcloud cr namespace-rm`
{: #bx_cr_namespace_rm}

Rimuove uno spazio dei nomi dal tuo account {{site.data.keyword.Bluemix_notm}}. Le immagini in questo spazio dei nomi vengono eliminate quando viene rimosso lo spazio dei nomi.

```
ibmcloud cr namespace-rm NAMESPACE
```
{: codeblock}

**Prerequisiti**

Per ulteriori informazioni sulle autorizzazioni necessarie, vedi [Ruoli di accesso per l'utilizzo di {{site.data.keyword.registrylong_notm}}](/docs/services/Registry/iam.html#access_roles_using)..

**Opzioni di comando**
<dl>
<dt>`NAMESPACE`</dt>
<dd>Lo spazio dei nomi che desideri rimuovere.</dd>
</dl>

**Esempio**

Rimuovi lo spazio dei nomi *`birds`*.

```
ibmcloud cr namespace-rm birds
```
{: pre}

## `ibmcloud cr plan`
{: #bx_cr_plan}

Visualizza il tuo piano dei prezzi.

```
ibmcloud cr plan
```
{: codeblock}

**Prerequisiti**

Per ulteriori informazioni sulle autorizzazioni necessarie, vedi [Ruoli di accesso per la configurazione di {{site.data.keyword.registrylong_notm}}](/docs/services/Registry/iam.html#access_roles_configure)..

## `ibmcloud cr plan-upgrade`
{: #bx_cr_plan_upgrade}

Esegue il tuo upgrade al piano standard.

Per informazioni sui piani, vedi [Piani del registro](/docs/services/Registry/registry_overview.html#registry_plans).

```
ibmcloud cr plan-upgrade [PLAN]
```
{: codeblock}

**Prerequisiti**

Per ulteriori informazioni sulle autorizzazioni necessarie, vedi [Ruoli di accesso per la configurazione di {{site.data.keyword.registrylong_notm}}](/docs/services/Registry/iam.html#access_roles_configure)..

**Opzioni di comando**
<dl>
<dt>`PLAN`</dt>
<dd>(Facoltativo) Il nome del piano dei prezzi a cui desideri eseguire l'upgrade. Se `PLAN` non viene specificato, il valore predefinito è `standard`.</dd>
</dl>

**Esempio**

Esegui l'upgrade al piano dei prezzi standard.

```
ibmcloud cr plan-upgrade standard
```
{: pre}

## `ibmcloud cr ppa-archive-load`
{: #bx_cr_ppa_archive_load}

Importa il software {{site.data.keyword.IBM_notm}} scaricato da [IBM Passport Advantage Online for customers ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://www.ibm.com/software/passportadvantage/pao_customer.html) e impachettato per l'utilizzo con Helm nel tuo spazio dei nomi {{site.data.keyword.registrylong_notm}}.

Viene eseguito il push delle immagini del contenitore al tuo spazio dei nomi {{site.data.keyword.registryshort_notm}} privato. I grafici Helm sono scritti in una directory `ppa-import` che viene creata nella directory da cui esegui il comando. Facoltativamente, puoi utilizzare il [progetto open source Chart Museum ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://github.com/kubernetes/charts/tree/master/stable/chartmuseum) per ospitare i grafici Helm.

```
ibmcloud cr ppa-archive-load --archive FILE --namespace NAMESPACE
```
{: codeblock}

**Prerequisiti**

Per ulteriori informazioni sulle autorizzazioni necessarie, vedi [Ruoli di accesso per l'utilizzo di {{site.data.keyword.registrylong_notm}}](/docs/services/Registry/iam.html#access_roles_using)..

**Opzioni di comando**
<dl>
  <dt>`--archive FILE`</dt>
  <dd>Il percorso al file compresso scaricato da IBM Passport Advantage.</dd>
  <dt>`--namespace NAMESPACE`</dt>
  <dd>Uno dei tuoi spazi dei nomi. Viene eseguito il push delle immagini contenitore dal file compresso a questo spazio dei nomi. Per elencare gli spazi dei nomi, esegui `ibmcloud cr namespace-list`.</dd>
  <dt>`--chartmuseum-uri URI`</dt>
  <dd>(Facoltativo) Il tuo identificativo di risorsa univoco di [Chart Museum ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://github.com/kubernetes/charts/tree/master/stable/chartmuseum).</dd>
  <dt>`--chartmuseum-user USER`</dt>
  <dd>(Facoltativo) Il tuo nome utente di [Chart Museum ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://github.com/kubernetes/charts/tree/master/stable/chartmuseum).</dd>
  <dt>`--chartmuseum-password PASSWORD`</dt>
  <dd>(Facoltativo) La tua password di [Chart Museum ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://github.com/kubernetes/charts/tree/master/stable/chartmuseum).</dd>
</dl>

**Esempio**

Importa il software IBM e impacchettalo per l'utilizzo con Helm nel tuo spazio dei nomi {{site.data.keyword.registrylong_notm}} *`birds`*, dove il percorso al file compresso è *`downloads/compressed_file.tgz`*.

```
ibmcloud cr ppa-archive-load --archive downloads/compressed_file.tgz --namespace birds
```
{: pre}

## `ibmcloud cr quota`
{: #bx_cr_quota}

Visualizza le tue quote correnti per traffico e archiviazione e le informazioni di utilizzo rispetto a tali quote.

```
ibmcloud cr quota
```
{: codeblock}

**Prerequisiti**

Per ulteriori informazioni sulle autorizzazioni necessarie, vedi [Ruoli di accesso per la configurazione di {{site.data.keyword.registrylong_notm}}](/docs/services/Registry/iam.html#access_roles_configure)..

## `ibmcloud cr quota-set`
{: #bx_cr_quota_set}

Modifica la quota specificata.

```
ibmcloud cr quota-set [--traffic TRAFFIC] [--storage STORAGE]
```
{: codeblock}

**Prerequisiti**

Per ulteriori informazioni sulle autorizzazioni necessarie, vedi [Ruoli di accesso per la configurazione di {{site.data.keyword.registrylong_notm}}](/docs/services/Registry/iam.html#access_roles_configure)..

**Opzioni di comando**
<dl>
<dt>`--traffic TRAFFIC`</dt>
<dd>(Facoltativo) Modifica la tua quota di traffico al valore specificato, in megabyte. L'operazione non riesce se non sei autorizzato a impostare il traffico o se imposti un valore che supera il tuo piano dei prezzi corrente.</dd>
<dt>`--storage STORAGE`</dt>
<dd>(Facoltativo) Modifica la tua quota di archiviazione al valore specificato, in megabyte. L'operazione non riesce se non sei autorizzato a impostare le quote di archiviazione o se imposti un valore che supera il tuo piano dei prezzi corrente.</dd>
</dl>

**Esempio**

Imposta il tuo limite di quota per il traffico di pull su *7000* megabyte e l'archiviazione su *600* megabyte.

```
ibmcloud cr quota-set --traffic 7000 --storage 600
```
{: pre}

## `ibmcloud cr region`
{: #bx_cr_region}

Visualizza la regione di destinazione e il registro.

```
ibmcloud cr region
```
{: codeblock}

**Prerequisiti**

Nessuno

Per ulteriori informazioni, vedi [Regioni](/docs/services/Registry/registry_overview.html#registry_regions).

## `ibmcloud cr region-set`
{: #bx_cr_region_set}

Imposta una regione di destinazione per i comandi {{site.data.keyword.registrylong_notm}}. Per elencare le regioni disponibili, esegui il comando senza alcun parametro.

```
ibmcloud cr region-set [REGION]
```
{: codeblock}

**Prerequisiti**

Nessuno

**Opzioni di comando**
<dl>
<dt>`REGION`</dt>
<dd>(Facoltativo) Il nome della tua regione di destinazione, ad esempio `us-south`.

Per ulteriori informazioni, vedi [Regioni](/docs/services/Registry/registry_overview.html#registry_regions).

</dd>
</dl>

**Esempio**

Indica come destinazione la regione Stati Uniti Sud.

```
ibmcloud cr region-set us-south
```
{: pre}

## `ibmcloud cr token-add`
{: #bx_cr_token_add}

Aggiungi un token che puoi utilizzare per controllare l'accesso a un registro.

```
ibmcloud cr token-add [--description DESCRIPTION] [-q, --quiet] [--non-expiring] [--readwrite]
```
{: codeblock}

**Prerequisiti**

Per informazioni sulle autorizzazioni necessarie, vedi [Ruoli di gestione della piattaforma](/docs/services/Registry/iam.html#platform_management_roles).

**Opzioni di comando**
<dl>
<dt>`--description DESCRIPTION`</dt>
<dd>(Facoltativo) Specifica il valore come una descrizione per il token, che viene visualizzata quando esegui `ibmcloud cr token-list`. Se il tuo token viene creato automaticamente da {{site.data.keyword.containerlong_notm}}, la descrizione viene impostata sul tuo nome cluster Kubernetes. In questo caso, il token viene rimosso automaticamente quando il tuo cluster viene rimosso.
  
<p> 
  <strong>Suggerimento</strong> Non inserire informazioni personali nella tua descrizione del token.
</p>

</dd>
<dt>`-q`, `--quiet`</dt>
<dd>(Facoltativo) Visualizza solo il token, senza alcun testo circostante.</dd>
<dt>`--non-expiring`</dt>
<dd>(Facoltativo) Crea un token con l'accesso che non scade. Senza questo parametro impostato, per impostazione predefinita l'accesso da un token scade dopo 24 ore.</dd>
<dt>`--readwrite`</dt>
<dd>(Facoltativo) Crea un token che concede l'accesso in lettura e scrittura. Senza questa opzione, per impostazione predefinita l'accesso è in sola lettura.</dd>
</dl>

**Esempio**

Aggiungi un token con la descrizione *Token for my account* che non scade e ha l'accesso in lettura/scrittura.

```
ibmcloud cr token-add --description "Token for my account" --non-expiring --readwrite
```
{: pre}

## `ibmcloud cr token-get`
{: #bx_cr_token_get}

Richiama il token specificato dal registro.

```
ibmcloud cr token-get TOKEN
```
{: codeblock}

**Prerequisiti**

Per informazioni sulle autorizzazioni necessarie, vedi [Ruoli di gestione della piattaforma](/docs/services/Registry/iam.html#platform_management_roles).

**Opzioni di comando**
<dl>
<dt>`TOKEN`</dt>
<dd>L'identificativo univoco del token che desideri richiamare. Per elencare i tuoi token, esegui `ibmcloud cr token-list`.</dd>
</dl>

**Esempio**

Richiama il token *10101010-101x-1x10-x1xx-x10xx10xxx10*.

```
ibmcloud cr token-get 10101010-101x-1x10-x1xx-x10xx10xxx10
```
{: pre}

## `ibmcloud cr token-list` (`ibmcloud cr tokens`)
{: #bx_cr_token_list}

Visualizza tutti i token che esistono per il tuo account {{site.data.keyword.Bluemix_notm}}.

```
ibmcloud cr token-list [--format FORMAT]
```
{: codeblock}

**Prerequisiti**

Per informazioni sulle autorizzazioni necessarie, vedi [Ruoli di gestione della piattaforma](/docs/services/Registry/iam.html#platform_management_roles).

**Opzioni di comando**
<dl>
<dt>`--format FORMAT`</dt>
<dd>(Facoltativo) Formatta gli elementi di output utilizzando un modello Go.

Per ulteriori informazioni, vedi [Formattazione e filtro dell'output della CLI per i comandi {{site.data.keyword.registrylong_notm}}](/docs/services/Registry/registry_cli_reference.html#registry_cli_listing).

</dd>
</dl>

**Esempio**

Visualizza tutti i token che sono di sola lettura, utilizzando la direttiva di formattazione *`"{{ if eq .ReadOnly true}}{{.ID}} - {{.Expiry}} - {{.ReadOnly}} - {{.Description}}{{ end }}"`*.

```
ibmcloud cr token-list --format "{{ if eq .ReadOnly true}}{{.ID}} - {{.Expiry}} - {{.ReadOnly}} - {{.Description}}{{ end }}"
```
{: pre}

Questo esempio produce output nel seguente formato:

```
10101010-101a-1a10-a1aa-a10aa10aaa10 - 1234567890 - true - demo
```
{: screen}

## `ibmcloud cr token-rm`
{: #bx_cr_token_rm}

Rimuovi uno o più token specificati.

```
ibmcloud cr token-rm TOKEN [TOKEN...]
```
{: codeblock}

**Prerequisiti**

Per informazioni sulle autorizzazioni necessarie, vedi [Ruoli di gestione della piattaforma](/docs/services/Registry/iam.html#platform_management_roles).

**Opzioni di comando**
<dl>
<dt>`TOKEN`</dt>
<dd>TOKEN può essere il token stesso o l'identificativo univoco del token, come mostrato in `ibmcloud cr token-list`. È possibile specificare più token e devono essere separati da uno spazio.</dd>
</dl>

**Esempio**

Rimuovi il token *10101010-101x-1x10-x1xx-x10xx10xxx10*.

```
ibmcloud cr token-rm 10101010-101x-1x10-x1xx-x10xx10xxx10
```
{: pre}

## `ibmcloud cr vulnerability-assessment` (`ibmcloud cr va`)
{: #bx_cr_va}

Visualizza un report di valutazione delle vulnerabilità per le tue immagini.

```
ibmcloud cr vulnerability-assessment [--extended | -e] [--vulnerabilities | -v] [--configuration-issues | -c] [--output FORMAT | -o FORMAT] IMAGE [IMAGE...]
```
{: codeblock}

**Prerequisiti**

Per ulteriori informazioni sulle autorizzazioni necessarie, vedi [Ruoli di accesso per l'utilizzo di {{site.data.keyword.registrylong_notm}}](/docs/services/Registry/iam.html#access_roles_using)..

**Opzioni di comando**
<dl>
<dt>`IMAGE`</dt>
<dd>Il nome dell'immagine per cui desideri ottenere un report. Il report ti informa se l'immagine ha delle vulnerabilità di pacchetto note. Puoi richiedere report per più immagini contemporaneamente elencando ogni immagine nel comando con uno spazio tra ciascun nome.

<p>Per trovare i nomi delle tue immagini, esegui `ibmcloud cr image-list`. Combina il contenuto delle colonne **Repository** e **Tag** per creare il nome dell'immagine nel formato `repository:tag`. Se nel nome immagine non è specificata una tag, il report valuta l'immagine contrassegnata con `latest`.</p>

<p>Sono supportati i seguenti sistemi operativi:

<ul>
<li>Alpine</li>
<li>CentOS</li>
<li>Debian</li>
<li>RHEL (Red Hat Enterprise Linux)</li>
<li>Ubuntu</li>
</ul>
</p>

Per ulteriori informazioni, vedi [Gestione della sicurezza delle immagini con il Controllo vulnerabilità](/docs/services/va/va_index.html).

</dd>
<dt>`--output FORMAT`, `-o FORMAT`</dt>
<dd>(Facoltativo) L'output del comando viene restituito nel formato scelto. Il formato predefinito è `text`. Sono supportati i seguenti formati:

<ul>
<li>`text`</li>
<li>`json`</li>
</ul>

</dd>
<dt>`--vulnerabilities`, `-v`</dt>
<dd>(Facoltativo) L'output del comando è limitato alla visualizzazione delle sole vulnerabilità.</dd>
<dt>`--configuration-issues`, `-c`</dt>
<dd>(Facoltativo) L'output del comando è limitato alla visualizzazione dei soli problemi di configurazione.</dd>
<dt>`--extended`, `-e`</dt>
<dd>(Facoltativo) L'output del comando mostra informazioni aggiuntive sulle correzioni per i pacchetti vulnerabili.</dd>

</dl>

**Esempi**

Visualizza un report di valutazione delle vulnerabilità standard per la tua immagine.

```
ibmcloud cr vulnerability-assessment registry.ng.bluemix.net/birds/bluebird:1
```
{: pre}

Visualizza un report di valutazione delle vulnerabilità per le tue immagini *`registry.ng.bluemix.net/birds/bluebird:1`* in formato JSON, visualizzando solo le vulnerabilità.

```
ibmcloud cr vulnerability-assessment --vulnerabilities  --output json registry.ng.bluemix.net/birds/bluebird:1
```
{: pre}