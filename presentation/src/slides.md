<h1 class="r-fit-text">Applications of Geospatial Data in</br> Digital Communication</h1>

William Svoboda '22

Adviser: Michael Freedman

---

## Motivation

<ul>
    <li class="fragment">Digital communication is important</li>
    <ul>
        <li class="fragment">Over <u>4.5 billion</u> social media users worldwide</li>
    </ul>
</ul>

---

## What's the problem?

<ul>
    <li class="fragment">We live in the real world!</li>
    <li class="fragment">Even though 99% of social media is through mobile devices&hellip;</li>
    <li class="fragment">Digital communication is mostly independent of the physical space around us</li>
</ul>

---

<!-- .slide: data-auto-animate -->

## My hypothesis?

---

<!-- .slide: data-auto-animate -->

## My hypothesis?

_Tying digital interaction to physical proximity increases user engagement_

---

<!-- .slide: data-auto-animate -->

## My hypothesis?

_Tying digital interaction to physical proximity increases user engagement_

## The goal?

---

<!-- .slide: data-auto-animate -->

## My hypothesis?

_Tying digital interaction to physical proximity increases user engagement_

## The goal?

_A social app that gates messaging to proximity_

---

## Background and related work

---

## Geospatial data

<ul>
    <li class="fragment">Data related to a specific geographic position</li>
    <li class="fragment">Combines location, attribute, and temporal information</li>
</ul>
<img class="r-stretch" src="./static/images/earth.webp">

---

## In social media

<div class="r-stack">
    <img class="fragment" src="./static/images/instagram.webp" height="525">
    <img class="fragment" src="./static/images/snapchat.webp" height="525">
    <img class="fragment" src="./static/images/yikyak.webp" height="525">
</div>

---

## Approach

---

## Key features

<ol>
    <li class="fragment">Main screen showing the user’s location and surrounding area</li>
    <li class="fragment">Ability to leave ephemeral messages at one’s location, visibly indicated to all users</li>
    <li class="fragment">Messages can only be read near coordinates where they were posted</li>
</ol>

---

<!-- .slide: data-auto-animate -->

## Why?

---

<!-- .slide: data-auto-animate -->

## Why?

_Messages are both spatially and temporally relevant!_

---

## Implementation

---

## Project architecture

<ul>
    <li class="fragment">Client-server model, client is iPhone app</li>
    <ul>
        <li class="fragment">Access to Apple's <code>MapKit</code> and <code>SwiftUI</code> frameworks</li>
    </ul>
    <li class="fragment">Client handles all user-facing tasks</li>
    <li class="fragment">Server handles content, authentication, geospatial queries, and other centralized functionality</li>
</ul>

---

<!-- .slide: data-auto-animate -->

## From design&hellip;

<img class="r-stretch" src="./static/images/wireframe.webp">

---

<!-- .slide: data-auto-animate -->

## &hellip;to implementation

<img class="r-stretch" src="./static/images/screenshot.webp">

---

## Client details

<ul>
    <li class="fragment"><code>SwiftUI</code> is a declarative framework</li>
    <ul>
        <li class="fragment">Describe interface you want, layout engine handles the details!</li>
        <li class="fragment">Separates presentation from business logic</li>
    </ul>
    <li class="fragment">Good fit for MVVM (Model–view–viewmodel) design pattern</li>
</ul>

---

## Shared services

- Wrote _managers_ to represent a shared service layer, injected at startup:

<pre><code data-line-numbers="*|6-9|10-15|17-25">
import SwiftUI

@main
struct Geo: App {

    let settingsManager: SettingsManager
    let authenticationManager: AuthenticationManager
    let locationManager: LocationManager

    init() {
        self.settingsManager = SettingsManager()
        self.authenticationManager = AuthenticationManager(settingsManager: settingsManager)
        self.locationManager = LocationManager(settingsManager: settingsManager)
    }

    // Inject required services after initializing the app
    var body: some Scene {
        WindowGroup {
            ContentView()
                .environmentObject(settingsManager)
                .environmentObject(authenticationManager)
                .environmentObject(locationManager)
        }
    }
}
</code></pre>

---

## Server details

<ul>
    <li class="fragment">Containerized application using Docker</li>
    <li class="fragment">Redis as database</li>
    <ul>
        <li class="fragment">Simple and fast ($\mathcal{O}(N+\log{M})$) geospatial queries</li>
    </ul>
    <li class="fragment"><code>FastAPI</code> as webserver</li>
    <ul>
        <li class="fragment">Entire server is $< 200$ LOC</li>
    </ul>
</ul>

---

## Demonstration
