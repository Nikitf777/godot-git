# Maintainer: VitalyR <vr@vitalyr.com>
# Contributor: tas <tasgon_@out/look.com>
# Contributor: QuantMint <qua/ntmint@/protonm/ail.com>
# Contributor: Daniel Segesdi <sege/sdi.d/ani@/gma/il.com>
# Contributor: Cristian Porras <porrascristian@gmail.com>
# Contributor: Matthew Bentley <matthew@mtbentley.us>

_pkgname=godot
pkgname=${_pkgname}-git
pkgver=4.5.r75775.e1b4101e34
pkgrel=1
pkgdesc="Godot Game Engine: An advanced, feature packed, multi-platform 2D and 3D game engine."
url="https://godotengine.org/"
license=('MIT')
arch=('i686' 'x86_64')

makedepends=(git alsa-lib scons wayland)
depends=(freetype2 graphite harfbuzz harfbuzz-icu libglvnd libspeechd
    libsquish libtheora libvorbis libwebp libwslay libxcursor libxi
    libxinerama libxrandr miniupnpc pcre2)
optdepends=('pipewire-alsa: for audio support'
    'pipewire-pulse: for audio support')
conflicts=("godot")
provides=("godot")
_arch=''
if test "$CARCH" == x86_64; then
    _arch=('x86_64')
else
    _arch=('32')
fi

source=(
    "${_pkgname}::git+https://github.com/godotengine/godot.git"
    godot.desktop
    icon.png
)
sha256sums=(
    'SKIP'
    '2ae039a3879b23e157f2125e0b326fa1ef66d56bfd596c790e8943d27652e93a'
    '99f9d17c4355b274ef0c08069cf6e756a98cd5c9d9d22d1b39f79538134277e1'
)

pkgver() {
    cd "${srcdir}/${_pkgname}"
    _major=$(cat version.py | grep "major" | sed 's/major = //')
    _minor=$(cat version.py | grep "minor" | sed 's/minor = //')
    _revision=$(printf "r%s.%s" "$(git rev-list --count HEAD)" "$(git rev-parse --short HEAD)")
    echo "${_major}.${_minor}.${_revision}"
}

build() {
    cd "${srcdir}"/${_pkgname}
    # by default built using LTO, use `lto=none` to disable
    scons platform=linuxbsd target=editor production=yes werror=no -j $(nproc)
}

package() {

    cd "${srcdir}"

    install -Dm644 godot.desktop "${pkgdir}"/usr/share/applications/godot.desktop
    install -Dm644 icon.png "${pkgdir}"/usr/share/pixmaps/godot.png

    cd "${srcdir}"/${_pkgname}

    install -D -m755 bin/godot.linuxbsd.editor.${_arch} "${pkgdir}"/usr/bin/godot
    install -D -m644 LICENSE.txt "${pkgdir}"/usr/share/licenses/godot-git/LICENSE
}
