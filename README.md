#Screensharing.extension.xsockets.webrtc

This repo contains a screensharing Chrome Extension.

##Using the extension 

Where `rtc` is your instance of `XSockets.WebRTC` 

    

       var $ = function(selector,context){
     		if(!context) context = document;
     		return context.querySelector(selector);
        }; 

    if (chrome.runtime) {

    var extensionId = "lelhnjfbpedmhkniggeecbammdhpfeik"; // replace with your extensionId
    var extPort = chrome.runtime.connect(extensionId);

    if (extPort) {

        $("#btn-screenshare").classList.remove("hide");

        // post a message to the extension ; show the dialog 
        $("somebutton").addEventListener("click", function () {
            extPort.postMessage("show", "dialog");
        });

        extPort.onMessage.addListener(function (msg) {

            navigator.webkitGetUserMedia({
                audio: false,
                video: {
                    mandatory: {
                        chromeMediaSource: 'desktop',
                        chromeMediaSourceId: msg.desktopId,
                        minWidth: 1280,
                        maxWidth: 1280,
                        minHeight: 720,
                        maxHeight: 720
                    }
                }
            }, function (stream) {
                rtc.addLocalStream(stream, function (completed) {
                    rtc.getRemotePeers().forEach(function (p) {
                        self.webRTC.refreshStreams(p);
                    });
                });
            }, function (err) {
                console.log(err);
            });
        });
    }
    };


As we call `addLocalStream` the event onLocalStream fill fire.  This piece of code shows how to add a new video element containing the screen/desktop shared using the extension ( choosen by the user )

    
    rtc.onlocalstream = function (stream) {
	    var video = document.createElement("video");
	    video.setAttribute("autoplay", "autoplay");
	    video.setAttribute("muted", "muted");

	    attachMediaStream(video, stream);

	    document.querySelector("#local-streams").appendChild(video);
    };
   
