A version of RTKLIB optimized for low cost GNSS receivers (single, dual, or triple frequency), especially u-blox receivers.  It is based on RTKLIB 2.4.3 and is kept reasonably closely synced to that branch.   This software is provided “AS IS” without any warranties of any kind so please be careful, especially if using it in any kind of real-time application. 

Releases and pre-releases for Windows executables are available at https://github.com/rtklibexplorer/RTKLIB/releases 

Tutorials for this code, and sample GPS data sets are available at http://rtkexplorer.com/  

The latest version of the user manual is at: https://rtkexplorer.com/pdfs/manual_demo5.pdf

   
WINDOWS: To build and install code for with Windows Embarcadero compiler:

GUIs: 
1) Build executables with app/winapp/rtklib_winapp.groupproj project file 
2) Install executables and DLLs to ../RTKLIB/bin by runnning app/winapp/install_winapp.bat

CUIs:
1) Build executables with app/consapp/rtklib_consapp.groupproj project file 
2) Install executables to ../RTKLIB/bin by runnning app/consapp/install_consapp.bat



WINDOWS/LINUX CLI & GUI (except for Embarcadero GUI) using CMake

1) create a build directory
 > mkdir build
 > cd build/
2) setup CMake project
 > cmake ..
3) compile CLI & GUI
 > make


WINDOWS: To build with MSVC and Qt using CMake

Requirements:
- CMake 3.16 or later
- Microsoft Visual Studio with MSVC compiler
- Qt 5.15+ or Qt 6.x

1) Open Developer Command Prompt for VS
2) Create and enter build directory:
 > mkdir build
 > cd build
3) Configure with CMake (adjust Qt path as needed):
 > cmake .. -DCMAKE_BUILD_TYPE=Release -DCMAKE_PREFIX_PATH="C:\Qt\5.15.2\msvc2019_64" -DDLL=OFF
4) Build the project:
 > cmake --build .

Build options:
- Use -DDLL=ON for shared library build (default)  
- Use -DDLL=OFF for static library build (recommended for Windows distribution)
- Qt applications will be built automatically if Qt is found


LINUX: To build and install code (DEPRECATED)

CUIs:
1) cd app/consapp/<appName>/gcc
2) make

GUIs (Qt based - Beta):
1) cd app/qtapp
2) qmake
3) make
4) ./install_qtapp

Windows binaries can be found on the release page.
Pre-complied linux packages are available at https://build.opensuse.org/package/show/home:ReimannJens/rtklib-qt.

The last step will copy the compiled executables into a new directory RTKLIB_bin next to the rtklib source directory.

