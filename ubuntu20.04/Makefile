IMG=modsecurity-nginx-focal

all: build

build:
	docker build -t ${IMG} .

run:
	mkdir -p out
	docker run -it -v `pwd`/out:/out ${IMG} /bin/bash
