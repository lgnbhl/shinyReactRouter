
<!-- README.md is generated from README.Rmd. Please edit that file -->

# reactRouter

<!-- badges: start -->

[![Lifecycle:
experimental](https://img.shields.io/badge/lifecycle-experimental-orange.svg)](https://lifecycle.r-lib.org/articles/stages.html#experimental)
[![CRAN
status](https://www.r-pkg.org/badges/version/reactRouter)](https://CRAN.R-project.org/package=reactRouter)
[![R-CMD-check](https://github.com/lgnbhl/reactRouter/actions/workflows/R-CMD-check.yaml/badge.svg)](https://github.com/lgnbhl/reactRouter/actions/workflows/R-CMD-check.yaml)
[![](https://img.shields.io/badge/react--router--dom-6.30.0-blue.svg)](https://mui.com/material-ui/getting-started/)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-Follow-E4405F?style=social&logo=linkedin)](https://www.linkedin.com/in/FelixLuginbuhl)
<!-- badges: end -->

The goal of **reactRouter** is to provide a wrapper around [React Router
(v6)](https://reactrouter.com/6.30.0).

### Usage

You can now add URL pages in Quarto document or R shiny like so:

``` r
library(reactRouter)

HashRouter(
  NavLink(to = "/", "Main"),
  NavLink(to = "/analysis", "Analysis"),
  Routes(
    Route(path = "/", element = "Main content"),
    Route(path = "/analysis", element = "Analysis content")
  )
)
```

### Install

Install **reactRouter** with:

``` r
remotes::install_github("lgnbhl/reactRouter")
```

### Usage with Quarto

As React Router provides client routing, you can easily create multiple
routes in a Quarto and R markdown documents:

``` r
# to run an a Quarto document
# example from: https://github.com/remix-run/react-router/tree/dev/examples/basic
library(htmltools)
library(reactRouter)

Layout <- div(
  # A "layout route" is a good place to put markup you want to
  # share across all the pages on your site, like navigation.
  tags$nav(
    tags$ul(
      tags$li(
        reactRouter::Link(to = "/", "Home")
      ),
      tags$li(
        reactRouter::Link(to = "/about", "About")
      ),
      tags$li(
        reactRouter::Link(to = "/dashboard", "Dashboard")
      ),
      tags$li(
        reactRouter::Link(to = "/nothing-here", "Nothing Here")
      )
    )
  ),
  tags$hr(),
  # An <Outlet> renders whatever child route is currently active,
  # so you can think about this <Outlet> as a placeholder for
  # the child routes we defined above.
  reactRouter::Outlet()
)

reactRouter::HashRouter(
  div(
    h1("Basic Example"),
    tags$p(
      paste0('This example demonstrates some of the core features of React Router
          including nested reactRouter::Route(), reactRouter::Outlet(), 
          reactRouter::Link(), and using a "*" route (aka "splat route") 
          to render a "not found" page when someone visits an unrecognized URL.'
      )
    ),
    reactRouter::Routes(
      Route(
        path = "/",
        element = Layout,
        Route(
          index = TRUE,
          element = div(
            tags$h2("Home")
          )
        ),
        Route(
          path = "about",
          element = div(
            tags$h2("About")
          )
        ),
        Route(
          path = "dashboard",
          element = div(
            tags$h2("Dashboard")
          )
        ),
        # Using path="*"" means "match anything", so this route
        # acts like a catch-all for URLs that we don't have explicit
        # routes for.
        Route(
          path = "*",
          element = div(
            tags$h2("Nothing to see here!"),
            tags$p(
              Link(to = "/", "Go to the home page")
            )
          )
        )
      )
    )
  )
)
```

### Usage with R Shiny

Below a simple example using R Shiny:

``` r
library(reactRouter)
library(shiny)
library(DT)

ui <- fluidPage(
  reactRouter::HashRouter(
    titlePanel("reactRouter"),
    sidebarLayout(
      sidebarPanel(
        reactRouter::NavLink(
          to = "/",
          style = JS('({isActive}) => { return isActive ? {color: "red", textDecoration:"none"} : {}; }'),
          "Home"),
        br(),
        reactRouter::NavLink(
          to = "/analysis",
          style = JS('({isActive}) => { return isActive ? {color: "red", textDecoration: "none"} : {}; }'),
          "Analysis"
        ),
        br(),
        reactRouter::NavLink(
          to = "/about",
          style = JS('({ isActive }) => { return isActive ? { color: "red", textDecoration: "none" } : {}; }'),
          "About"
        )
      ),
      mainPanel(
        reactRouter::Routes(
          reactRouter::Route(
            path = "/",
            element = div(
              tags$h1("Home page"),
              tags$h4("A basic example of reactRouter."),
              uiOutput(outputId = "contentHome")
            )
          ),
          reactRouter::Route(
            path = "/analysis",
            element = div(
              tags$h1("Analysis"),
              DT::DTOutput("table")
              )
          ),
          reactRouter::Route(
            path = "/about",
            element = div(
              uiOutput(outputId = "contentAbout")
            )
          ),
          reactRouter::Route(path = "*", element = div(tags$p("Error 404")))
        )
      )
    )
  )
)

server <- function(input, output, session) {
  output$contentHome <- renderUI({
    p("Content home")
  })
  output$table <- renderDT({
    data.frame(x = c(1, 2), y = c(3, 4))
  })
  output$contentAbout <- renderUI({
    div(
      tags$h1("About"),
      p("Content about")
    )
  })
}

shinyApp(ui, server)
```

### Usage with MUI Material UI

Below an example using MUI Material UI components from the
**shinyMaterialUI** R package:

``` r
# remotes::install_github("lgnbhl/shinyMaterialUI")
library(shiny)
library(shinyMaterialUI)
library(reactRouter)


ui <- reactRouter::HashRouter(
    CssBaseline(
    Typography("reactRouter with shinyMaterialUI", variant = "h5", m = 2),
    Stack(
      direction = "row", spacing = 2, p = 2,
      Paper(
        MenuList(
          reactRouter::NavLink(
            to = "/",
            style = JS('({isActive}) => { return isActive ? {color: "red", textDecoration:"none"} : { textDecoration: "none" }; }'),
            MenuItem(
              "home"
            )
          ),
          br(),
          reactRouter::NavLink(
            to = "/analysis",
            style = JS('({isActive}) => { return isActive ? {color: "red", textDecoration: "none"} : { textDecoration: "none" }; }'),
            MenuItem(
              "Analysis"
            )
          ),
          br(),
          reactRouter::NavLink(
            to = "/about",
            style = JS('({ isActive }) => { return isActive ? { color: "red", textDecoration: "none" } : { textDecoration: "none" }; }'),
            MenuItem(
              "About"
            )
          )
        )
      ),
      Box(
        reactRouter::Routes(
          reactRouter::Route(
            path = "/",
            element = div(
              tags$h1("Home page"),
              tags$h4("A basic example of reactRouter with shinyMaterialUI."),
              uiOutput(outputId = "contentHome")
            )
          ),
          reactRouter::Route(
            path = "/analysis",
            element = div(
              tags$h1("Analysis"),
              uiOutput(outputId = "contentAnalysis")
              )
          ),
          reactRouter::Route(
            path = "/about",
            element = uiOutput(outputId = "contentAbout")
          ),
          reactRouter::Route(path = "*", element = div(tags$p("Error 404")))
        )
      )
    )
  )
)

server <- function(input, output, session) {
  output$contentHome <- renderUI({
    p("Content home")
  })
  output$contentAnalysis <- renderUI({
    p("Content analysis")
  })
  output$contentAbout <- renderUI({
    p("Content about")
  })
}

shinyApp(ui, server)
```

### Propagation issue

It seems that {shiny} doesn’t show the content of a different route if
the structure of the `element` argument is identical in both routes.
This is an issue if multiple pages share the same HTML structure. More
examples of this issue are given
[here](https://github.com/lgnbhl/reactRouter/issues/1).

A possible workaround, in case the element argument structure is
similar, is to refresh the session each time the user click on a router
link by observing the `NavLink.shinyInput()` input in the session.

``` r
library(shiny)
library(reactRouter)

ui <- HashRouter(
  NavLink.shinyInput(inputId = "linkMain", to = "/", "Main"), br(),
  NavLink.shinyInput(inputId = "linkOther", to = "/other", "Other"),
  Routes(
    Route(
      path = "/", 
      element = div(uiOutput(outputId = "contentMain"))
    ),
    Route(
      path = "/other", 
      element = div(
        # A different HTML structure will make this route visible on user click
        #h1("Other"), # only a different structure makes Shiny execute / render this route
        uiOutput(outputId = "contentOther")
      )
    )
  )
)

# NOTE: please contact me if you know a way to refresh output content without reloading the session.
server <- function(input, output, session) {
  # reload session when user clicks on new page to refresh output content
  # observeEvent(c(input$linkMain, input$linkOther), {
  #   session$reload()
  # })
  output$contentMain <- renderUI( { p("Content home") } )
  output$contentOther <- renderUI( { p("New content") })
}

if (interactive()) {
  shinyApp(ui = ui, server = server)
}
```

Another solution is to render all the UI from the server.

``` r
library(shiny)
library(reactRouter)

ui <- uiOutput(outputId = "main")

server <- function(input, output, session) {
  output$main <- renderUI({
    HashRouter(
      NavLink.shinyInput(inputId = "linkMain", to = "/", "Main"), br(),
      NavLink.shinyInput(inputId = "linkOther", to = "/other", "Other"),
      Routes(
        Route(
          path = "/", 
          element = p("Content home")
        ),
        Route(
          path = "/other", 
          element = div(
            p("New content")
          )
        )
      )
    )
  })
}

if (interactive()) {
  shinyApp(ui = ui, server = server)
}
```

### Usage with Shiny modules

``` r
# adapted from example of shiny.router
# https://github.com/Appsilon/shiny.router/tree/main/examples/shiny_modules
library(shiny)
library(reactRouter)

# This creates UI for each page.
page <- function(title, content, id) {
  ns <- NS(id)
  div(
    titlePanel(title),
    p(content),
    textOutput(ns("click_me"))
  )
}

# Both sample pages.
root_page <- page("Home page", "Home page clicks", "root")
second_page <- page("Second page", "2nd page clicks", "second")

server_module <- function(id, clicks, power = 1) {
  moduleServer(id, function(input, output, session) {
    output$click_me <- renderText({
      as.numeric(clicks())^power
    })
  })
}

# Create output for our router in main UI of Shiny app.
ui <- reactRouter::HashRouter(
  NavLink.shinyInput(inputId = "linkMain", to = "/", "Main"), br(),
  NavLink.shinyInput(inputId = "linkOther", to = "/other", "Other"),
  actionButton("clicks", "Click me!"),
  Routes(
    Route(
      path = "/", 
      element = div(
        root_page
      )
    ),
    Route(
      path = "/other", 
      element = div(
        second_page
      )
    )
  )
)

# Plug router into Shiny server.
server <- function(input, output, session) {
  # reload the session to make the modules work
  # Not desired behavior as clicks object lost
  observeEvent(c(input$linkMain, input$linkOther), {
    session$reload()
  })
  clicks <- reactive({
    input$clicks
  })

  server_module("root", clicks = clicks, power = 1)
  server_module("second", clicks = clicks, power = 2)
}

# Run server in a standard way.
shinyApp(ui, server)
```

### Alternatives

- [shiny.router](https://appsilon.github.io/shiny.router/) implements a
  custom hash routing for shiny.
- [brochure](https://github.com/ColinFay/brochure) provide a mechanism
  for creating natively multi-page shiny applications (but is still
  WIP).

### More information

reactRouter implements React Router
[v.6.30.0](https://reactrouter.com/6.30.0).

More info about how to use React Router can be found in the [official
website](https://reactrouter.com/6.30.0).
