#	$OpenBSD: Makefile,v 1.20 2018/05/22 19:15:22 cheloha Exp $

PROG=	kqueue-test
CFLAGS+=-Wall
SRCS=	kqueue-pipe.c kqueue-fork.c main.c kqueue-process.c kqueue-random.c \
	kqueue-pty.c kqueue-tun.c kqueue-signal.c kqueue-fdpass.c \
	kqueue-flock.c kqueue-timer.c
LDADD=	-levent -lutil
DPADD=	${LIBEVENT} ${LIBUTIL}

kq-pipe: ${PROG}
	./${PROG} -p
kq-fork: ${PROG}
	./${PROG} -f
kq-process: ${PROG}
	./${PROG} -P
kq-random: ${PROG}
	./${PROG} -r
kq-tun: ${PROG}
	@-${SUDO} sh -c 'cd /dev && sh MAKEDEV tun98 tun99'
	@EVENT_SHOW_METHOD=yes EVENT_NOPOLL=yes EVENT_NOSELECT=yes ${SUDO} ./${PROG} -t
	@EVENT_SHOW_METHOD=yes EVENT_NOPOLL=yes EVENT_NOKQUEUE=yes ${SUDO} ./${PROG} -t
	@EVENT_SHOW_METHOD=yes EVENT_NOSELECT=yes EVENT_NOKQUEUE=yes ${SUDO} ./${PROG} -t
	@-${SUDO} rm -f /dev/tun98 /dev/tun99
kq-pty: ${PROG}
	${SUDO} ./${PROG} -T
kq-signal: ${PROG}
	./${PROG} -s
kq-fdpass: ${PROG}
	./${PROG} -F
kq-flock: ${PROG}
	./${PROG} -l
kq-timer: ${PROG}
	./${PROG} -i
kq-invalid-timer: ${PROG}
	./${PROG} -I

REGRESS_TARGETS=kq-pipe kq-fork kq-process kq-random kq-pty kq-signal \
	kq-fdpass kq-flock kq-timer kq-invalid-timer
# kq-tun broke at some point, apparently from a change in tun routing
REGRESS_ROOT_TARGETS=${REGRESS_TARGETS}
.PHONY: ${REGRESS_TARGETS}

.include <bsd.regress.mk>
