###########################################################
# Dockerfile that builds a CS2 Gameserver
###########################################################
FROM cm2network/steamcmd:root as build_stage

LABEL maintainer="fla@grapps.fr"

ENV STEAMAPPID 223350
ENV STEAMAPP dayz
ENV STEAMAPPDIR "${HOMEDIR}/${STEAMAPP}-dedicated"

COPY etc/entry.sh "${HOMEDIR}/entry.sh"
COPY etc/server.cfg "${STEAMAPPDIR}/game/csgo/cfg/server.cfg"

RUN set -x \
	# Install, update & upgrade packages
	&& apt-get update \
	&& apt-get install -y --no-install-recommends --no-install-suggests \
		wget=1.21-1+deb11u1 \
		ca-certificates=20210119 \
		lib32z1=1:1.2.11.dfsg-2+deb11u2 \
	&& mkdir -p "${STEAMAPPDIR}" \
	# Add entry script
	&& chmod +x "${HOMEDIR}/entry.sh" \
	&& chown -R "${USER}:${USER}" "${HOMEDIR}/entry.sh" "${STEAMAPPDIR}" \
	# Clean up
	&& rm -rf /var/lib/apt/lists/* 

FROM build_stage AS bullseye-base

ENV CS2_SERVERNAME="New \"${STEAMAPP}\" Server" \
    CS2_PORT=27015 \
    CS2_MAXPLAYERS=10 \
    CS2_RCONPW="changeme" \
    CS2_IP=0.0.0.0 \
    CS2_PW="changeme" \
    CS2_MAPGROUP="mg_active" \    
    CS2_STARTMAP="de_dust2" \
    CS2_GAMETYPE=0 \
    CS2_GAMEMODE=1 \
    CS2_LAN=0 \
    CS2_STEAMTOKEN="changeme" \
    CS2_ADDITIONAL_ARGS=""

# Switch to user
USER ${USER}

WORKDIR ${HOMEDIR}

CMD ["bash", "entry.sh"]

# 27015/udp = game traffic
# 27015/tcp = rcon traffic
EXPOSE 27015/tcp \
       27015/udp
