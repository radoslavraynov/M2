include ../Makefile

## Parameters
TAG = m2-cmake-build
STORAGE = `pwd`/../storage
M2_REPO = `pwd`/../../../..
M2_HOME = /home/macaulay
VOLUME = --volume $(STORAGE):$(M2_HOME) --volume $(M2_REPO):$(M2_HOME)/M2
BUILD_DIR = M2/M2/BUILD/build-docker
BUILD_OPT = -DCMAKE_BUILD_TYPE=Release -DUSING_MPIR=OFF

## Script for building Macaulay2
define M2_BUILD_SCRIPT
set -xe
## Full build
cmake -GNinja -S M2/M2 -B $(BUILD_DIR) $(BUILD_OPT)
cmake --build $(BUILD_DIR) --target build-libraries # build-programs
cmake --build $(BUILD_DIR) --target M2-core # install-packages
cmake --build $(BUILD_DIR) --target M2-emacs
## Changes to the docker container after it is created are ephemeral,
## but this is how one would install Macaulay2:
# cmake --install $(BUILD_DIR)
endef
export M2_BUILD_SCRIPT

###############################################################################

build: build-image
	docker run $(VOLUME) -it --entrypoint="" $(TAG) bash -c "$$M2_BUILD_SCRIPT"

# For anything else, run it in the build directory within the container
%:;	docker run $(VOLUME) -it --entrypoint="" $(TAG) cmake --build $(M2_HOME)/$(BUILD_DIR) --target $@
