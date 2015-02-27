# ofxVideoSlicer
OpenFrameworks addon to slice video files. Threaded concept, using ffmpeg.

Usage
-----

Create first a new  ofxVideoSlicer instance:

    ofxVideoSlicer      ffmpeg;

Start Function:

    ffmpeg.start();
    ffmpeg.start(string path_to_ffmpeg);    // Path with trailing slash required!

Create a slice of a movie clip in the background:

    ffmpeg.addTask(string file, float in_point, int frames_duration);

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
