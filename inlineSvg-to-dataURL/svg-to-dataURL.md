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
A Blob object represents a file-like object of immutable, raw data. Blobs represent data that isn't necessarily in a JavaScript-native format. A Blob is simply a better binary string than String(). a bytearray. no unicode coercion. you can use it to store a mp3 or jpg that won't store correctly in a normal js string. you use FileReader() or URL.createObjectURL() to read the contents, or in web workers, FileReaderSync.
```
let svgBlob = new Blob([svg], { type: 'image/svg+xml' });
```

##### Step 4 - Create a ObjectURL using the Blob
Grab the current URL
```
let domURL:any = self.URL || self || window;
```
Create an URL for the Blob
```
let urlString= domURL. (svgBlob);
```

##### Step 5 - Create a new Image()
```
let image = new Image();
```

##### Step 6 - Create a new Canvas Element
Wait for the image to load.
```
image.onload = () => {
    this.imageReady(image, domURL, urlString, svgElem);
}
```
Inside imageReady f()  create the canvas element
```
let canvas = document.createElement('canvas');
canvas.setAttribute('id', 'pseudo-canvas’);
```
Scale the canvas 
```
canvas.width = svgElem.width.baseVal.value * 2;
canvas.height = svgElem.height.baseVal.value * 2
```

##### Step 7 - Draw the Image on Canvas
Create canvas context
```
let ctx = canvas.getContext('2d’);
```
Clear canvas before drawing the image
```
ctx.clearRect(0, 0, canvas.width, canvas.height);
```
Draw image onto the canvas
```
ctx.drawImage(image, 50, 50, svgElem.width.baseVal.value, svgElem.height.baseVal.value);
```
 Revoke ObjectURL previously created 
 -> this static method releases an existing object URL which was previously created by calling URL.createObjectURL().  Call this method when you've finished using an object URL to let the browser know not to keep the reference to the file any longer. => allows the platform to handle garbage collection at an appropriate time.
 ```
 domURL.revokeObjectURL(urlString);
 ```
 Set the image src to the urlString - after the image.onload f( )
 ```
 image.src = urlString;
 ```

##### Step 8 - Create the Data URL from the canvas object
This method returns a data URI containing a representation of the image in the format specified by the type parameter (defaults to PNG). The returned image is in a resolution of 96 dpi.
```
this.imgData = canvas.toDataURL("image/png", 1.0);
```
