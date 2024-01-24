ENV ?= dev
SHELL := /bin/bash


.ONESHELL:

# ifeq ($(ENV), dev)
# 	include .env.dev
# 	$(shell export $(grep -v '^#' .env.dev | xargs))
# else ifeq ($(ENV), test)
# 	include .env.test
# 	$(shell export $(grep -v '^#' .env.test | xargs))
# else ifeq ($(ENV), prod)
# 	include .env.prod
# 	$(shell export $(grep -v '^#' .env.prod | xargs))
# endif

deploy:
	@echo "deploy files ...."
	@source .env.$(ENV)
	@echo $$BASE_PATH
	@echo $$NODE_NAME
	mkdir -p $$BASE_PATH/$$NODE_NAME/mariadb/conf.d
	@echo $$BASE_PATH/$$NODE_NAME/mariadb/conf.d
	mkdir -p $$BASE_PATH/$$NODE_NAME/mariadb/data
	cp ./mariadb.conf $$BASE_PATH/$$NODE_NAME/mariadb/conf.d/mariadb.conf
	@echo $$BASE_PATH/$$NODE_NAME/mariadb/conf.d/mariadb.conf
	mkdir -p $$BASE_PATH/$$NODE_NAME/mongodb/init
	cp ./init.js $$BASE_PATH/$$NODE_NAME/mongodb/init/init.js
	mkdir -p $$BASE_PATH/$$NODE_NAME/docs
	mkdir -p $$BASE_PATH/$$NODE_NAME/arc-storage/json
	mkdir -p $$BASE_PATH/$$NODE_NAME/arc-storage/files
	mkdir -p $$BASE_PATH/$$NODE_NAME/arc-storage/record
	mkdir -p $$BASE_PATH/$$NODE_NAME/arc-storage/data
	mkdir -p $$BASE_PATH/$$NODE_NAME/nginx/conf.d
	cp ./nginx.conf $$BASE_PATH/$$NODE_NAME/nginx/conf.d/default.conf

run:
	@echo "start compose..." 
	@source .env
	@source .env.$(ENV)
	@echo "##################################"
	@echo "IMAGE_ARC_STORAGE:            " $$IMAGE_ARC_STORAGE
	@echo "IMAGE_WEB:             " $$IMAGE_WEB
	@echo "NODE_NAME:             " $$NODE_NAME
	@echo "NODE_HOST:             " $$NODE_HOST
	@echo "WEB_PORT:              " $$WEB_PORT
	@echo "MARIADB_PORT:          " $$MARIADB_PORT
	@echo "REDIS_PORT:            " $$REDIS_PORT
	@echo "MONGO_PORT:            " $$MONGO_PORT
	@echo "MEXPRESS_PORT:         " $$MEXPRESS_PORT
	@echo "MEXPRESS_EXPORT:       " $$MEXPRESS_EXPORT
	@echo "##################################"
	@echo "MYSQL_USER:            " $$MYSQL_USER
	@echo "MYSQL_DATABASE:        " $$MYSQL_DATABASE
	@echo "MYSQL_ROOT_PASSWORD:   " $$MYSQL_ROOT_PASSWORD
	@echo "ARC_STORAGE_MONGO_OPTIONS:    " $$ARC_STORAGE_MONGO_OPTIONS
	@echo "MEXPRESS_USERNAME:     " $$MEXPRESS_USERNAME
	@echo "MEXPRESS_PASSWORD:     " $$MEXPRESS_PASSWORD
	@echo "MONGO_ROOT_USERNAME:   " $$MONGO_ROOT_USERNAME
	@echo "MONGO_ROOT_PASSWORD:   " $$MONGO_ROOT_PASSWORD
	@echo "MONGO_INITDB_DATABASE: " $$MONGO_INITDB_DATABASE
	@echo "COMPOSE_PROJECT_NAME:  " $$NODE_NAME
	@echo "##################################"

	IMAGE_ARC_STORAGE=$$IMAGE_ARC_STORAGE IMAGE_WEB=$$IMAGE_WEB \
	NODE_NAME=$$NODE_NAME NODE_HOST=$$NODE_HOST WEB_PORT=$$WEB_PORT \
	COMPOSE_PROJECT_NAME=$$NODE_NAME \

	docker compose -p $$COMPOSE_PROJECT_NAME up -d

clean:
	echo "stop and clean compose"
	@source .env
	@source .env.$(ENV)
	NODE_NAME=$$NODE_NAME COMPOSE_PROJECT_NAME=$$NODE_NAME docker compose stop
	NODE_NAME=$$NODE_NAME COMPOSE_PROJECT_NAME=$$NODE_NAME docker compose rm

portainer:
	docker volume create portainer_data
	docker run -d -p 8000:8000 -p 9443:9443 --name portainer --restart=always \
	-v /var/run/docker.sock:/var/run/docker.sock \
	-v portainer_data:/data \
	portainer/portainer-ce:latest
	