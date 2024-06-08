##################################################
### Common

GIT_COMMIT= $(shell git rev-parse HEAD)
COMPOSE_FILE := docker-compose.yml
UNAME_S := $(shell uname -s 2>/dev/null || echo Not_Unix)
IS_WINDOWS := $(filter Not_Unix,$(UNAME_S))


##################################################
# application up
##################################################
app_up:
ifeq ($(IS_WINDOWS),Not_Unix)
	@echo "Running application  on Windows..."
	@powershell -Command "docker-compose -f $(COMPOSE_FILE) up --build -d frontend backend;
else
	@echo "Running application  on Unix/Linux/Mac..."
	@docker-compose -f $(COMPOSE_FILE) up --build -d frontend backend;
endif

# Helper Commands
##################################################
lint:
	black .
	isort .
