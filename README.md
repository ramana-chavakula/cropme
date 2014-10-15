cropme Angular Module
========================

Drag and drop or select an image, crop it and get the blob, that you can use to upload wherever and however you want

Install
-------

Copy the cropme.js and cropme.css files into your project and add the following line with the correct path:

		<script src="/path/to/scripts/cropme.js"></script>
		<link rel="stylesheet" href="/path/to/scripts/cropme.css">


Alternatively, if you're using bower, you can add this to your component.json (or bower.json):

		"angular-cropme": "^0.2.6"

Or simply run

		bower install angular-cropme

Check the dependencies to your html (unless you're using wiredep):

		<script src="components/angular/angular.js"></script>
		<script src="components/angular-sanitize/angular-sanitize.js"></script>
		<script src="components/angular-touch/angular-touch.js"></script>
		<script src="components/angular-superswipe/superswipe.js"></script>

And (unless you're using wiredep):

		<script src="components/angular-cropme/cropme.js"></script>

And the css:

		<link rel="stylesheet" href="components/angular-cropme/cropme.css">


Usage
-----
		<cropme
			width="640"
			height="400"
			ratio="1"
			icon-class=""
			type="png"
			destination-width="300">
		</cropme>

- width: (optional) width of the container. The image you want to crop will be reduced to this width. Omit the width to make the box fit the size of the parent container
- height: (optional) height of the container. Default is 300
- icon-class: (optional) css class of the icon to be set in the middle of the drop box
- type: (optional) png or jpeg (might work with webm too, haven't tried it)
- ratio: (optional) destination-height = ratio x destination-width. So you can either define ratio, or add a destination-height parameter, or none.
- destination-width: (optional) target (cropped) picture width
- destination-height: (optional) target (cropped) picture height. Cannot be set if ratio is set.
- src: (optional) url of the image to preload (skips file selection)
- send-original: (default: false) If you want to send the original file
- send-cropped: (default: true) If you want to send the cropped image

And don't forget to add the module to your application

		angular.module("myApp", ["cropme"])

You can choose to hide the default ok and cancel buttons by adding this to your css

		#cropme-cancel, #cropme-ok { display: none; }

Events Sent
----------

The blob will be sent through an event, to catch it inside your app, you can do like this:

		$scope.$on("cropme:done", function(e, result, canvasEl) { /* do something */ });

Where result is

		x: x position of the crop image relative to the original image
		y: y position of the crop image relative to the original image
		height: height of the crop image
		width: width of the crop image
		croppedImage: crop image as a blob
		originalImage: original image as a blob

Also cropme will send an event when a picture has been chosen by the user, so you can do something like

		$scope.$on("cropme:loaded", function(width, height) { /* do something when the image is loaded */ });

Events Received
---------------

And you can trigger ok and cancel action by broadcasting the events cropme:cancel and cropme:ok, for example:

		$scope.$broadcast("cropme:cancel");

So, now, how do I send this image to my server?
-----------------------------------------------

		scope.$on("cropme:done", function(e, blob) {
			var xhr = new XMLHttpRequest;
			xhr.setRequestHeader("Content-Type", blob.type);
			xhr.onreadystatechange = function(e) {
				if (this.readyState === 4 && this.status === 200) {
					return console.log("done");
				} else if (this.readyState === 4 && this.status !== 200) {
					return console.log("failed");
				}
			};
			xhr.open("POST", url, true);
			xhr.send(blob);
		});


[Demo here!](http://standupweb.net/cropmedemo)


