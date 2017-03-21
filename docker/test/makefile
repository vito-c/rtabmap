Dockerfile: Dockerfile.in *.docker
	cpp -o Dockerfile Dockerfile.in

build: Dockerfile
	docker build -rm -t my/image .
