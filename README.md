# NGX-Leaflet-Fullscreen
This fork is a workaround that fixes build errors in Angular.

This is a wrapper for the [leaflet-fullscreen control](https://github.com/Leaflet/Leaflet.fullscreen) to make it easy to use in Angular 8+.

This wrapper is tested against the [@asymmetrik/ngx-leaflet](https://github.com/Asymmetrik/ngx-leaflet) library but it has no dependency on that library so *should* work without it. It does, obviously, have a dependency that leaflet is loaded.

For more detailed descriptions of how this wrapper was created : [Documentation](https://runette.gitbook.io/alcm/).

For detailed descriptions of how to use and worked examples : [Article](https://medium.com/runic-software/quick-guide-to-leaflet-controls-in-angular-io-1b35d0807bdb), [Article](https://medium.com/runic-software/advanced-interactive-maps-in-angular-with-leaflet-68baafa03f72)

For an example of this working in a real site - see [trackbash](https://trackbash.co.uk).

# Install

Install using npm:

```
npm install @kpaterakis/ngx-leaflet-fullscreen
```

>
> Note : this package currently installs a _fork_ of leaflet-fullscreen : [@kpaterakis/leaflet-fullscreen](https://github.com/runette/Leaflet.fullscreen).
>
>This is because the root repo has not been updated with several important PRs and has not been changed for a year.
# Usage

This library needs to be imported into the application module:

```ts
import {NgxLeafletFullscreenModule} from '@kpaterakis/ngx-leaflet-fullscreen'

imports: [
    ...
    NgxLeafletFullscreenModule,
  ],
```

Then, the control is inserted using the following directive:

```html
<leaflet-fullscreen-control
    [map]="..."
    [options]="..."
    ></leaflet-fullscreen-control>
```

Where `map` is an instance of a leaflet map and `options` is an object with valid options for the control.

# Usage with NGX-Leaflet

This library integrates very easily with ngx-leaflet using the onMapReady event:

```html
<div id='map' class="map-container" leaflet
     [leafletOptions]="options"
     (leafletMapReady)="onMapReady($event)"
     ></div>
<leaflet-fullscreen-control
    [map]="map"
    [options]="fullscreenOptions"
    ></leaflet-fullscreen-control>
```
by adding the following to your map component:

```ts
...
import { Map } from 'leaflet';


export class OsmMapComponent implements OnInit, OnDestroy {
  public map: Map;
  public fullscreenOptions: {[key:string]:any} = {
    position: 'topleft',
    title: 'View Fullscreen',
    titleCancel: 'Exit Fullscreen',
  };

  ...

  onMapReady(map: Map) {
    this.map = map;
  }
```

# Usage - CSS

Unfortunately - I think because the leaflet map is run outside of Angular by ngx-leaflet - the normal css encapsulation does not work and you have to load the css globally.

Add the following to the styles.css

```CSS
@import "@kpaterakis/leaflet-fullscreen/dist/leaflet.fullscreen.css";
```

# Build Config

For some reason yet to be found - this library does not like being built with `"buildOptimizer": true,` in the build environment - which is usually the default for the production environment in `angular.json`.

Always build with `"buildOptimizer": false,`.


# API Access to the Control

If you want access the control's methods directly from your typescript code - this can be done with `@ViewChild`

Use `ViewChild` to access the component, for instance 

```ts
@ViewChild(FullscreenControlComponent,{static: false}) fullscreenComponent: FullscreenControlComponent;
```

The actual instance of the control can then be accessed directly as `this.fullscreenComponent.control`

For more details and worked examples, see : [Article](https://medium.com/runic-software/advanced-interactive-maps-in-angular-with-leaflet-68baafa03f72)
