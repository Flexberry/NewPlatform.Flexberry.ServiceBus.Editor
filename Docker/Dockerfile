FROM mono:latest as backend

ARG TAG
RUN	echo TAG=$TAG ; \
	mono --version ; \
    apt update ; \
	apt install -y git ; \
	git clone --depth=50 --branch=$TAG https://github.com/Flexberry/NewPlatform.Flexberry.ServiceBus.Editor.git /Flexberry/NewPlatform.Flexberry.ServiceBus.Editor ; \
	msbuild /version ; \
	cd /Flexberry/NewPlatform.Flexberry.ServiceBus.Editor ; \
	nuget restore NewPlatform.Flexberry.ServiceBus.Editor.sln ; \
	msbuild /p:Configuration=Release NewPlatform.Flexberry.ServiceBus.Editor.sln


FROM node:6.13 as ember_app

ARG TAG
RUN	echo TAG=$TAG ; \
	node --version ; npm --version ; \
	yarn global add ember-cli@2.4.3 ; \
	yarn global add bower ; \
	git clone --depth=50 --branch=$TAG https://github.com/Flexberry/flexberry-service-bus-editor.git /Flexberry/flexberry-service-bus-editor ; \
	cd /Flexberry/flexberry-service-bus-editor ; \
	yarn install ; \
	bower install ; \
	ember build



FROM flexberry/servicebuseditor

MAINTAINER mail@flexberry.net

RUN 	rm -rf /var/www/vhosts/ServiceBusEditor/*

COPY --from=backend /Flexberry/NewPlatform.Flexberry.ServiceBus.Editor/bin/Release /var/www/vhosts/ServiceBusEditor
COPY --from=ember_app /Flexberry/flexberry-service-bus-editor /var/www/vhosts/ServiceBusEditor

RUN 	rm /var/www/vhosts/ServiceBusEditor/bin/Mono.Security.dll ; \
	rm /var/www/vhosts/ServiceBusEditor/Web.config
