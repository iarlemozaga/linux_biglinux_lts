# Maintainer: Bernhard Landauer <bernhard@manjaro.org>
# Maintainer: Philip Müller <philm[at]manjaro[dot]org>
# Archlinux maintainers:
# Tobias Powalowski <tpowa@archlinux.org>
# Thomas Baechler <thomas@archlinux.org>

_basekernel=6.6
_basever=${_basekernel//.}
_kernelname=-BIGLINUX
pkgbase=linux-biglinux-lts
pkgname=("$pkgbase" "$pkgbase-headers")
pkgver=6.6.15
pkgrel=1
arch=('x86_64')
url="https://www.kernel.org/"
license=('GPL2')
makedepends=(bc docbook-xsl libelf pahole python-sphinx git inetutils kmod xmlto cpio perl tar xz)
options=('!strip')
source=(https://git.kernel.org/torvalds/t/linux-${_basekernel}.tar.gz
        https://www.kernel.org/pub/linux/kernel/v6.x/patch-${pkgver}.xz
        config
        # Upstream Patches
        # ARCH Patches
        0101-ZEN_Add_sysctl_and_CONFIG_to_disallow_unprivileged_CLONE_NEWUSER.patch
        0102-drivers-firmware-skip-simpledrm-if-nvidia-drm.modese.patch
        # MANJARO Patches
        # Realtek patch
        0999-patch_realtek.patch
        # ROG ALLY Patches
        v14.7-0001-HID-asus-fix-more-n-key-report-descriptors-if-.patch
        v14.7-0002-HID-asus-make-asus_kbd_init-generic-remove-rog.patch
        v14.7-0003-HID-asus-add-ROG-Ally-N-Key-ID-and-keycodes.patch
        v14.7-0004-HID-asus-add-ROG-Ally-xpad-settings.patch
        0006-platform-x86-asus-wmi-disable-USB0-hub-on-ROG-Ally-b.patch
        0007-mt7921e_Perform_FLR_to_recovery_the_device.patch
        # AMD GPU reset patches
        0301-drm-Add_GPU_reset_sysfs_event.patch
        0302-drm-amdgpu-add_work_function_for_GPU_reset_event.patch
        0303-drm-amdgpu-schedule_GPU_reset_event_work_function.patch
        # No overrides ROG ally <= 323 BIOS
        0001-ALSA-hda-cs35l41-Support-ASUS-2023-laptops-with-miss.patch
        0001-ALSA-hda-cs35l41-Improve-support-for-ASUS-ROG-Ally.patch
        # Additional ALLY patches
        ROG-ALLY-NCT6775-PLATFORM.patch
        0001-iio-imu_Add_driver_for_BMI323_IMU.patch
        0002-iio-imu-bmi323-Make-the-local-structures-static.patch
        0003-iio-imu_Add_ROG_ALLY_bmi323-support.patch
        0004-iio-imu-Load_ROG_ALLY_mount_matrix.patch
        0005-iio-imu-ASUS-ROG-ALLY-force-INT1-IRQ.patch
        # Steamdeck HID patches
        0001-HID.patch
)

if [[ -z "$_rc" ]]; then
  _srcdir="linux-${_basekernel}"
else
  _srcdir="linux-${_basekernel}-${_rc}"
fi

# sha256sums=('9a72c005a62f109f96ee00552502d16c4f06c248e6baba1629506627396ac0a7'
#             '88a4e0bb1cb0dfadda16e0ca19b34138fcf15cbdf3902cddfa498de51f25174c'
#             'e508623ccc41e3bdf8923878332912ea781d5dea3131c7b23dcfb4199adb8f19'
#             '05f04019d4a2ee072238c32860fa80d673687d84d78ef436ae9332b6fb788467'
#             'e1d17690dd21e8d5482b63ca66bfe6b478d39e8e8b59eedd53adb0a55ebc308d'
#             'a99b684fe5bc7fdacc6f5b1f2b6593672fc5d1e676c4de03ec29723747fc574b'
#             'b099ae83a3b561b8bff8b32b44b6f4835b99eb150c2314177aa0bc8ca96e2ead'
#             '10b60663195a65ec3b0f50b49e4c0af952369ee5afe95e11a69ffccefc020eb2'
#             '3c8b877dfaf85acf45b54c85a44fa269aa1512ea3781fe551cf6d4e2d69c992d'
#             '73aa4be8c1abcf1b24c9a5c5072e68da3da82df807f3ff49660a100d7634da1d'
#             '836e88044263f7bc474ca466b3d0d98c39e265db94925c300d0b138492946a13'
#             'd673d034fbcd80426fd8d9c6af56537c5fe5b55fe49d74e313474d7fc285ecc1'
#             '1f62542a889a6c2eafd43acd0699f54720ed891eeda66a4a9261d75b92f28b7f'
#             '6bc2c1b9a485c852b45e4064e8b9b559b9b26113fdc80bf9653af44c0886fde2'
#             '559f01074cda3c161996617f1b7bc6cbbce0efc50e2cf9e843d60922ff2e8063'
#             '79970a4729572cb25fd4644d66f38ecd5b3e1610a42ea4bbe436b501f3469fa2'
#             '430a7f971d78d0873708e0ad38fba602ceafefd4da8ebbf9d9c591bc4799acb5'
#             'cfcd5c177423df8b7b98b0500fe7ab0757f895ed945c33e205963f0069c7a3be'
#             '708a9899f80db35fb0f06e0144c361eac9a9b2d154cf2fa388a0b4810847e24c'
#             '514fd03c17245ed0aaee63e8830c9b02b00efa0307f7e0989065edec6ae185f0'
#             'fccdf24b25620dd8271bb3b52ddc53f8882dec26518258dc47e1469fed33e516'
#             'c3b901db58288b5cc5d8a947ac8ffec339870b00aba493d68a39f65c4ff3d869'
#             '5792a59a0c726a205ae1c1728700ea3e6385231cadc2cfdd2db08295b100638c'
#             '7c948773d758418d8a436067265d678c444827562c46b9fced2ff31ced108481')

prepare() {
  cd "$_srcdir"

  # add upstream patch
  msg "add upstream patch"
  patch -p1 -i "../patch-${pkgver}"

  local src
  for src in "${source[@]}"; do
      src="${src%%::*}"
      src="${src##*/}"
      [[ $src = *.patch ]] || continue
      msg2 "Applying patch: $src..."
      patch -Np1 < "../$src"
  done

  msg2 "add config"
  cat "../config" > ./.config

  if [ "${_kernelname}" != "" ]; then
    sed -i "s|CONFIG_LOCALVERSION=.*|CONFIG_LOCALVERSION=\"${_kernelname}\"|g" ./.config
    sed -i "s|CONFIG_LOCALVERSION_AUTO=.*|CONFIG_LOCALVERSION_AUTO=n|" ./.config
  fi

  msg "set extraversion to pkgrel"
  sed -ri "s|^(EXTRAVERSION =).*|\1 -${pkgrel}|" Makefile

  msg "don't run depmod on 'make install'"
  # We'll do this ourselves in packaging
  sed -i '2iexit 0' scripts/depmod.sh

  msg "get kernel version"
  make prepare

  msg "rewrite configuration"
  yes "" | make config >/dev/null
}

build() {
  cd "$_srcdir"

  msg "build"
  make ${MAKEFLAGS} LOCALVERSION= bzImage modules
}

package_linux-biglinux-lts() {
  pkgdesc="The ${pkgbase/linux/Linux} kernel and modules"
  depends=('coreutils' 'linux-firmware' 'kmod' 'initramfs')
  optdepends=('wireless-regdb: to set the correct wireless channels of your country')
  provides=("linux=${pkgver}" VIRTUALBOX-GUEST-MODULES WIREGUARD-MODULE KSMBD-MODULE)

  cd "$_srcdir"

  # get kernel version
  _kernver="$(make LOCALVERSION= kernelrelease)"

  mkdir -p "${pkgdir}"/{boot,usr/lib/modules}
  ZSTD_CLEVEL=19 make LOCALVERSION= INSTALL_MOD_PATH="${pkgdir}/usr" \
  INSTALL_MOD_STRIP=1 modules_install

  # systemd expects to find the kernel here to allow hibernation
  # https://github.com/systemd/systemd/commit/edda44605f06a41fb86b7ab8128dcf99161d2344
  cp arch/x86/boot/bzImage "${pkgdir}/usr/lib/modules/${_kernver}/vmlinuz"

  # Used by mkinitcpio to name the kernel
  echo "${pkgbase}" | install -Dm644 /dev/stdin "${pkgdir}/usr/lib/modules/${_kernver}/pkgbase"
  echo "${_basekernel}-${CARCH}" | install -Dm644 /dev/stdin "${pkgdir}/usr/lib/modules/${_kernver}/kernelbase"

  # add kernel version
  echo "${pkgver}-${pkgrel}-BIGLINUX x64" > "${pkgdir}/boot/${pkgbase}-${CARCH}.kver"

  # remove build link
  rm "${pkgdir}"/usr/lib/modules/${_kernver}/build

  # now we call depmod...
  depmod -b "${pkgdir}/usr" -F System.map "${_kernver}"
}

package_linux-biglinux-lts-headers() {
  pkgdesc="Header files and scripts for building modules for ${pkgbase/linux/Linux} kernel"
  depends=('gawk' 'python' 'libelf' 'pahole')
  provides=("linux-headers=$pkgver")

  cd "$_srcdir"
  local _builddir="${pkgdir}/usr/lib/modules/${_kernver}/build"

  # add real version for building modules and running depmod from hook
  echo "${_kernver}" |
    install -Dm644 /dev/stdin "${_builddir}/version"

  install -Dt "${_builddir}" -m644 Makefile .config Module.symvers
  install -Dt "${_builddir}/kernel" -m644 kernel/Makefile
  install -Dt "${_builddir}" -m644 vmlinux

  mkdir "${_builddir}/.tmp_versions"

  cp -t "${_builddir}" -a include scripts

  install -Dt "${_builddir}/arch/x86" -m644 "arch/x86/Makefile"
  install -Dt "${_builddir}/arch/x86/kernel" -m644 "arch/x86/kernel/asm-offsets.s"

  cp -t "${_builddir}/arch/x86" -a "arch/x86/include"

  install -Dt "${_builddir}/drivers/md" -m644 drivers/md/*.h
  install -Dt "${_builddir}/net/mac80211" -m644 net/mac80211/*.h

  # https://bugs.archlinux.org/task/13146
  install -Dt "${_builddir}/drivers/media/i2c" -m644 drivers/media/i2c/msp3400-driver.h

  # https://bugs.archlinux.org/task/20402
  install -Dt "${_builddir}/drivers/media/usb/dvb-usb" -m644 drivers/media/usb/dvb-usb/*.h
  install -Dt "${_builddir}/drivers/media/dvb-frontends" -m644 drivers/media/dvb-frontends/*.h
  install -Dt "${_builddir}/drivers/media/tuners" -m644 drivers/media/tuners/*.h

  # https://bugs.archlinux.org/task/71392
  install -Dt "${_builddir}/drivers/iio/common/hid-sensors" -m644 drivers/iio/common/hid-sensors/*.h

  # add xfs and shmem for aufs building
  mkdir -p "${_builddir}"/{fs/xfs,mm}

  # copy in Kconfig files
  find . -name Kconfig\* -exec install -Dm644 {} "${_builddir}/{}" \;

  # add objtool for external module building and enabled VALIDATION_STACK option
  install -Dt "${_builddir}/tools/objtool" tools/objtool/objtool

  # https://forum.manjaro.org/t/90629/39
  install -Dt "${_builddir}/tools/bpf/resolve_btfids" tools/bpf/resolve_btfids/resolve_btfids

  # remove unneeded architectures
  local _arch
  for _arch in "${_builddir}"/arch/*/; do
    [[ ${_arch} == */x86/ ]] && continue
    rm -r "${_arch}"
  done

  # remove documentation files
  rm -r "${_builddir}/Documentation"

  # strip scripts directory
  local file
  while read -rd '' file; do
    case "$(file -bi "$file")" in
      application/x-sharedlib\;*)      # Libraries (.so)
        strip $STRIP_SHARED "$file" ;;
      application/x-archive\;*)        # Libraries (.a)
        strip $STRIP_STATIC "$file" ;;
      application/x-executable\;*)     # Binaries
        strip $STRIP_BINARIES "$file" ;;
      application/x-pie-executable\;*) # Relocatable binaries
        strip $STRIP_SHARED "$file" ;;
    esac
  done < <(find "${_builddir}" -type f -perm -u+x ! -name vmlinux -print0 2>/dev/null)
  strip $STRIP_STATIC "${_builddir}/vmlinux"

  echo "Adding symlink..."
  mkdir -p "${pkgdir}/usr/src"
  ln -sr "${_builddir}" "${pkgdir}/usr/src/${pkgbase}"

  # remove unwanted files
  find ${_builddir} -name '*.orig' -delete
}
