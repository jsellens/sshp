# sample dead simple Makefile to install sshe and sshp

PREFIX	= /usr/local

all:
	echo nothing to make, just install

install:
	install -c -m 755 sshe ${PREFIX}/bin/sshe
	install -c -m 755 sshp ${PREFIX}/bin/sshp
	install -c -m 644 sshe.1 ${PREFIX}/share/man/man1/sshe.1
	install -c -m 644 sshp.1 ${PREFIX}/share/man/man1/sshp.1

clean:
	echo nothing to clean, just install
