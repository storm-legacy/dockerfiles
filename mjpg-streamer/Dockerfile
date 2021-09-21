#
#	STAGE 1 - Build image with Archlinux
#
FROM archlinux:base-devel as builder

WORKDIR /src

# Install dependencies
RUN pacman -Sy --noconfirm git cmake libjpeg-turbo libjpeg6-turbo

# Clone Sources
RUN git clone https://github.com/jacksonliam/mjpg-streamer.git .
	
	
# Build app
RUN cd mjpg-streamer-experimental && make

#
#	STAGE 2 - Insert built sources to final image
#
FROM ubuntu:20.04
LABEL org.opencontainers.image.authors="storm-legacy <storm.legacy@github.com>"


# Copy build
COPY --from=builder [ "/src/mjpg-streamer-experimental/_build/mjpg_streamer", "/usr/bin/mjpg-streamer" ]
COPY --from=builder [ "/src/mjpg-streamer-experimental/*.so", "/usr/lib64/mjpg-streamer/" ]
COPY --from=builder [ "/src/mjpg-streamer-experimental/www/", "/srv/www/" ]

# Setup env
ENV LD_LIBRARY_PATH="/lib64/mjpg-streamer"

# Install libraries
RUN apt-get update && \
	apt-get install --no-install-recommends libjpeg-turbo8 && \
	rm -rf /var/lib/apt/lists/*
	
# Set default arguments
ENV INPUT_ARGS="-d /dev/video0 -l on"
ENV OUTPUT_ARGS="-p 8090"

# Set commands
CMD mjpg-streamer -i "input_uvc.so ${INPUT_ARGS}" -o "output_http.so -w /srv/www ${OUTPUT_ARGS}"