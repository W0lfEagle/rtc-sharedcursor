# rtc-sharedcursor

The `rtc-sharedcursor` module makes it simple to share and touch
mouse events for a target element across the wire one or more
peers across a data channel connection.


[![NPM](https://nodei.co/npm/rtc-sharedcursor.png)](https://nodei.co/npm/rtc-sharedcursor/)

[![experimental](http://hughsk.github.io/stability-badges/dist/experimental.svg)](http://github.com/hughsk/stability-badges)

## Example Usage

```js
var quickconnect = require('rtc-quickconnect');
var sharedcursor = require('rtc-sharedcursor');
var crel = require('crel');
var qc = quickconnect('http://rtc.io/switchboard/');
var cursor = sharedcursor(qc);

// create some test elements
var elements = [
  crel('canvas', { width: 200, height: 200 }),
  crel('canvas', { width: 500, height: 500 }),
  crel('canvas', { width: 20, height: 100 }),
];

// randomly select one of the canvases
var target = elements[(Math.random() * elements.length) | 0];
var context = target.getContext('2d');

// add our test elements to the dom
elements.forEach(function(el) {
  document.body.appendChild(el);
});

// color our target so we know the capture source
context.fillStyle = 'rgb(200, 200, 200)';
context.fillRect(0, 0, target.width, target.height);

// attach the cursor the element now it is in the dom and has
// valid DOM bounds
cursor.attach(target);

cursor.on('data', function(id, type, x, y) {
  // draw on the target
  context.fillStyle = 'rgb(200, 200, 200)';
  context.fillRect(0, 0, target.width, target.height);
  context.fillStyle = type === 'move' ? 'green' : 'red';
  context.fillRect(x - 5, y - 5, 10, 10);
});
```

## License(s)

### Apache 2.0

Copyright 2014 National ICT Australia Limited (NICTA)

   Licensed under the Apache License, Version 2.0 (the "License");
   you may not use this file except in compliance with the License.
   You may obtain a copy of the License at

     http://www.apache.org/licenses/LICENSE-2.0

   Unless required by applicable law or agreed to in writing, software
   distributed under the License is distributed on an "AS IS" BASIS,
   WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
   See the License for the specific language governing permissions and
   limitations under the License.
