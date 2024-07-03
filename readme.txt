1. Start a test server. the file used is loop.mp4, with an RTP server running on port 5000

gst-launch-1.0 -v rtpbin name=rtpbin filesrc location=loop.mp4 ! qtdemux name=demux demux.video_0 ! decodebin ! x264enc ! rtph264pay config-interval=1 pt=96 ! rtpbin.send_rtp_sink_0 rtpbin.send_rtp_src_0 ! udpsink host=127.0.0.1 port=5000 sync=true async=false

2. Start an RTP to HLS converter (gstreamer)

gst-launch-1.0 -v udpsrc port=5000 caps = "application/x-rtp, media=(string)video, clock-rate=(int)90000, encoding-name=(string)H264, payload=(int)96" ! rtph264depay ! h264parse ! mpegtsmux ! hlssink max-files=5 playlist-length=3 target-duration=5 location=hls/segment_%05d.ts playlist-location=hls/playlist.m3u8

3. run "python server.py" in the hls folder, to start the test http server where the hls files are served from. default port is 8000.

4. Open index.html#{HTTP server url here}
If running on localhost, use index.html#http://localhost:8000

This is a demonstration of the project.

To use this in electron, simply copy the js files and copy the source of index.html and use it in your electron app.