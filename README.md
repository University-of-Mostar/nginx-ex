## Template

Template predstavlja set objekata koji se mogu parametrizirati i procesuirati što rezultira kreiranjem njihovih instanci u OKD klasteru. Unutar templatea se može definirati bilo koji objekt, ukoliko korisnik ima odgovarajuće ovlasti za njegovo kreiranje, kao što su deploymenti, servisi, stateful setovi, build konfiguracije i slično.

## Upload templatea

Template se definira u okviru JSON ili YAML datoteke te se upload u klaster radi putem CLI-a. Ovakav način isporuke omogućava ponovno upotrebu predloška od strane bilo kojeg korisnika koji ima pristup projektu.

Naredba za upload templatea u OKD:

```
oc create -f <naziv-datoteke>
```

Ukoliko se template želi koristiti isključivo na jednom projektu, upload se radi korištenjem naredbe:

```
oc create -f <naziv-datoteke> -n <naziv-projekta>
```

## Kreiranje aplikacije kroz web sučelje

Putem web sučelja moguće je kreirati aplikaciju iz templatea.

`Proces`

1. Pozicioniranje u željeni projekt, potom klik na "Add to Project"
2. Odabir templatea iz kataloga predložaka
3. Modifikacija postavki u novom prozoru kako bi aplikacija radila prema korisnikovim potrebama

Kako bi vidjeli sve dostupne konfiguracijske parametre u templateu možemo iskoristiti naredbu:

```
oc process --parameters -n <projekt> <naziv-templatea>
```

Tako recimo za predložak Nginx web servera pokretanjem naredbe `oc process -- parameters -n openshift nginx-example` dobijemo popis parametara:

```
NAME                     DESCRIPTION                                                                                               GENERATOR           VALUE
NAME                     The name assigned to all of the frontend objects defined in this template.                                                    nginx-example
NAMESPACE                The OpenShift Namespace where the ImageStream resides.                                                                        openshift
NGINX_VERSION            Version of NGINX image to be used (1.16-el8 by default).                                                                      1.16-el8
MEMORY_LIMIT             Maximum amount of memory the container can use.                                                                               512Mi
SOURCE_REPOSITORY_URL    The URL of the repository with your application source code.                                                                  https://github.com/University-of-Mostar/nginx-ex
SOURCE_REPOSITORY_REF    Set this to a branch name, tag or other ref of your repository if you are not using the default branch.
CONTEXT_DIR              Set this to the relative path to your project if it is not in the root of your repository.
APPLICATION_DOMAIN       The exposed hostname that will route to the nginx service, if left blank a value will be defaulted.
GITHUB_WEBHOOK_SECRET    Github trigger secret.  A difficult to guess string encoded as part of the webhook URL.  Not encrypted.   expression          [a-zA-Z0-9]{40}
GENERIC_WEBHOOK_SECRET   A secret string used to configure the Generic webhook.                                                    expression          [a-zA-Z0-9]{40}
```

## Uređivanje templatea

Postojeći predlošci unutar OKD klastera mogu se uređivati pomoću narebe:

```
oc edit template <naziv-templatea>
```

## Pisanje vlastitih templatea

OKD omogućuje pisanje vlastitih predložaka koji se definiraju kroz YAML datoteku, gdje je ključno navesti `kind: Template`.

Parametri dopuštaju da vrijednost unesete ili generirate prilikom instalacije samog templatea. Zatim se ta vrijednost zamjenjuje gdje god se parametar poziva. Reference se mogu definirati u bilo kojem polju u polju za popis objekata. Ovo je korisno da navedete naziv hosta ili neku drugu vrijednost specifičnu za korisnika koja je potrebna za prilagodbu predloška.
Parametri u predlošku se definiraju kao `${NAZIV_PARAMETRA}`.

Dodatne informacije o pisanju templatea može se pronaći na službenoj [dokumentaciji](https://docs.okd.io/latest/openshift_images/using-templates.html#templates-writing_using-templates).
