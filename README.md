# ofxVideoSlicer
OpenFrameworks addon to slice video files. Threaded concept, using ffmpeg.

Usage
-----

Create first a new  ofxVideoSlicer instance:

    ofxVideoSlicer      ffmpeg;

Start Function:

    ffmpeg.start();

Create a slice of a movie clip in the background:

    ffmpeg.addTask(string file, float in_point, int frames_duration);

Please note:

- file is a string containing the __absolute__ path to the movie file.
- in_point is a float variable in split-seconds. For example: 23.03
- frames_duration is a integer for the number of frames you want to slice.

Resulting movie clips are stored in the same folder as the original. They are named after this pattern:
    [in]_[duration]_[movie].ext
