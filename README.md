# Overview

`redux-place-manager` is a [Redux](http://redux.js.org/)-friendly router / history manager that uses the "Place" abstraction made popular in web application development by [GWT](gwtproject.org) and the related [GWTP](https://github.com/ArcBees/GWTP) framework.

A `Place` is effectively a *permanent (deep) link* in the application. That is, it has a (canonical) URL. The library
enforces a specific sequence of steps in transitioning from one Place to another, so that:
* the current Place has a chance to confirm transition (e.g. if user is editing a form)
* authentication and authorisation checks are undertaken before transitioning
* data can be loaded prior to rendering (including a check for PlaceNotFound)


# Redux

In the [Redux](http://redux.js.org/) application architecture, (all) *application* state:
* is managed in a single *store*
* mutated through a sequence of *actions* (similar to events or commands)
* where the store is the *result* of applying a function `reduce(previousState, action) => newState` to each of the actions.

## Redux modularisation

We follow the [ducks](https://github.com/erikras/ducks-modular-redux) approach to modularisation in Redux, i.e.

1. MUST export default a function called reducer()
2. MUST export its action creators as functions
3. MUST have action types in the form npm-module-or-app/reducer/ACTION_TYPE
4. MAY export its action types as UPPER_SNAKE_CASE, if an external reducer needs to listen for them, or if it is a published reusable library


# Redux and Application Place Management

## Actions

The following actions can occur for URL changes:
* TRANSITION_REQUEST:
* TRANSITION_SUCCESS:
* TRANSITION_FAILED

`requestTransition({nextPlaceType, params})` is an action creator that can be invoked by application code. The PlaceManager is responsible for dispatching TRANSITION_SUCCESS or TRANSITION_FAILED to the Store.


## Application Place state shape

    reduxPlaceManager: {
        placeType: "...",
        placeParams: { /* ... */ }

##


## Universal (isomorphic) application considerations

In a universal (isomorphic) application, the server renders the initial page (completely). Subsequently, the client
responds to all state transactions (i.e. actions in Redux).


## Actions


# PlaceManager concepts

* PlaceType
* Place



# TBC - notes

The `PlaceManager` is responsible for wiring all this together. An example of the basic flow is as follows:

1. Browser URL event occurs (link clicked, `popState`, `pushState`)
2. `PlaceManager` maps URL to a `PlaceType`, or an error (~ HTTP 404)
3. PlaceType creates new Place as its factory
4. PlaceManager checks Place exists/accessible, or an error (~ HTTP 404, 401)
5. PlaceManager replaces window.location URL (if necessary) to "canonical" URL
6. Place is *rendered*

`PlaceManager` is a *singleton* service that is responsible for:

* turning browser URL events into loading the correct page components
* creating hyperlinks (i.e. a.href) at the request of other components
* handling place *transitions* (including user confirmation)
* enforcing enter conditions (e.g. security profile)
