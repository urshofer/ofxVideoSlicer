# ofxVideoSlicer
OpenFrameworks addon to slice video files. Threaded concept, using ffmpeg.

Prerequisites
-------------

ofxVideoSlicer needs the ffmpeg binary to process videos. It is only tested on OSX, but should run on linux as well. On OSX, ofxVideoSlicer looks for the ffmpeg binary in the __resources path__ of the app bundle by default. Like this, you can bundle ffmpeg in your XCode Project by adding a "Copy File" build phase.

Frame Accuracy
--------------

Although ofxVideoSlicer works quite accurate, absolute frame accuracy can not be guaranteed with all input formats. It performes very well with __prores__ encoded source material, since it is a format specially designed for video editing.

Usage
-----

Create first a new  ofxVideoSlicer instance:

    ofxVideoSlicer      ffmpeg;

Start Function:

    ffmpeg.start();
    ffmpeg.start(string path_to_ffmpeg);    // Path with trailing slash required!

Create a slice of a movie clip in the background. The message string is passed to the onFileProcessed event:

    ffmpeg.addTask(string file, float in_point, int frames_duration, string message);

Check how many clips are pending in the queue:

	int pending = ffmpeg.processingQueueSize();

Please note:

- file is a string containing the __absolute__ path to the movie file.
- in_point is a float variable in split-seconds. For example: 23.03
- frames_duration is a integer for the number of frames you want to slice.

Resulting movie clips are stored in the same folder as the original. They are named after this pattern:

    [in]_[duration]_[movie].ext

For example:

    22.3_337_clip.mp4
    
Options
-------

Transcode:

    ffmpeg.setTranscode(bool _switch = true)

Enables or disables Transcoding. If transcoding is disabled, the output clips will be rendered with the same audio and video codec as the input. To achieve frame accuracy, enable transcoding. ffmpeg can only Cut on i-frames.

Scaling:

    ffmpeg.setScale(bool _switch = true)

Enables or disables Scaling    

Codec:

    ffmpeg.setCodec(string _codec)  // Codec needs to be set to "h264" or "prores"

Sets the transcode codec. Currently only h264 or prores are supported.

Bitrate:

    ffmpeg.setBitrate(int _rate)

Set Bitrate of Mp4 Encoding (default: 500)

Width:

    ffmpeg.setWidth(int _width)

Set Width of scaled output (default: 640). Height is set automatically according to the original aspect ratio.

Events
------

ofxVideoSlicer fires an end Event after succesful processing. You can register it like this:
	
	ofEvent<endEvent> onFileProcessed;

In you application, register the event like (assuming your instance of the add-on is called ffmpeg):

    ofAddListener(ffmpeg.onFileProcessed,this, &ofApp::onFileProcessed);

The callback function receives a endEvent struct as parameter:

	void ofApp::onFileProcessed(ofxVideoSlicer::endEvent & ev) {
    	cout << ev.file << " processed with the message " << ev.message;
	}
	
As you can see, endEvent contains 3 strings, the filename of the clip and the screenshot and the message you passed on the beginning:

  struct endEvent {
    string file;
    string jpg;
    string message;
  };
    
You can use the message for various purposes. One is to pass metadata about the movie to the slicer and
upload it afterwards together with the movie clip to a web server.
