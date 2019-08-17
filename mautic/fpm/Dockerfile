FROM php:7.1-fpm

RUN apt-get update \
	&& apt-get -y --no-install-recommends dist-upgrade 											\
	&& apt-get install -y --no-install-recommends 												\
		wget 																					\
		libc-client-dev 																		\
		libicu-dev 																				\
		libmcrypt-dev 																			\
		libkrb5-dev 																			\
		libssl-dev 																				\
		libz-dev 																				\
		openssh-client 																			\
		unzip 																					\
		zip 																					\
		coreutils 																				\
	&& apt-get -y --auto-remove -o APT::AutoRemove::RecommendsImportant=false purge 			\
	&& rm -rf /var/lib/apt/lists/*

RUN pecl install mcrypt-1.0.0

RUN docker-php-ext-configure imap --with-imap --with-imap-ssl --with-kerberos 					\
	&& docker-php-ext-configure opcache --enable-opcache 										\
	&& docker-php-ext-install imap intl mbstring mbstring mysqli pdo_mysql zip opcache bcmath 	\
	&& docker-php-ext-enable imap intl mbstring mcrypt mysqli pdo_mysql zip opcache bcmath

ENV TINI_URL=https://github.com/krallin/tini/releases/download/v0.18.0/tini-amd64 \
	TINI=tini-amd64

RUN curl -fsSLO "$TINI_URL" && chmod +x "$TINI" && mv "$TINI" "/sbin/tini"

EXPOSE 9000
STOPSIGNAL SIGQUIT

ENTRYPOINT [ "/sbin/tini", "--" ]
CMD [ "php-fpm", "-F" ]