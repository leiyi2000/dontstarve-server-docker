###################################################################
# Dockerfile that builds a Don't Starve Together dedicated server #
###################################################################

# First stage: Download game
FROM cm2network/steamcmd:root AS downloader

# Build Arguments for downloader
ARG DEBIAN_FRONTEND=noninteractive
ARG LANG=C.UTF-8
ARG LC_ALL=C.UTF-8
ARG DST_BRANCH
ARG DST_BUILDID
ARG DST_TIMEUPDATED

ENV STEAM_APPID=343050
ENV STEAM_WORKSHOPID=322330
ENV DST_DIR=${HOMEDIR}/dst
ENV DST_GAMEDIR=${DST_DIR}/game
ENV DST_SAVEDIR=${DST_DIR}/save
ENV DST_MODDIR=${DST_GAMEDIR}/mods
ENV DST_UGCMODDIR=${DST_GAMEDIR}/ugc_mods

RUN dpkg --add-architecture i386 && \
    apt-get update && \
    apt-get install -y lib32gcc-s1 lib32stdc++6 libcurl4:i386 && \
    apt-get clean && rm -rf /var/lib/apt/lists/*

RUN mkdir -p "${DST_DIR}" "${DST_GAMEDIR}" "${DST_SAVEDIR}" "${DST_MODDIR}" "${DST_UGCMODDIR}" && \
    chown -R root:root "${STEAMCMDDIR}"

RUN bash "${STEAMCMDDIR}/steamcmd.sh" \
    +force_install_dir ${DST_GAMEDIR} \
    +login anonymous \
    +app_update ${STEAM_APPID} -beta "${DST_BRANCH}" validate \
    +quit

# Second stage: Setup environment
FROM downloader AS game

# Build Arguments
ARG DST_BRANCH
ARG DST_BUILDID
ARG DST_TIMEUPDATED
ARG DST_VERSION

# Metadata
LABEL branch="${DST_BRANCH}"
LABEL buildid="${DST_BUILDID}"
LABEL timeupdated="${DST_TIMEUPDATED}"
LABEL version="${DST_VERSION}"

# Additional environment settings
ENV DST_VERSION=${DST_VERSION}

# Set the working directory
WORKDIR ${DST_GAMEDIR}/bin64

# Specify the entrypoint
ENTRYPOINT ["./dontstarve_dedicated_server_nullrenderer_x64"]