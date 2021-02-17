# FFmpeg-svtav1
Testing FFmpeg build of svtav1/dav1d.

##### create directories

cd /Users/paulduchesne/git/ffmpeg-svtav1/
mkdir build
mkdir source

##### build svtav1
cd source
git clone --depth=1 https://github.com/AOMediaCodec/SVT-AV1
cd SVT-AV1
cd Build
cmake .. -G"Unix Makefiles" -DCMAKE_BUILD_TYPE=Release
make -j
sudo make install
cd ../..

##### build ffmpeg
git clone --depth=1 https://github.com/FFmpeg/FFmpeg ffmpeg
cd ffmpeg
export LD_LIBRARY_PATH+=":/usr/local/lib"
export PKG_CONFIG_PATH+=":/usr/local/lib/pkgconfig"
./configure --prefix=/Users/paulduchesne/git/ffmpeg-svtav1/build/ --enable-libsvtav1 --enable-libdav1d
make -j
sudo make install

##### test
./ffmpeg -f lavfi -i smptebars=duration=10:size=640x360:rate=30 -c:v libsvtav1 -strict -2 /Users/paulduchesne/git/ffmpeg-svtav1/smptebars-svtav1.mp4
./ffplay /Users/paulduchesne/git/ffmpeg-svtav1/smptebars-svtav1.mp4
