# Convert Agora C++ API to Python API on Win-amd64
Wrap Agora C++ API v2.4.1 to Python API on Win-amd64 platform.

## Requirements
- Windows 10
- Microsoft Visual C++ >= 11.0

## Download
Download Clang 3.7 from [here](http://releases.llvm.org/download.html) and put `libclang.dll` to the root directory.

## Install Python Packages
```bash
pip install Cython clang==3.7
```

## Generate Cython Codes
Firstly, to generate Cython codes from `IAgoraRtcEngine.h`, run
```bash
python C2py.py --head_path='include/IAgoraRtcEngine.h' --callback_wrapper='EventHandlerWrapper.h' --target_file_name='IAgoraRtcEngine'  --mode='new'
```
Then, to add Cython codes from `IAgoraMediaEngine.h`, run
```bash
python C2py.py --head_path='include/IAgoraMediaEngine.h' --callback_wrapper='VideoFrameObserverWrapper.h' --target_file_name='IAgoraRtcEngine'  --mode='add'  --extern_pxd='extern_pxd.txt'  --extern_pyx='extern_pyx.txt'
```
After running the above two commands, two files are generated: `IAgoraRtcEngine.pxd` and `IAgoraRtcEngine_pyx.pyx`. At this stage, the Cython codes can be compiled successfully. However, to make the extension work perfectly for Agora API, some neccessary modifications should be made based on `modify.txt`. The `IAgoraRtcEngine.pxd` and `IAgoraRtcEngine_pyx.pyx` files which have been modified are provided here.

## Build Python Extension
Run:
```bash
python setup.py build_ext --compiler=msvc 
```
This will generate Python extension `.pyd`. Don't forget to put `agora_rtc_sdk.dll` to the same path of `.pyd` file when using the generated Python API.
