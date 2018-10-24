## Angular7 Notes

- To get the latest builds you add a **@next** to the end of the install commands for cli etc... 
But be warned it may break other projects, for instance Node10 requires latest Kha, and that has unstable preview haxe, fun to play with latest but be prepared for googling!

- creating a new project can stall with the installation process so it maybe simpler to break up the process.

```ng new myAppName --skip-install```

then in command line go to the new project folder

```cd myAppName```

then run

```npm install```

- a simple slider from bootstrap4 to adjust properties is currently broken, styling is confusing and maybe hard to setup correctly with SCSS.

- when using canvas, svg or webgl see here:
[teropa on angular graphics ](https://teropa.info/blog/2016/12/12/graphics-in-angular-2.html)
Note using the lifecycle hooks ( they are called automagically in the creation and destruction of your component ) you can setup canvas stuff.
```
ngOnInit() {
  this.running = true;
  this.ngZone.runOutsideAngular(() => this.paint());
}
```
The ngZone stuff is to make sure that Angular does not try to react to your internal requestFrameAnimation and mouse events etc.. 

- To use Haxe inside Angular components is not easy because the default output is incompatible with even if you declare the code as untyped angular js. You will need to rename and restructure and rename stuff not fun.
The following general approach to non typescript/angular code was explained on [stackoverflow](https://stackoverflow.com/questions/37081943/angular2-import-external-js-file-into-component).

So first, simply created a new folder named externalJS on same level as the assets folder. Then copied the 2 following .js files.

    d3.v3.min.js
    d3gauge.js

Then made sure to declare both linked directives in main index.html

```<script src="./externalJS/d3.v3.min.js"></script>
<script src="./externalJS/d3gauge.js"></script>```
```
Then added a similar code in a gauge.component.ts component as followed:
```
import { Component, OnInit } from '@angular/core';

declare var d3gauge:any; <----- !
declare var drawGauge: any; <-----!

@Component({
  selector: 'app-gauge',
  templateUrl: './gauge.component.html'
})

export class GaugeComponent implements OnInit {
   constructor() { }

   ngOnInit() {
      this.createD3Gauge();
   }

   createD3Gauge() { 
      let gauges = []
      document.addEventListener("DOMContentLoaded", function (event) {      
      let opt = {
         gaugeRadius: 160,
         minVal: 0,
         maxVal: 100,
         needleVal: Math.round(30),
         tickSpaceMinVal: 1,
         tickSpaceMajVal: 10,
         divID: "gaugeBox",
         gaugeUnits: "%"
    } 

    gauges[0] = new drawGauge(opt);
    });
 }

}
```
and finally, I simply added a div in corresponding gauge.component.html
```
<div id="gaugeBox"></div>
```
- Component Lifecyle hooks ( preface **'ng'** omitted for my clarity ) my interpretaion subject to change.
  * **OnChanges** before onInit or when bound data changes.
  * **OnInit** when component is created
  * **DoCheck** called even if angular does not really care to do anything? and after OnInit. Examples needed
  * **AfterContentInit** before creation of content
  * **AfterContentChecked** once date is parsed properly
  * **AfterViewInit** when sub components are created
  * **AfterViewChecked** when sub components finished
  * **OnDestroy** just before component is removed
  
![](https://stepbystepschools.net/wp-content/uploads/2016/11/DOM.png)

Source ( https://stepbystepschools.net/?p=1088 purely included for personal educational use ).

