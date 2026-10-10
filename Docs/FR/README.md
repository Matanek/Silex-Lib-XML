# XML pour Silex

Le package `XML` construit, analyse, parcourt et sérialise des documents XML
stricts. Il préserve la structure du document tout en rejetant explicitement
les fonctionnalités susceptibles de lire ou développer des entités externes.

[Read this documentation in English.](../EN/README.md)

## Installer le package

```text
silex install XML
```

Importez ensuite son module public :

```sx
use XML
```

Cette révision en développement requiert la toolchain avec capture
`try` des opérations `Result` et extraction lors de l’appel ordinaire.

## Analyser et parcourir un document

`XML.parse` transforme un texte connu en arbre de nœuds :

```sx
use XML

func main() {
    var document = XML.parse(
        "<feed><entry id=\"1\"><title>Silex &amp; XML</title></entry></feed>"
    )
    var entry = document.elements("entry")[0]
    if let identifier = entry.attribute("id") {
        print("$(identifier): $(entry.text_content())")
    }
}
```

Le programme affiche :

```text
1: Silex & XML
```

Le parseur conserve la casse, les noms qualifiés comme `atom:link`, les
instructions de traitement, les commentaires et les sections CDATA. Il exige
des attributs entre guillemets, des noms d’attribut uniques, des balises de
fermeture correspondantes et exactement une racine. Les cinq entités XML
prédéfinies et les références numériques de caractères sont décodées.

## Construire et sérialiser

`XML.document()` et `XML.element(name)` créent des documents sans texte source.
Les nœuds permettent ensuite d’ajouter des éléments, attributs, textes,
commentaires et sections CDATA. `XML.serialize` produit un texte compact par
défaut; utilisez `XML.Format.pretty` pour une indentation lisible.

## Connaître les limites de sécurité

Les déclarations DTD et le développement d’entités personnalisées sont
rejetés. Un document non fiable ne peut donc pas déclencher un accès à une
entité externe ni une expansion exponentielle d’entités.

La version 0.2 conserve les déclarations de namespace et les noms qualifiés,
mais ne les résout pas encore en paires URI de namespace et nom local. Les noms
XML acceptés correspondent actuellement au sous-ensemble ASCII portable.

`parse` possède une seule déclaration `Result<Document, ParseError>`.
L’appel ordinaire rend le document ou arrête le programme sur erreur.
`try XML.parse(source)` conserve les erreurs de structure et de limite avec
leur position dans la source pour les traiter explicitement.
