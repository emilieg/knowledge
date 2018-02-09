# How to convert an inline SVG to a Data URL 
### *What problem does it solve?*
Dynamically generated SVG graph by the D3.JS library needs to be exported in the PDF format using the [jsPDF library](https://parall.ax/products/jspdf). The format that the jsPDF library requires to export an image is “DataURL”. 

The Data URL looks something like this (the string might be much longer):
```
var imgData = 'data:image/jpeg;base64,/9j/4AAQSkZJRgABAQEASABIAAD/2wBDAAIBAQEBAQIBAQECAgICAgQDAgICAgUEBAMEBgUGBgYFBgYGBw’;
```
### Steps: 
1. Grab SVG Element from the DOM
2. Convert SVG element to a string
3. Convert SVG string to a Blob
4. Create a ObjectURL using the Blob
5. Create a new Image()
6. Create a new Canvas Element
7. Draw the Image on Canvas
8. Create the Data URL from the canvas object

##### Step 1 - Select SVG from DOM
Grab the container - the SVG is actually the parent of the container. Depending if you have an ID on the actual SVG element - in this case I did not. 
```
let el = document.getElementById('main-container');
let parent = el.parentElement;
```

##### Step 2 - Convert SVG element to a string
XMLSerializer can be used to convert a DOM subtree or DOM document into text. 
```
let svg = new XMLSerializer().serializeToString(svgElem);
```

##### Step 3 - Convert SVG string to a Blob
A Blob object represents a file-like object of immutable, raw data. Blobs represent data that isn't necessarily in a JavaScript-native format. 
```
let svgBlob = new Blob([svg], { type: 'image/svg+xml' });
```
