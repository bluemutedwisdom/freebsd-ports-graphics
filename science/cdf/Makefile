# Created by: Thierry Thomas <thierry@pompo.net>
# $FreeBSD$

PORTNAME=	cdf3
PORTVERSION=	3.3.0
PORTREVISION=	8
CATEGORIES=	science
MASTER_SITES=	LOCAL/thierry/${PORTNAME} \
		ftp://cdaweb.gsfc.nasa.gov/pub/cdf/dist/cdf${VER}/unix/
DISTNAME=	cdf${VER}-dist-cdf
DIST_SUBDIR=	${PORTNAME}-${PORTVERSION}

MAINTAINER=	stephen@FreeBSD.org
COMMENT=	Device independent view of the CDF data model

LICENSE=	NASA-G
LICENSE_NAME=	NASA/Goddard Space Flight Center
LICENSE_FILE=	${WRKSRC}/CDF_copyright.txt
LICENSE_PERMS=	dist-mirror dist-sell pkg-mirror pkg-sell auto-accept

BROKEN_aarch64=		fails to compile: error: use of undeclared identifier FP1ZEROsingle; did you mean FP1zeroSingle?
BROKEN_armv6=		fails to compile: error: use of undeclared identifier FP1ZEROsingle; did you mean FP1zeroSingle?
BROKEN_armv7=		fails to compile: error: use of undeclared identifier FP1ZEROsingle; did you mean FP1zeroSingle?

USES=		fortran ncurses
USE_LDCONFIG=	yes
MAKE_FLAGS=	INSTALLDIR=${STAGEDIR}${PREFIX} OS=freebsd ENV=gnu \
		FORTRAN=yes FC_freebsd=${F77}
MAKE_JOBS_UNSAFE=	yes

WRKSRC=		${WRKDIR}/cdf${VER}-dist
SUB_FILES=	pkg-message

VER=		${PORTVERSION:S/.//:S/./_/}
DEFS=		B C K

PORTDOCS=	CDF_copyright.txt CHANGES.txt Release.notes Welcome.txt

OPTIONS_DEFINE=	DOCS EXAMPLES

post-patch:
.for def in ${DEFS}
	@${REINPLACE_CMD} -e 's|<path>|${PREFIX}|' \
		 ${WRKSRC}/src/definitions/definitions.${def}
.endfor

post-install:
	${MV} ${STAGEDIR}${PREFIX}/lib/libcdf.so \
		${STAGEDIR}${PREFIX}/lib/libcdf.so.0
	${LN} -sf libcdf.so.0 ${STAGEDIR}${PREFIX}/lib/libcdf.so
	${MKDIR} ${STAGEDIR}${DOCSDIR}
	${INSTALL_DATA} ${PORTDOCS:S|^|${WRKSRC}/|} ${STAGEDIR}${DOCSDIR}
	${MKDIR} ${STAGEDIR}${EXAMPLESDIR}
	${INSTALL_DATA} ${WRKSRC}/samples/* ${STAGEDIR}${EXAMPLESDIR}
	${SED} -i '' 's,${STAGEDIR},,g' ${STAGEDIR}${PREFIX}/bin/definitions.?

regression-test: build
	@${SETENV} ${MAKE_ENV} ${MAKE_CMD} ${MAKE_FLAGS} ${MAKEFILE} \
		${MAKE_ARGS} test -C ${BUILD_WRKSRC}

.include <bsd.port.mk>
