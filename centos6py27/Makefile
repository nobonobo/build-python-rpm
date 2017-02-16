TARGET:=python27-2.7.13
OUTPUT:=$(TARGET)-1.el6.x86_64.rpm
build: $(OUTPUT)

$(OUTPUT): Dockerfile
	docker build --rm -t localhost/cent6py27rpm .
	ID=$$(docker create localhost/cent6py27rpm) \
	&& docker cp $$ID:/$(TARGET)-1.x86_64.rpm $(TARGET)-1.el6.x86_64.rpm \
	&& docker rm $$ID
