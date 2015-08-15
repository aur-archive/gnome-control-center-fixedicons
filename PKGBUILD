# Maintainer: Kwpolska <kwpolska@kwpolska.tk>
# Contributor (official repo maintainer): Jan Alexander Steffens (heftig) <jan.steffens@gmail.com>
# Contributor: Jan de Groot <jgc@archlinux.org>

pkgname=gnome-control-center-fixedicons
pkgver=3.4.1
pkgrel=1
pkgdesc="The Control Center for GNOME"
arch=('i686' 'x86_64')
depends=('accountsservice' 'cups-pk-helper' 'gnome-bluetooth' 'gnome-desktop' 'gnome-menus'
         'gnome-online-accounts' 'gnome-settings-daemon' 'gsettings-desktop-schemas' 'gtk3'
         'libgtop' 'libsocialweb' 'network-manager-applet' 'sound-theme-freedesktop' 'upower'
         'libsystemd' 'cheese')
optdepends=('mesa-demos: provides glxinfo for graphics information'
            'apg: adds password generation for user accounts'
            'gnome-color-manager: for color management tasks')
makedepends=('gnome-doc-utils' 'intltool' 'gnome-common')
url="http://www.gnome.org"
groups=('gnome')
install=gnome-control-center.install
license=('GPL')
options=('!libtool' '!emptydirs')
source=(http://download.gnome.org/sources/$pkgname/${pkgver%.*}/$pkgname-$pkgver.tar.xz
        systemd-fallback.patch
        gvc-applet.c.patch)
sha256sums=('c6ce4ecf5b747aa33a5904b053c1c4fd18a39ddcd0908463558e8b4b40ec3fd1'
            '5fa706de582228df36dfc13eb37470e543b2f228f1fc4ad27e35a781a8779b39'
            '55839407b0407031341a9694f962a7fc1ec8126dde53abac30f0c097a05d1f9c')

build() {
  cd $pkgname-$pkgver

  patch -Np1 -i ../systemd-fallback.patch
  gnome-autogen.sh --prefix=/usr --sysconfdir=/etc \
      --localstatedir=/var --disable-static \
      --enable-systemd --with-libsocialweb \
      --disable-update-mimedb
  patch panels/sound/gvc-applet.c ../gvc-applet.c.patch

  #https://bugzilla.gnome.org/show_bug.cgi?id=656229
  sed -i -e 's/ -shared / -Wl,-O1,--as-needed\0 /g' -e 's/    if test "$export_dynamic" = yes && test -n "$export_dynamic_flag_spec"; then/      func_append compile_command " -Wl,-O1,--as-needed"\n      func_append finalize_command " -Wl,-O1,--as-needed"\n\0/' libtool
  make
}

package() {
  cd $pkgname-$pkgver

  make DESTDIR="$pkgdir" install
}
