# SPDX-License-Identifier: AGPL-3.0

#    ----------------------------------------------------------------------
#    Copyright © 2024, 2025  Pellegrino Prevete
#
#    All rights reserved
#    ----------------------------------------------------------------------
#
#    This program is free software: you can redistribute it and/or modify
#    it under the terms of the GNU Affero General Public License as published by
#    the Free Software Foundation, either version 3 of the License, or
#    (at your option) any later version.
#
#    This program is distributed in the hope that it will be useful,
#    but WITHOUT ANY WARRANTY; without even the implied warranty of
#    MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
#    GNU Affero General Public License for more details.
#
#    You should have received a copy of the GNU Affero General Public License
#    along with this program.  If not, see <https://www.gnu.org/licenses/>.

# Maintainer: Truocolo <truocolo@aol.com>
# Maintainer: Truocolo <truocolo@0x6E5163fC4BFc1511Dbe06bB605cc14a3e462332b>
# Maintainer: Pellegrino Prevete (dvorak) <pellegrinoprevete@gmail.com>
# Maintainer: Pellegrino Prevete (dvorak) <dvorak@0x87003Bd6C074C713783df04f36517451fF34CBEf>
# Maintainer: Daniel M. Capella <polyzen@archlinux.org>
# Contributor: Kyle Keen <keenerd@gmail.com>
# Contributor: PepeSmith
# Contributor: Aron Asor <aronasorman at gmail.com>
# Contributor: Chris Brannon <chris@the-brannons.com>
# Contributor: Douglas Soares de Andrade <dsa@aur.archlinux.org>

_py="python"
_pyver="$( \
  "${_py}" \
    -V | \
    awk \
      '{print $2}')"
_pymajver="${_pyver%.*}"
_pyminver="${_pymajver#*.}"
_pynextver="${_pymajver%.*}.$(( \
  ${_pyminver} + 1))"
_pkg=ipython
pkgname="${_pkg}"
pkgver=9.2.0
pkgrel=1
pkgdesc='Enhanced Interactive Python shell'
arch=(
  'any'
)
url="https://${_pkg}.org"
license=(
  'BSD-3-Clause'
)
depends=(
  "${_py}>=${_pymajver}"
  "${_py}<${_pynextver}"
  "${_py}-decorator"
  "${_py}-ipython-pygments-lexers"
  "${_py}-jedi"
  "${_py}-matplotlib-inline"
  "${_py}-pexpect"
  "${_py}-prompt_toolkit"
  "${_py}-pygments"
  "${_py}-stack-data"
  "${_py}-traitlets"
  "sqlite"
)
makedepends=(
  "git"
  "${_py}-build"
  "${_py}-installer"
  "${_py}-setuptools"
  "${_py}-wheel"
)
checkdepends=(
  jupyter-nbformat
  "${_py}-curio"
  "${_py}-ipykernel"
  "${_py}-matplotlib"
  "${_py}-numpy"
  "${_py}-pandas"
  "${_py}-pytest"
  "${_py}-pytest-asyncio"
  "${_py}-testpath"
  "${_py}-trio"
  "tcsh"
  "texlive-bin"
  "texlive-latex"
)
provides=(
  "${_py}-${_pkg}=${pkgver}"
)
optdepends=(
  "${_py}-black: to auto format with Black"
  "${_py}-pickleshare: for the ip.db database"
  "yapf: to auto format with YAPF"
)
_http="https://github.com"
_ns="${_pkg}"
_url="${_http}/${_ns}/${_pkg}"
_icon_uri='https://www.packal.org/sites/default/files/public/styles/icon_large/public/workflow-files/nkeimipynbworkflow/icon/icon.png'
source=(
  "git+${_url}.git#tag=${pkgver}?signed"
  "IPython-icon.png::${_icon_uri}"
)
b2sums=(
  'bf31cbbe95730c7dd038995026ba8a33ddd97e27fda7ff58e0c05012f3793a1cc3be33daaad660de48aa123481e21d55461350415a8f6a2ab52243c976dd7b61'
  'd445e2bc7a037db8715ea103611720e965987e155c32e445b0ef783e519fca8a0301b16c5763fd9a5d8d169c3b0d7b4db6c0bd0f9772842258b135dcb1d6d5a2')
validpgpkeys=(
  # Matthias Bussonnier <bussonniermatthias@gmail.com>
  99B17F64FD5C94692E9EF8064968B2CC0208DCC8
  # Michał Krassowski
  AB847D919065B1F4FF01AF30238E6384AF95AE6F
)

build() {
  cd \
    "${_pkg}"
  "${_py}" \
    -m \
      "build" \
    --wheel \
    --skip-dependency-check \
    --no-isolation
}

check() {
  cd \
    "${_pkg}"
  PYTHONPATH="${PWD}/${_pkg}:${PYTHONPATH}" \
  pytest
}

package() {
  local \
    site_packages
  site_packages=$( \
    "${_py}" \
      -c \
        "import site; print(site.getsitepackages()[0])")
  install \
    -d \
    "${pkgdir}/usr/share/licenses/${pkgname}"
  ln \
    -s \
    "${site_packages}/${pkgname}-${pkgver}.dist-info/LICENSE" \
    "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE"
  # FS#47046
  install \
    -Dm644 \
    "IPython-icon.png" \
    "${pkgdir}/usr/share/pixmaps/${_pkg}.png"
  cd \
    "${_pkg}"
  "${_py}" \
    -m \
      "installer" \
      --destdir="${pkgdir}" \
      "dist/"*".whl"
  cd \
    'examples/IPython Kernel'
  # FS#45120
  sed \
    -i \
    "s/gnome-netstatus-idle/${_pkg}/" \
    "${_pkg}.desktop"
  install \
    -Dm644 \
    -t \
    "${pkgdir}/usr/share/applications" \
    "${_pkg}.desktop"
}
