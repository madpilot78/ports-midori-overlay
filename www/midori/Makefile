# Since 0.10.4, which migrated to Gecko, the port is mostly based
# on www/firefox

PORTNAME=	midori
PORTVERSION=	10.0.4
DISTVERSIONPREFIX=	v
CATEGORIES=	www

MAINTAINER=	ports@FreeBSD.org
COMMENT=	Lightweight web browser using gecko browser engine
WWW=		https://astian.org/en/midori-browser/

LICENSE=	MPL20 MIT
LICENSE_COMB=	multi

BUILD_DEPENDS=	nspr>=4.32:devel/nspr \
		nss>=3.90:security/nss \
		icu>=73.1,1:devel/icu \
		libevent>=2.1.8:devel/libevent \
		harfbuzz>=7.3.0:print/harfbuzz \
		graphite2>=1.3.14:graphics/graphite2 \
		png>=1.6.39:graphics/png \
		dav1d>=1.0.0:multimedia/dav1d \
		libvpx>=1.13.0:multimedia/libvpx \
		${PYTHON_PKGNAMEPREFIX}sqlite3>0:databases/py-sqlite3@${PY_FLAVOR} \
		v4l_compat>0:multimedia/v4l_compat \
		autoconf2.13:devel/autoconf2.13 \
		nasm:devel/nasm \
		yasm:devel/yasm \
		zip:archivers/zip

USE_GECKO=	gecko
USE_MOZILLA=	-sqlite
CFLAGS_powerpc64le=     -DSQLITE_BYTEORDER=1234
# work around bindgen not finding ICU, e.g.
# dist/include/mozilla/intl/ICU4CGlue.h:8:10: fatal error: 'unicode/uenum.h' file not found, err: true
CONFIGURE_ENV+= BINDGEN_CFLAGS="-I${LOCALBASE}/include"

MOZ_EXPORT=	MOZ_TELEMETRY_REPORTING="" \
		MOZ_OFFICIAL_BRANDING="1" \
		MOZ_APP_DISPLAYNAME="Midori Browser" \
		MOZ_APP_NAME="midori"
MOZ_OPTIONS=	--enable-official-branding \
		--with-branding=browser/branding/official \
		--disable-updater

CPE_VENDOR=	midori-browser

USE_GITLAB=	yes
GL_ACCOUNT=	midori-web
GL_PROJECT=	midori-desktop

USE_GITHUB=	nodefault
GH_TUPLE=	Floorp-Projects:Unified-l10n-central:f1b3f2a1:floorp/floorp/browser/locales/en-US

.include "${.CURDIR}/../../www/firefox/Makefile.options"

.include <bsd.port.options.mk>

.if ${ARCH} == powerpc64
MOZ_OPTIONS+=	--without-wasm-sandboxed-libraries
.else
BUILD_DEPENDS+=	${LOCALBASE}/share/wasi-sysroot/lib/wasm32-wasi/libc++abi.a:devel/wasi-libcxx \
		${LOCALBASE}/share/wasi-sysroot/lib/wasm32-wasi/libc.a:devel/wasi-libc \
		${LOCALBASE}/llvm${LLVM_DEFAULT}/lib/clang/${LLVM_VERSION}/lib/wasi/libclang_rt.builtins-wasm32.a:devel/wasi-compiler-rt${LLVM_DEFAULT}
MOZ_OPTIONS+=	--with-wasi-sysroot=${LOCALBASE}/share/wasi-sysroot
.endif

.include <bsd.port.mk>
