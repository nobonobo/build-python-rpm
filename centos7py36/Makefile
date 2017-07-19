TARGET:=python36-3.6.2
OUTPUT:=$(TARGET)-1.el7.x86_64.rpm
build: $(OUTPUT)

$(OUTPUT): Dockerfile
	docker build --rm -t localhost/cent7py36rpm .
	ID=$$(docker create localhost/cent7py36rpm) \
	&& docker cp $$ID:/$(TARGET)-1.x86_64.rpm $(TARGET)-1.el7.x86_64.rpm \
	&& docker rm $$ID
