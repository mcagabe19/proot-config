Packages
```bash
pulseaudio proot-distro virglrenderer-android
```

Packages (PRoot)
```bash
xfce4 pulseaudio pavucontrol tumbler thunar-volman gvfs mesa-utils x11-utils alsa-utils
```

debian
```bash
pd login --shared-tmp --user lily --kernel $(uname -r) debian
```

debianx11
```bash
#!/data/data/com.termux/files/usr/bin/bash

# Start VirGL
MESA_NO_ERROR=1 MESA_GL_VERSION_OVERRIDE=4.3COMPAT MESA_GLES_VERSION_OVERRIDE=3.2 MESA_EXTENSION_OVERRIDE=GL_EXT_texture_compression_s3tc PAN_MESA_DEBUG=gofaster,gl3 mesa_glthread=true __GL_THREADED_OPTIMIZATIONS=1 virgl_test_server_android &

# Audio
pulseaudio --start --load="module-sles-source" --load="module-native-protocol-tcp auth-ip-acl=127.0.0.1 auth-anonymous=1" --exit-idle-time=-1

# Termux:X11
export XDG_RUNTIME_DIR=${TMPDIR}
termux-x11 :0 +xinerama +iglx +extension DOUBLE-BUFFER -legacy-drawing &

# Wait a bit until termux-x11 gets started.
sleep 3

# Start PRoot
pd login debian --kernel $(uname -r) --shared-tmp -- /bin/bash -c 'export PULSE_SERVER=127.0.0.1 && export XDG_RUNTIME_DIR=/tmp && su - lily -c "env DISPLAY=:0 dbus-launch --exit-with-session startxfce4"'

# Kill processes after exit
pkill -f "app_process / com.termux.x11"
killall pulseaudio
killall virgl_test_server
exit 0
```

bash.bashrc
```bash
...
export TERMUX_HOME=/data/data/com.termux/files/home
export TERMUX_PREFIX=/data/data/com.termux/files/usr
# export GALLIUM_DRIVER=virpipe
export MESA_NO_ERROR=1
export MESA_GL_VERSION_OVERRIDE=4.5COMPAT
export MESA_GLES_VERSION_OVERRIDE=3.2
export MESA_EXTENSION_OVERRIDE=GL_EXT_texture_compression_s3tc
export PAN_MESA_DEBUG=gofaster,gl3
export mesa_glthread=true
export __GL_THREADED_OPTIMIZATIONS=1
export BOX64_LOG=1
export BOX64_DYNAREC_BIGBLOCK=2
export BOX64_DYNAREC_CALLRET=1
```
