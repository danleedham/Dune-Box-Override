# Dune Box Override Information 
## Background

The Dune HD TV 102 (colloquially 'Dune Box') is a IPTV/VOD/OTT media player that has the ability to be controlled using a simple 'IP Control' feature. The feature allows a user to send HTTP commands to the player. Commands can be used to request various tasks such as:
* Start playback of the given media content
* Control the playback
* Set the behaviour at the end of media playback
* Report current player status
* Stop playback

By running the command in a web browser's URL bar (or other HTTP submission) we can control the behaviour of the Dune Box. 

## Playback Override
A simple web-application was written with two key functions:
* Load a specified stream
* Allow the stream to be ended manually and the playback set to black

![Dune Box Override User](https://image.ibb.co/focKac/dunebox.png "Dune Box Override User")

### Choosing which Dune Box
The application allows control of both the NAfW and Scottish Parl Dune Boxes. This was done using a select with the id ```location```. 

```html
<select id="location">
<option value="http://192.168.1.89">Wales Box</option>
<option value="http://192.168.1.64">Scotland Box</option>
</select>
```

### Entering a custom stream
The application requests a cusom stream URL that the Dune Box will attempt to play. This was done using a HTML ```input``` with the id ```streamUrl```. Please see [Dune Support](http://dune-hd.com/support/misc/media_url.txt) for a list of formats and protocols supported by the Dune Boxes. 

```html
 <input class="..." id="streamUrl" type="url">
```
An example URL would be: ```http://nafw-live.hls.adaptive.level3.net/d/933736c8-8d1f-4f08-bf53-3efe2cfaf030/interpretation/interpretation.isml/interpretation.m3u8```

### Starting a stream
The button to start a stream on the Dune Box is a simple HTML link, styled as a button, that fires the ```loadStream``` function.

```html
<a class="..." href="#" role="button" onclick="loadStream();return false;">Start stream</a> 
```

The ```loadStream``` JavaScript function collects the parts of the HTTP command (pretty much just building a URL) and then to run the command opens a new browser window with that command as the address. 

```javascript
function loadStream() {
    var location = document.getElementById("location").value;
    var playMediaUrl = "/cgi-bin/do?cmd=start_file_playback&media_url=";
    var streamUrl = document.getElementById("streamUrl").value;
    var fullUrl = location+playMediaUrl+streamUrl;
    window.open(fullUrl);    
}
```

The ```location``` variable pulls the correct IP address of the selected dune box such as  ```http://192.168.1.89```. The variable ```playMediaUrl``` is the specific portion of the command which tells the Dune Box to start playback of the media URL that we'll pass in the variable ```streamUrl``` which is set by the user. The full command will be something like:

```http://192.168.1.89/cgi-bin/do?cmd=start_file_playback&media_url=http://nafw-live.hls.adaptive.level3.net/d/933736c8-8d1f-4f08-bf53-3efe2cfaf030/interpretation/interpretation.isml/interpretation.m3u8```

### Setting the box to show a black screen
The button to make the Dune Box end a current stream and set output to black is a simple HTML link, styled as a button, that fires the ```blackScreen``` function.

The ```blackScreen``` JavaScript function HTTP command (pretty much just a URL) and then to run the command opens a new browser window with that command as the address. 

```javascript
function blackScreen() {
    var blackScreenUrl = "/cgi-bin/do?cmd=black_screen";
    var location = document.getElementById("location").value;
    window.open(location+blackScreenUrl);    
}
```

The ```location``` variable pulls the correct IP address of the selected dune box such as  ```http://192.168.1.89```. The full command will be something like: ```http://192.168.1.89/cgi-bin/do?cmd=black_screen``` which will set the NAfW Dune Box to output black.