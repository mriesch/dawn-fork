---
title: Spezifikation
---
# Entwickler-Dokumentation RPM Nomenklatur

Ein RPM besteht aus einer einzigen Datei. Normalerweise hat der Dateinamen folgendes Format:

```bash
 <name>-<version>-<release>
```

## Hinweis

Die Schlüsselwörter „MUSS“, „DARF NICHT“, „ERFORDERLICH“, „SOLL“, „VERBOTEN“, „NÖTIG“, „NICHT NÖTIG“, „SOLL NICHT“, „EMPFOHLEN“, „DARF“, „KANN“ und „OPTIONAL“ werden nach 2119de interpretiert. <https://goo.gl/6QZH4J>

## Spezifikation Build-Type

Im Kontext der DDS-Classic gibt es genau zwei Build Typen.

| Vollständige Liste aller gültigen Werte | Verwendung                                                                 |
|:----------------------------------------|:---------------------------------------------------------------------------|
| RELEASE                                 | Wird an den Test weitergeben. KANN auch in Produktions installiert werden. |
| SNAPSHOT                                | Wird NICHT an den Test weitergegeben werden                                |

Regeln:

- neu


- Wenn in Git Branch develop/ oder release/ gebaut ist der Build Typ ein RELEASE.

- Wenn in einem anderen Git Branch gebaut wird, ist Build Typ ein SNAPSHOT.

- Wenn der Git Branch nicht ermittelt werden kann (z.B. HEAD detached at <Commit-ID>  als Grund), wird auf die Jenkins Environment Variable ausgewertet.

## Spezifikation Zeichenkette RPM-Name

Im DDS-Classic Kontext MUSS eine Zeichenkette, die einen RPM-Name beschreibt, folgenden Werte haben:

### Für Builds in Git Branch develop/ und release/

| Vollständige Liste aller gültigen Werte |
|-----------------------------------------|
| c0hv_dds-application                    |
| c0hv_dds-domain                         |
| c0hv_dds-weblogic                       |

Sonstige Anforderungen:

- Diese Nomenklatur MUSS verwendet werden, wenn BUILD_TYPE gleich RELEASE ist.
- Der Name DARF sich NICHT ändern zwischen den Versionswechseln.

### Für Build aus allen anderen Git Zweigen

| Vollständige Liste aller gültigen Werte |
| --------------------------------------- |
| c0hv_dds-application-${branch_name}     |
| c0hv_dds-domain-${branch_name}          |
| c0hv_dds-weblogic-${branch_name}        |

Sonstige Anforderungen:

- Diese Nomenklatur MUSS verwendet werden wenn NICHT aus dem Git Branch /develop oder /release gebaut wird.
- Wenn die Zeichenkette `${branch_name}` am Ende `'_rpm'` enthält, MUSS die Zeichenkette `'_rpm'` entfernt werden.
- Der Implementierungsname ist SNAPSHOT

## Spezifikation Zeichenkette RPM-Version

Im DDS-Classic Kontext sind für RPM-Version folgende Werte gültig:

### Für Builds in Git Branch develop/ und release/

| Beispiele möglicher Werte |
| ------------------------- |
| 19.00.00.01               |
| 19.01.00.01               |
| 21.00.00.02               |
| Sonstige Anforderungen:   |

- Diese Nomenklatur MUSS verwendet werden, wenn aus dem Git Branch /develop oder /relase gebaut wird.
- Es ist immer ein Tupel der Länge 4
- Die Zahlen MÜSSEN mindestens 2-stellig sein.
- Wenn die Zahlen einstellig sind MÜSSEN sie mit einer führenden Null versehen werden.
- Die Zahlen KÖNNEN eine führende Null haben
- Die Zahlen KÖNNEN auch dreistellig werden.
- Die letzte Zahl ist die Jenkins Buildnummer.
- Der Implentierungsname ist release

### Für Build aus allen anderen Git Zweigen

| Beispiele möglicher Werte |
| ------------------------- |
| 19.0.0                    |
| 19.1.0                    |
| 21.0.0                    |
| Sonstige Anforderungen:   |

Sonstige Anforderungen:

- Diese Nomenklatur MUSS verwendet werden wenn NICHT aus dem Git Branch /develop oder /relase gebaut wird.
- Die Zahlen SOLLEN haben keine führende Null haben.
- Es MUSS ein Tupel der Länge 3 sein.
- Semantic Versioning KANN verwendet werden.
- Der Implementierungsname ist snapshot

## Spezifikation Zeichenkette RPM-Release

### Für Builds in Git Branch develop/ und release/

Im DDS-Classic Kontext sind für RPM-Release sind folgende Werte gültig:

| Beispiel möglicher Werte |
| ------------------------ |
| 00                       |
| Sonstige Anforderungen:  |

- Die Zahl MUSS immer Null sein.
- Die Zahl MUSS immer zweistellig sein.
- Es ist immer ein Tupel der Länge 1
- Der Implentierung name ist release

### Für Build aus allen anderen Git Zweigen

| Beispiel möglicher Werte |
| ------------------------ |
| 1                        |
| 2                        |
| 3                        |
| Sonstige Anforderungen:  |

- Es ist immer ein Tupel der Länge 1
- Der Implementierungsname ist snapshot

---

# Implementierung Details:

## Implentierung Weblogic RPM

### Gegeben:

`<name>-<version>-<release>`

### Wirken sich aus auf:

#### SPEC FILE

`_my_dir_name %{_rpm_name}-%{_rpm_version}-%{_rpm_release}`

#### Imstallation in Verzeichnis:

`/pkg/3550/C0hv/home/c0hvdds/%{_my_dir_name}`

#### Anpassung File \_path:

Nur die Zeile: `readonly base_dir=/var/apps/{$NAME}-{$VERSION}-{$RELEASE}`

#### Weblogic wird in der Verzeichnis

`/var/apps/{$NAME}-{$VERSION}-{$RELEASE} installiert.`

## Implentierung Domain RPM

### Gegeben:

`<name>-<version>-<release>`

### Wirken sich aus auf:

#### SPEC FILE

\_my_dir_name %{\_rpm_name}-%{\_rpm_version}-%{\_rpm_release}

#### Installation in der Verzeichnis:

/pkg/3550/C0hv/home/c0hvdds/%{\_my_dir_name}

#### Anpassung File \_parameter.bash

echo "${0} 'build_type'    -> Ausgabe >${build_type}<" echo "${0} 'branch_name'   -> Ausgabe >${branch_name}<" echo "${0} 'version'       -> Ausgabe >${version}<" echo "${0} 'buildnumber'   -> Ausgabe >${buildnumber}<" echo "${0} 'dds_setup_dir' -> Ausgabe >${dds_setup_dir}<"

#### SNAPSHOT Domäne wird in folgendes Verzeichnis installiert

/var/apps/weblogic/local/classic-21.0.0-build10

#### RELEASE Domäne wird in folgendes Verzeichnis installiert

/var/apps/weblogic/local/classic-21.00.00-build10

<SwmSnippet path="/snippets/price.liquid" line="1">

---

This code snippet is a Liquid template that renders the price of a product. It has several optional parameters such as `product`, `use_variant`, `show_badges`, `price_class`, and `show_compare_at_price` that allow customization of the rendered price. The code is intended to be used by calling the `render` tag with the `'price'` template and passing the `product` object as an argument.

```liquid
{% comment %}
  Renders a list of product's price (regular, sale)

  Accepts:
  - product: {Object} Product Liquid object (optional)
  - use_variant: {Boolean} Renders selected or first variant price instead of overall product pricing (optional)
  - show_badges: {Boolean} Renders 'Sale' and 'Sold Out' tags if the product matches the condition (optional)
  - price_class: {String} Adds a price class to the price element (optional)
  - show_compare_at_price: {Boolean} Renders the compare at price if the product matches the condition (optional)

  Usage:
  {% render 'price', product: product %}
{% endcomment %}
```

---

</SwmSnippet>

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBZGF3bi1mb3JrJTNBJTNBbXJpZXNjaA==" repo-name="dawn-fork"><sup>Powered by [Swimm](https://app.swimm.io/)</sup></SwmMeta>
