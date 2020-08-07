![](https://img.shields.io/npm/v/zingchart-angular)
![](https://github.com/zingchart/zingchart-angular/workflows/Build/badge.svg?branch=master)
![](https://github.com/zingchart/zingchart-angular/workflows/Test/badge.svg?branch=master)
![](https://img.shields.io/npm/dw/zingchart-angular)

![](https://img.shields.io/david/zingchart/zingchart-angular)
![](https://img.shields.io/david/peer/zingchart/zingchart-angular)
![](https://img.shields.io/david/dev/zingchart/zingchart-angular)

<iframe
  src="https://codesandbox.io/embed/zingchart-angular-wrapper-example-jm7jb?fontsize=14&hidenavigation=1&theme=dark"
  style="width:100%; height:550px; border:0; border-radius: 4px; overflow:hidden;"
  title="zingchart-react-wrapper-example"
  allow="geolocation; microphone; camera; midi; vr; accelerometer; gyroscope; payment; ambient-light-sensor; encrypted-media; usb"
  sandbox="allow-modals allow-forms allow-popups allow-scripts allow-same-origin"
></iframe>

## Quickstart Guide

Quickly add charts to your Angular application with our ZingChart component

This guide assumes some basic working knowledge of Angular and its Object Oriented interface.

## 1. Install

Install the `zingchart-angular` package via npm

`npm install zingchart-angular`

## 2. Include the `zingchartAngular` module in your project

You can import the module in your module declaration file. This is typically `app.module.ts` for many hello world examples. 


```
import { ZingchartAngularModule } from 'zingchart-angular';

@NgModule({
  imports: [
    ...
    ZingchartAngularModule,
  ],
})
```

## 3. Define ZingChart in your component

The `zingchart/es6` library is a direct dependency of the `ZingchartAngularModule` and you **do not** have to explicitly import the ZingChart library. 

### Default Use Case

The simple use case is defining a config (`zingchart.graphset`) object in your `.component.ts` file:

 
```
import { Component } from '@angular/core';

@Component({
  templateUrl: '...',
  styleUrls: ['...']
})

export class AppComponent {
  config:zingchart.graphset = {
    type: 'line',
    series: [{
      values: [3,6,4,6,4,6,4,6]
    }],
  };
}
```

Then add the `zingchart-angular` tag in your `.component.html` file to tie it all together!

```
  <zingchart-angular [config]="config" [height]="500"></zingchart-angular>
```

### Import ZingChart Modules

You must **EXPLICITLY IMPORT MODULE CHARTS**. There is **NO** default
export objects so just import them.

```js
import { Component } from '@angular/core';
// EXPLICITLY IMPORT MODULE from node_modules
import "zingchart/modules-es6/zingchart-maps.min.js";
import "zingchart/modules-es6/zingchart-maps-usa.min.js";

@Component({
  templateUrl: '...',
  styleUrls: ['...']
})

export class AppComponent {
  config:zingchart.graphset = {
    shapes: [
      {
        type: "zingchart.maps",
        options: {
          name: "usa",
          ignore: ["AK", "HI"]
        }
      }
    ]
  };
}
```

### `zingchart` Global Objects

If you need access to the `window.zingchart` objects for licensing or development flags.

```javascript
import { Component } from '@angular/core';
import zingchart from 'zingchart/es6';

// zingchart object for performance flags
zingchart.DEV.KEEPSOURCE = 0; // prevents lib from storing the original data package
zingchart.DEV.COPYDATA = 0; // prevents lib from creating a copy of the data package 

// ZC object for license key
zingchart.LICENSE = ['your_zingchart_license_key'];
// for enterprise licensing
zingchart.BUILDCODE = ['your_zingchart_license_buildcode'];

@Component({
  templateUrl: '...',
  styleUrls: ['...']
})

export class AppComponent {
  config:zingchart.graphset = {
    type: 'line',
    series: [{
      values: [3,6,4,6,4,6,4,6]
    }],
  };
}
```

## Parameters

### config [object]

The chart configuration (graphset)

```
config:zingchart.graphset = {
  type: 'line',
  series: [{
    values: [3,6,4,6,4,6,4,6]
  }],
};

<zingchart-angular [config]="config" [height]="500"></zingchart-angular>
```

### id [string] (optional)

The id for the DOM element for ZingChart to attach to. If no id is specified, the id will be autogenerated in the form of zingchart-angular-#

### series [array] (optional)

Accepts an array of series objects, and overrides a series if it was supplied into the config object. Varies by chart type used - **Refer to the [ZingChart documentation](https://zingchart.com/docs) for more details.**

```
  series:zingchart.series = {
    values: [3,6,4,6,4,6,4,6]
  }
  config:zingchart.graphset = {
    type: 'line',
  };
    <zingchart-angular [config]="config" [height]="500" [series] = "[series]"></zingchart-angular>
```

### width [string or number] (optional)
The width of the chart. Defaults to 100%

### height [string or number] (optional)
The height of the chart. Defaults to 480px.

### output [string] (optional)

The render type of the chart. **The default is `svg`** but you can also pass the string `canvas` to render the charts in canvas. 

### theme [object] (optional)
The theme or 'defaults' object defined by ZingChart. More information available here: https://www.zingchart.com/docs/api/themes

## Events

All [zingchart events](https://www.zingchart.com/docs/api/events) are readily available on the component to listen to. For example, to listen for the 'complete' event when the chart is finished rendering:

`.component.html` file:

```
<zingchart-angular[config]="config" [height]="300" (node_click)="nodeClick($event)"></zingchart-angular>
```

`.component.ts` file:

```
  export class AppComponent {
    ...
    nodeClick(event) {
      console.log('zingchart node clicked!', event);
    }
  }
```

For a list of all the events that you can listen to, refer to the complete documentation on https://www.zingchart.com/docs/events

## Methods

All [zingchart methods](https://www.zingchart.com/docs/api/methods) are readily available on the component's instance to call. For example, to retrieve data from the chart:

`.component.html` file:

```
<zingchart-angular #chart1 [config]="config"></zingchart-angular></zingchart-angular>
<button "chart1.getdata()">Fetch Data</button>
```

`.component.ts` file:
```

  export class AppComponent {
    ...
    getData() {
      console.log('Fetching zingchart config object', this.chart.getdata());
    }
  }
```

For a list of all the methods that you can call and the parameters each method can take, refer to the complete documentation on https://www.zingchart.com/docs/methods

## Working Example

This repository contains a "Hello world" example to give you an easy way to see the component in action. 

To start the sample application:

```
npm run build && npm run start
```
