FROM php:7.1-cli

RUN apt-get update \
	&& apt-get -y --no-install-recommends dist-upgrade 														\
	&& apt-get install -y --no-install-recommends 															\
		git 																								\
		wget 																								\
		cron 																								\
		curl																								\
		libc-client-dev 																					\
		libicu-dev 																							\
		libmcrypt-dev 																						\
		libkrb5-dev 																						\
		libssl-dev 																							\
		libz-dev 																							\
		openssh-client 																						\
		unzip 																								\
		zip 																								\
		coreutils 																							\
	&& curl -sL https://deb.nodesource.com/setup_12.x | bash -												\
	&& apt-get install -y nodejs 																			\
	&& apt-get -y --auto-remove -o APT::AutoRemove::RecommendsImportant=false purge 						\
	&& rm -rf /var/lib/apt/lists/*

RUN pecl install mcrypt-1.0.0

RUN docker-php-ext-configure imap --with-imap --with-imap-ssl --with-kerberos 								\
	&& docker-php-ext-configure opcache --enable-opcache 													\
	&& docker-php-ext-install imap intl mbstring mbstring mysqli pdo_mysql zip opcache bcmath 				\
	&& docker-php-ext-enable imap intl mbstring mcrypt mysqli pdo_mysql zip opcache bcmath

ENV SUPERCRONIC_URL=https://github.com/aptible/supercronic/releases/download/v0.1.9/supercronic-linux-amd64 \
    SUPERCRONIC=supercronic-linux-amd64 

RUN curl -fsSLO "$SUPERCRONIC_URL" && chmod +x "$SUPERCRONIC" && mv "$SUPERCRONIC" "/sbin/supercronic"

ENV TINI_URL=https://github.com/krallin/tini/releases/download/v0.18.0/tini-amd64 \
	TINI=tini-amd64

RUN curl -fsSLO "$TINI_URL" && chmod +x "$TINI" && mv "$TINI" "/sbin/tini"

RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/bin --filename=composer

RUN npm install -g grunt

ENTRYPOINT [ "/sbin/tini", "--" ]
CMD [ "/sbin/supercronic", "/crontab/crontab" ]