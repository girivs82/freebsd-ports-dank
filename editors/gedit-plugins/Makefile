# Created by: Adam Weinberger <adamw@FreeBSD.org>
# $FreeBSD$

PORTNAME=	gedit-plugins
PORTVERSION=	3.32.2
CATEGORIES=	editors gnome
MASTER_SITES=	GNOME
DIST_SUBDIR=	gnome

MAINTAINER=	gnome@FreeBSD.org
COMMENT=	Plugins for GEdit

LICENSE=	GPLv2
LICENSE_FILE=	${WRKSRC}/COPYING

BUILD_DEPENDS=	gedit>=0:editors/gedit \
		valac:lang/vala \
		itstool:textproc/itstool
LIB_DEPENDS=	libpeas-1.0.so:devel/libpeas \
		libzeitgeist-2.0.so:sysutils/zeitgeist \
		libgit2-glib-1.0.so:devel/libgit2-glib
RUN_DEPENDS=	gedit>=0:editors/gedit

PORTSCOUT=	limitw:1,even

USES=		compiler:c11 gettext gmake gnome libtool localbase pathfix pkgconfig \
		python:3.4+ tar:xz
USE_GNOME=	cairo gdkpixbuf2 gtk30 gtksourceview4 introspection
USE_CSTD=	c11
USE_LDCONFIG=	yes
GNU_CONFIGURE=	yes
INSTALL_TARGET=	install-strip

GLIB_SCHEMAS=	org.gnome.gedit.plugins.drawspaces.gschema.xml \
		org.gnome.gedit.plugins.wordcompletion.gschema.xml

USES+=		python
BUILD_DEPENDS+=	py3?-dbus>=0:devel/py-dbus@${PY_FLAVOR}
LIB_DEPENDS+=	libgucharmap_2_90.so:deskutils/gucharmap
RUN_DEPENDS+=	py3?-dbus>=0:devel/py-dbus@${PY_FLAVOR} \
		${PYTHON_PKGNAMEPREFIX}libpeas>=0:devel/py-libpeas@${PY_FLAVOR}
USE_GNOME+=	vte3
CONFIGURE_ARGS+=--enable-python
GLIB_SCHEMAS+=	org.gnome.gedit.plugins.terminal.gschema.xml \
		org.gnome.gedit.plugins.translate.gschema.xml

post-patch:
	@${REINPLACE_CMD} -e 's| \\t\\r\\n\\f|[:space:]|g' \
		${WRKSRC}/configure

PLUGINDIRS=	${PREFIX}/lib/gedit/plugins ${PREFIX}/share/gedit/plugins

.include <bsd.port.pre.mk>

post-install:
# generates .py[co] files for installed modules
	@${FIND} ${STAGEDIR}${PREFIX} -name \*.pyc -delete
.for dir in ${PLUGINDIRS}
	(cd ${STAGEDIR}${PREFIX} && \
	${PYTHON_CMD} ${PYTHON_LIBDIR}/compileall.py \
		-d ${dir} -f ${dir:S;${PREFIX}/;;} && \
	${PYTHON_CMD} -O ${PYTHON_LIBDIR}/compileall.py \
		-d ${dir} -f ${dir:S;${PREFIX}/;;})
.endfor

.include <bsd.port.post.mk>
