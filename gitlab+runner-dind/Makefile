# Load default configuration
-include .env.example

DOCKER_CMD := docker

DOCKER_COMPOSE_FILE := compose.yml
DOCKER_COMPOSE_CMD := $(DOCKER_CMD) compose -f $(DOCKER_COMPOSE_FILE)

# Override with custom configuration
-include .env

start:
	$(DOCKER_COMPOSE_CMD) up -d

up: start

start_%:
	$(DOCKER_COMPOSE_CMD) up -d $*

stop:
	$(DOCKER_COMPOSE_CMD) down --remove-orphans

down: stop

stop_%:
	$(DOCKER_COMPOSE_CMD) stop $*
	$(DOCKER_COMPOSE_CMD) rm -f $*

down: stop

pull:
	$(DOCKER_COMPOSE_CMD) pull

pull_%:
	$(DOCKER_COMPOSE_CMD) pull $*

logs:
	$(DOCKER_COMPOSE_CMD) logs --tail 100

logsf:
	$(DOCKER_COMPOSE_CMD) logs --tail 100 -f

logs_%:
	$(DOCKER_COMPOSE_CMD) logs --tail 100 $*

logsf_%:
	$(DOCKER_COMPOSE_CMD) logs --tail 100 -f $*

status:
	$(DOCKER_COMPOSE_CMD) ps -a

stats:
	docker stats

restart_%:
	$(DOCKER_COMPOSE_CMD) stop $*
	$(DOCKER_COMPOSE_CMD) rm -f $*
	$(DOCKER_COMPOSE_CMD) up -d $*

restart: stop start

build:
	$(DOCKER_COMPOSE_CMD) build

buildnc:
	$(DOCKER_COMPOSE_CMD) build --no-cache

__clean:
	$(DOCKER_COMPOSE_CMD) down -v --remove-orphans
	sudo rm -rf dind_data gitlab_data runner_data certs
