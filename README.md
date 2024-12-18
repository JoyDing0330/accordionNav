
<!-- README.md is generated from README.Rmd. Please edit that file -->

# accordionNav

<!-- badges: start -->
<!-- badges: end -->

This project showcases an easy way to create menu items stored within a
navigation dropdown menu in bslib.

## Installation

You can install the development version of accordionNav from
[GitHub](https://github.com/) with:

``` r
# install.packages("devtools")
devtools::install_github("B-Klaver/accordionNav")
```

## Example

Set up your menu using nested lists:

``` r
library(accordionNav)

## set up menu list

domain_list <- list(
  "Domain 1" = list(
    "Sub-domain 1.1",
    "Sub-domain 1.2"
  ),
  "Domain 2" = list(
    "Sub-domain 2.1"
  ),
  "Domain 3" = list(
    "Sub-domain 3.1",
    "Sub-domain 3.2",
    "Sub-domain 3.3"
  )
)
```

If you want to add icons in front of each domain, set up the icon list
using the following code and you can substitute the icons with anyone
you prefer from [here](https://icons.getbootstrap.com/). Please note
that the number of icon within the list should matches the number of
domains in the domain list. If you don’t want to use icon, then please
omit this part.

``` r

## set up icon list

domain_icon <- c(
  "people-fill",
  "house-door-fill",
  "person-walking"
  )
```

Each sub-menu item comes with a default icon (\>\>). To customize the
icon, use the sub_icon parameter in the `accordionTabset()` function..
You can explore all available icons
[here](https://fontawesome.com/icons).

``` r

# set up icon for sub-menu items

accordionTabset(
      id = "accord_select", 
      menu_list = domain_list,
      icon_list = domain_icon,
      class = "link-body-emphasis d-inline-flex text-decoration-none mb-3 rounded w-100",
      sub_icon = "square"
    )
```

Now you can use this menu list within a sidebar UI using the
`accordionTabset()` function. In addition, create a `navset_hidden()`
bin and place your panels that you’d like to show with
`nav_panel_hidden()`. The values should be set to the name of the menu
item it relates to:

``` r

ui <- bslib::page_sidebar(
  title = "Accordion Navigation Example",
  class = "bslib-page-dashboard",
  sidebar = bslib::sidebar(
    accordionTabset(
      id = "accord_select", 
      menu_list = domain_list,
      icon_list = domain_icon,
      class = "link-body-emphasis d-inline-flex text-decoration-none mb-3 rounded w-100"
    )
  ),
  bslib::navset_hidden(
    id = "hidden_tabs",
    bslib::nav_panel_hidden(
      value = domain_list$`Domain 1`[[1]],
      "Panel 1.1 content"
    ),
    bslib::nav_panel_hidden(
      value = domain_list$`Domain 1`[[2]],
      "Panel 1.2 content"
    ),
    bslib::nav_panel_hidden(
      value = domain_list$`Domain 2`[[1]],
      "Panel 2.1 content"
    ),
    bslib::nav_panel_hidden(
      value = domain_list$`Domain 3`[[1]],
      "Panel 3.1 content"
    ),
    bslib::nav_panel_hidden(
      value = domain_list$`Domain 3`[[2]],
      "Panel 3.2 content"
    ),
    bslib::nav_panel_hidden(
      value = domain_list$`Domain 3`[[3]],
      "Panel 3.3 content"
    )
  )
)
```

Easily link the accordion tabset buttons to the hidden nav panels using
the `accordionPanelSelect()` function:

``` r

server <- function(input, output, session) {
  
  accordionPanelSelect(
    input, 
    session,
    panel_id = "hidden_tabs",
    accordion_id = "accord_select",
    menu_list = domain_list
  )
  
}
```
