# Map-Data-Across-Globe

*Map of Meteorite data*

This is an app that maps meteorite data across the globe. Information about the meteorites can be viewed when hovering over one of the impact spots (i.e. dots) on the map (the size of the dot correlates to meteorite size).


...

**Home Page**

<img src="/MapDataAcrossGlobe.PNG" title="home page" alt="home page" width="500px">



---


## Table of Contents 

> Sections
- [Sample Code](#Sample_Code)
- [Installation](#installation)
- [Features](#features)
- [Contributing](#contributing)
- [Team](#team)
- [FAQ](#faq)
- [Support](#support)
- [License](#license)


---

## Sample Code

```javascript
// code

document.addEventListener('DOMContentLoaded', function(){
  req= new XMLHttpRequest();
  req.open('GET','https://raw.githubusercontent.com/freeCodeCamp/ProjectReferenceData/master/meteorite-strike-data.json',true);
  req.send();
  req.onload= function(){
    data=JSON.parse(req.responseText);

var width = '100%' ; var height = '100%';
var col = 0; var obj = {};
var meteorites;

var projection = d3.geo.mercator()
  .translate([800,370])
  .scale(300);

var zoom = d3.behavior.zoom()
  .translate([0, 0])
  .scale(1)
  .on("zoom", zoom);

var path = d3.geo.path()
  .projection(projection);

var svg = d3.select('#in')
  .append('svg')
  .attr('width', '100%')

svg.append('rect')
  .attr('width', width)
  .attr('height', height)
  .attr('fill', '#caecf7')
  .call(zoom)

d3.select(window).on("resize", resizeMap);

var tooltip = d3.select("body")
  .append('div')
  .attr('class', 'tooltip')
  .style('opacity', 0);

var map = svg.append('g');

d3.json('https://d3js.org/world-50m.v1.json', function(json) {
  map.selectAll('path')
    .data(topojson.feature(json, json.objects.countries).features)
    .enter()
    .append('path')
    .attr('fill', '#caf7d2')
    .attr('stroke', 'black')
    .attr('d', path)
    .call(zoom)
});

d3.json('https://raw.githubusercontent.com/freeCodeCamp/ProjectReferenceData/master/meteorite-strike-data.json', function(json) {
  json.features.sort(function(a,b) {
    return new Date(a.properties.year) - new Date(b.properties.year);
  })
  json.features.map(function(e) {
    col+=.45;
    obj[e.properties.year] = col;
    e.color = 'hsl(' + col + ',100%, 50%)';
  })

  json.features.sort(function(a,b) {
    return b.properties.mass - a.properties.mass
  })

  meteorites = svg.append('g')
      .selectAll('path')
      .data(json.features)
      .enter()
      .append('circle')
      .attr('cx', function(d) { return projection([d.properties.reclong,d.properties.reclat])[0] })
      .attr('cy', function(d) { return projection([d.properties.reclong,d.properties.reclat])[1] })
      .attr('r', function(d) { 
        var range = 180000;
    
        if (d.properties.mass <= range){
          return 3;
        }
        else if (d.properties.mass <= range*2){
          return 15;
        }
        else if (d.properties.mass <= range*3){
          return 30;
        }
        else if (d.properties.mass <= range*20){
          return 40;
        }
        else if (d.properties.mass <= range*100){
          return 60;
        }
        else{
          return 100;
        }        
      })
      .attr('fill-opacity', function(d) {
        var range = 180000;
        if (d.properties.mass <= range){
          return 0.75;
        }
        else{
          return 0.25;
        }
      })
      .attr('fill', function(d) {
          return d.color
       })
  
      .on('mouseover', function(d) {
        tooltip.transition()
          .duration(200)
          .style('opacity', .9);
        tooltip.html( 'Name: ' + d.properties.name + '<br>' + 
                  'Mass: ' + d.properties.mass + '<br>' + 
                  'Recclass: ' + d.properties.recclass + '<br>' + 
                  'Reclat: ' + d.properties.reclat + '<br>' + 
                  'Year: ' + (d.properties.year) + '<br>')
          .style('left', (d3.event.pageX+5) + 'px')
          .style('top', (d3.event.pageY+5) + 'px')
      })
  
      .on('mouseout', function(d) {
        tooltip.transition()
          .duration(500)
          .style('opacity', 0);
      });
  
  resizeMap();
});
    

function zoom() {
 // map.attr("transform", "translate(" + d3.event.translate + ")scale(" + d3.event.scale + ")");
 meteorites.attr('transform', 'translate(' + d3.event.translate + ')scale(' + d3.event.scale + ')');
 map.attr("transform", "translate(" + d3.event.translate + ")scale(" + d3.event.scale + ")");  
}


function resizeMap() {
 // d3.selectAll("g").attr("transform", "scale(" + $("#in").width()/2000 + ")");
//  $("svg").height($("#in").width()/2);
  svg.attr('width', '100vw')
  svg.attr('height', '90vh')
}    
    
    
  
  }
})
```

---

## Installation
## Features
## Usage (Optional)
## Documentation (Optional)
## Tests (Optional)
## Contributing
## Team

> Contributors/People

| [**seansangh**](https://github.com/seansangh) |
| :---: |
| [![seansangh](https://avatars0.githubusercontent.com/u/45724640?v=3&s=200)](https://github.com/seansangh)    |
| [`github.com/seansangh`](https://github.com/seansangh) | 

-  GitHub user profile

---

## FAQ

- **Have any *specific* questions?**
    - Use the information provided under *Support* for answers

---

## Support

Reach out to me at one of the following places!

- Twitter at [`@sean13nay`](https://twitter.com/sean13nay?lang=en)
- Github at [`seansangh`](https://github.com/seansangh)

---

## Donations (Optional)

- If you appreciate the code provided herein, feel free to donate to the author via [Paypal](https://www.paypal.com/cgi-bin/webscr?cmd=_s-xclick&hosted_button_id=4VED5H2K8Z4TU&source=url).

[<img src="https://www.paypalobjects.com/webstatic/en_US/i/buttons/cc-badges-ppppcmcvdam.png" alt="Pay with PayPal, PayPal Credit or any major credit card" />](https://www.paypal.com/cgi-bin/webscr?cmd=_s-xclick&hosted_button_id=4VED5H2K8Z4TU&source=url)

---

## License

[![License](http://img.shields.io/:license-mit-blue.svg?style=flat-square)](http://badges.mit-license.org)

- **[MIT license](http://opensource.org/licenses/mit-license.php)**
- Copyright 2019 © <a>S.S.</a>
