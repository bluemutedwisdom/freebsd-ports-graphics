# Created by: Nicola Vitale	<nivit@email.it>
# $FreeBSD$

PORTNAME=	muparser
PORTVERSION=	2.2.5
DISTVERSIONPREFIX=	v
CATEGORIES=	math devel

MAINTAINER=	nivit@FreeBSD.org
COMMENT=	Mathematical expressions parser library written in C++

LICENSE=	MIT
LICENSE_FILE=	${WRKSRC}/License.txt

USE_GITHUB=	yes
GH_ACCOUNT=	beltoforion

USES=		dos2unix pathfix
USE_LDCONFIG=	yes

ALL_TARGET=	lib
GNU_CONFIGURE=	yes
# otherwise it fails to build when the EXAMPLES option is ON
MAKE_JOBS_UNSAFE=	yes

OPTIONS_DEFINE=		DOXYGEN EXAMPLES

DOXYGEN_BUILD_DEPENDS=	doxygen:devel/doxygen \
			dot:graphics/graphviz
DOXYGEN_ALL_TARGET=	documentation
DOXYGEN_PORTDOCS=	*
# PORTDOCS macro won't work if NOPORTDOCS is defined or if DOCS is disabled
DOXYGEN_IMPLIES=	DOCS

EXAMPLES_CONFIGURE_ENABLE=	samples
EXAMPLES_ALL_TARGET=	samples

post-patch:
	@${REINPLACE_CMD} -E 's,^(CXXFLAGS|LDFLAGS) = ,\1 ?= ,g' \
		${WRKSRC}/Makefile.in

post-install-DOXYGEN-on:
	@${MKDIR} ${STAGEDIR}${DOCSDIR}
	(cd ${WRKSRC}/docs/html && \
	${COPYTREE_SHARE} . ${STAGEDIR}${DOCSDIR})

post-install-EXAMPLES-on:
	@${MKDIR} ${STAGEDIR}${EXAMPLESDIR}
	(cd ${WRKSRC}/samples/example1 && \
	${INSTALL_PROGRAM} example1 ${STAGEDIR}${EXAMPLESDIR} && \
	${INSTALL_DATA} example1.cpp ${STAGEDIR}${EXAMPLESDIR})

.include <bsd.port.mk>
