# Docker commands
DOCKER_COMPOSE = docker-compose

##
## Project
## -------
##

build: ## Build the project
	COMPOSE_DOCKER_CLI_BUILD=1 DOCKER_BUILDKIT=1 $(DOCKER_COMPOSE) build --pull

push: ## Push the project
push: build
	$(DOCKER_COMPOSE) push

.DEFAULT_GOAL := help
help:
	@printf " \033[33mCommands\033[0m:\n"
	@grep -E '(^[a-zA-Z_-]+:.*?##.*$$)|(^##)' Makefile | awk 'BEGIN {FS = ":.*?## "}; {printf "  \033[32m%-20s\033[0m %s\n", $$1, $$2}' | sed -e 's/\[32m##/[33m/'

.PHONY: help build
