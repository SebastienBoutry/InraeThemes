
<!-- README.md is generated from README.Rmd. Please edit that file -->

# InraeThemes <img src="man/figures/logo_hex.png" align="right" height="139"/>

<!-- badges: start -->

[![Lifecycle:experimental](https://img.shields.io/badge/lifecycle-experimental-orange.svg)](https://www.tidyverse.org/lifecycle/#experimental)
[![packageversion](https://img.shields.io/badge/Package%20version-2.1.0-green?style=flat-square)](commits/master)
[![Licence](https://img.shields.io/badge/licence-GPL--3-blue.svg)](https://www.gnu.org/licenses/gpl-3.0.en.html)
[![R-CMD-check](https://github.com/davidcarayon/InraeThemes/workflows/R-CMD-check/badge.svg)](https://github.com/davidcarayon/InraeThemes/actions)
<!-- badges: end -->

InraeThemes est une collection non officielle de templates, thèmes et
autres éléments graphiques basés sur des formats en lien avec R et en
accords avec la charte graphique INRAE.

# Installation

Le package peut-être installé via :

``` r
# install.packages("remotes")
remotes::install_github("davidcarayon/InraeThemes")
```

ou encore via :

``` r
# install.packages("remotes")
remotes::install_git("https://gitlab.irstea.fr/david.carayon/inraethemes")
```

> NB : L’ancienne version (1.0.1) peut toujours être installée via :

``` r
# install.packages("remotes")
remotes::install_github("davidcarayon/InraeThemes@v1.0.1")
```

# Pré-requis

-   Certaines fonctionnalités de ce package nécessitent l’installation
    de 2 polices adoptées dans la charte graphique INRAE : *Raleway* et
    *Avenir Next LT Pro*. Ces polices peuvent être téléchargées
    [ici](https://intranet.inrae.fr/charte-identitaire/content/download/3007/30036/version/5/file/POLICES.zip).

-   La police Fira Code est utilisée dans certains templates et est,
    d’ailleurs, recommandée sur votre Rstudio pour l’affichage du code
    avec ligatures : [Fira
    Code](https://fonts.google.com/specimen/Fira+Code)

-   Si vous ne possédez aucune installation de LaTeX sur votre machine,
    vous devrez également en installer une version minimale pour
    utiliser les modèles mobilisant LaTeX :

``` r
install.packages("tinytex")
tinytex::install_tinytex()
```

-   Certains modèles nécessitent l’utilisation de Quarto, successeur de
    Rmarkdown, qui peut être téléchargé ici :
    <https://quarto.org/docs/get-started/>.

-   Une version de Rstudio supérieure à la 2022.02.1 est nécessaire pour
    utiliser Quarto de manière conviviale.

# Présentation des fonctionnalités

InraeThemes propose ces éléments :

-   Une palette de couleur
-   Un thème ggplot2
-   Un thème Bootstrap Sass basé sur {bslib}
-   Deux versions de `scale_` : `scale_fill_inrae()` et
    `scale_color_inrae()`
-   Un template de présentation basé sur la classe `presentation` de
    Quarto et proposant 3 formats de sortie : HTML, PDF et PPTX
-   Un template de rapport basé sur la classe `book` de Quarto et
    proposant 3 formats de sortie : HTML, PDF et DOCX
-   Deux templates de présentation et de rapports au format Rmarkdown
-   Un template de carte de visite au format Rmarkdown
-   Un template de projet Rstudio suivant un certain nombre de “bonnes
    pratiques”

## Palette de couleurs

La palette de couleurs construite à partir de la charte graphique V3 est
la suivante :

``` r
palette_inrae()
```

![](man/figures/scales.png)

## Thème ggplot2 et scales

Voici des exemples de graphes utilisants le `theme_inrae()` ainsi que
les fonctions `scale_()`:

``` r
library(InraeThemes)
library(ggplot2)

## Ce package contient des données d'exemple
data("example_datasets")

ggplot(example_datasets$www, aes(x = Minute, y = Users, color = Measure, shape = Measure)) +
  geom_line() +
  geom_point(size = 3) +
  facet_wrap(~Measure) +
  geom_point(size = 1.8) +
  scale_color_inrae() +
  scale_shape_manual(values = c(15, 16)) +
  labs(title = "Titre", subtitle = "Sous-titre") +
  theme_inrae()
```

<img src="man/figures/README-example-1.png" width="100%" />

``` r
ggplot(example_datasets$cars, aes(x = mpg, fill = cyl,colour = cyl)) +
  geom_density(alpha = 0.75) +
  scale_fill_inrae() +
  scale_color_inrae() +
  labs(fill = "Cylinders", colour = "Cylinders", x = "MPG", y = "Density") +
  theme_inrae()
```

<img src="man/figures/README-example-2.png" width="100%" />

``` r
ggplot(example_datasets$dia, aes(x = price, fill = cut)) +
  geom_histogram(binwidth = 850) +
  xlab("Price (USD)") +
  ylab("Count") +
  scale_fill_inrae() +
  scale_x_continuous(label = function(x) paste0(x / 1000, "k")) +
  theme_inrae()
```

<img src="man/figures/README-example-3.png" width="100%" />

``` r
ggplot(example_datasets$drivers, aes(x = Year, y = Deaths,fill = Year)) +
  geom_boxplot(size = 0.25) +
  ylab("Monthly Deaths") +
  theme_inrae() +
  scale_fill_inrae() +
  coord_flip() +
  labs(caption = "Caption")
```

<img src="man/figures/README-example-4.png" width="100%" />

# Template Bootstrap Sass

Vous pouvez prévisualiser l’apparence de ce thème bootstrap pouvant être
utilisé avec Shiny et Rmarkdown (et probablement Quarto un jour) via :

``` r
bslib::bs_theme_preview(bs_inrae())
```

![](man/figures/cap_bslib.png)

Ce thème peut être utilisé dans un document Rmarkdown en préciseant dans
son YAML :

``` r
output:
  html_document:
    theme: !expr InraeThemes::bs_inrae()
```

Ou dans une application Shiny via :

``` r
ui <- fluidPage(
  theme = InraeThemes::bs_inrae(),
  ...
)
```

# Templates de documents/présentations Quarto

Ce package permet aussi de rédiger des rapports et/ou présentations
pré-formatés selon la charte graphique INRAE.

> **Attention, ces fonctions ne visent qu’à fournir des templates (css,
> LaTeX, docx, logos) correspondants à la charte INRAE, associés à des
> fichiers Quarto avec un YAML correctement configuré. Nous invitons les
> utilisateurs à se renseigner par la suite sur chacune des technologies
> utilisées pour aller plus loin dans la personnalisation des
> documents.**

Dans tous les cas, la documentation de Quarto est indispensable pour la
customisation de ces documents :

-   [Guide d’utilisation des formats
    quarto](https://quarto.org/docs/guide/)
-   [Liste exhaustive des options](https://quarto.org/docs/reference/)

Ces templates sont soit disponibles via une ligne de commande, en
l’absence pour le moment d’interface graphique de création de document
quarto avec template, soit via l’interface graphique Rstudio de création
de projet.

**A noter pour les rapports PDF :**

-   La sortie PDF (basée sur LaTeX) s’appuie sur des fichiers `.tex`
    indépendants qu’il faudra customiser pour l’image de couverture
    ainsi que pour la dernière page.

-   L’image sur la page de garde (photo.png) peut-être remplacée par
    n’importe quelle image. Si la hauteur de la nouvelle image diffère
    de celle d’origine, il faudra alors modifier la valeur en cm du
    `\vspace*{}` en L11 de `templates/page_de_garde.tex` pour retrouver
    une mise en forme correcte.

-   la cartouche “Centre” peut être remplacée par celle qui vous
    correspond à télécharger
    [ici](https://intranet.inrae.fr/charte-identitaire/content/download/3749/33311/version/1/file/Cartouches%20Centre.zip)

## Création via ligne de commande

En l’absence de module de création de document quarto basé sur un
template à la Rmarkdown (implémentation prévue pour juillet 2022), la
création d’un document Quarto avec un thème INRAE est pour le moment
uniquement possible via les fonctions suivantes (ou via l’interface de
création de projets présentée ci-dessous):

``` r
InraeThemes::create_presentation()
```

ou pour les rapports :

``` r
InraeThemes::create_report()
```

Ces fonctions vont copier, dans le répertoire courant (ou un autre
répertoire au choix) l’ensemble des fichiers associés aux templates
(fichiers .qmd, css, tex, etc.).

Le YAML de ces template contient les informations nécessaires pour un
rendu sous 3 formats possibles : HTML, PDF et PPTX/DOCX. Vous pouvez
soit laisser ce contenu et choisir le format de rendu via le menu
déroulant associé aux bouton `Render` pour les présentations et `Build`
(situé en haut à droite sur Rstudio) pour les rapports, soit effacer les
formats non souhaités.

## Création via projets Rstudio

L’interface graphique de création de projets Rstudio permet de créer des
documents avec des thèmes INRAE via :

-   `Projects > New Project > New Directory > Présentation INRAE`

pour les présentations ou

-   `Projects > New Project > New Directory > Rapport INRAE`

pour les rapports.

# Templates de documents/présentations Rmarkdown

Deux templates Rmarkdown issus de la version 1.0 d’INRAEThemes ont été
conservés car il n’existe pas encore d’équivalents sous Quarto (et
étaient les plus utilisés):

-   Présentation RemarkJS via {xaringan}
-   Rapport paginé via {pagedreport}

Ces deux templates sont accessibles via l’interface de création de
documents Rmarkdown sous Rstudio.

> **Attention, ces éléments ne visent encore une fois qu’à fournir des
> templates (css, LaTeX, logos) correspondants à la charte INRAE,
> associés à des fichiers Rmarkdown avec un YAML correctement configuré.
> Nous invitons les utilisateurs à se renseigner par la suite sur
> chacune des technologies utilisées pour aller plus loin dans la
> personnalisation des documents.**

# Template de carte de visite

Une carte de visite, basée sur {pagedown}, avec logo INRAE peut-être
produite en plusieurs exemplaires par page (nombre paramétrable) via
l’interface de création de documents Rmarkdown sous Rstudio.

# Création d’un répertoire d’analyse

Nous proposons dans ce package un template de projet pour l’analyse de
données, librement inspiré du package
[{ProjectTemplate}](https://cran.r-project.org/web/packages/ProjectTemplate/)
Ce template est directement accessible dans Rstudio via
`Projects > New Project > New Directory > Data Analysis Project`.
L’utilisateur peut ici définir la localisation de son projet et choisir
d’initialiser ou non un dépôt git.

> Note : Cette architecture n’est qu’un exemple de bonne pratiques
> parmis bien d’autres. Libre à l’utilisateur de modifier ce template
> selon ses habitudes. Vos suggestions d’améliorations sont évidemment
> les bienvenues
> [ici](https://github.com/davidcarayon/InraeThemes/issues).

# Work in Progress / TO-DO

-   Convertir les templates de projet Quarto en templates de documents
    sous Rstudio lorsque ce sera disponible (juillet 2022)

-   Création d’un format de rapport PDF “simple” et non sous format
    book, qui sera proposé une fois les templates de documents
    disponibles

-   Meilleure gestion de la page de garde PDF (photo) ainsi que des
    infos de bas de page directement dans le YAML
