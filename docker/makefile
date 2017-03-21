.DEFAULT_GOAL := all

# brew install gettext first
ENVSUB=/usr/local/opt/gettext/bin/envsubst

define build_container
	sed 's/%REPLACE_FROM%/$(1)/' ./$(2)/Dockerfile > ./$(2)/Dockerfile.tmp
	docker build -t rtabmap:$(2) -f ./$(2)/Dockerfile.tmp ./$(2)/
	rm ./$(2)/Dockerfile.tmp
endef

.PHONY: base
base: ./_base/Dockerfile
	docker build -t rtabmap:base ./_base

.PHONY: android_only
android_only: ./android/Dockerfile
	$(call build_container,ubuntu:latest,android)

.PHONY: android
android: base ./android/Dockerfile
	# first prereq is $< = base and target name is $@
	$(call build_container,rtabmap:$<,$@)

.PHONY: eigen
eigen: android ./eigen/Dockerfile
	$(call build_container,rtabmap:$<,$@)

.PHONY: boost
boost: eigen ./boost/Dockerfile
	$(call build_container,rtabmap:$<,$@)

.PHONY: flann
flann: boost ./flann/Dockerfile
	$(call build_container,rtabmap:$<,$@)

.PHONY: qhull
qhull: flann ./qhull/Dockerfile
	$(call build_container,rtabmap:$<,$@)

.PHONY: qt
qt: ghull ./qt/Dockerfile
	$(call build_container,rtabmap:$<,$@)

.PHONY: opencv
opencv: qt ./opencv/Dockerfile
	$(call build_container,rtabmap:$<,$@)

.PHONY: pcl
pcl: opencv ./pcl/Dockerfile
	$(call build_container,rtabmap:$<,$@)

# combine with trabmap?
.PHONY: tango
tango: pcl ./tango/Dockerfile
	$(call build_container,rtabmap:$<,$@)

.PHONY: rtabmap
rtabmap: tango ./rtabmap/Dockerfile
	$(call build_container,rtabmap:$<,$@)

# #libusb






# files :=$(shell find . -name \*.docker)
# Dockerfile: Dockerfile.in $(files)
# 	@echo $^	
