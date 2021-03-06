# AudioToolboxWrapper
This wrapper library enables FFmpeg to use AudioToolbox codecs on Windows, with DLLs shipped with iTunes.

i.e. You need to install iTunes, or be able to `LoadLibrary("CoreAudioToolbox.dll")`, for this to work.

## File description
* `bin/atw_ldwrapper`<br>
    replaces linker parameters so we don't need to modify FFmpeg source
* `include`<br>
    header files from [nu774/qaac](https://github.com/nu774/qaac), with some minor changes. Not guranteed to work with other programs.

Files in `bin` and `src` are licensed under WTFPL.


## Example
    # in MSYS2 MinGW shell
    cd path/to/this/repo
    mkdir build
    cd build
    prefix="$PWD/install-prefix"
    cmake -G "Unix Makefiles" -DCMAKE_INSTALL_PREFIX="$prefix" ..
    make install

    # test if we can use the wrapper library, it should not crash
    ./testapp

    mkdir ffmpeg
    cd ffmpeg
    export CFLAGS="-I${prefix}/include" LDFLAGS="-L${prefix}/lib"
    path/to/ffmpeg/source/tree/configure  --enable-audiotoolbox
    make LD="${prefix}/bin/atw_ldwrapper"

    # test if ffmpeg can use it
    ./ffmpeg -f lavfi -i sine=1000 -c aac_at -f mp4 -y NUL
