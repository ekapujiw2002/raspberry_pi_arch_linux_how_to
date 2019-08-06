# Info Beamer

## Apakah Info Beamer?
Info Beamer merupakan sebuah program kecil untuk menampilkan berbagai *resource* multimedia baik yang bersifat statik seperti gambar, teks, atau apapun. Maupun konten yang bergerak seperti film dan animasi. Adapun fitur dari Info Beamer sebagai berikut :
 1. Rendering konten dengan kualitas tinggi dan FPS sebesar 60
 2. Animasi yang halus untuk video, gambar, teks dengan API yang sangat simpel
 3. Reload dinamis, artinya konten berubah di disk, maka secara otomatis Info Beamer akan menampilkan perubahannya
 4. Integrasi yang sederhana (hanya mempergunakan JSON saja sudah cukup)
 5. Pengembangan cepat dengan *live reloading feature*
 6. Stabil dalam pengembangan dan aplikasi riil
 
 ## Instalasi
 1. Download binari info-beamer dari link https://info-beamer.com/download/player . Pilih sesuai OS Anda. Saya menggunakan Arch Linux ARM. Untuk instalasinya saya memakai keluaran untuk Raspbian Jessie.
 `wget https://info-beamer.com/jump/download/player/info-beamer-pi-0.9.8-beta.caeb0e-jessie.tar.gz`
 2. Extract file tar gz yang sudah di-download sehingga akan didapatkan isian sebagai berikut :
 ```
 [enco1@qpi2 info-beamer-pi]$ tree
.
|-- CHANGELOG.txt
|-- LICENSE.txt
|-- README.txt
|-- info-beamer
`-- samples
    |-- README.txt
    |-- green
    |   |-- README.txt
    |   |-- blue
    |   |   `-- node.lua
    |   |-- node.lua
    |   `-- red
    |       `-- node.lua
    |-- hello
    |   |-- README.silkscreen.txt
    |   |-- README.txt
    |   |-- node.lua
    |   `-- silkscreen.ttf
    |-- image
    |   |-- beamer.png
    |   `-- node.lua
    |-- parrot
    |   |-- README
    |   |-- blue_macaw.png
    |   `-- node.lua
    `-- shader
        |-- lua.png
        |-- node.lua
        `-- shader.frag

8 directories, 21 files
 ```
 3. Cek dependency dari file info-beamer dengan perintah : `ldd info-beamer` . Pastikan tidak ada yang dalam status **not found**!!!
 ```
 [enco1@qpi2 info-beamer-pi]$ ldd info-beamer
        linux-vdso.so.1 (0x7e9d3000)
        libm.so.6 => /usr/lib/libm.so.6 (0x76f13000)
        librt.so.1 => /usr/lib/librt.so.1 (0x76efc000)
        libdl.so.2 => /usr/lib/libdl.so.2 (0x76ee9000)
        libevent-2.0.so.5 => /usr/lib/libevent-2.0.so.5 (0x76e8f000)
        libpng12.so.0 => /usr/lib/libpng12.so.0 (0x76e5e000)
        libavformat.so.56 => /usr/lib/ffmpeg2.8/libavformat.so.56 (0x76c8f000)
        libavcodec.so.56 => /usr/lib/ffmpeg2.8/libavcodec.so.56 (0x75c8f000)
        libavutil.so.54 => /usr/lib/ffmpeg2.8/libavutil.so.54 (0x75c12000)
        libavresample.so.2 => /usr/lib/ffmpeg2.8/libavresample.so.2 (0x75bf2000)
        libfreetype.so.6 => /usr/lib/libfreetype.so.6 (0x75b41000)
        libopenmaxil.so => /opt/vc/lib/libopenmaxil.so (0x75b2b000)
        libGLESv2.so => /opt/vc/lib/libGLESv2.so (0x75b06000)
        libEGL.so => /opt/vc/lib/libEGL.so (0x75acd000)
        libbcm_host.so => /opt/vc/lib/libbcm_host.so (0x75aa6000)
        libvcos.so => /opt/vc/lib/libvcos.so (0x75a8c000)
        libpthread.so.0 => /usr/lib/libpthread.so.0 (0x75a63000)
        libmmal.so => /opt/vc/lib/libmmal.so (0x75a50000)
        libmmal_util.so => /opt/vc/lib/libmmal_util.so (0x75a30000)
        libmmal_core.so => /opt/vc/lib/libmmal_core.so (0x75a12000)
        libvchiq_arm.so => /opt/vc/lib/libvchiq_arm.so (0x759fc000)
        libgcc_s.so.1 => /usr/lib/libgcc_s.so.1 (0x759cf000)
        libc.so.6 => /usr/lib/libc.so.6 (0x7588b000)
        /lib/ld-linux-armhf.so.3 (0x76fb2000)
        libcrypto.so.1.1 => /usr/lib/libcrypto.so.1.1 (0x756c0000)
        libz.so.1 => /usr/lib/libz.so.1 (0x7569c000)
        libgmp.so.10 => /usr/lib/libgmp.so.10 (0x7562e000)
        libssh.so.4 => /usr/lib/libssh.so.4 (0x755b7000)
        libmodplug.so.1 => /usr/lib/libmodplug.so.1 (0x75427000)
        libbluray.so.2 => /usr/lib/libbluray.so.2 (0x753ce000)
        libgnutls.so.30 => /usr/lib/libgnutls.so.30 (0x75273000)
        libbz2.so.1.0 => /usr/lib/libbz2.so.1.0 (0x75254000)
        libswresample.so.1 => /usr/lib/ffmpeg2.8/libswresample.so.1 (0x75232000)
        libva.so.1 => /usr/lib/libva.so.1 (0x75205000)
        libxvidcore.so.4 => /usr/lib/libxvidcore.so.4 (0x75113000)
        libx265.so.116 => /usr/lib/libx265.so.116 (0x74ee6000)
        libx264.so.148 => /usr/lib/libx264.so.148 (0x74daf000)
        libwebpmux.so.3 => /usr/lib/libwebpmux.so.3 (0x74d97000)
        libwebp.so.7 => /usr/lib/libwebp.so.7 (0x74d3f000)
        libvpx.so.4 => /usr/lib/libvpx.so.4 (0x74b9a000)
        libvorbisenc.so.2 => /usr/lib/libvorbisenc.so.2 (0x74b08000)
        libvorbis.so.0 => /usr/lib/libvorbis.so.0 (0x74ad0000)
        libtheoraenc.so.1 => /usr/lib/libtheoraenc.so.1 (0x74a8d000)
        libtheoradec.so.1 => /usr/lib/libtheoradec.so.1 (0x74a6f000)
        libspeex.so.1 => /usr/lib/libspeex.so.1 (0x74a4b000)
        libschroedinger-1.0.so.0 => /usr/lib/libschroedinger-1.0.so.0 (0x74998000)
        libopus.so.0 => /usr/lib/libopus.so.0 (0x7493e000)
        libopenjpeg.so.1 => /usr/lib/libopenjpeg.so.1 (0x74915000)
        libopencore-amrwb.so.0 => /usr/lib/libopencore-amrwb.so.0 (0x748f4000)
        libopencore-amrnb.so.0 => /usr/lib/libopencore-amrnb.so.0 (0x748c1000)
        libmp3lame.so.0 => /usr/lib/libmp3lame.so.0 (0x74849000)
        libgsm.so.1 => /usr/lib/libgsm.so.1 (0x74830000)
        libdcadec.so.0 => /usr/lib/libdcadec.so.0 (0x747ef000)
        liblzma.so.5 => /usr/lib/liblzma.so.5 (0x747be000)
        libpng16.so.16 => /usr/lib/libpng16.so.16 (0x74782000)
        libharfbuzz.so.0 => /usr/lib/libharfbuzz.so.0 (0x746e9000)
        libbrcmGLESv2.so => /opt/vc/lib/libbrcmGLESv2.so (0x746c4000)
        libbrcmEGL.so => /opt/vc/lib/libbrcmEGL.so (0x7468b000)
        libmmal_vc_client.so => /opt/vc/lib/libmmal_vc_client.so (0x74670000)
        libmmal_components.so => /opt/vc/lib/libmmal_components.so (0x74655000)
        libvcsm.so => /opt/vc/lib/libvcsm.so (0x74641000)
        libcontainers.so => /opt/vc/lib/libcontainers.so (0x74620000)
        libgcrypt.so.20 => /usr/lib/libgcrypt.so.20 (0x74555000)
        libstdc++.so.6 => /usr/lib/libstdc++.so.6 (0x74403000)
        libxml2.so.2 => /usr/lib/libxml2.so.2 (0x742c5000)
        libfontconfig.so.1 => /usr/lib/libfontconfig.so.1 (0x7427c000)
        libp11-kit.so.0 => /usr/lib/libp11-kit.so.0 (0x74177000)
        libunistring.so.2 => /usr/lib/libunistring.so.2 (0x7400d000)
        libtasn1.so.6 => /usr/lib/libtasn1.so.6 (0x73fed000)
        libnettle.so.6 => /usr/lib/libnettle.so.6 (0x73fa6000)
        libhogweed.so.4 => /usr/lib/libhogweed.so.4 (0x73f6a000)
        libsoxr.so.0 => /usr/lib/libsoxr.so.0 (0x73f05000)
        libogg.so.0 => /usr/lib/libogg.so.0 (0x73ef8000)
        liborc-0.4.so.0 => /usr/lib/liborc-0.4.so.0 (0x73e80000)
        libglib-2.0.so.0 => /usr/lib/libglib-2.0.so.0 (0x73d7a000)
        libgraphite2.so.3 => /usr/lib/libgraphite2.so.3 (0x73d49000)
        libgpg-error.so.0 => /usr/lib/libgpg-error.so.0 (0x73d26000)
        libicuuc.so.59 => /usr/lib/libicuuc.so.59 (0x73bba000)
        libexpat.so.1 => /usr/lib/libexpat.so.1 (0x73b7e000)
        libffi.so.6 => /usr/lib/libffi.so.6 (0x73b66000)
        libgomp.so.1 => /usr/lib/libgomp.so.1 (0x73b2e000)
        libpcre.so.1 => /usr/lib/libpcre.so.1 (0x73ab6000)
        libicudata.so.59 => /usr/lib/libicudata.so.59 (0x72191000)
 ```
 4. Jika menggunakan Raspbian dan keturunannya maka silakan lakukan update dan instalai beberapa paket berikut ini jika belum ada : `sudo apt-get install libevent-2.0-5 libavformat56 libpng12-0 libfreetype6 libavresample2`
 5. Set flag executable pada file info-beamer jika belum aktif dengan `chmod +x info-beamer` . Setelah itu kopi ke **/usr/bin** dengan `sudo cp info-beamer /usr/bin`
 6. Teslah info-beamer Anda dengan mengetikkan perintah ini `sudo info-beamer samples/shader` dan lihatlah hasilnya ke layar Anda.
 7. Jika eror atau kurang optimal maka silakan ubah beberapa opsi ini untuk Raspberry Pi Anda dengan mengedit **/boot/config.txt** dan tambahkan **gpu_mem=192** serta **dtoverlay=vc4-kms-v3d**
 8. Run dengan `sudo INFOBEAMER_ADDR=0.0.0.0 INFOBEAMER_AUDIO_TARGET=hdmi INFOBEAMER_LOG_LEVEL=3 info-beamer path_folder_node_lua`
 
 Selebihnya bisa dieksplorasi lebih lanjut pada link di bawah ini.

Referensi :
- https://info-beamer.com/
- https://info-beamer.com/doc/info-beamer
- https://github.com/dividuum/info-beamer-nodes
- https://github.com/info-beamer
