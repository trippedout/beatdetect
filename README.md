beatdetect
==========

Stupid simple port of Minim for Processing's Beat Detector


install
-------

i like submodules so go into your apps 'js' folder:

```
git submodule add https://github.com/trippedout/beatdetect.git
```

then add to app

```
<script type="text/javascript" src="js/beatdetect/fft.js"></script>
<script type="text/javascript" src="js/beatdetect/beatdetect.js"></script>
```

usage
-----

meant to be used with the an AudioContext in browsers that support it.

```
window.AudioContext = window.AudioContext || window.webkitAudioContext;
var audioContext = new AudioContext();
var sampleRate = audioContext.sampleRate;

// if you create an analyser node with fft size of 2048, your bin count
// will be half that when you get the bin count (1024)

var analyser = audioContext.createAnalyser();
analyser.fftSize = 2048;

var beatdetect = new FFT.BeatDetect(analyser.frequencyBinCount, sampleRate);


// then inside your onaudioprocess loop you need to grab the
// float time domain data, as opposed to most libs that use 
// byte freq data or float freq data
javascriptNode = audioContext.createScriptProcessor(1024, 1, 1);        
javascriptNode.onaudioprocess = function() 
{
	var floats = new Float32Array(analyser.frequencyBinCount);
	analyser.getFloatTimeDomainData(floats);
	
	beatdetect.detect(floats);

	if(beatdetect.isKick() ) console.log("isKick()");
	if(beatdetect.isSnare() ) console.log("isSnare()");
}
```

implementation notes
--------------------

disclaimer - i don't write js code day to day (android and c++) so this is garbage. it (surprisingly) does
pretty much what i want it to do (detect beats, duh), but i can definitely use some help. this was thrown
together for a two week (more like 3 day) [project for mashable](https://github.com/trippedout/rackcity), and thus, needs work.

as noted in the original docs - https://github.com/ddf/Minim/blob/master/src/ddf/minim/analysis/BeatDetect.java 
this library is intended for use with more electronic/techno/dance music and might not work as well with super loud
rock and roll/etc.

using ```analyser.smoothingTimeConstant = 0.85;``` seems to help with the output, so experiment with values here

there is also a sensitivity variable in beatdetect that i default to 300 - varying this number helps as well, especially making it larger
if you have too many beats being detected in short time spans.

i'd also love if someone could school me on bower/npm and get this setup for proper usage in apps since not everyone loves submodules

thanks
======

obviously none of this is possible without the great work of the guys at http://code.compartmental.net/ (ddf and co)

